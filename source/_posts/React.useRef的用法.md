---
title: React.useRef的用法
date: 2021-02-19 09:02:44
tags: React, createRef, useRef
---

## 前言

React.createRef和React.useRef都可以用来创建可变对象，这个对象包含current属性，可以用来保存和引用一些值，并且修改这个属性不会触发组件更新。
接下来聊它们的不同。

## useRef vs createRef

useRef仅能用在FunctionComponent，createRef仅能用在ClassComponent。
首先hooks不能用在ClassCompnent中, createRef在FunctionComponent是无效的，因为FunctionComponent每次更新createRef都会重新初始化，所以ref会不断改变，看下面代码

```js
function App() {
  // 错误用法，ref.current永远是null
  const ref = React.createRef();
  return <div ref={ref} />;
}
```

useRef的用法

```js
function App() {
  // ref.current 可以保存div的dom实例
  const ref = React.useRef();
  return <div>
    <div ref={ref}>hello world</div>
  </div>
}
```

也可以使用callback ref保存dom实例。但是这个方法会有个[小问题](https://reactjs.org/docs/refs-and-the-dom.html#caveats-with-callback-refs)，官方文档也有说明。
```js
class App extends React.Component {
  render() {
    return (
      <input type="text" ref={el => this.inputRef = el} />
    )
  }
}
```

## 总结

useRef有两种用例：

1. 直接访问DOM节点以强制执行DOM操作
2. 在组件实例的整个生命周期中，在所有渲染器中保持一个值
与useEffect，useState和useCallback结合使用时，可以保留以前的状态。

最后，不要滥用 Ref，Mutable 引用越多，对 React 来说可维护性一般会越差。
