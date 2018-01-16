# 单例模式
### 含义
保证一个类仅有一个实例，并提供一个访问它的全局访问点
### 场景
有一些对象往往只需要一个，比如线程池、全局缓存、浏览器中的window <br>例如：登录窗口、loading等等，只会被创建一次
## 代理单例
```
var CreateDiv = function (html) {
  this.html = html
  this.init()
}

CreateDiv.prototype.init = function () {
  var div = document.createElement('div')
  div.innerHTML = this.html
  document.body.appendChild(div)
}

var ProxySingletonCreateDiv = (function () {
  var instance
  return function (html) {
    if(!instance){
      instance = new CreateDiv(html)
    }
    return instance
  }
})()

var a = new ProxySingletonCreateDiv('sven1')
var b = new ProxySingletonCreateDiv('sven2')
alert(a===b)
```
## 惰性单例
```
var getSingle = function (fn) {
  var result
  return function () {
    return result || (result = fn.apply(this, arguments))
  }
}

var createLoginLayer = function () {
  var div = document.createElement('div')
  div.innerHTML = '登录窗口'
  div.style.display = 'none'
  document.body.appendChild(div)
  return div
}
var createSingleLoginLayer = getSingle(createLoginLayer)
document.getElementById('loginBtn').onclick = function () {
  var loginLayer = createSingleLoginLayer()
  loginLayer.style.display = 'block'
}
```
