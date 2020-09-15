---
title: 使用IntersectionObserver检查页面是否滚动到特定点
date: 2020-8-29 22:19:41
tags: JavaScript, IntersectionObserver
---

网页滚动就是一个DOM事件。我可以使用window.scrollY随时查看窗口滚动了多远。我可以监听该事件并获取该数值：

```javascript
window.addEventListener("scroll", () => {
  console.log(window.scrollY)
});
```

假设我想知道用户是否向下滚动了100px或更多，我可以通过判断window.Y > 100来进行测试。这里，我将结果log出来：

```javascript
window.addEventListener("scroll", () => {
  if (window.scrollY < 100) {
    console.log("Not past 100px");
  } else {
    console.log("Past 100px!");
  }
});
```

```javascript
<div id="pixel-to-watch"></div>
```

虽然它简单，易于理解且有效，但这是一个坏主意。因为这有点反模式，这种事件会发生很多次。当用户向下滚动页面时，它可以轻松触发几十，数百或数千次。每次这样做，我们都必须在JavaScript的单线程上运行。这意味着需要更多的时间来解决滚动问题，而花费较少的时间进行其他重要的事情。

有很多方法可以减少这种强度，并且自然地，这是一个非常好的主意。Throttling和Debouncing是JavaScript中提高性能的良好模式。其要点是，在它们执行之前，它们阻止执行较大的JavaScript。

不过，还有更好的方法:

```javascript
<div id="pixel-to-watch"></div>
```

```javascript
#pixel-to-watch {
  position: absolute;
  width: 1px;
  height: 1px;
  top: 100px;
  left: 0;
}
```

使用IntersectionObserver:

```javascript
let observer = new IntersectionObserver(entries => {
  console.log(entries);
  if (entries[0].boundingClientRect.y < 0) {
    console.log("Past 100px!");
  } else {
    console.log("Not past 100px");
  }
});
observer.observe(document.querySelector("#pixel-to-watch"));
```
