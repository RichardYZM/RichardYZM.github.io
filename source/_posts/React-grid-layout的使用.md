---
title: React-grid-layout的使用    
cover: https://images.pexels.com/photos/37347/office-sitting-room-executive-sitting.jpg
date: 2021-08-23 12:00:00
updated: 2021-08-23 12:00:00
---

## 简介
React-grid-layout 是一个网格布局系统，可以实现响应式的网格布局，灵活运用可以方便的实现拖拽式组件的实现

## 用法
#### 基本用法
```javascript
import GridLayout from 'react-grid-layout';

class MyFirstGrid extends React.Component {
  render() {
    // layout is an array of objects, see the demo for more complete usage
    const layout = [
      {i: 'a', x: 0, y: 0, w: 1, h: 2, static: true},
      {i: 'b', x: 1, y: 0, w: 3, h: 2, minW: 2, maxW: 4},
      {i: 'c', x: 4, y: 0, w: 1, h: 2}
    ];
    return (
      <GridLayout className="layout" layout={layout} cols={12} rowHeight={30} width={1200}>
        <div key="a">a</div>
        <div key="b">b</div>
        <div key="c">c</div>
      </GridLayout>
    )
  }
}
```
上方是一个简单的用法实现，其中有a、b、c三个项，a 设置了 static: true，因此它是不可拖拽和缩放的，b 设置了 minW: 2 和 maxW: 4，因此它在拖拽时的宽度在2个单位到4个单位，而c项未做任何限制，可以自由的拖拽和缩放

