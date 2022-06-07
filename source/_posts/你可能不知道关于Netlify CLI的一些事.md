---
title: 你可能不知道关于Netlify CLI的一些事
date: 2022-05-21 18:35:30
tags: Netlify, CLI
---

## 前言

首先，如果你不知道 Netlify 有一个 CLI，[他们知道](https://www.netlify.com/products/cli/)。我最喜欢的一件事就是在我的静态页面的项目里运行*netlify dev*命令时看到它检测到它正在做什么并在开发服务器启动站点，进度条慢慢填满。但不仅仅是一个开发服务器，而是复制 Netlify 环境的开发服务器，这意味着你可以在本地运行微服务和使环境变量可用之类的事情。

这里还有五件你可能没有意识到的事情。

## 1.使用模版创建网页

是的，就一行命令这么简单

```bash
netlify sites:create-template=
```

## 2.管理环境变量

你可以使用*netlify env*命令（Beta）来管理环境变量。

- *netlify env:list*：列出所有环境变量
- *netlify env:get / set*：获取设置变量
- *env:migrate --to <to-site-id>*：迁移整个项目的变量到另外一个

## 3.测试无服务器功能

通过使用 *Netlify CLI*在本地启动站点，你的微服务器功能将被运行。你可以测试它们是否正常工作并检查网络流量。但是 CLI 也可以帮助您，*netlify functions* 命令能够在命令行级别测试函数。例如，*netlify functions:invoke* 可以触发带有模拟数据的函数。

## 4.*直播*开发环境

记得之前提到你如何使用 *netlify dev* 在本地创建一个开发环境吗？这里的诀窍是执行 *netlify dev --live*。因此，你获得一个全世界都可以看到的特殊 *netlify.live URL*，而不是*localhost URL*。

## 5.运行*netlify switch*在不同的 Netlify 帐户之间切换

比如从你的个人项目到工作项目切换，实际上是使用 CLI 进行身份验证（netlify 登录，想象一下），这样您就可以代表你自己的 Netlify 帐户进行操作。部署网站等等。但是，你拥有多个 Netlify 帐户（如工作帐户和个人帐户）是完全合理的。

## BONUS！

推荐一个[视频](https://www.youtube.com/watch?v=4qR*Qs7s7CQ)时长 50 秒，展示了如何从在本地城建一些静态文件到使用 CLI 进行部署的过程。
