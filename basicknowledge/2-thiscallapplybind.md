## this
this总是指向一个对象，具体哪个是在运行时基于函数的执行环境**动态绑定**的，而非函数被声明时的环境
1. 作为对象的方法调用
this指向该对象
```
var obj = {
  a:1,
  getA: function(){
    alert(this===obj) //true
    alert(this.a)  //1
  }
}
obj.getA()
```
2. 作为普通函数调用
this指向全局对象，如果是浏览器则为window
```
window.name = 'globalName'
var myObject = {
  name: 'sven',
  getName: function(){
    return this.name
  }
}
var getName = myObject.getName
alert(getName()) //globalName

window.id = 'window'
document.getElementById('div1').onclick = function(){
  alert(this.id) //div1
  var callback = function(){
    alert(this.id)
  }
  callback() //window
}
```
3. 构造器调用

4. call、apply调用

## call
<[].concat.apply([], arr)>
调用一个对象的一个方法，以另一个对象替换当前对象
call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisObj 指定的新对象。
## apply

## bind
