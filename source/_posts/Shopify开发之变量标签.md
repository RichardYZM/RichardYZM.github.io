---
title: Shopify开发之变量标签
cover: https://images.pexels.com/photos/1266834/pexels-photo-1266834.jpeg
date: 2023-02-22 12:00:00
updated: 2023-02-22 12:00:00
---
## 变量标签
变量标签用来创建一个 liquid 变量

### assign
创建一个命名变量
```js
{% assign favorite_food = 'apples' %}
My favorite food is {{ favorite_food }}.
```
### capture
捕获开始和结束标记内的字符串，并将其赋值给一个变量。 使用 capture 创建的变量存储为字符串
使用 capture，您可以混合使用 assign 创建的其他变量来创建复杂字符串
```js
{% assign favorite_food = 'pizza' %}
{% assign age = 35 %}
{% capture about_me %}
I am {{ age }} and my favorite food is {{ favorite_food }}.
{% endcapture %}
{{ about_me }}
```
输出：
```js
I am 35 and my favorite food is pizza. 
```
### increment
创建一个新的数字变量，并在每次调用该变量的增量时将其值增加 1。 计数器的初始值为 0
```js
{% increment counter %}
{% increment counter %}
{% increment counter %}
```
输出
```js
0
1
2
```
使用 increment 创建的变量与使用 assign 或 capture 创建的变量是分开的
```js
{% assign my_number = 10 %}
{% increment my_number %}
{% increment my_number %}
{% increment my_number %}

{{ my_number }}
```
输出
```js
0
1
2

10
```
### decrement
创建一个新的数字变量，并在每次对该变量进行递减调用时将其值减少 1。 计数器的初始值为 -1
```js
{% decrement variable %}
{% decrement variable %}
{% decrement variable %}
```
输出：
```js
-1
-2
-3
```