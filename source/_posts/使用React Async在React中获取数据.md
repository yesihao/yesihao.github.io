---
title: 使用React Async在React中获取数据
date: 2022-02-26 22:11:35
tags: React, Async, 数据
---

## 前言

你可能已经习惯了在 React 中使用 axios 或 fetch 来获取数据。处理数据获取的通常方法是:

- 进行 API 调用。
- 如果一切按计划进行，使用响应更新状态。
- 或者，在遇到错误的情况下，会向用户显示一条错误消息。

当通过网络处理请求时，总是会有延迟。当你提出请求并等待回应时，这只是过程的一部分。这就是为什么我们经常使用加载转轮来告诉用户预期的响应正在加载。

## 例子

所有这些都可以使用一个名为[React Async](https://github.com/async-library/react-async)的库来完成。

React Async 是一个基于承诺的库，可以让你在 React 应用程序中获取数据。让我们来看看各种使用组件、钩子和帮助器的例子，看看我们如何在发出请求时实现加载状态。

在本教程中，我们将使用 Create React App。你可以运行以下命令来创建一个项目:

完成后，运行 yarn 或 npm 命令在你的项目中安装 React Async:

```bash
## yarn
yarn add react-async

## npm
npm install react-async --save
```

### 组件中的加载器

这个库允许我们在 JSX 中直接使用<Async>。因此，组件的示例如下所示;

```js
// Let's import React, our styles and React Async
import React from 'react';
import './App.css';
import Async from 'react-async';

// We'll request user data from this API
const loadUsers = () =>
  fetch('https://jsonplaceholder.typicode.com/users')
    .then((res) => (res.ok ? res : Promise.reject(res)))
    .then((res) => res.json());

// Our component
function App() {
  return (
    <div className="container">
      <Async promiseFn={loadUsers}>
        {({data, err, isLoading}) => {
          if (isLoading) return 'Loading...';
          if (err) return `Something went wrong: ${err.message}`;

          if (data)
            return (
              <div>
                <div>
                  <h2>React Async - Random Users</h2>
                </div>
                {data.map((user) => (
                  <div key={user.username} className="row">
                    <div className="col-md-12">
                      <p>{user.name}</p>
                      <p>{user.email}</p>
                    </div>
                  </div>
                ))}
              </div>
            );
        }}
      </Async>
    </div>
  );
}

export default App;
```

首先，我们创建了一个名为 loadUsers 的函数。这将使用 fetch API 进行 API 调用。当它这样做时，它返回一个得到解决的承诺。之后，组件就可以使用所需的包了。

这些包为:

- isLoading：处理尚未从服务器接收到响应的情况。
- err：用于遇到错误的情况。您也可以将其重命名为 error 错误.
- data：这是从服务器获得的预期数据。

正如你在这个例子中看到的，我们要显示一些东西给依赖于包的用户。

### 示例 2: 钩子上的加载器

如果你喜欢钩子，在使用 React Async 时有一个钩子选项可用。它看起来是这样的:

```js
// Let's import React, our styles and React Async
import React from 'react';
import './App.css';
import {useAsync} from 'react-async';

// Then we'll fetch user data from this API
const loadUsers = async () =>
  await fetch('https://jsonplaceholder.typicode.com/users')
    .then((res) => (res.ok ? res : Promise.reject(res)))
    .then((res) => res.json());

// Our component
function App() {
  const {data, error, isLoading} = useAsync({promiseFn: loadUsers});
  if (isLoading) return 'Loading...';
  if (error) return `Something went wrong: ${error.message}`;
  if (data)
    // The rendered component
    return (
      <div className="container">
        <div>
          <h2>React Async - Random Users</h2>
        </div>
        {data.map((user) => (
          <div key={user.username} className="row">
            <div className="col-md-12">
              <p>{user.name}</p>
              <p>{user.email}</p>
            </div>
          </div>
        ))}
      </div>
    );
}

export default App;
```

这看起来与组件示例类似，但在本场景中，我们使用的是 useAsync 组件，而不是 Async 组件。响应返回一个被解析的承诺，我们也可以访问类似的包，就像我们在上一个例子中做的，然后我们可以返回到渲染的 UI。

### 示例 3: 帮助程序中的加载器

Helper 组件在提高我们的代码的可读性方面很有帮助。这些帮助程序可以在使用 useAsync 钩子或 Async 组件时使用，这两个都是我们刚刚看到的。下面是一个在 Async 组件中使用 helper 的例子。

```js
// Let's import React, our styles and React Async
import React from 'react';
import './App.css';
import Async from 'react-async';

// This is the API we'll use to request user data
const loadUsers = () =>
  fetch('https://jsonplaceholder.typicode.com/users')
    .then((res) => (res.ok ? res : Promise.reject(res)))
    .then((res) => res.json());

// Our App component
function App() {
  return (
    <div className="container">
      <Async promiseFn={loadUsers}>
        <Async.Loading>Loading...</Async.Loading>
        <Async.Fulfilled>
          {(data) => {
            return (
              <div>
                <div>
                  <h2>React Async - Random Users</h2>
                </div>
                {data.map((user) => (
                  <div key={user.username} className="row">
                    <div className="col-md-12">
                      <p>{user.name}</p>
                      <p>{user.email}</p>
                    </div>
                  </div>
                ))}
              </div>
            );
          }}
        </Async.Fulfilled>
        <Async.Rejected>
          {(error) => `Something went wrong: ${error.message}`}
        </Async.Rejected>
      </Async>
    </div>
  );
}

export default App;
```

这看起来和我们使用包的时候很类似。完成这些之后，你可以将应用程序的不同部分分解成微小的组件。

## 总结

如果你已经厌倦了走我在本教程开始部分提到的路线，你可以在你正在做的项目中开始使用 React Async。本教程中使用的源代码可以在 GitHub 上的不同分支中找到。
