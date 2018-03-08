# Javascript 日常积累

* void(0)与 undefined 区别：void(0)永远返回 undefined。在 void(0)中的计算中存在 getvalue 的方法。void(0）计算 \sigma 然后返回 undefined。然而，有些算符，比如 [] 和点，计算的结果是一个 Reference，它只记录了基底和属性，并没有进行取值，而取值这个过程有可能有副作用（因为调用了 Getter），所以要多一个 GetValue 布骤。

```js
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

> Promise 新建后立即执行，首先输出的是 promise，然后 then 方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，所以 resolved 最后输出

* Promise 的错误会具有冒泡的性质，会一直向后传递，直到被捕获为止，也就是错误总会被下一个 catch 语句捕获
* Promise 内部的错误不会影响 Promise 外部的代码
* js 中 indexof 方法底层使用===来进行判断，所以当数组中存在 NaN 时会出线[NaN,1].indexOf(NaN)=-1
* js是单线程语言，所以引入event loop（事件循环）解释js运行机制。Event Loop，需要区分 task 和 microtask 两种不同的概念。task包括settimeout、setinterval、setimmediate、I／O、UI交互事件；
microtask包括promise、process.nextTick、mutationObserve
* MutationObserver 是为开发者提供的一种能在某个特定的范围内的 DOM 树发生变化时做出适当反应的能力。在回调函数中设置对应的动作。

```js
// 选择目标节点
var target = document.querySelector('#some-id');
 
// 创建观察者对象
var observer = new MutationObserver(function callback(mutations) {
    // mutations是一个mutationrecord对象，是target发生的变化组成的队列
    mutations.forEach(function(mutation) {
        console.log(mutation.type);
    });
});
// 配置观察选项:
var config = { attributes: true, childList: true, characterData: true }
 
// 传入目标节点和观察选项
observer.observe(target, config);

// 随后,你还可以停止观察
observer.disconnect();
```
> 每次target发生变化都会触发回调函数callback

* websocket

* 对象属性循环方法的区别
    * for...in 循环会遍历原型链上所有可枚举属性
    * Object.key()遍历自身全部可美枚举属性（返回一个数组）
    * Object.getOwnPropertyNames()遍历自身所有属性的属性名(返回一个数组)