---
title: 在 Umi 中打包与加载自定义字体
cover: https://images.pexels.com/photos/76969/cold-front-warm-front-hurricane-felix-76969.jpeg
date: 2022-10-18 12:00:00
updated: 2022-10-18 12:00:00
---
使用 Webpack 打包字体文件的时候需要使用 file-loader 来处理打包文件，在 UmiJS 3 中可通过配置文件中的 chainWebpack 函数来自定义 Webpack 的配置。

1. 首先安装 file-loader
```javascript
npm install --save file-loader
```

2. 然后再umi的配置文件 ./umirc.ts 或 config.js 中的 chainWebpack 加上如下配置
```javascript
export default defineConfig({
  // ...
  chainWebpack(config){
    config.module // 配置 file-loader
      .rule('otf')
      .test(/.otf$/)
      .use('file-loader')
      .loader('file-loader');
  },
})
```

3. 在全局的 less 字体样式文件中声明字体
```css
@font-face {
  font-family: 'CustomFont';
  src: url('./font.otf');
}
```

4. 最后就可以直接使用声明好的字体啦
```css
.customFont {
	font-family: 'CustomFont'
}
```
