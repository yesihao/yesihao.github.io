---
title: CSS “decorations” 装饰
date: 2021-12-26 23:41:09
tags: CSS, decorations, 装饰
---

## 前言

在 Wikipedia 的[Common.css](https://en.wikipedia.org/wiki/MediaWiki:Common.css)有一段 CSS

```css
.mw-collapsible-leftside-toggle .mw-collapsible-toggle {
  /* @noflip */
  float: left;
  /* @noflip */
  text-align: left;
}
```

这里的@noflip 有什么用呢？这就是一些人所说的“CSS 装饰”，我觉得这很形象。实际上，它们只是 CSS 注释而已，但显然这里还有更多内容，因为它们看起来像具有功能的语句。

如果没有某种 CSS 处理过程，这些注释将无济于事。我不是很确定这里使用的是什么 CSS 处理器，但我认为假设它运行时会产生一个“从右到左”的样式表是合理的，它会变成浮动：左 进入浮动：右和文本对齐：左进入文本对齐：右。

## 替代方案？

我认为值得注意的是，现在使用大多数浏览器支持的 `text-align: start` 效果可能更好，这样就不必依赖 CSS 处理和备用样式表来修改 CSS。但是我觉得 `float` 是没有“逻辑”等价物，但可能有一种方法可以重构布局（使用`grid`？），这样“翻转”就是不必要的。将元素包裹在一个元素周围对于浮动来说是非常独特的，所以这里可能没有简单的替代方案。

稍微搜索了一下，似乎是 `/* @noflip */` 的来源[is CSSJanus](https://cssjanus.github.io/#input/.mw-collapsible-leftside-toggle%20.mw-collapsible-toggle%20%7B%0A%5C%20%20float%3A%20left%3B%0A%20%20%2F*%20%40noflip%20*%2F%0A%20%20text-align%3A%20left%3B%0A%7D)

![网页截图](https://i2.wp.com/css-tricks.com/wp-content/uploads/2021/11/Screen-Shot-2021-11-16-at-5.52.59-AM.png?w=1842&ssl=1)

这个[仓库](https://github.com/cssjanus/cssjanus)显示它是由维基媒体制作的，所以我认为这是一个已解决的案例。看起来这项技术已经运用到其他，比如样式组件的插件，Sublime Text 的插件，Salesforce 甚至在他们的设计系统中使用了它。

还有另一个名为 `css-flip` 的处理器（现在已归档，出自 Twitter）看起来它做了完全相同的事情，自述文件显示了有多少属性可能需要这个：

```css
background-position, background-position-x, border-bottom-left-radius, border-bottom-right-radius, border-color, border-left, border-left-color, border-left-style, border-left-width, border-radius, border-right, border-right-color, border-right-style, border-right-width, border-style, border-top-left-radius, border-top-right-radius, border-width, box-shadow, clear, direction, float, left, margin, margin-left, margin-right, padding, padding-left, padding-right, right, text-align transition transition-property
```

## 总结

It would have hugely surprised me if there wasn’t a PostCSS plugin for this, and a little searching turned up postcss-rtl, but alas, it’s also been deprecated by the owner.

This all started with talking about “CSS decorators” though, which I guess we’re defining as “CSS comments that have processor directives in them.” The one I personally use the most is this:

我想一定会一个实现 flip 的`PostCSS` 插件，然后我就搜索到了 `postcss-rtl`，但是它也被所有者弃用了 ☹️。

不过，这一切都始于谈论“CSS 装饰”，我想我们应该将其定义为“其中包含处理器指令的 CSS 注释”。 我个人用得最多的是这个：

```css
/* prettier-ignore */
.cool {
  linear-gradient(
    to left,
    pink
    pink 20%
    red  20%
    red
  )
}
```

我很喜欢 `Prettier`，但如果我花时间自己格式化一些 CSS 以提高可读性，我会在前一行中添加一个 /* prettier-ignore */ 以格式化的时候弄乱它。
