---
title: 如何使用VS Code快速部署Azure Function
date: 2020-05-5 21:15:33
tags: Serverless, Azure, Azure Function, VS Code
---

## 前言

本文将介绍部署Azure Function的完整过程。这一次会使用**Node.js**作为开发语言，以及使用**VS Code**作为编辑器。
![如何使用VS Code如何快速部署Azure Function-1](/images/如何使用VS Code如何快速部署Azure Function-1.jpg)

在后面的博文中会写到如何提高Node.js Azure functions的性能。

## 进入正题

##### 首先在Azure portal上创建一个Function App然后选择使用**VS Code**做为开发环境。

![1](https://tangocode.com/wp-content/uploads/2019/09/image19.png)

我们先了解一些基本参数：

1. App name：functions群组的唯一名称。这也是资源最终URL的尾部。比如App name是example，URL就会是example.azurewebsites.net
2. Subscription: 选择你的订阅
3. Resource group & version: 选择开发语言和版本
4. OS: Windows是默认的选项，但是你也可以使用Linux
5. Runtime stack: 我们选择node，版本选择10
6. Publish: 发布类型，我们选择code
7. Hosting plan: 现在一个消费计划，Azure Fuctions计费已次计算，很便宜。具体参考[Azure functions定价](https://azure.microsoft.com/zh-cn/pricing/details/functions/)
8. Location: 选择一个资源的位置，中国速度最快应该是东亚地区
9. Application Insights: 用于记录日志定位问题
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image12.png)

点击**创建**过几分钟之后，会提示创建成功。
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image13.png)

然后我们选择VS Code作为开发环境
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image22.png)

提示我们需要安装所需工具，在上一篇博文中，我们介绍安装了开发Azure Functions的工具。这里不再赘述。
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image23.png)


我们推荐在VS Code中安装两个扩展：
1. Azure Account: 整合Azure账户和VS Code.
2. Azure Functions: 帮助我们创建，维护，管理，部署function
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image9.png)

下一步我们登录的我们的Azure账号，然后我们就可以看到我们刚刚建立的Function App
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image8.png)

现在可以使用Azure Functions扩展床一个HTTP trigger的function
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image15.png)

然后输入Function名称，选择‘Function’作为缱绻等级
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image18.png)
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image21.png)

现在我们已经创建了一个function。主文件是index.js。这是一个模版，它接受一个get或post请求，在参数中如果有name字段，则返回“Hello ” + (req.query.name || req.body.name)，否则返回400错误.
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image4.png)

另外一个重要的文件是function.json，它包含了一些配置。比较常用的配置是HTTP请求方式。你可以选择GET, POST, PUT, DELETE, OPTIONS等。
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image6.png)

##### 测试你的Function然后部署到Azure

本地运行之后，我们可以断点进行调试。
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image14.png)

接下来我们要部署到这个function，选中function名称，右键，选择“Deploy to Function App...”
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image20.png)


选择你想部署到的Fnction App
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image3.png)
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image16-1.png)
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image7.png)

如果没有问题，我们可以收到一个部署成功的提示
![xxxx](https://tangocode.com/wp-content/uploads/2019/09/image2.png)

## 总结
使用VS Code开发是非常高效的，它集合Azure官方的扩展，所以很容易地本地开发和部署到Azure。
