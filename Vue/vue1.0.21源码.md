## 一、index文件入口
```
import Vue from './instance/vue’
import installGlobalAPI from './global-api'

installGlobalAPI(Vue)
```
设置Vue版本号
执行setTimeout...

## 二、Vue对象实例
./instance/vue
```
function Vue (options) {
  this._init(options)
}

// install internals
initMixin(Vue)
stateMixin(Vue)
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
