---
title: 静谧的CSS斑驳灯光效果
date: 2022-01-16 18:44:11
tags: CSS, Effect, 灯光效果
---

## 前言

傍晚的阳光透过沙沙作响的树叶照射进来，给人宁静而温暖的感觉。许多艺术家使用斑驳的光来营造一种柔和、催眠的效果。

## 斑驳效果

我们可以在网页设计中使用同样的斑驳效果。使用在照片和插图上，可以为原本单调乏味的内容墙增添魔力，让它们恢复生机。有一种只用 CSS 的简单而快速的方法来添加这个效果。

在我们开始谈代码之前，了解斑驳光的组成很重要。它是光透过树叶和树枝形成的大光点组成的。因为我们正在谈论的是穿过许多不确定的空间的光，有时阴影区域有清晰的边缘，但更多时候是模糊的。当光从更远的距离投射阴影时，会扩散和扭曲光线附近的墙壁，使得阴影的边界变得模糊。
这是有和没有斑驳光照射的白墙的区别：

![效果](https://i0.wp.com/css-tricks.com/wp-content/uploads/2022/01/s_CD332C21E06B9998C2C4A90E3C0E480B2753BB0A4B42D91B522F923752A2AB0A_1640085806943_ss.png?resize=1824%2C762&ssl=1)

我将会使用纯文本和有趣的表情符号来创造斑驳的灯光效果，使用 CSS 阴影和混合来模仿自然。我也将会介绍一些其他的方法。

## 设置场景

我们将使用文本（字母表中的字母、特殊字符、表情符号等）来创建光的形状。我所说的光，是指半透明的乳白色。同时，我们想要的是柔和的灯光效果，而不是那种锐利、清晰或鲜明的效果。

由斑驳的光产生的斑点有多种形状，所以最好的选择是椭圆或长方形的字符。我会使用 🍃、🍂，\因为它们是椭圆的、长方形的和倾斜的，有点混乱且难以预测。

我将它们包装在 .backdrop 父元素中包含的段落中：

```js
<div class="backdrop">
  <p class="shapes">🍃</p>
  <p class="shapes">🍂</p>
  <p class="shapes">\</p>
</div>
```

我使用父元素作为投射斑驳光影的表面，为其纹理应用背景图像。我不仅给表面一个明确的 width 和 height，而且在它上面设置了隐藏的溢出，这样我就可以投射超出表面的阴影而不暴露它们。由于 CSS 网格，投射出斑驳光效果的对象在背景表面的中间对齐：

```css
.backdrop {
  background: center / cover no-repeat url('image.jpeg');
  width: 400px;
  height: 240px;
  overflow: hidden;
  display: grid;
}
.backdrop > * {
  grid-area: 1/1;
}
```

如果形状没有完全对齐也没有关系，只要它们能够达到你想要的斑驳的效果，就可以了。所以没有必要完全按照我的做法来放置这些东西，你可以尝试调整这些值来获得不同图案的斑驳光！

## 在 CSS 中设计斑驳的灯光

这些是表情符号应该具有的关键属性——transparent 颜色、黑色半透明背景（使用 alpha 通道 inrgbargba()）、font-size 漂亮的模糊白色 text-shadow，最后是 mix-blend-mode 来平滑过渡。
mix-blend-mode 设置元素的颜色与其容器的内容的融合方式。该 multiply 值使元素的背景通过元素的浅色显示并保持深色相同，从而产生更好、更自然的斑驳光效果。

```css
.shapes {
  color: transparent;
  background-color: rgba(0, 0, 0, 0.3); // Use alpha transparency
  text-shadow: 0 0 40px #fff; // Blurry white shadow
  font: bolder 320pt/320pt monospace;
  mix-blend-mode: multiply;
}
```

## 细化颜色和对比度

我希望背景上的 background-image 更亮一点，所以我还添加了 filter: brightness(1.6). 另一种实现方法是 background-blend-mode，将一个元素的所有不同背景混合在一起，而不是将表情符号添加为单独的元素，我们将它们添加为背景图像。
请注意，在最后一个示例中，我使用了不同的表情符号，以及 floralwhite 一些比纯白色的光线强度更低的颜色。这是展开的表情符号背景图像之一：

```xml
<svg xmlns='http://www.w3.org/2000/svg'>
  <foreignObject width='400px' height='240px'>
    <div xmlns='http://www.w3.org/1999/xhtml' style=
      'font: bolder 720pt/220pt monospace;
       color: transparent;
       text-shadow: 0 0 40px floralwhite;
       background: rgba(0, 0, 0, 0.3);'
    >
      🌾
    </div>
  </foreignObject>
</svg>
```

如果想将自己的图像用于形状，要确保边界模糊以创造柔和的光线。CSS blur()过滤器可以很方便地处理同样的事情。我还使用 CSS@supports 来调整某些浏览器的阴影模糊值作为后备。
现在让我们回到第一个示例并添加一些内容：

```html
<div class="backdrop">
  <p class="shapes">🍃</p>
  <p class="shapes">🍂</p>
  <p class="shapes">\</p>
</div>

<p class="content">
  <img
    width="70px"
    style="float: left; margin-right: 10px;"
    src="image.jpeg"
    alt=""
  />
  Top ten tourists spots for the summer vacation <br /><br /><i
    style="font-weight: normal;"
    >Here are the most popular places...</i
  >
</p>
```

.backdrop 和.shapes 以前的风格基本相同。至于.content 同样位于 之上的.backdrop，我添加 isolation: isolate 以形成一个新的堆叠上下文，将元素从混合中排除，作为一种精炼的触感。

## 动画光源

我还决定添加一个简单的 CSS 动画，并将@keyframes 其应用于.backdrop :hover：

```css
.backdrop:hover > .shapes:nth-of-type(1) {
  animation: 2s ease-in-out infinite alternate move;
}
.backdrop:hover > .shapes:nth-of-type(2):hover {
  animation: 4s ease-in-out infinite alternate move-1;
}

@keyframes move {
  from {
    text-indent: -20px;
  }
  to {
    text-indent: 20px;
  }
}
@keyframes move-1 {
  from {
    text-indent: -60px;
  }
  to {
    text-indent: 40px;
  }
}
```

为 emoji 上的 text-indent 属性设置动画会产生一种非常微妙的运动，就像移动的云层改变光线的方向。

## 总结

好了！我们从自然和艺术中汲取了一些灵感，模仿了太阳穿过树木和灌木丛，将斑驳的光和阴影点投射到物体的表面。我们用 CSS 和几个表情符号完成了所有这些。

这个方法的关键在于我们如何在表情符号上应用颜色，在浅色中使用额外的模糊 text-shadow 来设置光线，半透明的 background-color 定义阴影斑点。我们所要做的就是确保光影的背景使用具有足够对比度的逼真的纹理，这样就能看到斑驳的光线了。
