---
title: SVG图案填充-Pattern
date: 2020-11-13 23:42:31
tags: svg, xml
---

## 前言

SVG图案一般用于在图图形对象内部定义形状，创建一个新形状并用图案填充它。一个SVG元素或是位图图像，都可以通过<pattern>元素在x轴或y轴方向以固定的间隔平铺。
下面会展示几个的简单SVG形状的。这个列表可能会随着时间的推移而增长，但是要想拥有一个全面的集合，可以在这基础上创造新图案。

## 圆形图案

```html
<svg width="100%" height="100%">

  <mask maskUnits="userSpaceOnUse" id="fade">
    <linearGradient id="gradient" x1="0" y1="0" x2="0" y2="100%">
      <stop offset="0" style="stop-color: #FFFFFF"></stop>
      <stop offset="1" style="stop-color: #000000"></stop>
    </linearGradient>
    <rect fill="url(#gradient)" width="100%" height="100%"></rect>
  </mask>

  <pattern id="pattern-circles" x="0" y="0" width="40" height="40" patternUnits="userSpaceOnUse">
    <circle mask="url(#fade)" cx="20" cy="20" r="20"></circle>
  </pattern>
  <rect x="0" y="0" width="100%" height="100%" fill="url(#pattern-circles)"></rect>

</svg>
```

![SVG图案填充-Pattern1](/images/SVG图案填充-Pattern1.jpg)

## 棋盘图案

```html
<svg width="100%" height="100%">

  <pattern id="pattern-checkers" x="0" y="0" width="200" height="200" patternUnits="userSpaceOnUse">
    <rect class="checker" x="0" width="100" height="100" y="0"></rect>
    <rect class="checker" x="100" width="100" height="100" y="100"></rect>
  </pattern>

  <rect x="0" y="0" width="100%" height="100%" fill="url(#pattern-checkers)"></rect>

</svg>

```

![SVG图案填充-Pattern2](/images/SVG图案填充-Pattern2.jpg)

## 六角形图案

```html
<svg width="100%" height="100%">

    <pattern id="pattern-hex" x="0" y="0" width="112" height="190" patternUnits="userSpaceOnUse" viewBox="56 -254 112 190">

      <g id="hexagon">
        <path d="M168-127.1c0.5,0,1,0.1,1.3,0.3l53.4,30.5c0.7,0.4,1.3,1.4,1.3,2.2v61c0,0.8-0.6,1.8-1.3,2.2L169.3-0.3 c-0.7,0.4-1.9,0.4-2.6,0l-53.4-30.5c-0.7-0.4-1.3-1.4-1.3-2.2v-61c0-0.8,0.6-1.8,1.3-2.2l53.4-30.5C167-127,167.5-127.1,168-127.1 L168-127.1z"></path>
        <path d="M112-222.5c0.5,0,1,0.1,1.3,0.3l53.4,30.5c0.7,0.4,1.3,1.4,1.3,2.2v61c0,0.8-0.6,1.8-1.3,2.2l-53.4,30.5 c-0.7,0.4-1.9,0.4-2.6,0l-53.4-30.5c-0.7-0.4-1.3-1.4-1.3-2.2v-61c0-0.8,0.6-1.8,1.3-2.2l53.4-30.5 C111-222.4,111.5-222.5,112-222.5L112-222.5z"></path>
        <path d="M168-317.8c0.5,0,1,0.1,1.3,0.3l53.4,30.5c0.7,0.4,1.3,1.4,1.3,2.2v61c0,0.8-0.6,1.8-1.3,2.2L169.3-191 c-0.7,0.4-1.9,0.4-2.6,0l-53.4-30.5c-0.7-0.4-1.3-1.4-1.3-2.2v-61c0-0.8,0.6-1.8,1.3-2.2l53.4-30.5 C167-317.7,167.5-317.8,168-317.8L168-317.8z"></path>
      </g>

  </pattern>
  <rect x="0" y="0" width="100%" height="100%" fill="url(#pattern-hex)"></rect>

</svg>
```

![SVG图案填充-Pattern3](/images/SVG图案填充-Pattern3.jpg)

## 立方体图案


```html
<svg width="100%" height="100%">

   <pattern id="pattern-cubes" x="0" y="126" patternUnits="userSpaceOnUse" width="126" height="200" viewBox="0 0 10 16">

     <g id="cube">
       <path class="left-shade" d="M0 0l5 3v5l-5 -3z"></path>
       <path class="right-shade" d="M10 0l-5 3v5l5 -3"></path>
     </g>

     <use x="5" y="8" xlink:href="#cube"></use>
     <use x="-5" y="8" xlink:href="#cube"></use>

   </pattern>

   <rect x="0" y="0" width="100%" height="100%" fill="url(#pattern-cubes)"></rect>

</svg>
```

![SVG图案填充-Pattern4](/images/SVG图案填充-Pattern4.jpg)
