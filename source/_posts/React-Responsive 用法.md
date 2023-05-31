---
title: React-Responsive 用法
cover: https://images.pexels.com/photos/13458334/pexels-photo-13458334.jpeg
date: 2023-05-27 12:00:00
updated: 2023-05-27 12:00:00
tags: React-Responsive
---
# 1. 简介
- React Responsive是React的用于响应式设计的媒体查询组件。该模块非常简单，用户只需要指定一组要求，如果满足这些要求，则将渲染子项。还可以根据页面窗口的调整而改变。

# 2. 使用方法
## 1. 使用 Hooks 的方式
```js
import React from 'react'
import { useMediaQuery } from 'react-responsive'

const Example = () => {
  const isDesktopOrLaptop = useMediaQuery({
    query: '(min-device-width: 1224px)'
  })
  const isBigScreen = useMediaQuery({ query: '(min-device-width: 1824px)' })
  const isTabletOrMobile = useMediaQuery({ query: '(max-width: 1224px)' })
  const isTabletOrMobileDevice = useMediaQuery({
    query: '(max-device-width: 1224px)'
  })
  const isPortrait = useMediaQuery({ query: '(orientation: portrait)' })
  const isRetina = useMediaQuery({ query: '(min-resolution: 2dppx)' })

  return (
    <div>
      <h1>Device Test!</h1>
      {isDesktopOrLaptop && <>
        <p>You are a desktop or laptop</p>
        {isBigScreen && <p>You also have a huge screen</p>}
        {isTabletOrMobile && <p>You are sized like a tablet or mobile phone though</p>}
      </>}
      {isTabletOrMobileDevice && <p>You are a tablet or mobile phone</p>}
      <p>Your are in {isPortrait ? 'portrait' : 'landscape'} orientation</p>
      {isRetina && <p>You are retina</p>}
    </div>
  )
}
```
## 2. 使用组件的方式
```js
import MediaQuery from 'react-responsive'

const Example = () => (
  <div>
    <h1>Device Test!</h1>
    <MediaQuery minDeviceWidth={1224} device={{ deviceWidth: 1600 }}>
      <p>You are a desktop or laptop</p>
      <MediaQuery minDeviceWidth={1824}>
        <p>You also have a huge screen</p>
      </MediaQuery>
    </MediaQuery>
    <MediaQuery minResolution='2dppx'>
      {/* You can also use a function (render prop) as a child */}
      {(matches) =>
        matches
          ? <p>You are retina</p>
          : <p>You are not retina</p>
      }
    </MediaQuery>
  </div>
)
```

# 3. API
## 1. 使用属性
- 为了适应react，你可以使用驼峰格式来构造media查询(如使用minDeviceWidth 代替 min-device-width)。作为缩写，任何数字都会添加'px'单位 **```(1234 ```** -> **```'1234px') ```** 。上面的写法可以改为下面的写法
```js
import React from 'react'
import { useMediaQuery } from 'react-responsive'

const Example = () => {
  const isDesktopOrLaptop = useMediaQuery({ minDeviceWidth: 1224 })
  const isBigScreen = useMediaQuery({ minDeviceWidth: 1824 })
  const isTabletOrMobile = useMediaQuery({ maxWidth: 1224 })
  const isTabletOrMobileDevice = useMediaQuery({ maxDeviceWidth: 1224 })
  const isPortrait = useMediaQuery({ orientation: 'portrait' })
  const isRetina = useMediaQuery({ minResolution: '2dppx' })

  return (
    <div>
      ...
    </div>
  )
}
```
## 2. onChange
- 可以使用```onChange```回调来指定当media 查询的值更改时将调用的更改处理程序。
```js
import React from 'react'
import { useMediaQuery } from 'react-responsive'

const Example = () => {

  const handleMediaQueryChange = (matches) => {
    // matches will be true or false based on the value for the media query
  }
  const isDesktopOrLaptop = useMediaQuery(
    { minDeviceWidth: 1224 }, undefined,  handleMediaQueryChange
  );

  return (
    <div>
      ...
    </div>
  )
}
```
```js
import React from 'react'
import MediaQuery from 'react-responsive'

const Example = () => {

  const handleMediaQueryChange = (matches) => {
    // matches will be true or false based on the value for the media query
  }

  return (
    <MediaQuery minDeviceWidth={1224} onChange={handleMediaQueryChange}>
      ...
    </MediaQuery>
  )
}
```
# 4. 简易模式
- 如下

```js
import { useMediaQuery } from 'react-responsive'

const Desktop = ({ children }) => {
  const isDesktop = useMediaQuery({ minWidth: 992 })
  return isDesktop ? children : null
}
const Tablet = ({ children }) => {
  const isTablet = useMediaQuery({ minWidth: 768, maxWidth: 991 })
  return isTablet ? children : null
}
const Mobile = ({ children }) => {
  const isMobile = useMediaQuery({ maxWidth: 767 })
  return isMobile ? children : null
}
const Default = ({ children }) => {
  const isNotMobile = useMediaQuery({ minWidth: 768 })
  return isNotMobile ? children : null
}

const Example = () => (
  <div>
    <Desktop>Desktop or laptop</Desktop>
    <Tablet>Tablet</Tablet>
    <Mobile>Mobile</Mobile>
    <Default>Not mobile (desktop or laptop or tablet)</Default>
  </div>
)

export default Example
```

