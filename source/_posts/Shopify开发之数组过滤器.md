---
title: Shopify开发之数组过滤器
cover: https://images.pexels.com/photos/9754/mountains-clouds-forest-fog.jpg
date: 2023-03-16 12:00:00
updated: 2023-03-16 12:00:00
---

## 过滤器
过滤器是修改数字、字符串、变量和对象输出的简单方法。 它们被放置在一个输出标记{{\}\}中，并由一个管道字符 | 表示。可以在一个输出上使用多个过滤器。 它们是从左到右应用的。过滤器冒号后面接参数，比如：
```js
<!-- product.title = "Awesome Shoes" -->
{{ product.title | upcase | remove: "AWESOME"  }}

// 输出
"SHOES"
```

## 数组过滤器
### join
- 通过指定分隔符将数组所有元素拼接成一个字符串
```js
{{ product.tags | join: ', ' }}
```

### first
- 返回数组第一个元素
```js
{{ product.tags | first }}
```
- 或者通过点符号获取数组第一个元素
```js
{% if product.tags.first == "sale" %}
  This product is on sale!
{% endif %}
```

### last
- 返回数组最后一个元素
```js
{{ product.tags | last }}
```
- 或者通过点符号获取数组最后一个元素
```js
{% if product.tags.last== "sale" %}
  This product is on sale!
{% endif %}
```
- 在字符串中使用 last 过滤器，返回字符串的最有一个字符：
```js
<!-- product.title = "Awesome Shoes" -->
{{ product.title | last }} //s
```
### concat
- 合并两个或多个数组
```js
{% assign fruits = "apples, oranges, peaches, tomatoes" | split: ", " %}
{% assign vegetables = "broccoli, carrots, lettuce, tomatoes" | split: ", " %}
{% assign plants = fruits | concat: vegetables %}
```
### index
- 返回数组中指定下标的元素，注意：并不是通过管道符的方式，而是通过中括号
```js
<!-- product.tags = "sale", "mens", "womens", "awesome" -->
{{ product.tags[2] }} //womens
```
### map
- 接受数组元素的属性作为参数，并根据每个数组元素的值创建数组
```js
<!-- collection.title = "Spring", "Summer", "Fall", "Winter" -->
{% assign collection_titles = collections | map: 'title' %}
{{ collection_titles }}
```

### reverse
- 颠倒数组中元素的位置
```js
{% assign my_array = "apples, oranges, peaches, plums" | split: ", " %}
{{ my_array | reverse }}
```

### size
- 返回数组元素个数，或字符串字符个数
```js
{{ 'The quick brown fox jumps over a lazy dog.' | size }}
```
- 或通过点号的方式
```js
{% if collections.frontpage.products.size > 10 %}
  There are more than 10 products in this collection!
{% endif %}
```
### sort
- 按数组中元素的给定属性对数组中的元素进行排序
```js
{% assign products = collection.products | sort: 'price' %}
{% for product in products %}
  <h4>{{ product.title }}</h4>
{% endfor %}
```
### where
- 对数组中的元素进行过滤筛选，类似于js的Array.prototype.filter，过滤器接收键（值为truthy），或键值，符合要求的将其过滤出来组成新的数组。


```js
//过滤出类型为 ”wheels“ 的产品
{% assign kitchen_products = collection.products | where: "type", "kitchen" %}
//过滤出可用的产品
{% assign available_products = collection.products | where: "available" %}
```
### uniq
- 数组去重
```js
{% assign fruits = "orange apple banana apple orange" %}
{{ fruits | split: ' ' | uniq | join: ' ' }}
//orange apple banana
```


