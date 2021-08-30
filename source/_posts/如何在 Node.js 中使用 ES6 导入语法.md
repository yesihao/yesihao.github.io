---
title: 如何在 Node.js 中使用 ES6 import语法
date: 2021-08-28 22:56:21
tags: Node, import
---

## 新版Node.js

模块是导出一个或多个值的 JavaScript 文件。导出的值可以是变量、对象或函数。

ES6 导入语法允许导入从不同 JavaScript 文件导出的模块。在 React 和 React Native 应用程序中使用模块是一种常见的模式。语法由以下 ES 模块标准组成：

``` javascript
import XXX from 'xxx';
```

ES 模块是使用模块的 ECMAScript 标准。Node.js 使用 CommonJS 标准来导入模块。这种标准的语法可以这样写：

``` javascript
const XXX = require('xxx');
```

Node js 不直接支持 ES6 导入。尝试*import*在 JS 文件中编写语法：

``` javascript
import { ApolloServer, gql } from 'apollo-server';
```

使用*npm start*或运行*Node.js*服务器*npm run dev*

![Node Error](https://z3.ax1x.com/2021/08/30/hN3Q6H.png)

此错误的解决方案位于上述错误代码段的第一行，现在是 Node.js 推荐的方法。设置 *"type": "module"* 在*package.json*文件。

``` javascript
{
  "type": "module"
}
```

但是这个方案只适用于最新的 Node.js 和14.x.x+。

## 低于 14 的 Node.js

这个问题的另一个解决方案是使用Babel。它是一个 JavaScript 编译器，允许使用最新的语法编写 JS。Babel 不是一个框架或平台，所以它可以用在任何用 JavaScript 编写的项目中，因此也可以用在 Node.js 项目中。

首先从终端窗口安装以下开发依赖项：

``` javascript
npm i -D @babel/core @babel/preset-env @babel/node
```

然后在名为 Node.js 项目的根目录创建一个文件*babel.config.json*并添加以下内容：

``` javascript
{
  "presets": ["@babel/preset-env"]
}
```

该包@babel/node是一个 CLI 程序，它在运行之前使用 Babel 预设和插件编译 Node.js 项目中的 JS 代码。这意味着它会babel.config.json在执行 Node 项目之前读取并应用任何提及的配置。

更换node用*babel-node*在执行服务器*start*或*dev*脚本。

使用*npm run dev*脚本运行 Node 服务器的示例：

``` javascript
{
  "scripts": {
    "dev": "nodemon --exec babel-node server.js"
  }
}
```
