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
### 嵌套路由
在模板里加入<router-view> 配置在routes中的children
```
routes: [{
    path: ‘/user/:id’, component: User,
    children: [
       { path: ‘profile’ , component:UserProfile },   /user/:id/profile
       { path: ‘posts’ , component:UserPosts }      /user/:id/posts
    ] 
}]
```
### 编程式的导航
```
router.push( location )  router.replace( location )   router.go( n )  n向前向后多少步
router.push( ‘home’ )    { path: ’home’ }   { name:’user’, params: { userId:123 }}
```
### 命名路由
```
routes: [{ path:’/user/:userId’, name: ‘user’, component: User }]
```
### 命名视图
需要在同时展示多个视图
```
<router-view></router-view>
<router-view name=“a”></router-view>
routes:[{
    path:’/‘, components:{ default:Foo, a:Bar }
}]
```
### 重命名和别名
```
需要访问/a 重定向URL替换为/b
routes:[{
    path:’/a’, redirect: ’/b'
}]
需要访问/a 与访问/b一样的页面
routes:[{
    path:’/a’, component:A, alias:’/b'
}]
```
### 路由组件传参
组件如果需要用到路由获取值，会高度耦合。可以借助props传递
```
const User = {
  props: ['id'],
  template: '<div>User {{ id }}</div>'
}
const router = new VueRouter({
  routes: [
    { path: '/user/:id', component: User, props: true },

    // 对于包含命名视图的路由，你必须分别为每个命名视图添加 `props` 选项：
    {
      path: '/user/:id',
      components: { default: User, sidebar: Sidebar },
      props: { default: true, sidebar: false }
    }
  ]
})
```

### 导航守卫
用来拦截导航，让它完成跳转或取消

##### 全局钩子
当一个导航触发时，全局前置守卫按照创建顺序调用。守卫是异步解析执行，此时导航在所有守卫 resolve 完之前一直处于 等待中。
```
const router = new VueRouter({ ... })

router.beforeEach((to, from, next) => {
  // ...
})
```
接收三个参数 to from next
一定要调用next方法来resolve这个钩子。next()、next(false)、next('/')、next({ path: '/' })
```
router.afterEach((to, from) => {
  // ...
})
```
##### 路由独享
```
const router = new VueRouter({
  routes: [
    {
      path: '/foo',
      component: Foo,
      beforeEnter: (to, from, next) => {
        // ...
      }
    }
  ]
})
```

##### 完整的导航解析流程
1. 导航被触发。
2. 在失活的组件里调用离开守卫。
3. 调用全局的 beforeEach 守卫。
4. 在重用的组件里调用 beforeRouteUpdate 守卫 (2.2+)。
5. 在路由配置里调用 beforeEnter。
6. 解析异步路由组件。
7. 在被激活的组件里调用 beforeRouteEnter。
8. 调用全局的 beforeResolve 守卫 (2.5+)。
导航被确认。
调用全局的 afterEach 钩子。
触发 DOM 更新。
用创建好的实例调用 beforeRouteEnter 守卫中传给 next 的回调函数。

### 过渡动效
包裹一层<transition>在外面，依靠name区分
```
watch:{
    ‘$route’ ( to,from ){
        const toDepth = to.path.split(‘/‘).length
        const fromDepth = from.path.split(‘/‘).length
        this.transitionName = toDepth< fromDepth ? ’slide-right’ : ’slide-left'
    }
}
```
