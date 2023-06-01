---
title: React性能优化的几种方式
cover: https://images.pexels.com/photos/16972904/pexels-photo-16972904.jpeg
date: 2022-10-10 12:00:00
updated: 2022-10-10 12:00:00
---
# 1. 使用React.Memo来缓存组件
提升应用程序性能的一种方法是实现memoization。Memoization是一种优化技术，主要通过存储昂贵的函数调用的结果，并在再次发生相同的输入时返回缓存的结果，以此来加速程序。
父组件的每次状态更新，都会导致子组件重新渲染，即使传入子组件的状态没有变化，为了减少重复渲染，我们可以使用React.memo来缓存组件，这样只有当传入组件的状态值发生变化时才会重新渲染。如果传入相同的值，则返回缓存的组件。示例如下：
```js
export default React.memo((props) => {
  return (
    <div>{props.value}</div>  
  )
});s
```
# 2. 使用useMemo缓存大量的计算
有时渲染是不可避免的，但如果您的组件是一个功能组件，重新渲染会导致每次都调用大型计算函数，这是非常消耗性能的，我们可以使用新的useMemo钩子来“记忆”这个计算函数的计算结果。这样只有传入的参数发生变化后，该计算函数才会重新调用计算新的结果。
通过这种方式，您可以使用从先前渲染计算的结果来挽救昂贵的计算耗时。总体目标是减少JavaScript在呈现组件期间必须执行的工作量，以便主线程被阻塞的时间更短。
```js
// 避免这样做
function Component(props) {
  const someProp = heavyCalculation(props.item);
  return <AnotherComponent someProp={someProp} /> 
}
  
// 只有 `props.item` 改变时someProp的值才会被重新计算
function Component(props) {
  const someProp = useMemo(() => heavyCalculation(props.item), [props.item]);
  return <AnotherComponent someProp={someProp} /> 
}
```
# 3. 避免使用内联对象
使用内联对象时，react会在每次渲染时重新创建对此对象的引用，这会导致接收此对象的组件将其视为不同的对象,因此，该组件对于prop的浅层比较始终返回false,导致组件一直重新渲染。
许多人使用的内联样式的间接引用，就会使组件重新渲染，可能会导致性能问题。为了解决这个问题，我们可以保证该对象只初始化一次，指向相同引用。另外一种情况是传递一个对象，同样会在渲染时创建不同的引用，也有可能导致性能问题，我们可以利用ES6扩展运算符将传递的对象解构。这样组件接收到的便是基本类型的props，组件通过浅层比较发现接受的prop没有变化，则不会重新渲染。示例如下：
```js

function Component(props) {
  const aProp = { someProp: 'someValue' }
  return <AnotherComponent style={{ margin: 0 }} aProp={aProp} />  
}
 
// Do this instead :)
const styles = { margin: 0 };
function Component(props) {
  const aProp = { someProp: 'someValue' }
  return <AnotherComponent style={styles} {...aProp} />  
}
```
# 4. 避免使用匿名函数
虽然匿名函数是传递函数的好方法（特别是需要用另一个prop作为参数调用的函数），但它们在每次渲染上都有不同的引用。这类似于上面描述的内联对象。为了保持对作为prop传递给React组件的函数的相同引用，您可以将其声明为类方法（如果您使用的是基于类的组件）或使用useCallback钩子来帮助您保持相同的引用（如果您使用功能组件）。
当然，有时内联匿名函数是最简单的方法，实际上并不会导致应用程序出现性能问题。这可能是因为在一个非常“轻量级”的组件上使用它，或者因为父组件实际上必须在每次props更改时重新渲染其所有内容。因此不用关心该函数是否是不同的引用，因为无论如何，组件都会重新渲染。
```js
// 避免这样做
function Component(props) {
  return <AnotherComponent onChange={() => props.callback(props.id)} />  
}
 
// 优化方法一
function Component(props) {
  const handleChange = useCallback(() => props.callback(props.id), [props.id]);
  return <AnotherComponent onChange={handleChange} />  
}
 
// 优化方法二
class Component extends React.Component {
  handleChange = () => {
   this.props.callback(this.props.id) 
  }
  render() {
    return <AnotherComponent onChange={this.handleChange} />
  }
}
```
# 5. 延迟加载不是立即需要的组件
延迟加载实际上不可见（或不是立即需要）的组件，React加载的组件越少，加载组件的速度就越快。因此，如果您的初始渲染感觉相当粗糙，则可以在初始安装完成后通过在需要时加载组件来减少加载的组件数量。同时，这将允许用户更快地加载您的平台/应用程序。最后，通过拆分初始渲染，您将JS工作负载拆分为较小的任务，这将为您的页面提供响应的时间。这可以使用新的React.Lazy和React.Suspense轻松完成。
```js
// 延迟加载不是立即需要的组件
const MUITooltip = React.lazy(() => import('@material-ui/core/Tooltip'));
function Tooltip({ children, title }) {
  return (
    <React.Suspense fallback={children}>
      <MUITooltip title={title}>
        {children}
      </MUITooltip>
    </React.Suspense>
  );
}
 
function Component(props) {
  return (
    <Tooltip title={props.title}>
      <AnotherComponent />
    </Tooltip>
  )
}
```