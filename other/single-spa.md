# single-spa文档翻译

## 概况
兼容性 <br>
Chrome, Firefox, Safari, IE11, and Edge

每个前端都可以用自己的框架编写，构建共存的微前端。
* 在同一页面上使用多个框架而无需刷新页面（React，Vue，AngularJS，Angular，Ember或您正在使用的任何内容）
* 使用新框架编写代码，而无需重写现有应用程序
* 延迟加载代码，用于改善初始加载时间

案例 （https://github.com/CanopyTax/single-spa-examples） <br>
简单的getStart （https://github.com/joeldenning/simple-single-spa-webpack-example）

Single-spa通过将生命周期应用于整个应用程序，从React组件生命周期中获取灵感。 它最初是出于使用React + react-router而不是永远坚持使用我们的AngularJS + ui-router应用程序的愿望，但现在单一spa支持几乎任何与其他框架共存的框架。

Single-spa项目包含如下
1. 许多应用程序，每个应用程序有点像整个SPA本身。 <br>
应用程序响应url路由事件，并且必须知道如何从DOM引导，装载和卸载它们。 SPA和应用程序之间的主要区别在于应用程序必须共存，并且每个应用程序都不具有自己的html页面。 例如，您的React或Angular应用程序是活动或休眠的应用程序。 处于活动状态时，它们会侦听url路由事件并将内容放在DOM上。 休眠时，它们不会侦听url路由事件，而是完全从DOM中删除。
2. 一个single-spa-config <br>
这个config是个html页面，里面的JavaScript注册了各应用的名字、加载应用程序代码的方法、确定应用程序何时处于活动/休眠状态的函数

如果你的项目没有通过scratch开始，那么需要迁移spa成为一个独立应用
React - Migrating to single-spa
AngularJS - Migrating to single-spa
