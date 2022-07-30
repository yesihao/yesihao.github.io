---
title: Azure Functions - 配置开发环境
date: 2020-03-29 20:55:10
tags: Serverless, Azure, Azure Function
---

## 前言

说[Azure functions](https://azure.microsoft.com/services/functions/)之前先来说说Serverless。Serverless是最近很火的一个概念，它是一种基于互联网的技术架构理念，应用逻辑并非全部在服务端实现，而是采用FAAS（Function as a Service）架构，通过功能组合来实现应用程序逻辑。应用业务逻辑将基于FAAS架构形成独立为多个相互独立功能组件，并以API服务的形式向外提供服务；很多公司都在自己的云平台推出了FAAS，比如Amazon Lambda，Azure Function，Google Cloud Functions，阿里云Function Compute等。

微软的Azure functions是基于Azure的无服务器计算服务，可以快速的帮助用户构建一个基于事件驱动的应用架构。用户无需关心服务器的部署运营，只需专注于核心的逻辑，既可以发布上线。费用按照实际调用次数与时长计费，自动缩放，仅在运行函数时为计算资源付费。更棒的是Azure Functions支持C#，Python，Javascript，TypeScript，F#和Java，本文带大家快速看在本地搭建Javascript开发环境开发Azure Functions。

## 环境安裝

可以使用[Visual Studio Code](https://code.visualstudio.com/)和[Visual Studio](https://visualstudio.microsoft.com/)开发，本文将会使用的VS Code。


首先在扩展市场中搜索Azure functions，你也可以直接安裝Azure App Service，这样你就拥有所有相关扩展服务，也可以仅安裝Azure Functions。

![Azure Function扩展](/images/Azure-Function-Ext.jpg)

接著，我们要安裝 Azure Function Core Tools，在命令行安装：

``` bash
#via npm
npm i -g azure-functions-core-tools --unsafe-perm true

#Linux
wget -q https://packages.microsoft.com/config/ubuntu/19.04/packages-microsoft-prod.deb
sudo dpkg -i packages-microsoft-prod.deb

#Mac
brew tap azure/functions
brew install azure-functions-core-tools
```

或者我们可以点击Debug | Run, VS Code会自动提示安装Azure Function Core Tools，确定后自动执行上方命令。

![Azure Function Core Tools 自动安装](/images/Azure-Function-Debug1.jpg)

安裝成功之后，我们可以在命令行输入 **func** 来检查是否安装成功。

![Azure Function Core Tools 安装成功](/images/Azure-Function-Core-Tools.jpg)

出现 **闪电** 就说明安装成功了！

##预告

下一篇博文将会讲到 **如何快速部署Azure Function**
