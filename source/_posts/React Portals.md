---
title: React Portals
date: 2021-05-30 20:55:16
tags: React, React Portals
---

## 前言

什么是*Portals*？如何以及何时使用它
React v16 引入了一个称为*Portals*的新功能。 Portals 提供了一种快速简便的方法来将子组件渲染到存在于父组件的 DOM 层次结构之外的 DOM 节点中。 React 在单个 DOM 节点下渲染整个应用程序 - 应用程序根（root）。但是如果你想在根 DOM 节点之外渲染子节点怎么办？那是你使用*Portals*的时候。比如当父元素具有样式（如 z-index 将其推到页面的最前面或overflow: hidden）你又希望子元素在视觉上显示在其容器顶部时。

## 它是如何工作的
*Portals*可以位于 DOM 树中的任何位置，它在其他方面的行为都像一个普通的 React 子节点。无论子节点是否是*Portals*，诸如上下文之类的功能都完全相同，因为无论在 DOM 树中的位置如何，*Portals*仍然存在于 React 树中。这包括事件冒泡。即使那些元素不是DOM树中的祖先，从*Portals*内部触发的事件也将传播到包含React的树中的祖先。

## 用法
我建议它创建一个组件，然后在每次需要时使用。首先，我们需要在 index.html 中创建一个 DOM 元素（就像我们为附加应用程序而创建的根元素），这是所有*Portals*组件将被挂载的地方。

```html
 <div id="portal-components"></div>
  <div id="root">
```

然后创建Portal组件

```js
import { PureComponent } from 'react'
import ReactDOM from 'react-dom'

const rootElement = document.getElementById('portal-components')

class Portal extends PureComponent {
  constructor(props) {
    super(props)
    this.el = document.createElement('div')
  }

  componentDidMount() {
    rootElement.appendChild(this.el)
  }

  componentWillUnmount() {
    rootElement.removeChild(this.el)
  }

  render() {
    return ReactDOM.createPortal(
      this.props.children,
      this.el
    )
  }
}

export default Portal
```

最后这样使用它

```js
<Portal>
  {Everithing here is going to be attachaded outside the root DOM node}
</Portal>
```

## 注意
在使用*Portals*的时候，所有在<Portal>组件中被渲染的子元素都会相对与屏幕绝对定位，我建议使用css：*position: fixed*来调整位置。
