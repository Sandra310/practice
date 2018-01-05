## 安装
1. npm install -g create-react-app
2. create-react-app studyreact
3. cd studyreact
4. npm start

## 1、元素渲染
1. 在HTML中定义一个根节点`<div id="root"></div>`,一般来讲只会定义一个根节点，我们会把所有React渲染到根DOM节点中
```
import React from 'react'
import ReactDOM from 'react-dom'

const element = <h1>Hello, world</h1>;  //一个元素
ReactDOM.render(
  element,
  document.getElementById('root')
);
```
按照安装步骤装好，删除src下全部文件，添加index.js，将上文代码拷入，此时，会在页面生成很大的Hello World
2.  

## 2、组件

## 3、Props
