---
title: React最常用的Hook
cover: https://images.pexels.com/photos/3601422/pexels-photo-3601422.jpeg
date: 2022-06-26 12:00:00
updated: 2022-06-26 12:00:00
---
#### 1. 什么是hook
hook是React 16.8新增的特性，专门用在函数式组件，它可以代替class组件中react的其他特性，是实际工作中要常用到的
hook是专门配合函数式组件开发使用的，可以用它代替class组件的一些生命周期，避免大量this引起的混乱，因此hook便于开发，且更易于让开发者理解代码

#### 2. useState
```javascript
import React,(useState) from 'react'
export default function useState_Demo() {
    const [count, setCount] = useState(0);//通过解构赋值，我们拿到的count、setCount
    function changeCount(){
        setCount((count) => count + 1) //setCount的回调中传进来的参数是count
    }
    render (
        <div> 
            <button onclick = {changeCount}>点我会使count+1</button>
        </div>
    )
}
```
- React.useState(0)相当于class组件中的this.state添加一个属性值,count相当于class组件中的this.state. count的值setCount可以用来修改count的值，可以相当于class组件中的this.setState

#### 3. useEffect
```javascript
import React,(useState, useEffect) from 'react'
export default function useState_Demo() {
   const [count, setCount] = useState(0);//通过解构赋值，我们拿到的count、setCount
    
    useEffect(() => {
        //这个return是在该组件监测的数据变化时或者被卸载时调用的，被卸载时调用可以相当于componentWillUnmount钩子 
        return () => {
            console.log('该组件被卸载了')
        }
    }, [count])//第二个参数传了[count]，表示检测count的更新变化，一旦count变化就会再次执行useEffect的回调
                  //第二个参数传了[],表示谁都不检测只执行一次useEffect的回调，相当于componentDidMount钩子
                  //第二个参数不传参，只要该组件有state变化就会执行useEffect的回调，相当于componentDidUpdate钩子
    function changeCount(){
        setCount((count) => count +1) //setCount的回调中传进来的参数是count
    }
    render (
        <div> 
            <button onclick = {changeCount}>点我会使count+1</button>
        </div>
    )
}
```
- useEffect的使用代替了在class组件中componentDidMoun、componentDidUpdate、componentWillUnmount的使用

#### 4. 使用hook需要注意的
##### 1. 只在组件函数的最外层使用Hook，不要在循环，条件或嵌套函数中调用 Hook
```javascript
import React,(useEffect) from 'react'
export default function useState_Demo() {
   //这里才是正确的
   useEffect(() => {})
    
   //错误1，使用了条件判断
   if(true) {
        useEffect(() => {})
   }
   //错误2，使用了循环
   while(true) {
        useEffect(() => {})
   }
  //错误3,使用了嵌套
  useEffect(() => {
      useEffect(() => {})
  })
}
```
##### 2. 不能在组件的函数外使用hook
```javascript
import React,(useState, useEffect) from 'react'
//错误1，在组件函数外使用了useState
const [variable, setVariable] = useState(0);
//错误2，在组件函数外使用了useEffect
useEffect(() => {})
export default function useState_Demo() {
   //在组件函数里使用才是正确的
   const [variable, setVariable] = useState(0);
}
```