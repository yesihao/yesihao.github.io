---
title: object-view-box 初体验
date: 2022-03-24 21:25:34
tags: Css, object-view-box
---

## 前言

Ahmad Shadeed 的文章[css-object-view-box](https://ishadeed.com/article/css-object-view-box/)写得很好，他可以让我们对`object-view-box`属性的有一定的了解，他将其描述为在浏览器中使用 CSS 裁剪图像的原生方式。

## 如何使用

使用案例呢？好吧，Ahmad 开头展示了如何在没有`css-object-view-box`的情况下来完成一些图片裁剪需求。
1. [codepan例子1](https://codepen.io/geoffgraham/pen/WNMOyxG)：一个带有隐藏溢出的包装元素，该元素围绕一个尺寸和定位在该元素内的图像或
2. [codepan例子2](https://codepen.io/geoffgraham/pen/BaYZVLo)：背景图`background-image`。

如果使用 `object-view-box`，我们基本上可以像使用 SVG 的 `viewbox` 一样绘制图像边界。因此，我们可以使用普通的 `<img>` 标签并调用 `object-view-box` 以使用 `inset` 函数修剪边缘。我把Ahmad的例子放这里。

```css
img {
  width: 220px;
  aspect-ratio: 3 / 2;
  object-fit: cover;
}

.img-0 {
  aspect-ratio: 1 / 2;
}

.img-1 {
  object-view-box: inset(0% 19% -33% 57%);
}

.img-2 {
  object-view-box: inset(0% 15% -52% 3%);
}

.img-3 {
  object-view-box: inset(0% 0% 17% 0%);
}

.group {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 1rem 1.5rem;
}

/* Other styles */
.item {
  text-align: center;
  margin-bottom: 2rem;

  p {
    padding: 0.5rem;
  }

  code {
    font-size: 14px;
  }
}
.alert {
  display: block;
  max-width: 70ch;
  padding: 1rem;
  text-align: center;
  margin: 0 auto 1rem;
  background-color: #ffeeee;
  border: 1px solid red;
  border-radius: 5px;
  font-size: 0.75rem;
}

@supports (object-view-box: inset(0)) {
  .alert {
    display: none;
  }
}

body {
  font-family: "system-ui";
  padding: 1rem;
  line-height: 1.45;
}

img {
  max-width: 100%;
  box-shadow: 0 8px 15px 0 rgba(#000, 0.1), 0 2px 5px 0 rgba(#000, 0.2);
  border-radius: 12px;
}

*,
*:before,
*:after {
  box-sizing: border-box;
}
```

## 结尾

可惜`css-object-view-box`现在只支持*Chrome Canary*。 但是谷歌计划在[Chrome 104]中(https://chromestatus.com/feature/5213032857731072)支持！
