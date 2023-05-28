---
title: React.memo、useMemo和useCallback
cover: https://images.pexels.com/photos/1174732/pexels-photo-1174732.jpeg
date: 2022-05-09 12:00:00
updated: 2022-05-09 12:00:00
---
- 我们在使用React开发过程中经常会遇到父组件引入子组件的情况，在不做其他处理的时候，很容易造成子组件不必要的重复渲染。看下面一个的例子，这种情况下，每次父组件更新num值的时候，子组件也会重复渲染。

```javascript

function Child(props){
    return(
        <div>{props.title}</div>
    )
}
function Parent(){
    const [num, setNum] = useState(1);
    return(
        <div>
            <Child title="子组件" />
            <button onClick={() => setNum(num+1)}>addNum</button>
            <div>num:{num}</div>
        </div>
    )
}
```
#### 1. React.memo
- React.memo(component, func)，第一个参数是自定义组件，第二个参数是一个函数，用来判断组件需不需要重新渲染。如果省略第二个参数，默认会对该组件的props进行浅比较，下面这种情况下，Child的props经浅比较无变化，则不重复渲染

```javascript
function Child(props){
    return(
        <div>{props.title}</div>
    )
}
const NewChild = React.memo(Child, (prevProps, nextProps) => {
      // return true 不会重新渲染
      // return false 重新渲染
})
function Parent(){
    const [num, setNum] = useState(1);
    return(
        <div>
            <NewChild title="我是子组件" />
            <button onClick={() => setNum(num+1)}>addNum</button>
            <div>num:{num}</div>
        </div>
    )
}
```
- 然而在某些情况下，光靠React.memo包裹子组件，子组件还是会进行不必要的重复渲染更新，这时useMemo和useCallback则可以进行更细粒度的性能优化。看下面这个例子，这种情况下，每次父组件更新的时候，子组件的title和onChangeTitle值都会生成新的内存地址，所以即使title没改变，子组件也会重新渲染，memo失效。

```javascript
function Child({title, onChangeTitle}){
  return(
      <>
        <div>{title}</div>
        <button onClick={() => onChangeTitle('我是新的title')}>change title</button>
      </>
  )
}
const NewChild = React.memo(Child)；

function Parent(){
    const [num, setNum] = useState(1);
    const [title, setTitle] = useState('我是子组件');
    const onChangeTitle = (text) => {
        setTitle(text)
    }
    return(
        <div>
            <NewChild title={title} onChangeTitle={onChangeTitle}>
            <button onClick={() => setNum(num+1)}>addNum</button>
            <div>num:{num}</div>
        </div>
    )
}
```
#### 2. useMemo
- 第一次渲染时执行，缓存变量，之后只有在依赖项改变时才会重新计算记忆值

#### 3. useCallback
- 第一次渲染时执行，缓存函数，之后只有在依赖项改变时才会更新缓存
```javascript
function Child({title, onChangeTitle}){
  return(
      <>
        <div>{title}</div>
        <button onClick={() => onChangeTitle('我是新的title')}>change title</button>
      </>
  )
}
const NewChild = React.memo(Child)

function Parent(){
  const [num, setNum] = useState(1);
  const [title, setTitle] = useState('我是子组件');
  const onChangeTitle = useCallback((text) => {
      setTitle(text)
  },[])
  const memoTitle = useMemo(() => (title), [title])
  return(
      <div>
          <NewChild title={memoTitle} onChangeTitle={onChangeTitle} />
          <button onClick={() => setNum(num+1)}>add</button>
          <div>num:{num}</div>
      </div>
  )
}
```
- useMemo和useCallback用法差不多，都是在第一次渲染的时候执行，然后在依赖项改变时再次执行，不同点在于，useMemo返回缓存的变量，useCallback返回缓存的函数