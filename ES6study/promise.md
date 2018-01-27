
# promise

## 基本含义
1. promise 是一个对象，可以获取异步操作的消息。
2. 对象有三种状态 pending、fullfilled、rejected。状态不受外界影响。
3. 状态能够pending->fullfilled (resolve) 或者pending->rejected (reject) 除此外不可。
4. 状态一旦更改就不会再变了

## 基本用法
```
  //生成实例
  const promise = new Promise(function(resolve, reject) {
    // ... some code
    console.log('Promise');

    if (true){
      resolve();
      console.log("resolve后")
    } else {
      reject(error);
    }
  });

  //then方法分别指定resolved状态和rejected状态的回调函数

  promise.then(function() {
    console.log('resolved.');
    // success
  }, function(error) {
    console.log('rejected.'+ error);
    // failure
  });
  
  const child = new Promise(function (resolve,reject) {
    resolve(promise)
  })
  child.then(function () {
    console.log("child resolve")
  },function (error) {
    console.log("child reject")
  })

  console.log('Hi!');
```
输出：Promise resolve后 Hi! resolved. child resolve <br>
* 可以看出promise的执行顺序，新建立即执行。而后走其余js代码，待异步返回时走then方法。
* child的resolve方法将promise作为参数，即模拟一个异步操作的结果是返回另一个异步操作，此时child就要等待promise的返回结果。如果promise在pending，child也会等待，直到promise返回结果。

## Promise.prototype.then
then方法返回的是一个新的Promise实例（注意，不是原来那个Promise实例）。因此可以采用链式写法，即then方法后面再调用另一个then方法。
```
getJSON("/post/1.json").then(function(post) {
  return getJSON(post.commentURL);
}).then(function funcA(comments) {
  console.log("resolved: ", comments);
}, function funcB(err){
  console.log("rejected: ", err);
});


//箭头函数简写
getJSON("/post/1.json").then(
  post => getJSON(post.commentURL)
).then(
  comments => console.log("resolved: ", comments),
  err => console.log("rejected: ", err)
);
```
第一个回调函数完成以后，会将返回结果作为参数，传入第二个回调函数。

## Promise.prototype.catch
用于指定发生错误时的回调函数。
如果对象状态变为resolved，则会调用then方法指定的回调函数；如果异步操作抛出错误，状态就会变为rejected，就会调用catch方法指定的回调函数，处理这个错误。如果运行中抛出错误，也会被catch方法捕获。<br>
错误具有“冒泡”性质，会一直向后传递，直到被捕获为止。也就是说，错误总是会被下一个catch语句捕获
```
// bad
promise
  .then(function(data) {
    // success
  }, function(err) {
    // error
  });

// good
promise
  .then(function(data) { //cb
    // success
  })
  .catch(function(err) {
    // error
  });
```
一般来说，不要在then方法里面定义 Reject 状态的回调函数（即then的第二个参数），总是使用catch方法。<br>

跟传统的try/catch代码块不同的是，如果没有使用catch方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码，即不会有任何反应。<br>
js代码仍进行，promise会吃掉错误。


## Promise.all()
Promise.all方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
`const p = Promise.all([p1, p2, p3]);`
如果promise自身有catch方法，那么不会触发all的catch，一样的吃掉错误


## Promise.race()
all相当于&& race相当于||操作
`const p = Promise.race([p1, p2, p3]);`
下面代码中，如果 5 秒之内fetch方法无法返回结果，变量p的状态就会变为rejected，从而触发catch方法指定的回调函数
```
const p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
]);
p.then(response => console.log(response));
p.catch(error => console.log(error));
```


## Promise.resolve()

## Promise.reject()

## Promise.try()

## 应用案例




