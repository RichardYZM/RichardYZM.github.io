---
title: js中常见的几种递归用法
cover: https://images.pexels.com/photos/1402787/pexels-photo-1402787.jpeg
date: 2022-07-21 12:00:00
updated: 2022-07-21 12:00:00
---
### 1. 什么是递归
- 函数自己调用自己本身，或者在自己函数调用的下级函数中调用自己

注意：使用递归函数要注意加上函数终止条件，避免进入死循环。

### 2. 案例
####  1. 求1,2,3,…n的和
```javascript
function sum(n){
    if( n == 1 ) return 1;
    return sum(n-1) + n;
}
```

#### 2. 求1,3,5,7,…第n项和前n项的和
1. 先求出第n项的值
```javascript
function getN(n){
    if(n == 0) return 1;
    return getN(n-1) + 2;
}
```
2. 接着求前n项的和
```javascript
function getN(n){
    if(n == 0) return 1;
    return getN(n-1) + 2;
 }

function sum(n){
    if(n == 0) return 1;
    return getN(n) + sum(n-1);
}
```

#### 3. 斐波那契数列
- 1,1,2,3,5,8,13,21,34,55,89…求第 n 项 (后一项为前两项的和)
```javascript
function fib(n){
    if(n == 0 || n ==1) return 1;
    return fib(n-1) + fib(n-2);
}
```

#### 4. 求1,2,3,…到n的阶乘
```javascript
function foo(n){
    if( n == 1) return 1;
    return foo(n - 1) * n;
}
```

#### 5. 求 n 的 m 幂次方
```javascript
function foo(n,m){
    if(m == 1) return n;
    return foo(n,m-1) * n;
}
```

#### 6. 使用递归实现深拷贝
- 如果拷贝的时候, 将数据的所有引用结构都拷贝一份, 那么数据在内存中独立就是深拷贝(内存隔离,完全独立)
- 如果拷贝的时候, 只针对当前对象的属性进行拷贝, 而属性是引用类型这个不考虑, 那就是浅拷贝

要实现深拷贝那么就需要考虑将对象的属性, 与属性的属性,都拷贝过来
```javascript
function clone(obj){
    var temp = {};	//创建一个新对象
    for(var key in obj){
        if(typeof obj[key] == 'object'){
            temp[key] = clone(obj[key]);
        }else{
            temp[key] = obj[key];
        }
    }
    return temp;		
}
```
