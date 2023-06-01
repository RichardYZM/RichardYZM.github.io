---
title: Shopify应用开发，应用扩展插件开发
cover: https://images.pexels.com/photos/614484/pexels-photo-614484.jpeg
date: 2021-06-02 12:00:00
updated: 2021-06-02 12:00:00
---
# 1. 注册Shopify账号
要开发应用并用于测试，需要先进行shopify平台账号注册，平台有14天的免费试用权限， shopify分商家平台www.shopify.com/ 和 开发者平台 partners.shopify.com/ ，进入平台按照要求填写即可完成注册。

# 2. 开发 Shopify App 应用
## 1. 阅读文档
开始之前，可先阅读 Shopify 的开发文档 https://shopify.dev/docs/apps/getting-started/create

## 2. 安装工具
需要先安装 `Ruby, Git, Node, Ngrok`，下面是下载地址
Node: https://nodejs.org/en
Git: https://git-scm.com/downloads
Ruby: https://rubyinstaller.org/downloads/
Ngrok: https://ngrok.com/download

## 3. 安装脚手架 shopify-cli
执行命令安装脚手架 shopify-cli
```
npm install -g @shopify/cli
```
安装完成后检查是否安装成功
```
shopify version
```
## 4. 登录授权shopify
在终端执行登录命令`shopify login --store test.shopify.com`, `test.shopify.com` 是shopify商店地址域名

