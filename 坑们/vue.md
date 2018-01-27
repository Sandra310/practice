# Vue

## 1、data functions should return an object
data必须return一个对象data(){return {}}
data 必须声明为返回一个初始数据对象的函数，因为组件可能被用来创建多个实例。如果 data 仍然是一个纯粹的对象，则所有的实例将共享引用同一个数据对象

## 2、城市列表切换
#### 页面结构：
我来描述一下，头部是tab切换（国内城市、国际城市）上部分是热门城市列表（一堆banner）下部分是按照拼音排序的热门城市cell  A 阿姆斯特丹...等等。右侧是ABCD...的快速导航条。
#### 问题：
以上这个页面在渲染国内城市时候正常显示loading后出现数据，而切换到国际城市时，loading不转了，页面卡顿，而后突然出现数据loading消失。
#### 思考过程：
首先想到的，是不是先去计算渲染数据耗费了太多性能，导致页面loading不转。于是尝试加个setTimeOut将计算操作延后进行。结果loading也会卡顿。应该是计算渲染消耗，导致loading都卡死。<br>
接下来考虑是dom渲染问题还是js计算，我们把vue循环渲染数组slice(0,10)发现并没有好转。判断应该是js计算导致。<br>
于是看到js代码的计算过程

## 3、axios兼容
因为 IE 整个家族都不支持 promise, 解决方案:
```
npm install es6-promise

// 在 main.js 引入即可
// ES6的polyfill
require("es6-promise").polyfill();
```

## 4、UC访问空白
有可能是有些ES6的代码没有降阶彻底。查看是否使用某些特性没有对应的polyfill

## 5、如何调用子组件方法
$refs  这点与react相同。

## 6、组件通讯
父传子：props；子传父：emit。其余vuex

## 7、vuex、localStorage、sessionStorage
vuex
可维护性: 因为是单向数据流,所有状态是有迹可循的...数据的传递也可以及时分发响应
易用性: 它使得我们组件间的通讯变得更强大,而不用借助中间件这类来实现不同组件间的通讯
但是vuex得到数据要在localStorage或sessionStorage做持久化，不然一刷新就没了。

## 8、首屏加载慢，打包文件大
减少第三方库的使用,比如jquey这些都可以不要了
若是引入moment这些,webpack 排除国际化语言包
webpack 常规压缩js,css
路由组件采用懒加载



### 参考
https://juejin.im/post/59fa9257f265da43062a1b0e










