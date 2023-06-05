---
title: React 调用手机摄像头识别二维码
cover: https://images.pexels.com/photos/16780574/pexels-photo-16780574.jpeg
date: 2021-11-20 12:00:00
updated: 2021-11-20 12:00:00
---
# 1. 安装插件 jsqr
```powershell
npm i jsqr --save
```
jsqr 导出一个方法，该方法接受 3 个参数，表示您要解码的图像数据。另外，可以使用选项对象来进一步配置扫描行为。
```js
const code = jsQR（imageData，width，height，options ？）;
if（code）{
 console.log（“找到二维码”，code）;
}
```
- imageData- Uint8ClampedArray 表单中的一个 RGBA 像素值 [r0, g0, b0, a0, r1, g1, b1, a1, ...]。因此，这个数组的长度应该是 4 * width * height。此数据与 ImageData 接口的格式相同，并且通常 由节点模块返回以读取图像。
- width - 要解码的图像的宽度。
- height - 要解码的图像的高度。
- options （可选） - 附加选项。

另外，需要结合 [navigator.mediaDevices.getUserMedia](https://developer.mozilla.org/zh-CN/docs/Web/API/MediaDevices/getUserMedia) 获取这三个参数，通过mediaDevices.getUserMedia方法调起录像功能，把视频赋给video，在截取视频中的某一帧传给jsqr进行解析，如果解析出二维码则返回解析数据，没有的话就继续截取。

# 2. 实现
1. 调起摄像头拍摄视频
```js
// 老的浏览器可能根本没有实现 mediaDevices，所以我们可以先设置一个空的对象

if (navigator.mediaDevices === undefined) {
  navigator.mediaDevices = {};
}

// 一些浏览器部分支持 mediaDevices。我们不能直接给对象设置 getUserMedia
// 因为这样可能会覆盖已有的属性。这里我们只会在没有getUserMedia属性的时候添加它。
if (navigator.mediaDevices.getUserMedia === undefined) {
  navigator.mediaDevices.getUserMedia = function(constraints) {

    // 首先，如果有getUserMedia的话，就获得它
    var getUserMedia = navigator.webkitGetUserMedia || navigator.mozGetUserMedia;

    // 一些浏览器根本没实现它 - 那么就返回一个error到promise的reject来保持一个统一的接口
    if (!getUserMedia) {
      return Promise.reject(new Error('getUserMedia is not implemented in this browser'));
    }

    // 否则，为老的navigator.getUserMedia方法包裹一个Promise
    return new Promise(function(resolve, reject) {
      getUserMedia.call(navigator, constraints, resolve, reject);
    });
  }
}

navigator.mediaDevices.getUserMedia({ audio: true, video: true })
.then(function(stream) {
  var video = document.querySelector('video');
  // 旧的浏览器可能没有srcObject
  if ("srcObject" in video) {
    video.srcObject = stream;
  } else {
    // 防止在新的浏览器里使用它，应为它已经不再支持了
    video.src = window.URL.createObjectURL(stream);
  }
  video.onloadedmetadata = function(e) {
    video.play();
  };
})
.catch(function(err) {
  console.log(err.name + ": " + err.message);
});
```
2. 结合jsqr解析视频中出现的二维码
```js
import React, { PureComponent } from 'react';
import '../index.less';
import jsQR from 'jsqr';

let scanner = null;

let video = null;
let canvasElement = null;
let canvas = null;
let loadingMessage = null;
let outputContainer = null;
let outputMessage = null;
let outputData = null;

export default class scanPage extends PureComponent {
  componentDidMount() {
    const that = this;
    video = document.createElement('video');
    canvasElement = document.getElementById('canvas');
    canvas = canvasElement.getContext('2d');
    loadingMessage = document.getElementById('loadingMessage');
    outputContainer = document.getElementById('output');
    outputMessage = document.getElementById('outputMessage');
    outputData = document.getElementById('outputData');

    console.log(navigator.mediaDevices )

    // 老的浏览器可能根本没有实现 mediaDevices，所以我们可以先设置一个空的对象
    if (navigator.mediaDevices === undefined) {
      navigator.mediaDevices = {};
    }

    // 一些浏览器部分支持 mediaDevices。我们不能直接给对象设置 getUserMedia
    // 因为这样可能会覆盖已有的属性。这里我们只会在没有getUserMedia属性的时候添加它。
    if (navigator.mediaDevices.getUserMedia === undefined) {
      navigator.mediaDevices.getUserMedia = function (constraints) {
        // 首先，如果有getUserMedia的话，就获得它
        var getUserMedia = navigator.webkitGetUserMedia || navigator.mozGetUserMedia;

        // 一些浏览器根本没实现它 - 那么就返回一个error到promise的reject来保持一个统一的接口
        if (!getUserMedia) {
          return Promise.reject(new Error('getUserMedia is not implemented in this browser'));
        }

        // 否则，为老的navigator.getUserMedia方法包裹一个Promise
        return new Promise(function (resolve, reject) {
          getUserMedia.call(navigator, constraints, resolve, reject);
        });
      };
    }

    navigator.mediaDevices
      .getUserMedia({ audio: false, video: { facingMode: "environment" } })
      .then(function (stream) {
        // 旧的浏览器可能没有srcObject
        video.srcObject = stream;
        video.setAttribute('playsinline', true); // required to tell iOS safari we don't want fullscreen
        video.play();
        window.requestAnimationFrame(that.tick);
      })
      .catch(function (err) {
        console.log(err.name + ': ' + err.message);
      });
  }

  handleStartCode = () => {
    scanner.start();
  };

  componentWillUnmount() {}

  drawLine = (begin, end, color) => {
    canvas.beginPath();
    canvas.moveTo(begin.x, begin.y);
    canvas.lineTo(end.x, end.y);
    canvas.lineWidth = 4;
    canvas.strokeStyle = color;
    canvas.stroke();
  };

  tick = () => {
    loadingMessage.innerText = '⌛ Loading video...';
    if (video.readyState === video.HAVE_ENOUGH_DATA) {
      loadingMessage.hidden = true;
      canvasElement.hidden = false;
      outputContainer.hidden = false;

      canvasElement.height = video.videoHeight;
      canvasElement.width = video.videoWidth;
      canvas.drawImage(video, 0, 0, canvasElement.width, canvasElement.height);
      var imageData = canvas.getImageData(0, 0, canvasElement.width, canvasElement.height);
      var code = jsQR(imageData.data, imageData.width, imageData.height, {
        inversionAttempts: 'dontInvert',
      });
      if (code && code.data) {
        console.log(code);
        this.drawLine(code.location.topLeftCorner, code.location.topRightCorner, '#FF3B58');
        this.drawLine(code.location.topRightCorner, code.location.bottomRightCorner, '#FF3B58');
        this.drawLine(code.location.bottomRightCorner, code.location.bottomLeftCorner, '#FF3B58');
        this.drawLine(code.location.bottomLeftCorner, code.location.topLeftCorner, '#FF3B58');
        outputMessage.hidden = true;
        outputData.parentElement.hidden = false;
        outputData.innerText = code.data;
        this.props.history.replace(
          `/client-mng/individual-business-mng/list/detail/${code.data}-selfEERegister-scan`
        );
      } else {
        outputMessage.hidden = false;
        outputData.parentElement.hidden = true;
      }
    }
    window.requestAnimationFrame(this.tick);
  };

  render() {
    return (
      <div>
        <div id="loadingMessage">
          🎥 Unable to access video stream (please make sure you have a webcam enabled)
        </div>
        <canvas id="canvas" hidden></canvas>
        <div id="output" hidden>
          <div id="outputMessage">No QR code detected.</div>
          <div hidden>
            <b>Data:</b> <span id="outputData"></span>
          </div>
        </div>
      </div>
    );
  }
}
```
# 3. 注意
1. 调起摄像头必须在https的协议下，否则会不起作用
2. navigator.mediaDevices .getUserMedia({ audio: false, video: { facingMode: "environment" } })，这一段代码的意思是禁用录音功能，facingMode: "environment" 是强制使用后置摄像头