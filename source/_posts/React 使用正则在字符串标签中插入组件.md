---
title: React 使用正则在字符串标签中插入组件
cover: https://images.pexels.com/photos/866496/pexels-photo-866496.jpeg
date: 2022-07-30 12:00:00
updated: 2022-07-30 12:00:00
---
1. 最近遇到一个需求，需要将后端返回的字符串标签中的span标签替换为组件，且渲染在页面上，字符串标签的格式如下
```javascript
const str = `<p>分析发现xx街道辖区内上报商铺飞线充电事件<span class="dataItem">${"id":1,"name":"xx区X街道X事件X时间段X问题来源X数据来源的数量"}</span>起，
    占街道事件总数的<span class="dataItem">${"id":5,"name":"xx区X街道X事件X时间段X问题来源X数据来源的数量在相同条件下Y事件中的占比"}</span>，
    占街道消防安全类事件总数的<span class="dataItem">${"id":6,"name":"xx区X街道X事件X时间段X问题来源X数据来源的数量在相同条件下所有事件中的占比"}</span>，
    占全区商铺飞线充电事件总数的<span class="dataItem">${"id":7,"name":"xx区X街道X事件X时间段X问题来源X数据来源的数量在相同条件下xx区的占比"}</span>，
    与其它街道相比排名第<span class="dataItem">${"id":8,"name":"xx区X街道X事件X时间段X来源的数量在相同条件下xx区的排名"}</span>。</p >`
```
- 需要将 class 为 "dataItem" 的 span 标签替换为下拉组件，且 span 标签中显示为 ${} 中的 name

2. 解决方案
```javascript
class Test extends React.Component {
  constructor(props) {
    super(props);
    
  }
    getContent = (info) => {
        let list = []

        let reg = /<span (.*?)>(.*?)<\/span>/g, match, lastIndex = 0, ret = [], i = 0, bindId = 0;
        while (match = reg.exec(info)) {

            if (match.index !== lastIndex) {
                ret.push(React.createElement("p", { className: styles.text, key: i++ }, info.slice(lastIndex, match.index)));
            }

            let data = JSON.parse(match[2].substr(1))
            data.bindId = bindId++
            list.push(data)

            ret.push(<Dropdown
                visible={showTemplateEdit && currentDataItem.bindId === data.bindId}
                destroyPopupOnHide
                key={i++}
                overlay={<div>Hello</div>}
                trigger={['click']}
            >
                <span className={styles.dataItem} data={{ ...data }}><i>{' {'}</i>{`${data.name}`}<i>{'} '}</i></span>
            </Dropdown>);
            lastIndex = match.index + match[0].length

        }
        if (info && lastIndex !== info.length) {
            ret.push(info.slice(lastIndex))
        }
        return ret;
    }
  render() {
    return <div>
      <h1>hello</h1>
      {this.getContent(str)}
    </div>
  }
}
ReactDOM.render(<Test />, document.querySelector("#root"))
```