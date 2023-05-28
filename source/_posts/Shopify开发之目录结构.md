---
title: Shopify开发之目录结构
cover: https://images.pexels.com/photos/4091975/pexels-photo-4091975.jpeg
date: 2022-12-27 12:00:00
updated: 2022-12-27 12:00:00
---
##### 每个商店主题下有7个目录，在templates目录下有一个customers文件夹，一共8个目录。除此之外，不能有其他的目录。如下：
```
└── theme
    ├── assets
    ├── config
    ├── layout
    ├── locales
    ├── sections
    ├── snippets
    └── templates
        └── customers
```

#### 1. assets
assets目录包含主题中使用的所有资源文件，包括图像、CSS和JavaScript文件。在模板中使用 asset_url 过滤器来引用主题中的资源文件。例如
```
{{ 'custom.js' | asset_url }}
```
#### 2. config
config目录包含主题的配置文件。 配置文件在主题编辑器的主题设置区域中定义设置，并存储它们的值。

config目录下有两个配置文件：
```
└── theme
    ...
    ├── config
    |   ├── settings_data.json
    |   └── settings_schema.json
    ...
```
- settings_data.json
包含settings_schema.json中保存的设置值。

- settings_schema.json
控制主题编辑器的“主题设置”区域的组织和内容。

#### 3. layout
layout目录包含主题的布局文件，模板文件通过这些文件呈现。

布局是一种Liquid文件，它允许你在多个页面上都引用同一个布局文件。 例如，多个包含相同头部和底部的页面可以引用同一个布局文件，布局文件的主体内容可以通过以下方式渲染出来：
```
{{ content_for_layout }}
```

#### 4. locales
locale目录包含主题的locale文件，这些文件用于提供翻译后的内容。 Locale文件允许您在主题编辑器中提供翻译体验，为在线商店提供翻译，并允许商家在在线商店中定制文本。

#### 5. sections
sections目录包含一个主题的sections。

section是Liquid文件，它允许您创建可重用的内容模块，商家可以对其进行定制。它们还可以包含允许商家在一个区域内添加、删除和重新排序内容的区块。

#### 6. snippets
snippets目录包含较小的可重用代码片段的Liquid文件。 您可以使用Liquid **render**标记在整个主题中引用这些片段。
```
{%- render 'icon-caret' -%}
```
#### 7. templates
templates目录下的每一个json文件是一个模板，每一个模板对应着一个页面。

json模板是存储要呈现的section列表及其相关设置的数据文件，每一个section是一个块级元素，可以在后台的主题编辑器中添加、删除、排序这些块。

json文件里面是一个对象，模板对象有五个属性：
属性名|数据类型|是否必须|描述
-|-|-|-
name|string|非必须|模板名称
layout|string \| false|非必须|指定模板所属的布局文件；也可以设为false，表示不需要模板。默认布局文件为：theme.liquid
wrapper|string|非必须|如果指定了一个标签名(div、main、section三选一)，则会动态生成这个标签，并把sections全部塞入到这个标签元素里面。也可以给这个标签指定id或类名等，例如：div#wrapper.wrapper[data-attr="wrapper"]
sections|Object|必须|定义各个section对象，详细section对象见下表。一个模板文件里最多只能有20个section，每一个section最有只能有16个block。
order|Array|必须|定义sections的顺序

json示例
```json
{
  "name": "Default product template",
  "layout": "product",
  "wrapper": "div#div_id.div_class[attribute-one=value]",
  "sections": {
    "main": {
      "type": "product"
    }
  },
  "order": [
    "main"
  ]
}
```
section对象各个特性定义如下：
属性名|数据类型|是否必须|描述
-|-|-|-
\<SettingID>|string|-|在block或section的scheme中定义的setting的ID。
\<SectionType>|string|必须|要渲染的section文件名，不用写扩展名。
\<SectionDisabled>|boolean|非必须|是否渲染这个section。
\<BlockID>|string	|-|block的id，只接受字母数字。
\<BlockType>|string|必须|要渲染的block的类型，在section文件的scheme中定义。
\<BlockOrder>|Array|非必须|多个block的id数组，id必须存在对象中，且不能重复。
\<SettingID>|string|必须|在block或section的scheme中定义的setting的ID。
\<SettingValue>|(multiple)|-|	一个setting的有效值。

section对象结构如下：
```json
sections: {
  <SectionID>: {
    "type": <SectionType>,
    "disabled": <SectionDisabled>,
    "settings": {
      <SettingID>: <SettingValue>
    },
    "blocks": {
      <BlockID>: {
        "type": <BlockType>,
        "settings": {
          <SettingID>: <SettingValue>
        }
      }
    },
    "block_order": <BlockOrder>
  }
}
```
section对象示例：
```json
# templates/product.json
{
  "name": "Default product template",
  "sections": {
    "main": {
      "type": "product",
      "settings": {
        "show_vendor": true
      }
    },
    "recommendations": {
      "type": "product-recommendations"
    }
  },
  "order": [
    "main",
    "recommendations"
  ]
}
```