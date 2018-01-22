
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
      console.log("resolve 后")
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
输出：Promise resolve 后 Hi! resolved. child resolve <br>
* 可以看出promise的执行顺序，新建立即执行。而后走其余js代码，待异步返回时走then方法。
* child的resolve方法将promise作为参数，即模拟一个异步操作的结果是返回另一个异步操作，此时child就要等待promise的返回结果。如果promise在pending，child也会等待，直到promise返回结果。





