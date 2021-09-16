# Promise特点

## Promise状态

pending 初始状态

fulfilled(resolved)  成功终止状态

rejected  失败终止状态

## 优缺点

避免层层嵌套回调，统一接口使异步更容易

开始后无法取消，不可回退，最终结局只能是resolved或rejected

如果不设回调， 内部错误不会反应到外部。



# Promise创建

```javascript
// 也可以只有一个参数resolve
var promise = new Promise(function(resolve, reject) {
    // 异步处理
    // 处理结束后、调用resolve 或 reject
});

// 创建后可以有两种调法. onFulfilled和onRejected) 是两个回调函数， 可以接收参数
// 如果promise的结局是resolved， 则onFullfilled接收到resolve调用时传的参数， 不会触发onRejected，
// 如果promise的结局是rejected， 则onRejected接收到的是reject(xxx)调用时的参数，不回触发onFullfilled
promise.then(onFulfilled, onRejected) 
promise.then(onFulfilled).catch(onRejected)


```

# 链式then

promise.then的返回值也是一个promise， 因此可以链式继续.then

第一个then的返回值（或这个返回promise中resolve的值），作为参数，传给第二个then的回调函数。

## catch方法

Promise.prototype.catch 方法是Promise.prototype.then(null, rejection)的别名。

Promise对象的错误具有“冒泡”性质， 会一直向后传递， 直到捕获为止。也就是说，错误总是会被下一个catch语句博客。



# Promise.all 和 Promise.race

Promise.all 是等待全部resolve，返回就是resolve的promise，否则返回rejected

Promise.race 只要有一个跳出pending状态， 就返回他。



# Promise.resolve 和Promise.reject

对于Promise.resolve ，

 如果参数是普通对象或变量， 返回就是一个resolve状态的Promise， 其值为参数值本身。

如果参数是promise实例对象， 则原样返回。

如果参数是一个具有then(resolve,reject)方法的对象， 则立即执行then方法， 将他的状态作为返回promise的状态。

一般来说Promise.resolve(x) 相当于 new Promise(r=>r(x)) , **但是** 当x是一个promise实例时，有点时序上的区别。



Promise.reject 返回一个状态为rejected的promise。



## 未接之
深度揭秘 Promise 微任务和执行过程 https://blog.csdn.net/lqyygyss/article/details/102662606 

```javascript
new Promise((resolve, reject) => {
    console.log("外部promise");
    resolve();
}).then(() => {
        console.log("外部第一个then");
        return new Promise((resolve, reject) => {
            console.log("内部promise");
            resolve();
        }).then(() => {
                console.log("内部第一个then");
            })
            .then(() => {
                console.log("内部第二个then");
            }).then(() => {
                console.log("内部第3个then");
            }).then(() => {
                console.log("内部第4个then");
            });
    })
    .then(() => {
        console.log("外部第二个then");
    });

```

```javascript
//比上一个少个return
new Promise((resolve, reject) => {
    console.log("外部promise");
    resolve();
}).then(() => {
        console.log("外部第一个then");
        new Promise((resolve, reject) => {
            console.log("内部promise");
            resolve();
        }).then(() => {
                console.log("内部第一个then");
            })
            .then(() => {
                console.log("内部第二个then");
            }).then(() => {
                console.log("内部第3个then");
            }).then(() => {
                console.log("内部第4个then");
            });
    })
    .then(() => {
        console.log("外部第二个then");
    });
```

 Promise.resolve()与new Promise(r => r(v))   https://blog.csdn.net/my_new_way/article/details/104838192
```javascript
let v = new Promise(resolve => {
    console.log("begin");
    resolve("then");
});

new Promise(resolve => {
    resolve(v);
}).then((v)=>{
    console.log(v)
});

new Promise(resolve => {
    console.log(1);
    resolve();
})
.then(() => {
    console.log(2);
})
.then(() => {
    console.log(3);
})
.then(() => {
    console.log(4);
});

```

```javascript

let v = new Promise(resolve => {
    console.log("begin");
    resolve("then");
});

Promise.resolve(v).then((v)=>{
    console.log(v)
});

new Promise(resolve => {
    console.log(1);
    resolve();
})
.then(() => {
    console.log(2);
})
.then(() => {
    console.log(3);
})
.then(() => {
    console.log(4);
});

```



# 参考

 JavaScript Promise 对象 https://www.runoob.com/w3cnote/javascript-promise-object.html   

 https://blog.csdn.net/my_new_way/article/details/104838192




promise JS宏任务微任务的执行顺序 https://www.wodecun.com/blog/7841.html

