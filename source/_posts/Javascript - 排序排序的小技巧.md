---
title: Javascript - 排序排序的小技巧
date: 2020-02-28 19:34:40
tags: Javascript, array
---

### 普通数组:

``` javascript
let fruits = [`bananas`, `Apples`, `Oranges`];
```

很简单，一行搞定：

``` javascript
fruits.sort();
```

**但是**要注意数组中不同字符串的大小写不一致... 大写的字符会被排在小写字符之前😂
所以还有其他的步骤：

``` javascript
let fruits = [`bananas`, `Apples`, `Oranges`];
fruits.sort((a, b) => {
  return a.toLowerCase().localeCompare(b.toLowerCase());
})
console.log(fruits);

// ["Apples", "bananas", "Oranges"]
```

### 对象数组（按对象键值排序）
对对象数组的排序会变得很稍微更复杂一些。经常我们会在处理JSON API的时候遇到。

``` javascript
let fruits = [
  {
    fruit: `Bananas`
  },
  {
    fruit: `apples`
  },
  {
    fruit: `Oranges`
  }
];
```

我们可以为此写一个排序函数，但是更进一步我们需要一个更通用的方法把需要排序的键以参数的形式传进去

``` javascript
const propComparator = (propName) =>
  (a, b) => a[propName].toLowerCase() == b[propName].toLowerCase() ? 0 : a[propName].toLowerCase() < b[propName].toLowerCase() ? -1 : 1
So now we can use it to sort:

fruits.sort(propComparator(`fruit`));
console.log(fruits);

/*
[
  {fruit: "apples"},
  {fruit: "Bananas"},
  {fruit: "Oranges"}
]
*/
```

### 普通对象

如果我们有个对象是这样的：

``` javascript
let fruits = {
  Bananas: true,
  apples: false,
  Oranges: true
};
```

我们依然可以将那些键小写化，然后我们先排序键的数组，然后用排序好的键数组建一个新对象

``` javascript
let sortedFruits = {};
Object.keys(fruits).sort((a, b) => {
  return a.toLowerCase().localeCompare(b.toLowerCase());
}).forEach(function(key) {
  sortedFruits[key] = fruits[key];
});
console.log(sortedFruits);

/*
{
  apples: false,
  Bananas: true,
  Oranges: true
}
*/
```

###  对象数组（按对象键排序）

``` javascript
let fruits = [
  {
    Bananas: true
  },
  {
    Apples: false
  },
  {
    oranges: true
  }
];
```

可能这是上述情况中最复杂，但是结合所有的方法，其实也很简单🐶
