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
setData方法如下：
* 先循环oldData的key,如果newData中没有了，执行_unproxy移除代理
* 循环newData的key，如果this中没有该key，执行_proxy添加代理
* observe(newData, this)
```
Vue.prototype._setData = function (newData) {
  newData = newData || {}
  var oldData = this._data
  this._data = newData
  var keys, key, i
  // unproxy keys not present in new data
  keys = Object.keys(oldData)
  i = keys.length
  while (i--) {
    key = keys[i]
    if (!(key in newData)) {
      this._unproxy(key)
    }
  }
  // proxy keys not already proxied,
  // and trigger change for changed values
  keys = Object.keys(newData)
  i = keys.length
  while (i--) {
    key = keys[i]
    if (!hasOwn(this, key)) {
      // new property
      this._proxy(key)
    }
  }
  oldData.__ob__.removeVm(this)
  observe(newData, this)
  this._digest()
}
```
unproxy
```
if (!isReserved(key)) {
  delete this[key]
}
```
proxy:这样 this.prop === this._data.prop
```
if (!isReserved(key)) {
  var self = this
  Object.defineProperty(self, key, {
    configurable: true,
    enumerable: true,
    get: function proxyGetter () {
      return self._data[key]
    },
    set: function proxySetter (val) {
      self._data[key] = val
    }
  })
}
```
isReserved：看是否以$或者_开头 
```
var c = (str + '').charCodeAt(0)
return c === 0x24 || c === 0x5F
```

2、定义_initState方法，顺次执行各种初始化
```
Vue.prototype._initState = function () {
  this._initProps()
  this._initMeta()
  this._initMethods()
  this._initData()
  this._initComputed()
}
```