#### 直接在子项中设置
```javascript
import GridLayout from 'react-grid-layout';

class MyFirstGrid extends React.Component {
  render() {
    return (
      <GridLayout className="layout" cols={12} rowHeight={30} width={1200}>
        <div key="a" data-grid={{x: 0, y: 0, w: 1, h: 2, static: true}}>a</div>
        <div key="b" data-grid={{x: 1, y: 0, w: 3, h: 2, minW: 2, maxW: 4}}>b</div>
        <div key="c" data-grid={{x: 4, y: 0, w: 1, h: 2}}>c</div>
      </GridLayout>
    )
  }
}
```
#### 响应式用法
```javascript
import { Responsive as ResponsiveGridLayout } from 'react-grid-layout';

class MyResponsiveGrid extends React.Component {
  render() {
    // {lg: layout1, md: layout2, ...}
    const layouts = getLayoutsFromSomewhere();
    return (
      <ResponsiveGridLayout className="layout" layouts={layouts}
        breakpoints={{lg: 1200, md: 996, sm: 768, xs: 480, xxs: 0}}
        cols={{lg: 12, md: 10, sm: 6, xs: 4, xxs: 2}}>
        <div key="1">1</div>
        <div key="2">2</div>
        <div key="3">3</div>
      </ResponsiveGridLayout>
    )
  }
}
```
通过 breakpoints 和 cols 设置在不同分辨率的设备下，一行中横向有多少个单位的网格
#### 自动确定宽度
```javascript
import { Responsive, WidthProvider } from 'react-grid-layout';

const ResponsiveGridLayout = WidthProvider(Responsive);

class MyResponsiveGrid extends React.Component {
  render() {
    // {lg: layout1, md: layout2, ...}
    var layouts = getLayoutsFromSomewhere();
    return (
      <ResponsiveGridLayout className="layout" layouts={layouts}
        breakpoints={{lg: 1200, md: 996, sm: 768, xs: 480, xxs: 0}}
        cols={{lg: 12, md: 10, sm: 6, xs: 4, xxs: 2}}>
        <div key="1">1</div>
        <div key="2">2</div>
        <div key="3">3</div>
      </ResponsiveGridLayout>
    )
  }
}
```
通过 WidthProvider 结合 Responsive, 在窗口调整大小时就可自动调整宽度
## 属性
#### 父容器属性
* **width: number**
设置宽度，使用了WidthProvider时不用设置
* **autoSize:  ```boolean  ```**
为真时，容器的高度会自适应内容的高度
* **cols:  ```number  ```**
布局的列数
* **draggableCancel:  ```string ```**
取消拖动时被拖动项的类选择器的名称
* **draggableHandle:  ```string ```**
拖动时被拖动项的类选择器的名称
* **verticalCompact:  ```boolean ```**
为真时，布局在竖直方向上会紧凑排列
* **compactType:  ```'vertical' || 'horizontal' ```'**
紧凑排列类型
* **layout:  ```array  ```**
布局数组，每一项是一个对象，形如：{i: string, x: number, y: number, w: number, h: number}，如果子组件不设置layout，需要在子组件中设置data-grid
* **margin:  ```[number, number]  ```**
项与项之间的间距，单位是px
* **containerPadding:  ```[number, number]  ```**
项的内边距
* **rowHeight:  ```number ```**
行高
* **droppingItem:  ```{ i: string, w: number, h: number } ```**
放置元素的配置（放置元素即是当你拖动某一项时出现的虚拟元素）
* **isDraggable:  ```boolean ```**
是否可拖拽
* **isResizable:  ```boolean ```**
是否可重置大小
* **isBounded:  ```boolean ```**
是否设置边界
* **useCSSTransforms:  ```boolean ```**
是否使用CSS 3 的translate() 来代替 position left/top（可加快渲染速度）
* **transformScale:  ```number  ```**
若ResponsiveReactGridLayout 或者 ReactGridLayout的父组件具有"transform: scale(n)"这一css属性，应该设置这一比例系数以避免拖拽时出现“伪影”
* **preventCollision:  ```boolean ```**
为真时，项在拖动时不会变更位置
* **isDroppable:  ```boolean ```**
为真时，设置了draggable={true}属性的项可以被放置入其他项
* **resizeHandles:  ```Array[s' , 'w' , 'e' , 'n', 'sw', 'nw', 'se', 'ne'] ```**
设置重置大小的图标出现的位置，默认是在右下角
* **resizeHandle?: ReactElement<any> | ((resizeHandleAxis: ResizeHandleAxis) =>  ReactElement<any>)**
自定义重置大小组件（即默认出现在右下角的那个小图标，可自定义）
* **onLayoutChange: (layout: Layout) => void**
布局发生变化的回调函数
* **type ItemCallback = (layout: Layout, oldItem: LayoutItem, newItem: LayoutItem, placeholder: LayoutItem, e: MouseEvent, element: HTMLElement) => void**
ItemCallback的类型
* **onDragStart:  ```function```**
某一项开始拖动的回调函数
* **onDrag:  ```function```**
某一项正在拖动中的回调函数
* **onDragStop:  ```function```**
某一项停止拖动的回调函数
* **onResizeStart:  ```function ```**
某一项开始重置大小的回调函数
* **onResize:  ```function ```**
某一项正在重置大小中的回调函数
* **onResizeStop:  ```function ```**
某一项停止重置大小的回调函数
* **onDrop: (layout: Layout, item: ?LayoutItem, e: Event) => void**
某一项所包含的内容中被放置其他元素后的回调函数
* **innerRef: ?React.Ref<"div">**
网格的ref
#### 子项属性
* **i:  ```string ```**
组件的key
* **x:  ```number ```**
横坐标
* **y:  ```number ```**
纵坐标
* **w: ```number ```**
宽
* **h:  ```number ```**
高
* **minW:  ```number ```**
最小宽度
* **maxW:  ```number ```**
最大宽度
* **minH:  ```number ```**
最小高度
* **maxH:  ```number ```**
最大高度
* **static:  ```boolean ```**
为真时，项不可拖拽和缩放
* **isDraggable:  ```boolean ```**
是否可拖拽
* **isResizable:  ```boolean ```**
是否可缩放
* **resizeHandles?:  ```Array['s' , 'w' , 'e' , 'n' , 'sw' , 'nw' , 'se', 'ne']```**
设置重置大小的图标出现的位置，默认是在右下角
* **isBounded: ```boolean```**
是否设置边界（若为真且该项可被拖动，则该项只能在网格边界范围内拖动