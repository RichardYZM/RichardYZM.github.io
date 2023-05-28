---
title: Shopify开发之运算符和数据类型
cover: https://images.pexels.com/photos/247599/pexels-photo-247599.jpeg
date: 2023-01-17 12:00:00
updated: 2023-01-17 12:00:00
---
### 基础
Liquid使用标签、对象和过滤器的组合来加载动态内容。 它们在Liquid模板文件中使用，这些文件构成了一个主题
#### 1. 标签
标签用来控制模板的逻辑，如条件语句、循环语句、赋值语句等；除了这些基本的逻辑处理语句，Liquid还定义了一些主题标签，如注释标签、Liquid标签、表单标签、layout标签、paginate标签、raw标签、render标签、section标签、style标签等，如：
```js
{% if user.name == 'elvis' %}
  Hey Elvis
{% endif %}
```
#### 2. 对象
对象也叫做变量，包含用于在页面上显示动态内容的属性。包括全局对象、内容对象、其他变量。输出一个对象的属性：
```js
{{ product.title }} 
```
#### 3. 过滤器
Liquid过滤器用于修改数字、字符串、对象和变量的输出。 它们被放置在输出标记 {{\}\} 或者 echo 语句中，并由管道字符 | 表示。如：
```js
{{ 'hello, world!' | capitalize | remove: "world" }}
```

### 运算符
运算符|说明
-|-
==|等于
!=|不等
\>|大于
<|小于
\>=|大于等于
<=|小于等于
or|或
and|与
contains|包含(只能用于检查字符串)
#### 运算符优先级
比较运算符, 包含运算符 > 逻辑运算符

1. Liquid运算符不包含圆括号，使用圆括号进行运算会报错；
2. Liquid逻辑运算符 and 和 or 具有相同优先级的，且结合性是从右向左
```js
{% if true or false and false %}
  //结果为true，上面的运算顺序为“true or (false and false)”与运算返回false，或晕眩
{% endif %}
```

### 数据类型
#### 1. String
- 使用单引号或双引号包裹
```js
{% assign my_string = "Hello World!" %}
```
#### 2. Number
- 数值包含整数和小数
```js
{% assign my_int = 25 %}
{% assign my_float = 39.756 %}
```
#### 3. Boolean
- 布尔值包含 true 和 false
```js
{% assign foo = true %}
{% assign bar = false %}
```
#### 4. Nil
- nil是一个特殊的空值，是Liquid代码没有值返回时的值，打印nil值到页面上，不会展示任何内容
#### 5. Array
- 数组是可以用来保存任何变量类型的列表，使用循环访问数组中的所有项
```js
{% for tag in product.tags %}
  {{ tag }}
{% endfor %}
```
- 使用中括号访问数组中特定下标的项（下标从0开始）
```js
<!-- if product.tags = "sale", "spring", "summer", "wholesale" -->
{{ product.tag[0] }}
{{ product.tag[1] }}
{{ product.tag[2] }}
```
- 在Liquid中创建数组只有一种方式：使用split过滤器将一个字符串分解为字符串数组
```js
{% assign words = 'Hi, how are you today?' | split: ' ' %}
{% for word in words %}
{{ word }}
{% endfor %}
```
#### 6. EmptyDrop
通过 handle 访问一个已被删除的对象（例如 page 或 post）时会返回 EmptyDrop 对象。
例如，以下的page_1, page_2, page_3都是 EmptyDrop 对象：
```js
{% assign variable = "hello" %}
{% assign page_1 = pages[variable] %}
{% assign page_2 = pages["does-not-exist"] %}
{% assign page_3 = pages.this-handle-does-not-exist %}
```
### 真值和假值（truthy 和falsy）
除了 nil 和 false 为假值，其他值都为真值，包括 0、空字符串、EmptyDrop对象

### 去除标签空白符
{{ \}\} 和 {% %} 标签即使不输出任何内容，但仍然会在html中渲染一个空行；如果需要删除这些空行，可以在标签中包含连字符，如：{{-, -\}\}, {%-, -%}

