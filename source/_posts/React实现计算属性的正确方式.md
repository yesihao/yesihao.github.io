---
title: React实现计算属性的正确方式
date: 2021-04-21 22:12:41
tags: React, Computed
---

## 前言

React没有像其他框架比如（Ember、Vue 等）那样开箱即用的计算属性，所以我们如何以正确的方式实现这个功能呢？最常见的做法是如果是类组件，在 *render* 方法中“计算”，或者是函数组件就是在主体中。

## 问题
每次渲染都会进行计算，这意味着即使需要计算的值没有改变，也会生成一个新变量。另外，如果我们将它作为 prop 传递给子组件并且不是原始类型（字符串、数字或布尔值），则子组件将重新渲染，因为它认为这是一个新值，这可能会导致性能问题。

## 解决方案
如果参数没有改变，将计算提取到一个函数并使用记忆化来防止被重新初始化，如果我们在做大量数据等昂贵的计算，这会给我们带来性能上的提升。

## 例子

类组件
```js
import React from 'react'
import memoize from 'lodash/memoize'
import MenuItem from './MenuItem'

class RenderSuggestion extends React.Component {

  renderSuggestionStyles = (suggestionValue) => ({ pointerEvents: suggestionValue ? 'all' : 'none' })
  renderSuggestionStylesMemo = memoize(this.renderSuggestionStyles)

  render(){
    const { suggestion, ...other } = this.props
    return (
      <MenuItem
        {...other}
        style={this.renderSuggestionStylesMemo(suggestion.value + 1)}
      />
    )
  }
}
```

函数组件

```js
import React from 'react'
import memoize from 'lodash/memoize'
import MenuItem from './MenuItem'

const renderSuggestionStyles = suggestionValue => ({ pointerEvents: suggestionValue ? 'all' : 'none' })
const renderSuggestionStylesMemo = memoize(renderSuggestionStyles)

function RenderSuggestion({ suggestion, ...other }) {
  return (
    <MenuItem
      {...other}
      style={renderSuggestionStylesMemo(suggestion.value + 1)}
    />
  )
}
```
在这里，如果我们不以正确的方式（没有记忆化）使用计算属性，则当道具更改时，子组件（MenuItem）将不必要地重新渲染。

Reselect
解决此问题的另一种很酷的方法是使用 reselect

```js
import React from 'react'
import { createSelector } from "reselect";
import MenuItem from './MenuItem'

class RenderSuggestion extends React.Component {

  renderSuggestionStyles = createSelector(
     (suggestionValue) => ({ pointerEvents: suggestionValue ? 'all' : 'none' })
  )

  render(){
    const { suggestion, ...other } = this.props
    return (
      <MenuItem
        {...other}
        style={this.renderSuggestionStyles(suggestion.value + 1)}
      />
    )
  }
}
```
