# JS - 异步进阶

## 1.event loop 事件循环/事件轮询

简单概述一下`事件循环`：

- 因为JS是单线程运行
- 而异步又是基于回调实现
- event loop 就是浏览器关于异步回调的实现原理

`事件循环`的过程，简单来说：

- 同步代码，一行一行在`call stack`执行栈中执行
- 遇到异步代码，先记录下来，等待时机（定时，网络请求，DOM交互事件）
- 时机到了，将异步代码放入`callback queue`回调队列中
- 如果`call stack`执行栈为空（即同步代码执行完毕），`event loop`开始工作
- 轮询查找`callback queue`，有代码则放入`call stack`执行
- 然后重复查找执行

## 2. Promise

### 2.1 三种状态

`pending,resolved(fullfilled),rejected`

Promise对象处理异步只能由 `pending => resolved`或者`pending => rejected`，并且这种变化不可逆。

### 2.2 状态的表现和变化

- `pending`状态，不会触发`then`方法和`catch`方法
- `resolved`状态，会触发后续的`then`方法回调函数
- `rejected`状态，会触发后续的`catch`方法回调函数

### 2.3 then 和 catch对状态的影响

`then`和`catch`本身不返回对应的状态，是根据内部执行结果来返回状态。

- `then`方法内部正常返回`resolved`状态，若里面出现错误则返回`rejected`

- `catch`方法内部正常返回`resolved`状态，若里面出现错误则返回`rejected`



## async/await

异步回调可能会导致回调地狱`callback hell`，`Promise`的链式调用，基于回调函数，而`async/await` 从**语法层面**做到了同步效果，代码更加易读。

- 执行`async`函数，返回的是`Promise`对象

- `await`相当于`Promise`的`then`
- `try...catch`可捕获异常，代替了`Promise`的`catch`

虽然`async/await`是消灭异步调用的终极武器，JS还是单线程，还是有异步，还得基于`event loop`，`async/await`也只是**语法糖**。



## 3.宏任务和微任务

### 3.1 什么是宏任务，微任务？

- 宏任务：`setTimeout`,`setInterval`,`Ajax`,`DOM事件`

- 微任务：`Promise` `async/await`
- 微任务执行时机比宏任务要早

### 3.2 event loop 和DOM渲染

前面说`event loop`中，讲到JS是单线程的，而且和DOM渲染**共用一个线程**，所以JS执行时，得留一些时机供DOM渲染。

- 每次`call stack` 清空（即每次轮询结束），即同步任务执行完
- 都是DOM重新渲染的机会，DOM结构如有变化则重新渲染
- 然后再去触发下一次`event loop`

### 3.3 微任务和宏任务的区别

- 宏任务：DOM渲染后触发，如setTimeout

- 微任务：DOM渲染前触发，如Promise

```js
// 使用jQuery语法 测试
const p1 = $('<p>一段文字</p>')
const p2 = $('<p>一段文字</p>')
const p3 = $('<p>一段文字</p>')
$('#container').append(p1).append(p2).append(p3)

// 微任务： DOM渲染前触发 通过alert()可以阻断DOM渲染的特性来观察两者的区别
Promise.resolve().then(()=> {
    console.log('length1',$('#container').children().length)
    alert('Promise then') // 此时DOM未渲染
})

// 宏任务：DOM渲染后触发
setTimeout(()=> {
	console.log('length2',$('#container').children().length)
    alert('setTimeout') // 此时DOM渲染了
})
```

## 题目

### 1.描述event loop

- 同步代码，一行一行在`call stack`执行栈中执行
- 遇到异步代码，先记录下来，等待时机（定时，网络请求，DOM交互事件）
- 时机到了，将异步代码放入`callback queue`回调队列中
- 如果`call stack`执行栈为空（即同步代码执行完毕），`event loop`开始工作
- 轮询查找`callback queue`，有代码则放入`call stack`执行
- 然后重复查找执行

**面试官提到微任务，宏任务，与DOM渲染关系时再补充进去**

### 2. 什么是宏任务，微任务，两者区别

- 宏任务：`setTimeout`,`setInterval`,`Ajax`,`DOM事件`

- 微任务：`Promise` `async/await`
- 微任务执行时机比宏任务要早；宏任务在DOM渲染后触发，微任务在DOM渲染前触发

### 3. Promise的三种状态，如何变化

`pending,resolved(fullfilled),rejected`

Promise对象处理异步只能由 `pending => resolved`或者`pending => rejected`，并且这种变化不可逆。

