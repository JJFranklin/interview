### Promise

解决异步编程的一种方案

> ##### 零散总结
>
> 1、Promise 中，then 返回的是一个新的promise 实例；
>
> 2、Promise 中链式调用，会将第一个then中 返回的结果，作为参数，传入第二个回调函数；
>
> 3、状态变为resolved 会调用then指定的回调函数，变为reject或者报错也会调用catch 指定的回调函数；当然在then 中报错，也会被catch 捕获；
>
> 4、在resolved 后面抛出错误,不会被捕获，因为状态已经变成resolved 不能变更；
>
> 5、如果没有使用`catch`方法指定错误处理的回调函数，Promise 对象抛出的错误不会传递到外层代码(不会被外层的代码捕获到)，即不会影响到外部代码执行；
>
> 6、Promise 中的finally 方法总是会最后执行，并且不接受参数，`finally`方法里面的操作，应该是与状态无关的，不依赖于 Promise 的执行结果；
>
> 7、Promise 新建后就会立即执行



######Promise.all()

```javascript
Promise.all()方法用于将多个 Promise 实例，包装成一个新的 Promise 实例。
const p = Promise.all([p1, p2, p3]);
上面代码中，Promise.all()方法接受一个数组作为参数，p1、p2、p3都是 Promise 实例，如果不是，就会先调用下面讲到的Promise.resolve方法，将参数转为 Promise 实例，再进一步处理。另外，Promise.all()方法的参数可以不是数组，但必须具有 Iterator 接口，且返回的每个成员都是 Promise 实例。

p的状态由p1、p2、p3决定，分成两种情况。
（1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。
（2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。
```



```javascript
// 执行顺序的问题
let promise = new Promise(function(resolve, reject) {
  console.log('Promise');
  resolve();
});

promise.then(function() {
  console.log('resolved.');
});

console.log('Hi!');

// Promise
// Hi!
// resolved
```

> 上面代码中，Promise 新建后立即执行，所以首先输出的是`Promise`。然后，`then`方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，所以`resolved`最后输出。



### async/await

终极异步解决方案

######async：用于声明异步函数，返回一个promise对象

######await：写在异步的函数前，代码就会等待异步的函数执行完之后，继续执行下面的代码

```javascript
function getJson(){
    return new Promise((reslove,reject) => {
      setTimeout(function(){
        console.log(2)
        reslove(2)
      },2000)
    })
   }

async function testAsync() {
   await getJson()
   console.log(3)
}

testAsync()

// 实际执行的效果
function getJson(){
    return new Promise((reslove,reject) => {
      setTimeout(function(){
        console.log(2)
        reslove()
      },2000)
    })
}

function testAsync() {
    return new Promise((reslove,reject) => {
        getJson().then(function (res) {
          console.log(3)
        })
    })
}

testAsync()
```



async/await的本身还是基于Promise

因为await本身返回的也是一个Promise,它只是把await后面的代码放到了await返回的Promise的.then后面，以此来实现的



