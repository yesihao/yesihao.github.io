---
title: 在React项目中使用Font-Awesome图标
date: 2021-01-12 20:22:11
tags: React, Font-Awesome
---

## 前言

如果你曾经不得不在网页上显示某种图标，那你和可能就使用或看过Font Awesome。 Font Awesome是一个很棒的图标库，它还提供了一个不错的React组件，所以在React程序中使用非常简单。

## 准备

为了快速搭建项目，我们使用[create-react-app](https://github.com/facebook/create-react-app)。


## 安装 Font-Awesome

真棒字体
启动React应用程序后，我们需要安装Font Awesome提供的库：
``` js
＃SVG渲染库
npm我 --save fortawesome / fontawesome-svg-core
＃Font Awesome提供的图标集
npm install --save fortawesome / free-solid-svg-icons
＃我们将使用的实际React组件
npm install --save fortawesome / react-fontawesome
```
这将安装所有必要的部件，接下来就可以使用了。还可以安装许多其他样式不同的图标集，包括Pro图标。这里我们只使用纯风格的免费图标。

> 注意：要使用Pro图标，需要一个付费的Pro帐户。

## 使用图标
现在，让我们打开App.js文件。它只包含JSX create-react-app提供的样板。我们先删除标头标签中的所有内容，保留有些样式，以便后续开发。

我们将需要导入我们安装的FontAwesomeIcon组件和一个SVG Icon进行渲染，然后使用fa-rocket图标。

SVG图案一般用于在图图形对象内部定义形状，创建一个新形状并用图案填充它。一个SVG元素或是位图图像，都可以通过<pattern>元素在x轴或y轴方向以固定的间隔平铺。
下面会展示几个的简单SVG形状的。这个列表可能会随着时间的推移而增长，但是要想拥有一个全面的集合，可以在这基础上创造新图案。

```js
import './App.css';

// Font Awesome Imports
import { faRocket } from '@fortawesome/free-solid-svg-icons';
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';

function App() {
  return (
    <div className="App">
        <FontAwesomeIcon icon={faRocket} />
    </div>
  );
}

export default App;
```

## 设置图标库
如果我们要使用大量图标，要怎么做呢？
其实我们不须要在要使用它们的任何地方重新导入，Font Awesome提供了一种创建图标库的方法，该图标库在导入后可在全球范围内使用。
要进行设置，我们首先创建一个名为fontawesome.js的新文件。我们将库设置添加到此文件中。

```js
// Import the library
import { library } from '@fortawesome/fontawesome-svg-core'
// Import whichever icons you want to use
import { faRocket, faHome } from '@fortawesome/free-solid-svg-icons'

// Add the icons to your library
library.add(
    faRocket,
    faHome
)
```

> 注意：你也可以从“ @ fortawesome / free-solid-svg-icons”中将*作为图标导入； 并将它们映射到您的库中以获取所有图标，但是包的非常大！ 最好只选择需要的那些。

这就是库的所有设置！唯一改变的是渲染<FontAwesomeIcon>组件时我们如何指定图标。 让我们看下App.js文件
```js

import './App.css';
// NOTICE we don't need to import the individual icons!
import { FontAwesomeIcon } from '@fortawesome/react-fontawesome';

function App() {
  return (
    <div className="App">
      <header className="App-header">
        <FontAwesomeIcon icon={['fa', 'rocket']} />
        <br/>
        <FontAwesomeIcon icon={['fa', 'home']} />
      </header>
    </div>
  );
}

export default App;
```

## 总结

现在，你可以在整个应用中随意使用图标了。

我强烈建把配置Font Awesome的文档中介绍的其他选项和属性，配置图标库，然后可以使用这些图标！
