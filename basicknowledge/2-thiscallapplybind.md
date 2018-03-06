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
```
fun.call(thisArg, arg1, arg2, ...)
```
调用一个对象的一个方法，以另一个对象替换当前对象
call 方法可将一个函数的对象上下文从初始的上下文改变为由 thisArg 指定的新对象。
thisArg的取值有以下4种情况：
1. 不传，或者传null,undefined， 函数中的this指向window对象
2. 传递另一个函数的函数名，函数中的this指向这个函数的引用
3. 传递字符串、数值或布尔类型等基础类型，函数中的this指向其对应的包装对象，如 String、Number、Boolean
4. 传递一个对象，函数中的this指向这个对象
```
function a(){   
  console.log(this);   //输出函数a中的this对象
}       

function b(){}       

var c={name:"call"};    //定义对象c  

a.call();   //window
a.call(null);   //window
a.call(undefined);   //window
a.call(1);   //Number
a.call('');   //String
a.call(true);   //Boolean
a.call(b);   //function b(){}
a.call(c);   //Object
```

### 应用
1、使用call方法调用父构造函数来实现继承
```
function Product(name, price) {
  this.name = name;
  this.price = price;

  if (price < 0) {
    throw RangeError(
      'Cannot create product ' + this.name + ' with a negative price'
    );
  }
}

function Food(name, price) {
  Product.call(this, name, price);
  this.category = 'food';
}

//等同于
function Food(name, price) {
  this.name = name;
  this.price = price;
  if (price < 0) {
    throw RangeError(
      'Cannot create product ' + this.name + ' with a negative price'
    );
  }

  this.category = 'food';
}

//function Toy 同上
function Toy(name, price) {
  Product.call(this, name, price);
  this.category = 'toy';
}

var cheese = new Food('feta', 5);
var fun = new Toy('robot', 40);
```
2、使用call方法调用匿名函数

```
var animals = [
  {species: 'Lion', name: 'King'},
  {species: 'Whale', name: 'Fail'}
];

for (var i = 0; i < animals.length; i++) {
  (function (i) { 
    this.print = function () { 
      console.log('#' + i  + ' ' + this.species + ': ' + this.name); 
    } 
    this.print();
  }).call(animals[i], i);
}
```
3、使用call方法调用函数并指定上下文的this
```
function greet() {
  var reply = [this.person, 'Is An Awesome', this.role].join(' ');
  console.log(reply);
}

var i = {
  person: 'Douglas Crockford', role: 'Javascript Developer'
};

greet.call(i); // Douglas Crockford Is An Awesome Javascript Developer
```

## apply

## bind

### 参考
https://www.cnblogs.com/libin-1/p/6069031.html
