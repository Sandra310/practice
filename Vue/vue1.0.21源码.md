## 一、index文件入口
```
import Vue from './instance/vue’
import installGlobalAPI from './global-api'

installGlobalAPI(Vue)
```
接着执行设置Vue版本号
执行setTimeout...

## 二、Vue对象实例
./instance/vue
```
function Vue (options) {
  this._init(options)
}

// install internals
initMixin(Vue)  定义Vue.prototype._init方法
stateMixin(Vue) 定义一些与数据状态有关的方法
eventsMixin(Vue)
lifecycleMixin(Vue)
miscMixin(Vue)

// install instance APIs  
dataAPI(Vue)
domAPI(Vue)
eventsAPI(Vue)
lifecycleAPI(Vue)

export default Vue
```

### initMixin
```
// ...一堆初始化

// call init hook
this._callHook('init')  //执行init钩子

// initialize data observation and scope inheritance.
this._initState()  //初始化数据observation和scope继承。

// setup event system and option events.
this._initEvents() //设置事件系统

// call created hook
this._callHook('created') //执行create钩子

// if `el` option is passed, start compilation.
if (options.el) {
  this.$mount(options.el)  //开始编译
}
```
### stateMixin
1、改造数据的访问器属性get set方法。如果更改了调用_setData(newData)
```
Object.defineProperty(Vue.prototype, '$data', {
  get () {
    return this._data
  },
  set (newData) {
    if (newData !== this._data) {
      this._setData(newData)
    }
  }
})
```
