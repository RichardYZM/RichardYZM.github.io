---
title: Promise解决回调地狱
cover: https://images.pexels.com/photos/3608542/pexels-photo-3608542.jpeg
date: 2022-02-20 12:00:00
updated: 2022-02-20 12:00:00
---
### 回调函数
当一个函数作为参数传入另一个参数中，并且它不会立即执行，只有当满足一定条件后该函数才可以执行，这种函数就称为回调函数。我们熟悉的定时器和Ajax中就存在有回调函数

```javascript
function show(){
  console.log('不讲武德');
}
setTimeout(show,5000)
```
这里的回调函数是show()，在满足时间5秒后执行
### 异步操作
同步操作在主线程上排队执行,只有将上一个任务执行完后,才能进行下一步操作.异步任务不进入主线程，而是进入异步队列，前一个任务是否执行完毕不影响下一个任务的执行
```javascript
setTimeout(function(){
  console.log(123456); //后输出
},3000)
console.log(11111) //先输出
```
### 回调地狱
如果我们想按照这样的顺序请求接口,data成功后,请求data1的接口,data1接口成功后请求data2接口,为了保证请求顺序不出错,我们只能这样进行嵌套
```javascript
$.ajax({
           url: 'http://localhost:3000/data',
           success: function(data) {
               $.ajax({
                   url: 'http://localhost:3000/data1',
                   success: function(data) {
                       console.log(data)
                       $.ajax({
                           url: 'http://localhost:3000/data2',
                           success: function(data) {
                               console.log(data)
                           }
                       });
                   }
               });
           }
       });
```
代码中的回调函数套回调函数，居然套了3层，这种回调函数中嵌套回调函数的情况就叫做 回调地狱 。

总结一下，回调地狱就是为是实现代码顺序执行而出现的一种操作，它会造成我们的代码可读性非常差，后期不好维护
### promise
Promise是js中的一个原生对象，是一种异步编程的解决方案，可以替换掉传统的回调函数解决方案。

* Promise构造函数接收一个函数作为参数，我们需要处理的异步任务就卸载该函数体内，该函数的两个参数是resolve，reject。异步任务执行成功时调用resolve函数返回结果，反之调用reject。
* Promise对象的then方法用来接收处理成功时响应的数据，catch方法用来接收处理失败时相应的数据。
* Promise的链式编程可以保证代码的执行顺序，前提是每一次在than做完处理后，一定要return一个Promise对象，这样才能在下一次than时接收到数据
```javascript
// 基本Promise对象发送ajax请求
        function queryData(url){
            // 创建一个promise 实例
            var p=new Promise(function(resolve,reject){
                // 异步操作 原生ajax
                var xhr=new XMLHttpRequest();
                xhr.onreadystatechange=function(){
                    if (xhr.readyState!=4) {
                        return;
                    }
                    if (xhr.readyState==4 && xhr.status==200) {
                        // 处理正常的请求
                        resolve(xhr.responseText)
                    }else{
                        // 处理异常请求
                        reject('服务端异常错误')
                    }
                }
                xhr.open('get',url)
                xhr.send(null)
            })
            return p;
        }

        // 在then方法中  可以使用return 数据  而不是 Promise对象
        // then方法 得到异步任务正确的结果
        queryData('http://localhost:3000/data')
        .then(function(data){
            return queryData('http://localhost:3000/data1')
        })
        .then(function(data){
            return queryData('http://localhost:3000/data2')
        })
        .then(function(data){
            console.log(data);
        })
```