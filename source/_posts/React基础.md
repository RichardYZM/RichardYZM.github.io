---
title: React基础
cover: https://images.pexels.com/photos/614484/pexels-photo-614484.jpeg
date: 2021-06-02 12:00:00
updated: 2021-06-02 12:00:00
---

### 1.函数式组件（无状态(state)组件）
#### 写法
```javascript
const App = (props) => {
	return <div>我是函数式组件</div>
}
export default App
```
*组件名必须大写，否则报错
#### props
*props可以接收绑定在函数式组价身上的属性
### 2. class组件
#### 写法
```javascript
import React,{Component} from 'react'

class App extends Component {
	​render(){
	​ 	return <div>class组件</div>
	}
}
exprot deaflut App
```
#### props
在class 关键字创建的组件中，如果想使用 外界传递过来的 props 参数， 不需要接收，直接通过 this.props.xxx 访问即可
### 3. 组件的嵌套和组合
#### 组件嵌套
将子组件在父组件的jsx中以标签的形式使用，组件嵌套的方式就是将子组件写入到父组件的模板中去
#### 组件组合
将一个组件写在另一个组件的内容中，然后在外层组件中通过 this.props.children来接收内容中的组件
### 4.列表渲染
```javascript
this.state = {
	arr: [
​                { id: 1, name: '电脑' },
​                { id: 2, name: '手机' },
​                { id: 3, name: '笔记本' }
		]
}

listitem=()=>{
	const {arr} => this.state
	return arr.map(item => <li key={item.id}>{item.name}</li>)
}
render(){
	return (<div>
		<ul>{this.listitem()}</ul>    
	</div>)
}
```


