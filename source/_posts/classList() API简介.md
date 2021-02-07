---
title: classList() API简介
date: 2020-10-06 13:02:12
tags: classList, javascript
---

## 前言

之前为了操纵DOM元素class列表，经常会引入jQuery之类的库。其实html5的classList API还是非常方便的，而且兼容性也不错。

## API用法

假设我们有这样一个DOM元素


```html
<div id="el"></div>
```

获取DOM元素
```javascript
const el = document.querySelector("#el");
```

然后你可以用classList方法操纵该元素上所有的类
```javascript
// 添加一个类
el.classList.add("open");

// 添加多个类
el.classList.add("this", "little", "piggy");
let classes = ["is-message", "is-warning"];
el.classList.add(...classes);

// 删除一个类
el.classList.remove("open");

// 删除多个类
el.classList.remove("this", "little", "piggy");

// 遍历每个类
el.classList.forEach(className => {
  // 不要使用“class”作为变量名，因为这是保留字
  console.log(className);
});
for (let className of $0.classList) {
  console.log(className);
}

el.classList.length; // 获取类的个数

// 替换类
el.classList.replace("is-big", "is-small");

// 切换一个类（如果存在，则将其删除，如果不存在，则将其添加）
el.classList.toggle("open");
// 删除类
el.classList.toggle("open", false);
// 增加类
el.classList.toggle("open", true);

// 查看各个类是否存在
el.classList.contains("open");

// 按顺序查看每个类：<div class="hot dog">
el.classList.item(0); // hot
el.classList.item(1); // dog
el.classList.item(2); // null
el.classList[1]; // dog
```

## 兼容性

![.classList() API简介](/images/classListAPI1.jpg)
