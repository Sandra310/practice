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
4. 构造函数模式 <br>
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
```
function Person() {

}
Person.prototype.name = 'Nicholas'
Person.prototype.age = 29
Person.prototype.job = "Software Engineer"
Person.prototype.sayName = function () {
  alert(this.name)
}
var person1 = new Person()
var person2 = new Person()
alert(person1.sayName == person2.sayName())  //true
```
![](https://github.com/Sandra310/practice/blob/master/basicknowledge/images/1.png)

#### 判断属性的各种方法
1. isPrototypeOf()<br>
虽然无法通过访问[[Prototype]] 来确定对象之间的关系，但是可以用 isPrototypeOf(),如果[[Prototype]]指向调用isPrototypeOf()方法的对象，那么就返回true
```
alert(Person.prototype.isPrototypeOf(person1)) //true
alert(Person.prototype.isPrototypeOf(person2)) //true
```
2. Object.getPrototypeOf()<br>
ES5增加了一个新方法 Object.getPrototypeOf() 返回[[Prototype]]的值，即该对象的原型
```
alert(Object.getPrototypeOf(person1) == Person.prototype)  //true
alert(Object.getPrototypeOf(person1).name)  //Nicholas
```
3. hasOwnPrototype()<br>
使用 hasOwnPrototype() 方法可以检测属性是存在于实例还是原型中。只有存在于实例中会返回true
```
alert(person1.hasOwnProperty('name')) //false
```
4. in<br>
使用 in 可以判断属性是否存在于对象中，无论实例还是原型。使用 for-in 循环，返回的是所有能够通过对象访问、可枚举的属性，屏蔽了原型中不可枚举的属性<br>
所以如果要判断一个属性是否是原型的
```
function hasPrototypeProperty(object, name) {
  return !object.hasOwnProperty(name) && (name in object)
}
```
5. Object.keys()<br>
返回一个包含所有可枚举的实例属性的字符串数组
```
var key = Object.keys(Person.prototype)
alert(key) //"name,age,job,sayName"
```
6. Object.getOwnPropertyNames()<br>
如果要得到所有实例的属性，注意是实例不包含原型属性，无论是否可枚举，使用Object.getOwnPropertyNames()
```
var keys = Object.getOwnPropertyNames(Person.prototype)
alert(keys) //"constructor,name,age,job,sayName"
```
6. 组合使用构造函数模式和原型模式
```
function Person(name, age) {
  this.name = name
  this.age = age
  this.friends = ['apple','orange']
}
Person.prototype = {
  constructor: Person,
  sayName : function () {
    alert(this.name)
  }
}

var person1 = new Person('小明',17)
var person1 = new Person('Greg',27)
```
7. 动态原型模式
```
function Person(name, age) {
  this.name = name
  this.age = age
  if(typeof this.sayName() != "function"){
    Person.prototype.sayName = function () {
      alert(this.name)
    }
  }
}
var friend = new Person("Nicholas", 29)
friend.sayName()
```
8. 寄生构造函数模式<br>
除了new操作符并把包装的叫构造函数外，和工厂函数一模一样
```
function Person(name, age) {
  var o = new Object()
  o.name = name
  o.age = age
  o.sayName = function () {
    alert(this.name)
  }
  return o
}
var person1 = new Person('小明', 15)
```
9. 稳妥构造函数模式 <br>
没有公共属性，而且其方法也不引用this的对象。下面的例子除了使用sayName()方法外，没有其他方法可以访问name的值
```
function Person(name, age) {
  var o = new Object()
  o.name = name
  o.age = age
  o.sayName = function () {
    alert(name)
  }
  return o
}
```
### 三、继承
