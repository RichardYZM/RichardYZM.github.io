---
title: github + hexo 搭建个人博客
cover: https://images.pexels.com/photos/2325446/pexels-photo-2325446.jpeg
date: 2023-01-15 12:00:00
updated: 2021-01-15 12:00:00
---
# 1. 准备工作
- 准备一个文件夹存放源文件
- 安装 git
- 安装 Node.js
- 注册 github

# 2. 初始化 Hexo 博客
1. 打开命令行工具，输入 npm 命令安装 Hexo：
```
npm install -g hexo-cli
```
2. 安装完成后，进入源文件夹初始化博客
```
hexo init
```
3. 执行静态部署命令
```
hexo g
```
4. 本地启动，本地预览
```
hexo s
```
此时浏览器访问 http://localhost:4000 就可以打开初始的博客了

# 3. 将 Hexo 部署到 github
1. 首先需要在github上新建一个仓库，仓库名称是```你的用户名+github.io```
![Alt](../img/github%2Bhexo%E6%90%AD%E5%BB%BA%E4%B8%AA%E4%BA%BA%E5%8D%9A%E5%AE%A2/newRepo.png)

2. 回到源文件夹下，打开文件```_config.yml```, 找到```deploy```配置项进行修改
```
deploy:
  type: git
  repo: https://github.com/RichardYZM/RichardYZM.github.io.git // github 仓库地址
  branch: main // 绑定的仓库分支
```
3. 在源文件夹下打开命令行工具，安装git的部署插件
```
npm install hexo-deployer-git --save
```
4. 安装完成后，执行下面三个命令
```
hexo clean  //清除缓存文件 db.json 和已生成的静态文件 public
hexo g      //生成网站静态文件到默认设置的 public 文件夹(hexo generate 的缩写)
hexo d      //自动生成网站静态文件，并部署到设定的仓库(hexo deploy 的缩写)
```
5. 完成以后，打开```https://你的用户名.github.io```就可以打开你的博客了

# 4. 安装并切换主题
1. 使用 npm 安装 butterfly 主题（可自行查找其他主题）
```
npm i hexo-theme-butterfly
```
2. 应用主题
 - 修改源文件夹目录下的```_config.yml```, 修改```theme```
```
theme: butterfly
```
3. 部署
 - 依然是执行下面三个命令便可进行部署
```
hexo clean
hexo g
hexo d
```