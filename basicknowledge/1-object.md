## 对象
### 一、类型属性
1) 数据属性
 - [[Configurable]] true  可配置  一旦变为不可配置就不能还原了 
 - [[Enumerrable]] true   可枚举
 - [[Writable]] true      可变值
 - [[Value]] undefined    数据值 <br><br>
若要修改以上属性默认的特性，必须使用ES5的 Object.defineProperty(对象, 属性名, 特性)
```
var person = {}
Object.defineProperty(person, name, {
  configurable: false,
  writable: false,
  value: 'Nicholas'
})
alert(person.name) //Nicholas
person.name = 'xiaofang'   //不可写
alert(person.name) //Nicholas
delete person.name         //不可配置
alert(person.name) //Nicholas
```
2) 访问器属性
 - [[Configurable]] true
 - [[Enumerrable]] true
 - [[Get]] undefined  在读取属性时调用的函数
 - [[Set]] undefined  在写入属性时调用的函数 <br><br>
 同样的，若要修改以上属性默认的特性，必须使用ES5的 Object.defineProperty
 ```
 var book = {
  _year: 2004,
  edition: 1
}
Object.defineProperty(book, "year",{
  get:function () {
    return this._year
  },
  set:function (newValue) {
    if(newValue > 2004){
      this._year = newValue
      this.edition += newValue - 2004
    }
  }
})
book.year = 2005
alert(book.edition)  //2
 ```
* 可以批量设置特性 Object.defineProperties() <br>
* 也可以读取属性特性 Object.getOwnPropertyDescriptor()
```
var book = {}
Object.defineProperties(book,{
  _year:{
    value: 2004
  },
  edition:{
    value: 1
  },
  year:{
    get:function () {
      return this._year
    },
    set:function (newValue) {
      if(newValue > 2004){
        this._year = newValue
        this.edition += newValue - 2004
      }
    }
  }
})

var descriptor = Object.getOwnPropertyDescriptor(book, "_year")
alert(descriptor.value)  //2004
alert(descriptor.configurable)  //false
```

### 二、创建对象的方法
1. 创建Object实例
```
var person = new Object()
person.name = '小明'
person.age = 15
person.sayName = function () {
  alert(this.name)
}
```
2. 创建对象字面量
```
var person = {
  name:'小明',
  age: 15,
  sayName:function () {
    alert(this.name)
  }
}
```
3. 工厂模式
```
function createPerson(name, age) {
  var o = new Object()
  o.name = name
  o.age = age
  o.sayName = function () {
    alert(this.name)
  }
  return o
}
var person1 = createPerson('小明', 15)
```
4. 构造函数模式
步骤：1) 创建一个新对象 2) 把构造函数的作用域指向新对象，this就指向新对象了 3) 执行 4) 返回新对象
```
function Person(name, age) {
  this.name = name
  this.age = age
  this.sayName = function () {
    alert(this.name)
  }
}
var person1 = new Person('小明', 15)
alert(person1.constructor === Person)  //true
alert(person1 instanceof Person)  //true
Person("Greg", 17) //构造函数作为普通函数调用
window.sayName() //'Greg'
var o = new Object()
Person.call(o, "Kitty", 13) //在另一个对象的作用域中调用
o.sayName()  //"Kitty"

```
5. 原型模式


### 三、继承
