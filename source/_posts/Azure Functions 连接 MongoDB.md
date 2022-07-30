---
title: Azure Functions 连接 MongoDB
date: 2020-06-22 22:01:36
tags: Serverless, Azure, Azure Function, MongoDB
---

## 前言

无服务器架构计算变得越来越流行，但是，当使用无服务器架构来构建后端API时，经常会遇到一些小问题。使用无状态函数是否意味着必须在每次运行该函数时都建立与数据库的新连接？ 其实大多数问题都有解决方法，因此不必每次运行函数时花费额外的时间来连接数据库。所以今天准备写一下关于使用MongoDB驱动程序和mongoose与Azure Functions重复使用数据库连接的文章。本文将完成在Node.js中创建Azure Functions的过程，以及将该函数将连接到MongoDB并在请求之间重用数据库连接。

## 创建一个简单的Azure函数

在之前的文章中已经讲解了如何创建Azure Functions，这里也不再赘述。首先我们在Azure Portal上创建一个Azure Function。
![创建好的Azure Function](/images/Azure-Functions-连接-MongoDB1.jpg)

## 安装MongoDB驱动程序
接下来让我们将该功能连接到数据库。在Azure Function中添加npm模块与在AWS Lambda的过程非常不同。使用Azure Functions，必须登录到服务器，然后创建package.json，然后运行npm install。对于“无服务器”架构来说，这似乎很奇怪，但是好处就是不必一遍又一遍地捆绑相同的依赖，也不必担心node_modules运行在Lambda对函数大小的限制。

要安装MongoDB Node.js驱动程序，请首先转到<your-function-name>.scm.azurewebsites.net，然后单击“调试控制台”->“ PowerShell”。
![PowerShell](/images/Azure-Functions-连接-MongoDB2.jpg)

转到D:\home\site\wwwroot目录下，新建名为的package.json文件，增加MongoDB一览，最后保存。
![创建好package.json](/images/Azure-Functions-连接-MongoDB3.jpg)

执行`npm i`
![执行`npm i`](/images/Azure-Functions-连接-MongoDB4.jpg)


回到Azure Function编辑界面，写入代码。
``` javascript
const mongodb = require('mongodb');

// URI for MongoDB Atlas xxx为省略 https://www.mongodb.com/cloud/atlas
const uri = 'mongodb+srv://xxx.mongodb.net/test';

module.exports = function (context, req) {
  context.log('Running');
  mongodb.MongoClient.connect(uri, function(error, client) {
    if (error) {
      context.log('Failed to connect');
      context.res = { status: 500, body: res.stack }
      return context.done();
    }
    context.log('Connected');

    client.db('test').collection('tests').find().toArray(function(error, docs) {
      if (error) {
        context.log('Error running query');
        context.res = { status: 500, body: res.stack }
        return context.done();
      }

      context.log('Success!');
      context.res = {
        headers: { 'Content-Type': 'application/json' },
        body: JSON.stringify({ res: docs })
      };
      context.done();
    });
  });
};
```

## 重用数据库连接
但是每次该函数运行时创建一个新的数据库连接会降低性能。所以与Lambda一样，可以使用Node.js运行时的小技巧来保留调用之间的数据库连接。具体来说，脚本中的全局变量可能在函数调用之间保留，因此，如果向MongoDB客户端添加全局指针，则它将一直保留，直到它被Azure清除它。

``` javascript
const mongodb = require('mongodb');

const uri = 'mongodb+srv://xxx/test';

// May be retained between function executions depending on whether Azure
// cleans up memory
let client = null;

module.exports = function (context, req) {
  context.log('Running');

  let hasClient = client != null;

  if (client == null) {
    mongodb.MongoClient.connect(uri, function(error, _client) {
      if (error) {
        context.log('Failed to connect');
        context.res = { status: 500, body: res.stack }
        return context.done();
      }
      client = _client;
      context.log('Connected');
      query();
    });
  } else {
    query();
  }

  function query() {
    client.db('test').collection('tests').find().toArray(function(error, docs) {
      if (error) {
        context.log('Error running query');
        context.res = { status: 500, body: res.stack }
        return context.done();
      }

      context.log('Success!');
      context.res = {
        headers: { 'Content-Type': 'application/json' },
        body: 'Num docs ' + docs.length + ', reused connection ' + hasClient
      };
      context.done();
    });
  }
};
```
第一次运行会创杰数据库连接，第二次运行就会从全局变量中拿到以前的连接。很明显会更快。

## 最后
Azure Functions不需要您捆绑node_modules，而是让您在功能应用程序中的多个功能之间共享node_modules。
Azure Functions可以大规模执行代码，而不必担心置备和管理基础虚拟机和操作系统。MongoDB使开发人员能够快速将想法变为现实，而不会影响数据库的工作效率。 Azure平台和MongoDB一起为开发人员提供了一套快速开发的环境。
