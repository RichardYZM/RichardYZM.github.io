---
title: antd table 拖拽排序的问题
cover: https://images.pexels.com/photos/2072600/pexels-photo-2072600.jpeg
date: 2022-11-08 12:00:00
updated: 2022-11-08 12:00:00
---
## 1. 问题： 使用antd table中的拖拽手柄时出现拖拽行消失
![在这里插入图片描述](https://img-blog.csdnimg.cn/dac50fa3ed31467fb1c314b545c8e4a6.jpeg)
## 2. 描述
排序时项目消失/CSS问题
排序后，react-sortable-hoc创建正在排序的元素的克隆（即sortable-helper），并将其附加到<body标记的末尾。原始元素仍然是in-place，以保持其在DOM中的位置，直到拖动结束（使用inline-styling使其不可见）。如果sortable-helper从CSS的角度来看变得一团糟，那么考虑一下，您的可拖动项的选择器可能依赖于一个不再存在的父元素（同样，因为sortable-helper位于<body>的末尾）。这也可能是一个z-index问题，例如，当在引导模式中使用react-sortable-hoc时，您需要增加SortableHelper的z-index，以便它显示在模式的顶部

## 3. 解决方案
#### 1. 设置z-index
```javascript
.row-dragging {
  background: #fafafa;
  border: 1px solid #ccc;
  z-index: 10000 !important;
}
.row-dragging td {
  padding: 16px;
}
.row-dragging .drag-visible {
  visibility: visible;
}
```
#### 2. z-index无效的情况，是不是使用了Modal
![在这里插入图片描述](https://img-blog.csdnimg.cn/115082ae21a1448a8b2c37c9ff0b0e70.png)
```javascript
const modalBody = useRef(null);
<Modal ref={modalBody}>
	<SortableBody
      useDragHandle
      disableAutoscroll
      helperClass="row-dragging"
      onSortEnd={onSortEnd}
      helperContainer={modalBody.current}
      {...props}
    />
</Modal>
```

### 总结
一般来说设置z-index都可解决问题，个别特殊组件需要把挂载的组件ref作为属性传入
