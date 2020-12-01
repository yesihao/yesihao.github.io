---
title: 反向字符串 - JavaScript
date: 2020-7-30 18:29:11
tags: JavaScript, string
---

## 前言

反向字符串问题是常见的算法问题。我们需要写一个反向字符串的函数，如果将“tom”传递给函数，则返回“mot”。在本文中，我们将考虑针对它的4个JavaScript解决方案。

## 方法1.使用数组reverse·方法

有了Array.reverse() 方法，我们可以轻松地反转数组。 reverse() 方法将数组反转到位。但我们正在处理字符串，这意味着我们必须使用split方法将字符串转换为数组，使用reverse方法将其反向，并使用join方法将其转换回字符串。 这是代码示例。

```javascript
function reverseString(string) {
	let array = string.split('');

  array.reverse()

  return array.join('')
}
```

我们可以使用箭头函数写成一行

```javascript
const reverseString = (string) => string.split('').reverse().join('');
```
## 方法2.使用For Of循环

这是反向转换字符串的经典示例。我们在这里要做的是创建一个空字符串，该字符串将保留反向的string，循环遍历字符串中的每个字符，并将其附加到新字符串的开头。

```javascript
function reverse(str) {
  let reverseString = '';

  for (let character of str) {
    reverseString = character + reverseString;
  }

  return reverseString
}
```
## 方法3.使用Array.reduce()

[reduce](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)方法在数组的每个元素上执行reducer函数，从而产生单个输出值。要使用reduce方法，我们需要将字符串转换为数组。接下来，我们使用reduce方法将其转换为字符串。这样，reducer将字符串的每个字符附加到累加值，该累加器值就是反转的字符串。

```javascript
function reverseString(string) {
	const array = string.split('');

  const reversedString = array.reduce((reversed, character) => {
    return character + reversed
  }, '')

  return reversedString
}
```

上述函数也可以写成下面的形式

```javascript
const reverseString = (string) => {
	return string.split('').reduce((reversed, character) => character + reversed, '')
}
```
甚至可以写成更短的单行

```javascript
const reverseString = (string) => string.split('').reduce((rev, char) => char + rev, '')
```

## 方法4.使用递归

你熟悉递归的吗？ 递归是一种通过使用调用自身的函数来解决问题的方法。每次函数调用自身时，它将问题减少为子问题。该递归调用继续进行，直到到达无需进一步递归。
你可以[参考](https://dev.to/sloan/explain-recursion-like-im-five-5c6)。

首先我们使用string.substring（）方法获取删除字符串中的第一个字符，并将其他字符传递给函数。 然后，将第一个字符添加到return语句中，如下面的代码所示。

```javascript
function reverse(string){
  if(string === '') {
    return string
  } else{
    return reverse(string.substring(1)) + string[0]
  }
}
```
我们可以使用三元操作符简化上述函数

```javascript
function reverse(string) {
 	return string ? reverse(string.substring(1)) + string[0] : string
}
```

## 总结

这样就可以用四种很酷的方法来反转JavaScript中的字符串。扎实的数据结构和算法知识来自许多实践，还有什么更好的方式欢迎留言。
