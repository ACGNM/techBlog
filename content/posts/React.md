---
title: "React"
date: 2021-03-21T01:07:15+09:00
draft: false
toc: false
images:
tags:
  - react
---

# React

## terms
- JSX: Javascript Syntax Extention (allow us to write dynamic html)
- Virtual DOM (Document Object Model)
- ternary operator 三元运算符


## Component
- 两种形式：函数形式和类形式
- 使用props在组件之间传递值（类似于JS中的函数参数）

## State
- determines how a component reders and behaves
- 相当于属性，在组件中存储数据
- "app" state and "global" state refers to state that is avalable to the entire UI
- 如果最后出现一大堆全局state, 可以使用context api或者使用第三方的state管理器`redux`

## React Hooks
- 是一系列函数让函数形式的组件可以像类组件一样使用state和其lifecycle

## create react app

### Tips
- `react-dom`: responsilble for rendering react application out to the domcument object model to the browser
- `react-javascript`: comes with development server and build tools
- `index.js`: entry point of react
- 组件函数的返回值必须是一个有父母元素的JSX。如果不想让返回的要素再包裹一层div（作为empty fragment返回），则使用`<>...</>`包裹即可
- 在嵌入的JSX代码中设置style需要用双花括号 `return <div style={{ color: 'red' }}>...</div>`
- 使用js的map函数生成的list的每个要素都要有一个唯一的key。不然控制台会报警
- 需要使用icon的话可以安装`react-icon`: `npm -i react-icon`
