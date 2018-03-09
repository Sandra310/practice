### 安装
--npm install vue-router
```
import Vue from ‘vue'
import VueRouter from ‘vue-router'
Vue.use(VueRouter) 
```

### 基本语法
```
html:
<router-link to=“/foo”>Go to Foo</router-link>
<router-view></router-view>

js:
const Foo = { template:’<div>foo</div>' }
const routes = [ { path: ‘/foo' } ]
const router = new VueRouter({ routes })
const app = new Vue({
    router
}).$mount(‘#app')

```
