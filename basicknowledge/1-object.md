## 对象
### 一、类型属性
1) 数据属性
 - [[Configurable]] true  可配置  一旦变为不可配置就不能还原了 
 - [[Enumerable]] true   可枚举
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
 - [[Enumerable]] true
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
#### 1. 创建Object实例
```
var person = new Object()
person.name = '小明'
person.age = 15
person.sayName = function () {
  alert(this.name)
}
```
#### 2. 创建对象字面量
```
var person = {
  name:'小明',
  age: 15,
  sayName:function () {
    alert(this.name)
  }
}
```
#### 3. 工厂模式
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
#### 4. 构造函数模式 <br>
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
#### 5. 原型模式
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
3. hasOwnProperty()<br>
使用 hasOwnProperty() 方法可以检测属性是存在于实例还是原型中。只有存在于实例中会返回true
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
#### 6. 组合使用构造函数模式和原型模式
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
#### 7. 动态原型模式
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
#### 8. 寄生构造函数模式<br>
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
#### 9. 稳妥构造函数模式 <br>
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
#### 1.原型链
引用类型值会所有原型链上对象共同使用，会造成问题
```
function SuperType() {
  this.property = true
}
SuperType.prototype.getSuperValue = function () {
  return this.property
}
function SubType() {
  this.subproperty = false
}
SubType.prototype = new SuperType()
SubType.prototype.getSubValue = function () {
  return this.subproperty
}

var instance = new SubType()
alert(instance.getSuperValue())  //true
alert(instance.getSubValue())  //false
```

![](https://github.com/Sandra310/practice/blob/master/basicknowledge/images/2.png)

#### 2.组合继承
最常用的继承模式
```
function SuperType(name) {
  this.name = name
  this.colors = ["red", "blue", "green"]
}
SuperType.prototype.sayName = function () {
  alert(this.name)
}

function SubType(name, age) {
  //通过apply和call方法可以在新创建的对象上执行构造函数
  //继承了SuperType,同时还传递了参数(这里只能继承到Super本人的属性，例如colors、name，并不能将Super原型上的属性继承到)
  SuperType.call(this, name) 
  this.age = age
}
//实现继承
SubType.prototype = new SuperType()
SubType.prototype.constructor = SubType
SubType.prototype.sayAge = function () {
  alert(this.age)
}

var instance1 = new SubType("Nicholas", 29)
instance1.colors.push("black")
alert(instance1.colors) //"red,blue,green,black"
instance1.sayName() // "Nicholas"
instance1.sayAge() // 29

var instance2 = new SubType("Greg", 27)
alert(instance2.colors) //"red,blue,green"
instance2.sayName() // "Greg"
instance2.sayAge() // 27
```
#### 3、原型式继承
相当于对传入的o进行了浅复制
```
function object(o) {
  function F() {}
  F.prototype = o
  return new F()
}
var person = {
  name: "Nicholas",
  colors: ["red", "blue", "green"]
}

var person1 = object(person)
person1.name = "Greg"
person1.colors.push("black")

var person2 = object(person)
person2.name = "Linda"
person2.colors.push("orange")

console.log(person.colors)//["red", "blue", "green", "black", "orange"]
console.log(person.isPrototypeOf(person1)) //true
console.log(Object.getPrototypeOf(person1) == person.prototype) //false
console.log(Object.getPrototypeOf(person1) == person) //true
```
ES5新增了Object.create()方法规范了原型式继承，传入一个参数时同object方法、第二个参数为每个属性通过自己的描述符定义
```
var anotherPerson = Object.create(person,{
  name:{
    value: "Greg"
  }
})
```
#### 4、寄生式继承
```
function createAnother(original) {
  var clone = object(original)
  clone.sayHi = function () { //做了某些工作
    alert("hi")
  }
  return clone
}
```
#### 5、寄生组合式继承（最理想）
组合继承的不足：无论什么情况，都会调用两次超类型构造函数；<br>
一次是创建子类型原型的时候 SubType.prototype = new SuperType() <br>
另一次是在子类型构造函数内部 SuperType.call(this,name) <br>
子类型会包含超类型对象的全部实例属性，不得不在调用子类型构造函数时重写这些属性。<br><br>
寄生组合式继承的基本模式
```
function inheritPrototype(subType, superType) {
  var prototype = object(superType.prototype) //创建对象
  prototype.constructor = subType  //增强对象
  subType.prototype = prototype    //指定对象
}
```
