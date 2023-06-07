---
title: 如何使用 js 向 iframe 内传参
cover: https://images.pexels.com/photos/12136072/pexels-photo-12136072.jpeg
date: 2023-02-11 12:00:00
updated: 2023-02-11 12:00:00
---
# 1. 父页面
1. 获取 iframe 对象：可以通过 document.getElementById() 或 document.querySelector() 方法获取到 iframe 对象
```js
var iframe = document.getElementById('myIframe');
```
2. 使用 contentWindow 属性获取到 iframe 内部的 window 对象。

```js
var iframeWindow = iframe.contentWindow;
```
3. 在 iframe 内部的 window 对象上调用 `postMessage` 方法，将参数传递给 iframe 内部
```js
// 向 iframe 标签内部传递参数 param1 和 param2
iframeWindow.postMessage({param1: 'value1', param2: 'value2'}, src);
// src 写成子页面的域名或者是ip
```

# 2. 子页面
1. 通过以下代码接收来自父页面的参数
```js
window.addEventListener('message', function(event) {
  // 只接收来自指定域名（或所有域名 - *）的消息
  if (event.origin !== 'http://example.com') return;
  
  var message = event.data;
  console.log(message.param1); // 输出 'value1'
  console.log(message.param2); // 输出 value2
});
```