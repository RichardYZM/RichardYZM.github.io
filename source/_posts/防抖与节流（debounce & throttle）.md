---
title: 防抖与节流（debounce & throttle）
cover: https://images.pexels.com/photos/16780574/pexels-photo-16780574.jpeg
date: 2022-01-07 12:00:00
updated: 2022-01-07 12:00:00
---
## 防抖

 - 防抖是指当一个事件被触发时，设定一个延时时间去执行，如果在这个延时时间内又再次触发此事件，则重新开始设定延时时间去执行
![在这里插入图片描述](https://img-blog.csdnimg.cn/bda0b3d2fb744511aff6a30ed6cb0460.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YyG5YyG5Li25Li2,size_20,color_FFFFFF,t_70,g_se,x_16)
 - 例如：重复点击按钮发送 ajax 请求，可以通过防抖去避免重复发送请求
```javascript
        let time = null
        btn.onclick = function () {
            clearTimeout(time)

            time = setTimeout(() => {
                console.log('发送请求')
            }, 1000)
        }
```

## 节流
 - 节流是指设定一个周期，当一个事件触发执行后，在这个周期内再次触发这个事件，则此事件不会执行

![在这里插入图片描述](https://img-blog.csdnimg.cn/02cee7c4b88a41e8b35d08000ba9c7ab.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5YyG5YyG5Li25Li2,size_20,color_FFFFFF,t_70,g_se,x_16)
 - 例如：重复点击按钮发送 ajax 请求，可以通过节流是其发送请求的频率没这么快

```javascript
        let time = null, flag = true
        btn.onclick = function () {
            if (flag) {
                flag = false
                console.log('发送请求')

                setTimeout(() => {
                    flag = true
                }, 1000)
            }
        }
```
## lodash

 - 可以通过引入lodash直接使用防抖和节流
 
 ```javascript
 // 防抖，fn(执行函数)
 btn.onclick = _.debounce(fn, time)
 // 节流
 btn.onclick = _.throttle(fn.time)
```
