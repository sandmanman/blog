---
title: ECMAScript 6 入门 -- Promise
date: 2017-07-23 20:33:46
categories: javaScript日常笔记
tags: promise
---

> 转载：[阮一峰 ECMAScript 6入门](http://es6.ruanyifeng.com/#docs/promise)

## Promise 的含义

Promise 是异步编程的一种解决方案。所谓 `Promise`，简单说就是一个容器，里面保存着某个未来才会结束的事件（通常是一个异步操作）的结果。从语法上说，Promise 是一个对象，从它可以获取异步操作的消息。Promise 提供统一的 API，各种异步操作都可以用同样的方法进行处理。

`Promise` 对象有以下两个特点。

(1) 对象的状态不受外界影响。`Promise` 对象代表一个异步操作，有三种状态：`Pending`（进行中）、`Resolved`（已完成，又称 Fulfilled）和 `Rejected`（已失败）。只有异步操作的结果，可以决定当前是哪一种状态，任何其他操作都无法改变这个状态。这也是Promise这个名字的由来，它的英语意思就是“承诺”，表示其他手段无法改变。

(2) 一旦状态改变，就不会再变，任何时候都可以得到这个结果。`Promise` 对象的状态改变，只有两种可能：从 `Pending` 变为 `Resolved` 和从 `Pending` 变为 `Rejected`。只要这两种情况发生，状态就凝固了，不会再变了，会一直保持这个结果。如果改变已经发生了，你再对Promise对象添加回调函数，也会立即得到这个结果。这与事件（Event）完全不同，事件的特点是，如果你错过了它，再去监听，是得不到结果的。

有了 `Promise` 对象，就可以将异步操作以同步操作的流程表达出来，避免了层层嵌套的回调函数。此外，`Promise` 对象提供统一的接口，使得控制异步操作更加容易。

`Promise` 也有一些缺点。首先，无法取消Promise，一旦新建它就会立即执行，无法中途取消。其次，如果不设置回调函数，Promise内部抛出的错误，不会反应到外部。第三，当处于Pending状态时，无法得知目前进展到哪一个阶段（刚刚开始还是即将完成）。


## 基本用法

ES6 规定，`Promise` 对象是一个构造函数，用来生成 `Promise` 实例。
创造一个 `Promise` 实例：

```javascript
var promise = new Promise(function(resolve, resject){
    // do something

    if( /* 异步操作成功 */ ){
        resolve(value)
    } else {
        resject(error)
    }
})
```

`Promise` 构造函数接受一个函数作为参数，该函数的两个参数分别是 `resolve` 和 `reject`。它们是两个函数，由 JavaScript 引擎提供，不用自己部署。

`resolve` 函数的作用是，将Promise对象的状态从“未完成”变为“成功”（即从 Pending 变为 Resolved），在异步操作成功时调用，并将异步操作的结果，作为参数传递出去；
`reject` 函数的作用是，将Promise对象的状态从“未完成”变为“失败”（即从 Pending 变为 Rejected），在异步操作失败时调用，并将异步操作报出的错误，作为参数传递出去。

`Promise` 实例生成以后，可以用 `then` 方法分别指定Resolved状态和Reject状态的回调函数。

```javascript
promise.then(function(value){
    // success
}, function(error){
    // failure
})
```

`then` 方法可以接受两个回调函数作为参数。第一个回调函数是`Promise`对象的状态变为`Resolved`时调用，第二个回调函数是Promise对象的状态变为`Rejected`时调用。其中，第二个函数是可选的，不一定要提供。这两个函数都接受`Promise`对象传出的值作为参数。

下面是一个Promise对象的简单例子：

```javascript
function timeout(ms) {
    return new Promise((resolve, reject) => {
        setTomeout(resolve, ms, 'done');
    })
}

timeout(100).then((value) => {
    console.log(value)
})
```

上面代码中，`timeout` 方法返回一个 `Promise` 实例，表示一段时间以后才会发生的结果。过了指定的时间（`ms`参数）以后，`Promise`实例的状态变为`Resolved`，就会触发`then`方法绑定的回调函数。

Promise 新建后就会立即执行。

```javascript
let promise = new Promise(function(resolve, reject){
    console.log('Promise')
    resolve()
})
promise.then(function(){
    console.log('Resolved')
})
console.log('Hi!')
// Promise
// Resolved
// Hi!
```

上面代码中，Promise新建后立即执行，所以首先输出的是 `Promise`。然后，`then`方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，所以Resolved最后输出。

下面是异步加载图片的例子。

```javascript
function loadImageAsync(url) {
    return new Promise(function(resolve, reject){
        var image = new Image()
        image.onload = function() {
            resolve(image)
        }

        image.onerror = function() {
            reject( new Error('Could not load image at ' + url) )
        }

        image.src = url
    })
}
```

上面代码中，使用`Promise`包装了一个图片加载的异步操作。如果加载成功，就调用`resolve`方法，否则就调用`reject`方法。

下面是一个用`Promise`对象实现的 `Ajax` 操作的例子。

```javascript
var getJSON = function(url) {
    var promise = new Promise(function(resolve, reject){
        var client = new XMLHttpRequest()
        client.open('GET', url)
        client.onreadystatechange = handler
        client.responeType = 'json'
        client.setRequestHeader('Accept', 'application/json')
        client.send()

        function handler() {
            if( this.readyState !== 4 ){
                return
            }
            if( this.status === 200 ) {
                resolve(this.response)
            } else {
                reject( new Error(this.statusText) )
            }

        }
    })

    return promise
}

getJSON('/post.json').then(function(json){
    console.log('Contents: ' + json);
}, function(error) {
    console.error('出错了', error);
})
```

上面代码中，`getJSON`是对 `XMLHttpRequest` 对象的封装，用于发出一个针对 `JSON` 数据的 `HTTP` 请求，并且返回一个`Promise`对象。需要注意的是，在`getJSON`内部，`resolve`函数和`reject`函数调用时，都带有参数。

如果调用`resolve`函数和`reject`函数时带有参数，那么它们的参数会被传递给回调函数。`reject`函数的参数通常是`Error`对象的实例，表示抛出的错误；`resolve`函数的参数除了正常的值以外，还可能是另一个 `Promise` 实例，比如像下面这样。

```javascript
var p1 = new Promise(function (resolve, reject) {
    // ...
});

var p2 = new Promise(function (resolve, reject) {
    // ...
    resolve(p1);
})
```

上面代码中，`p1`和`p2`都是 Promise 的实例，但是`p2`的`resolve`方法将`p1`作为参数，即一个异步操作的结果是返回另一个异步操作。

注意，这时`p1`的状态就会传递给`p2`，也就是说，`p1`的状态决定了`p2`的状态。如果`p1`的状态是`Pending`，那么`p2`的回调函数就会等待`p1`的状态改变；如果`p1`的状态已经是`Resolved`或者`Rejected`，那么`p2`的回调函数将会立刻执行。

```javascript
var p1 = new Promise(function (resolve, reject) {
    setTimeout(function(){
        return reject(new Error('fail'))
    }, 3000)
});

var p2 = new Promise(function (resolve, reject) {
    setTimeout(function(){
        return resolve(p1)
    }, 1000)
})
p2.then(function(result){
    console.log(result)
}).catch(function(error){
    console.log(error)
})
```

上面代码中，`p1`是一个`Promise`，3秒之后变为`rejected`。`p2`的状态在1秒之后改变，`resolve`方法返回的是`p1`。由于`p2`返回的是另一个 `Promise`，导致`p2`自己的状态无效了，由`p1`的状态决定`p2`的状态。所以，后面的then语句都变成针对后者（p1）。又过了2秒，p1变为`rejected`，导致触发`catch`方法指定的回调函数。

注意，调用`resolve`或`reject`并不会终结 `Promise` 的参数函数的执行。

```javascript
new Promise((resolve, reject) => {
    resolve(1);
    console.log(2);
}).then(r => {
    console.log(r);
});
// 2
// 1
```

上面代码中，调用`resolve(1)`以后，后面的`console.log(2)`还是会执行，并且会首先打印出来。这是因为立即 `resolved` 的 `Promise` 是在本轮事件循环的末尾执行，总是晚于本轮循环的同步任务。

一般来说，调用`resolve`或`reject`以后，`Promise` 的使命就完成了，后继操作应该放到`then`方法里面，而不应该直接写在`resolve`或`reject`的后面。所以，最好在它们前面加上`return`语句，这样就不会有意外。

```javascript
new Promise((resolve, reject) => {
    return resolve(1);
    // 后面的语句不会执行
    console.log(2);
})
```