---
title: 初识Remix（下）
date: 2022-10-24 11:12:55
tags: Remix, React
---

接上上一篇

### 文件结构

任何一个再*app/routes/* 目录下的路由都会转换为对应名字的 URL 路由。一个目录里也可以放一个 index.tsx 文件

```
app/
├── routes/
│   │
│   └── blog
|   |   ├── index.tsx ## The /blog route
│   └── about.tsx  ## The /about route
│   ├── index.tsx  ## The / or home route
└── root.tsx
```

如果路由与目录同名，则命名文件将成为目录内文件的布局文件，并且布局文件需要一个 Outlet 组件来放置嵌套路由。

```
app/
├── routes/
│   │
│   └── about
│   │   ├── index.tsx
│   ├── about.tsx ## this is a layout for /about/index.tsx
│   ├── index.tsx
└── root.tsx
```

布局组件（Layout）也可以用双下划线开头命名的文件来创建。

```
app/
├── routes/
│   │
│   └── about
│   │   ├── index.tsx
│   ├── index.tsx
│   ├── about.tsx
│   ├── __blog.tsx ## this is also a layout
└── root.tsx
```

https://your-url.com/about 还是会渲染 /routes/about.tsx 文件, 但是也会渲染 app/routes/about/index.tsx，如果它用了 Outlet 组件。

### 动态路线

动态路由是根据 url 中的信息发生变化的路由。 这可能是博客文章的名称或客户 ID，但无论添加到路由前面的 $ 语法是什么，都向 Remix 表明它是动态的。 除了 $ 前缀之外，名称无关紧要。

```
app/
├── routes/
│   │
│   └── about
│   │   ├── $id.tsx
│   │   ├── index.tsx
│   ├── about.tsx ## this is a layout for /about/index.tsx
│   ├── index.tsx
└── root.tsx
```

## 获取数据！

由于 Remix 在服务器上呈现其所有数据，因此在 Remix 中你看不到很多已成为 React 应用程序标准的东西，例如 useState() 和 useEffect() 挂钩。 对客户端状态的需求较少，因为它已经在服务器上进行了评估。

你使用哪种类型的服务器来获取数据也无关紧要。 由于 Remix 位于请求和响应之间并进行适当的翻译，因此你可以使用标准的 Web Fetch API。 Remix 在仅在服务器上运行的加载器函数中执行此操作，并使用 useLoaderData() 挂钩来呈现组件中的数据。 这是一个使用 Cat as a Service API 渲染随机猫图像的示例。

```
import { Outlet, useLoaderData } from '@remix-run/react'

export async function loader() {
  const response = await fetch('<https://cataas.com/cat?json=true>')
  const data = await response.json()
  return {
    data
  }
}

export default function AboutLayout() {
  const cat = useLoaderData<typeof loader>()
  return (
    <>
      <img
        src={`https://cataas.com/cat/${cat}`}
        alt="A random cat."
      />
      <Outlet />
    </>
  )
}
```

## 路由参数

在动态路由中，前缀为 $ 的路由需要能够访问 URL 参数来处理应该呈现的数据。 loader 函数可以通过 params 参数访问这些。

```
import { useLoaderData } from '@remix-run/react'
import type { LoaderArgs } from '@remix-run/node'

export async function loader({ params }: LoaderArgs) {
  return {
      params
  }
}

export default function AboutLayout() {
  const { params } = useLoaderData<typeof loader>()
  return <p>The url parameter is {params.tag}.</p>
}

```

## 其他 Remix 功能

Remix 有一些其他的辅助函数，可以为路由模块 API 中的普通 HTML 元素和属性添加额外的功能。每个路由都可以定义自己的这些类型的功能。

## 动作功能

操作函数允许你使用标准 Web FormData API 向表单操作添加额外的功能。

导出异步函数操作（{请求}）{

```js
export async function action({ request }) {
  const body = await request.formData();
  const todo = await fakeCreateTodo({
    title: body.get('title'),
  });
  return redirect(`/todos/${todo.id}`);
}
```

## 标头功能

任何 HTTP 标准标头都可以放入标头函数中。因为每个路由都可以有一个标头，为了避免与嵌套路由冲突，最深的路由 - 或具有最多正斜杠 (/) 的 URL - 获胜。还可以通过 actionHeaders、loaderHeaders 或 parentHeaders 来获取 headers

```js
export function headers({ actionHeaders, loaderHeaders, parentHeaders }) {
  return {
    'Cache-Control': loaderHeaders.get('Cache-Control'),
  };
}
```

## 元函数

此函数将为 HTML 文档设置元标记。默认情况下在 root.tsx 文件中设置一个，但可以为每个路由更新它们。

```js
export function meta() {
  return {
    title: 'Your page title',
    description: 'A new description for each route.',
  };
}
```

## 链接功能

HTML 链接元素位于 HTML 文档的 <head> 标记中，它们会导入 CSS 等。 链接功能，不要与 <Link /> 组件混淆，它允许你只导入需要它们的路由中的东西。 因此，例如，CSS 文件可以限定范围，并且仅在需要这些特定文件的路由上导入。 链接元素作为对象数组从 links() 函数返回，可以是来自链接 API 的 HtmlLinkDescriptor，也可以是可以预取页面数据的 PageLinkDescriptor。

```js
export function links() {
  return [
    // add a favicon
    {
      rel: 'icon',
      href: '/favicon.png',
      type: 'image/png',
    },
    // add an external stylesheet
    {
      rel: 'stylesheet',
      href: '<https://example.com/some/styles.css>',
      crossOrigin: 'true',
    },
    // add a local stylesheet,
    { rel: 'stylesheet', href: stylesHref },

    // prefetch a page's data
    { page: '/about/community' },
  ];
}
```

## 总结

看完上面的文章你应该对 Remix 的基础知识有一了解了，并且已经准备好开始实际构建应用程序了，对吧？ Remix 提供了一个 Jokes 应用程序和一个博客教程，让您开始实施这些基本知识。 您也可以从头开始创建一个全新的 Remix 应用程序。 或者，如果您准备好深入探掘，不妨试试 K-Pop Stack。 现在轮到你开始创作了！
