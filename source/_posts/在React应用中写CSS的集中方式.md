---
title: 在React应用中写CSS的集中方式
date: 2022-06-20 20:31:48
tags: React, CSS
---

## 前言

我们都熟悉将 CSS 样式以 link 的形式引用到 HTML 文档`<head>`标签的方法，这只是我们写 CSS 的几种方法之一。但在一个单页面应用程序(SPA)中，比如在 React 项目中，它是什么样子的呢?有几种方法可以对 React 应用程序增加样式。有些与传统方式重叠，有些则没有那么多规则。

## 引入外部样式表

就像标题所说，React 可以引入样式文件。这个过程有点像 html 用 link 引入 CSS

- 在项目目录创建创建新的 CSS 文件
- 写 CSS 样式
- 在 React 的文件头部 import CSS 文件，如下：

```js
import { React } from 'react';
import './Components/css/App.css';

function App() {
  return <div className="main"></div>;
}
export default App;
```

### 写内联样式

您可能经常听到内联样式对于可维护性和诸如此类的东西并不是那么好，但是在某些情况下内联样式是有意义的。在 React 中，可维护性不是什么大问题，因为 CSS 通常已经放在同一个文件中了。

这是 React 中一个超级简单的内联样式示例:

```js
<div className="main" style={{color:"red"}}>
```

更好的方式是是先创建样式对象，然后放入 style 参数中

```js
import { React } from 'react';
function App() {
  const styles = {
    main: {
      backgroundColor: '#f1f1f1',
      width: '100%',
    },
    inputText: {
      padding: '10px',
      color: 'red',
    },
  };
  return (
    <div className="main" style={styles.main}>
      <input type="text" style={styles.inputText}></input>
    </div>
  );
}
export default App;
```

## CSS Module

CSS 模块具有局部作用域变量的优点，可以与 React 一起使用。它到底是什么呢?

CSS 模块的工作原理是将单个 CSS 文件编译成 CSS。CSS 输出是正常的全局 CSS，可以直接注入到浏览器中，也可以连接在一起并写入文件以供生产使用。数据用于将您在文件中使用的人类可读的名称映射到全局安全的输出 CSS。

简单地说，CSS Modules 允许我们在多个文件中使用相同的类名而不会发生冲突，因为每个类名都有一个唯一的 id 名。这在较大的应用程序中特别有用。每个类名的局部作用域都限定在它被导入的特定组件上。

```js
/* styles.module.css */
.font {
  color: #f00;
  font-size: 20px;
}

import { React } from "react";
import styles from "./styles.module.css";
function App() {
  return (
    <h1 className={styles.heading}>Hello World</h1>
  );
}
export default App;
```

## styled-components

Have you used styled-components? It’s quite popular and allows you to build custom components using actual CSS in your JavaScript. A styled-component is basically a React component with — get ready for it — styles. Some of the features include unique class names, dynamic styling and better management of the CSS as each component has its own separate styles.

Install the styled-components npm package in the command line:
你使用过`styled-components`吗? 可以使用它在 JavaScript 中使用实际的 CSS 构建自定义组件。一个`styled-components`基本上是一个带有样式的 React 组件。其中的一些特性包括惟一的类名、动态样式和更好的 CSS 管理，因为每个组件都有自己独立的样式。

在命令行中安装`styled-components`的 npm 包:

```bash
npm install styled-components
```

```js
import { React } from 'react';
import styled from 'styled-components';
function App() {
  const Wrapper = styled.div`
    width: 100%;
    height: 100px;
    background-color: red;
    display: block;
  `;
  return <Wrapper />;
}
export default App;
```

条件样式

`styled-components`的优点之一是组件本身是具有函数功能的，因为你可以在 CSS 中使用 props。

```js
import { React, useState } from 'react';
import styled from 'styled-components';
function App() {
  // display state
  const [display, setDisplay] = useState(true);
  return (
    <>
      <Wrapper $display={display} />
      <button onClick={() => setDisplay(!display)}>Toggle</button>
    </>
  );
}
// the wrapper styled component
const Wrapper = styled.div`
  width: 100%;
  height: 100px;
  background-color: red;
  display: ${(props) => (props.$display ? 'block' : 'none')};
`;
export default App;
```

## 总结
我们研究了几种在React应用程序中编写样式的不同方法。并不是说哪一个比其他的好，因为每种方法的好坏取决于具体的情况。希望现在你已经很好地理解了它们，并知道在React样式工具中有找到合适的。
