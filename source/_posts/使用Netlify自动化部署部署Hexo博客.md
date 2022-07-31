---
title: 使用Netlify自动化部署部署Hexo博客
date: 2022-07-18 23:14:45
tags: Netlify, Hexo, CICD
---

## 前言

现在有越来越多的平台支持用户分享自己创作的文章，常见的就有知乎、微信公众号、简书、CSDN 等，这些平台为你的内容带来流量的同时，也要你承担相应的限制，比如 CSDN 就会出现各种广告，微信公众号就不能添加外部链接等等。总的来说还是不自由，内容创作者只是把内容托管到了它们的平台上面而已。尽管博客在中国流行的时间还是在十几年前，这个时间点搭建一个博客甚至还有点逆时代潮流的感觉，但是也正是自己的站点，能给予自己更多的自由，更多的表达空间，尤其是程序员、摄影师等，更需要一个展示自己内容的空间。

## Hexo

Hexo 博客本质上是一个 CMS(Content Management System)，内容管理系统，从官网的解释里面可以看到，这是一个静态的博客，也就是说，写作的流程是这样的：

- 用 Markdown 写作
- 使用 Hexo 生成静态页面
- 使用 Hexo 部署静态页面

## Netlify

Netlify 是一个提供 Web 项目托管的平台，支持 Github、Gitlab、Bitbucket 等平台的项目，这里我们主要提供 Netlify 来部署我们的网站，并且自动发布最新的网站更改，其次 Netlify 还提供免费的二级域名。

## 创建发布博客

### 创建 Hexo 博客项目

```bash
npm i -g hexo-cli # 安装hexo cli
hexo init project-name # 创建项目
```

### 创建 Github 仓库 push 代码

Github 就不多提了，push 更新到 GitHub 的仓库，让 Netlify 自动部署就行。

### 创建 Netlify 账户

- 使用 GitHub 账号登录
- `Add a new site` 然后选择 `Import an existing project` ，最后选择对应的 Github 仓库
- 填写表单如下，然后点击`Deploy Site`
  ![](/images/netlify-hexo-1.jpg)
- 设置域名，点击`Add a custom domain to your site`。根据提示在 DNS 平台设置 CNAME 的 DNS，然后点击`Verify`，基本两分钟内搞定
- 使用 HTTPS，可以使用自定义的证书，也可以使用 Netlify 申请免费证书

大功告成 [Demo](https://blog.shaneye.fit)

当使用 Github 将网站项目文件夹里的所有东西上传完毕之后，那么就可以打开 Netlify 给予它访问 Github 仓库的权限。
在此之后，这一切都是自动的：包括当修改了哪篇文章之后：只要 Github 上你网站的仓库发生了变化，Netlify 就会自动重新构建网页文件并且自动发布。
