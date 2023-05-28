---
title: Shopify开发之Liquid对象
cover: https://images.pexels.com/photos/675764/pexels-photo-675764.jpeg
date: 2023-04-12 12:00:00
updated: 2023-04-12 12:00:00
---
## 一、概述
- Liquid 对象包含在页面上输出动态内容的属性。
- Liquid 对象是主题主要数据来源。（其他方式如ajax也可以获取数据）
- Liquid 中包括80多个对象。
- Liquid 对象通常也称为 Liquid 变量。
- Liquid 对象有三大类型：全局对象、内容对象、其他对象。

### 1. 全局对象
全局对象可以在主题的任何文件中使用。例如，所有页面都可以访问当前的页面标题：

```js
{{ page_title }}
```
### 2. 内容对象
内容对象用于输出模板和 section 文件的内容，以及由 shopify 加载的脚本和样式表。例如，在布局文件的body标签中使用 content_for_layout 对象输出模板的内容：
```js
{{ content_for_layout }}
```
### 3. 其他对象
其他对象只有在特定的情况下使用，例如，可以在产品详情页使用 product 对象获取产品信息，比如输出产品标题：
```js
{{ product.title }}
```

## 二、全局对象
### 1. all_country_option_tags
- all_country_option_tags 变量用于获取每个国家与其子区域数据。
- all_country_option_tags 变量为每个国家输出一个<option>标签。
- 每个<option>标签都有一个data-province属性，该属性包含国家的子区域数组。
- all_country_option_tags 需要被 <select> 标签包裹。
- all_country_option_tags 对象应该被包装在<select>标签中:
```js
<select name="country">
  {{ all_country_option_tags }}
</select>
```
输出：
```js
<select name="country">
  ...
  <option value="China" data-provinces="[[&quot;Anhui&quot;,&quot;Anhui&quot;],[&quot;Beijing&quot;,&quot;Beijing&quot;],...]">China</option>
  <option value="Christmas Island" data-provinces="[]">Christmas Island</option>
  ...
</select>
```
### 2. country_option_tags
和 all_country_option_tags 类似，但只获取商店后台“发货和配送”页面“可发货区域”的国家或地区。
### 3. cart
cart 对象用于获取购物车数据。
### 4. customer
- customer 对象用于获取当前登录账户的信息。
- 如果用户未登录，customer 值为 nil。
- 如果在需要登录才能访问的页面，例如 /accounts 下属页面，则不需要判断是否未 nil，直接使用即可。
### 5. customer_address
customer_address 对象可通过 customer.addresses 获取。
## 三、内容对象

## 四、其他对象
### 1. address
- 地址对象包含客户在Shopify的结帐页面中输入的信息。 请注意，客户可以输入两个地址:账单地址或送货地址。
- 访问地址对象的属性时，必须指定要访问的地址。 这可以通过在属性前使用shipping_address或billing_address来实现。
- 地址对象可以在电子邮件模板，结帐的订单状态页面，以及订单打印机等应用程序中使用。
### 2. article
- article 对象用于获取某篇文章数据。
- article 对象可在 article 页面获取。
- article 属于某个 blog ，blog 相当于 article 的类别。
### 3. block
- block 对象用于获取 sections 的 block 数据。
- block 对象可在 section 文件或 section 文件引入的 snippet 文件中获取。
- 通过循环 section.blocks 来获取 block 对象。
### 4. blog
- blog 对象用于获取博客数据。
- blog 对象可在博客页面或文章页面获取。
- 博客是文章的类别，每篇文章都要属于某个博客。
- 博客可以统一设置其下的文章的评论规则（禁止评论、需审核评论、自动发布评论）。
### 5. checkout
checkout 对象用于结账页。只有Shopify Plus商家才能自定义结账页。
### 6. collection
- collection 对象用于获取产品系列数据（shopify 将 collection 翻译为 产品系列）。
- collection 对象可在产品系列页面获取。
- 某个系列下可包含任意个产品。
### 7. color
- color 对象用于颜色的详细信息，比如rgba，hsl。直接打印此对象会得到颜色值。
- color 对象从 color 类型的 settings 获取。
### 8. comment
- comment 对象用于获取某篇文章的评论数据。
- comment 对象可通过遍历 article.comments 获取。
### 9. currency
- currency 对象用于获取货币的信息（符号、名称等）。
- currency 对象可通过 shop.currency、cart.currency、checkout.currency 等对象获取。
### 10. current_page
current_page 变量用于获取浏览分页内容时的页码，一般用在 paginate 标签内，也可以用在外面。