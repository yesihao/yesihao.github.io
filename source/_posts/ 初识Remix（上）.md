---
title: 初识Remix（上）
date: 2022-09-10 21:22:13
tags: Remix, React
---

## 前言

你可能听说过很多关于 Remix 框架的炒作。它其实再 2019 就已经开发使用，但它最初仅提供给高级框架的订阅者使用。2021 年，创始人筹集种子资金并将框架开源，让用户开始免费使用 Remix。闸门打开了之后，每个人似乎都在谈论它，无论好坏。让我们深入了解 Remix 的一些基础知识。

Remix 是一个服务器“边缘”优先的 JavaScript 框架。它至少目前使用 React 作为前端，并优先考虑服务器端在边缘渲染应用程序。平台可以获取服务器端代码并 ​​ 将其作为无服务器或边缘功能运行，使其比传统服务器更便宜，并更贴近用户。 Remix 的创始人喜欢将其称为“中心堆栈”框架，因为它可以针对正在运行的平台调整服务器和客户端之间的请求和响应。

![](https://i0.wp.com/css-tricks.com/wp-content/uploads/2022/08/aefd6f6bd29afb5e06d19fca53bf3bb22df8707b-521x209-1.png?w=450&ssl=1)

## 部署 Remix

因为 Remix 需要服务器，所以让我们说说如何部署它。 Remix 不提供服务器本身——你自带服务器——允许它在任何 Node.js 或 Deno 环境中运行，包括 Netlify Edge 和 DigitalOcean 的应用平台。 Remix 本身是一个编译器，一个为它运行的平台翻译请求的程序。此过程使用 esbuild 为对服务器的请求创建处理程序。它使用的 HTTP 处理程序建立在 Web Fetch API 之上，并通过调整它们以适应将要部署到的平台而在服务器上运行。

## 混音堆栈

混音堆栈是为你预先配置了一些常用工具的项目。 Remix 团队维护了三个官方堆栈，它们都以音乐流派命名。还有许多社区混音堆栈，包括由 Netlify 的模板团队创建的 K-Pop 堆栈。这个堆栈非常强大，包括 Supbase 数据库和身份验证、用于样式的 Tailwind、Cypress 端到端测试、Prettier 代码格式化、ESLint linting 和 TypeScript 静态类型。查看 Tara Manicsic 关于部署 K-Pop 堆栈的帖子。

## 缓存路由

即使 Remix 需要服务器，它仍然可以通过缓存路由来利用 Jamstack 的优势。 静态站点或静态站点生成 (SSG) 是指你的所有内容都在构建时呈现并保持静态直到另一个重建。 内容是预先生成的，可以放在 CDN 上。 这为最终用户提供了许多好处和快速的站点加载。 但是，Remix 不像其他流行的 React 框架（包括 Next.js 和 Gatsby）那样做典型的 SSG。 要获得 SSG 的一些好处，你可以在 Remix 标头函数中使用本机 Cache-Control HTTP 标头来缓存特定路由或直接在 root.tsx 文件中。

```
[[headers]]
  for = "/build/*"
  [headers.values]
    "Cache-Control" = "public, max-age=31536000, s-maxage=31536000"
```

Then add in your headers function where you want it. This caches for one hour:

```
export function headers() {
  return {
    "Cache-Control": "public, s-maxage=360",
  };
};

```

## Remix 路由

许多框架都倾向于基于文件系统的路由。这是一种使用指定文件夹为应用程序定义路由的技术。它们通常具有用于声明动态路由和端点的特殊语法。目前 Remix 和其他流行框架之间最大的区别是能够使用嵌套路由。

每个 Remix 应用程序都以 root.tsx 文件开头。这是呈现应用程序的整个基础的地方。你会在这里找到一些常见的 HTML 布局，例如 <html> 标签、<head> 标签，然后是带有渲染应用程序所需组件的 <body> 标签。这里要指出的一件事是 <Scripts> 组件是在网站上启用 JavaScript 的原因。没有它，有些事情会起作用，但不是一切。 root.tsx 文件充当 routes 目录中所有内容的父布局，路由中的所有内容都在 <Outlet/> 组件在 root.tsx 中的位置呈现。这是 Remix 中嵌套路由的基础。

### 嵌套路由

Remix 不仅是由 React Router 的一些成员创建的，它还使用 React Router。事实上，他们将 Remix 的一些优点带回了 React Router。 Next.js 和 SvelteKit 的维护者现在试图解决的一个复杂问题是嵌套路由。

嵌套路由不同于传统路由。在新路由将用户带到新页面的情况下，每个嵌套路由都是同一页面的单独部分。它允许通过保持业务逻辑仅与需要它的文件相关联来分离关注点。 Remix 能够处理仅限于嵌套路由所在页面部分的错误。页面上的其他路由仍然可用，并且中断的路由可以为错误提供相关上下文，而不会导致整个页面崩溃。

当 app/routes 中的根文件与将在基本文件中加载的文件目录命名相同时，Remix 会执行此操作。通过使用 <Outlet /> 组件告诉 Remix 在哪里加载其他路由，根文件成为目录中文件的布局。

### Outlet 组件

<Outlet /> 组件是 Remix 的一个信号，用于指示它应该在哪里渲染嵌套路由的内容。它放在 app/routes 目录根目录下的文件中，与嵌套路由同名。以下代码位于 app/routes/about.tsx 文件中，并包含 app/routes/about 文件夹中文件的出口：

```js
import { Outlet } from '@remix-run/react';

export default function About() {
  return (
    <>
      <section>
        I am the parent layout. I will be on any page inside of my named
        directory.
      </section>
      {/* All of my children, the files in the named directory, will go here. */}
      <Outlet />
    </>
  );
}
```

未完待续...
