---
title: 前端 WebSocket 的使用
cover: https://images.pexels.com/photos/147411/italy-mountains-dawn-daybreak-147411.jpeg
date: 2022-08-17 12:00:00
updated: 2022-08-17 12:00:00
---
### 1. 什么是 WebSocket
websocket是HTML5开始提供的一种网络通信协议，它的目的是在浏览器之间建立一个不受限的双方通信的通道，比如说，服务器可以在任意时刻发送信息给浏览器。在websocket的API 中，浏览器和服务器只需要完成一次握手，两者之间就直接可以创建持久性的连接，并进行双向数据传输

### 2. WebSocket 的方法
 - ws.send() - 向服务器发送数据
 - ws.close() - 关闭连接

### 3. WebSocket 的事件
 - ws.onopen - 建立连接时触发
 - ws.onmessage - 客户端接受服务端数据时触发
 - ws.onerror - 通信错误时触发
 - ws.onclose - 连接关闭时触发
 
### 4. WebSocket.readyState
- readyState 属性返回实例对象的当前状态，共有四种状态
- 0 - 表示正在连接
- 1 - 表示连接成功，可以进行通信
- 2 - 表示连接正在关闭
- 3 - 表示连接已经关闭，或者打开连接失败

### 使用
```javascript
// 创建一个WebSocket对象
var ws = new WebSocket("接口地址")

// 连接成功时触发
ws.onopen = function() {
    alert("连接成功")
}
// 连接失败时触发
ws.onerror = function() {
    alert("连接失败")
}
// 发送数据
ws.send(); // 向服务端发送请求

// 接收消息时触发
ws.onmessage = function(MessagEvent) {
    console.log(MessagEvent.data)
}
// 连接关闭的回调函数
ws.onclose = function（）{
	alert（"close"）
}
```
