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

### 动态路由匹配
适用于传参 id=… 需要 /user/foo 与 /user/bar 映射到相同路由
```
routes: [{ path: ‘/user/:id’, component: User }]
```
页面取值 {{ $route.params.id }}
```
/user/:username                    /user/evan            { username: ‘evan' }
/user/:username/post/:post_id      /user/evan/post/123   { username:’evan’, post_id:123 }
```
响应路由参数的变化：
从/user/foo 到/user/bar 原来的组件实例被复用，因此组件声明周期钩子不会再被调用
复用组件时，想对路由参数的变化作出响应的话，可以简单地 watch（监测变化） $route 对象
```
const User = {
  template: '...',
  watch: {
    '$route' (to, from) {
      // 对路由变化作出响应...
    }
  }
}

//或者
const User = {
  template: '...',
  beforeRouteUpdate (to, from, next) {
    // react to route changes...
    // don't forget to call next()
  }
}
```
