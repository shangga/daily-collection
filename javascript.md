# Javascript日常积累
- void(0)与undefined区别：void(0)永远返回undefined。在void(0)中的计算中存在getvalue的方法。void(0）计算 \sigma 然后返回 undefined。然而，有些算符，比如 [] 和点，计算的结果是一个 Reference，它只记录了基底和属性，并没有进行取值，而取值这个过程有可能有副作用（因为调用了 Getter），所以要多一个 GetValue 布骤。
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
> Promise新建后立即执行，首先输出的是promise，然后then 方法指定的回调函数，将在当前脚本所有同步任务执行完才会执行，所以resolved最后输出
- Promise的错误会具有冒泡的性质，会一直向后传递，直到被捕获为止，也就是错误总会被下一个catch语句捕获
- Promise内部的错误不会影响Promise外部的代码