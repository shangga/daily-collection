# Javascript 日常积累

-   void(0)与 undefined 区别：void(0)永远返回 undefined。在 void(0)中的计算中存在 getvalue 的方法。void(0）计算 \sigma 然后返回 undefined。然而，有些算符，比如 [] 和点，计算的结果是一个 Reference，它只记录了基底和属性，并没有进行取值，而取值这个过程有可能有副作用（因为调用了 Getter），所以要多一个 GetValue 布骤。

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

-   Promise 的错误会具有冒泡的性质，会一直向后传递，直到被捕获为止，也就是错误总会被下一个 catch 语句捕获
-   Promise 内部的错误不会影响 Promise 外部的代码
-   js 中 indexof 方法底层使用===来进行判断，所以当数组中存在 NaN 时会出线[NaN,1].indexOf(NaN)=-1
-   js 是单线程语言，所以引入 event loop（事件循环）解释 js 运行机制。Event Loop，需要区分 task 和 microtask 两种不同的概念。task 包括 settimeout、setinterval、setimmediate、I／O、UI 交互事件；
    microtask 包括 promise、process.nextTick、mutationObserve
-   MutationObserver 是为开发者提供的一种能在某个特定的范围内的 DOM 树发生变化时做出适当反应的能力。在回调函数中设置对应的动作。

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
var config = {attributes: true, childList: true, characterData: true};

// 传入目标节点和观察选项
observer.observe(target, config);

// 随后,你还可以停止观察
observer.disconnect();
```

> 每次 target 发生变化都会触发回调函数 callback

-   websocket

-   对象属性循环方法的区别

    -   for...in 循环会遍历原型链上所有可枚举属性
    -   Object.key()遍历  自身全部可枚举属性（返回一个数组）
    -   Object.getOwnPropertyNames()遍历自身所有属性的属性名( 返回一个数组)

-   requestAnimationFrame

-   eventLoop
    -   解释：因为主线程从“任务队列”种读取事件的过程是循环不断的，因此这种运行机制又称为 Event Loop
    ```javascript
    console.log(1);
    setTimeout(function() {
        console.log(2);
    }, 5000);
    console.log(3);
    ```
    -   JavaScript 执行引擎主线程运行，产生 heap(堆)和 stack(栈)
        -   代码依次进入栈中等待执行，遇到异步方法，该异步方法
    -   从上往下执行同步代码,log(1)被压入执行栈，因为 log 是 webkit 内核支持的普通方法而非 WebAPIs 的方法，因此立即出栈被引擎执行，输出 1
    -   JavaScript 执行引擎继续往下，遇到 setTimeout()异步方法（setTimeout 属于 WebAPIs，DOM，ajax（XMLHttpRequest）都属于 WebAPIs 方法），将 setTimeout(callback,5000)添加到执行栈
    -   因为 setTimeout()属于 WebAPIs 中的方法，JavaScript 执行引擎在将 setTimeout()出栈执行时，注册 setTimeout()延时方法交由浏览器内核其他模块（以 webkit 为例，是 webcore 模块）处理
    -   继续运行 settimeout 下面的 log（3）代码，原理同步骤 2
    -   当延时方法到达触发条件，即到达设置的延时时间时，该延时方法就会被添加至任务队列里。这一过程由浏览器内核其他模块处理，与执行引擎主线程独立
    -   JavaScript 执行引擎在主线程方法执行完毕，到达空闲状态时，会从任务队列中顺序获取任务来执行
    -   将队列的第一个回调函数重新压入执行栈，执行回调函数中的代码 log(2)，原理同步骤 2，回调函数的代码执行完毕，清空执行栈
    -   JavaScript 执行引擎继续轮循队列，直到队列为空
    -   执行完毕
-   macrotask(宏任务) 和 microtask(微任务)
    -   macrotask：setTimeout, setInterval, setImmediate, I/O, UI rendering
    -   microtasks: process.nextTick, Promises, Object.observe(废弃), MutationObserver
    -   区别：
    ```javascript
    console.log('1');
    setTimeout(function() {
        console.log('2');
    }, 0);
    Promise.resolve()
        .then(function() {
            console.log('3');
        })
        .then(function() {
            console.log('4');
        });
    console.log('5');
    // 1,5,3,4,2
    ```
    - 原因是Promise中的then方法的函数会被推入 microtasks 队列，而setTimeout的任务会被推入 macrotasks 队列。在每一次事件循环中，macrotask 只会提取一个执行，而 microtask 会一直提取，直到 microtasks 队列清空。结论如下：

        microtask会优先macrotask执行
        microtasks会被循环提取到执行引擎主线程的执行栈，直到microtasks任务队列清空，才会执行macrotask
        【注：一般情况下，macrotask queues 我们会直接称为 task queues，只有 microtask queues 才会特别指明。】

- jQuery获取checkbox的值：低版本jquery中使用attr方法，attr('checked')返回checked，但是在非选中状态会返回undefined，这个不可用；应该使用prop('checked') 返回true或者false方法或者 .is(':checked')返回true或者false
- 截流和防抖
    - 防抖（debounce）：等待最后一次操作执行完成后间隔一段时间再执行；截流（throttle）：持续触发事件，每隔一段时间只执行一次操作；
- match方法：str.match（regexp）
    - 如果传入一个非正则表达式对象，则会隐式的使用new RegExp方法将其转换为一个RegExp。如果未传入任何参数，直接使用match则会得到一个包含空字符串的数组：[""]
    - 返回值： array，数组第一项是进行匹配完整的字符串，**之后的项是用圆括号捕获的结果**。如果没有匹配到，则会返回null
- NULL **表示一个空指针对象这也正是typeof null返回“object”的原因**。如果定一个变量准备用来存储对象的话，应该初始化为null而不是其他值.
- javascript中函数调用可以任意传递参数个数是因为ECMAscript中的参数在内部是用一个数组来表示的
- arguments的长度是由传入的参数个数决定的，不是由定义函数时的命名函数时的命名参数的个数决定的
- 当一个对象向另外一个对象复制引用类型的值时，同样也会将存储在对象变量中的值复制一份放到新分配的内存空间中，这个值是一个指针，指向存储在堆中的一个对象；javascript中所有的函数传参都是按值传递的，基本类型的传递和复制是一样，只传值；引用类型的传递和复制是一样，传递的是引用类型的数据的指针，指向堆中存储的那个变量
- javascript 没有块级作用域，而是函数作用域：变量在声明它的函数体以及这个函数体嵌套的任意函数体内部都是有定义的。因此引发了一个变量提升的概念。
- 如果一个对象创建表达式不需要传入任何参数给构造函数的话，那么最后圆括号是可以省略掉的
- 检测数组的方法：value instanceof Array或者 Array.isArray(value)
- 左值：指的是表达式只能出现在复制运算符的左侧，在javascript中，变量、对象属性和数组元素均是左值。
- instanceof的左操作数是一个对象，右操作数是一个函数,标识对象的类
- 函数定义表达式是var a = function (param) {}; 函数声明表达式是function a(param) {},**这里存在一个变量提升问题,函数定义表达式只是变量声明提升，变量初始化仍然在原来的位置；函数声明时函数名和函数体均提前，脚本中的所有函数和函数中所有嵌套的函数都会在当前上下文中其他代码之前声明，也就说，可以在一个javascript函数声明之前调用它**
- ES6中的class是一种语法糖，它的大部分功能ES5都能实现，新的class写法只是让对象原型的写法更清晰，更像面向对象编程语法，可以看作是构造函数的另外一种写法;
    - class的数据类型就是function，类本身就是指向构造函数
    - 在ES6中，构造函数的prototype属性在ES6上的“类”上继续存在，类的所有方法都定义在类的prototype属性上
    ```javascript
            class Point() {
                constructor() {
                }
                toString() {
                }
                toValue() {
                }
            }
            // 等同于
            Point.prototype = {
                constructor() {
                }
                toString() {
                }
                toValue() {
                }
            }
    ```
    - prototype对象的constructor属性直接指向“类”本身，这与ES5的行为是一致的。
    - **类的内部定义的所有方法都是不可枚举的**。比如上面的toString和toValue就是不可枚举的。**这一点与ES5不一致**
    - constructor是类的默认方法，通过new命令生成对象实例时自动调用该方法，一个类必须有constructor方法，如果没有显示的定义，则会被默认添加
    - constructor默认返回实例对象（即this），不过完全可以指定另外一个对象。
    ```javascript
        class Foo {
            constructor() {
                return Obejct.create(null)
            }
        }
    ```
    - 类必须使用new进行调用，否则会报错。
    - 类的属性除非显式的定义在其本身（this），否则全部定义在原型上（class）
    ```javascript
    class Point {
        constructor(x, y) {
            this.x = x;
            this.y = y;
        }
        toString() {
            return '(' + this.x + ', ' + this.y + ')';
        }
        }
        var point = new Point(2, 3);
        point.toString() // (2, 3)
        point.hasOwnProperty('x') // true
        point.hasOwnProperty('y') // true
        point.hasOwnProperty('toString') // false
        point.__proto__.hasOwnProperty('toString') // true
    ```
    - 上述代码中,x和y都是实例对象point的自身属性，因为定义在this变量上，toString是原型对象的属性，因为定义在Point类上
    - 类的所有实例共享一个原型对象
    - Class中不存在变量提升
    - 与函数表达式一样，也可以使用表达式的形式定义
    - Class继承：可以通过extends关键字实现继承 class ColorPoint extends Point｛｝
    ```javascript
        class ColorPoint extends Point {
            constructor() {
                super(x,y) // 调用父类的constructor(x,y)
                this.color = color;
            }
            toString() {
                return this.color + '' + super.toString(); // 调用父类的toString()
            }
        }
    ```
    - 上面的代码中，constructor方法和toString方法中，都出现了super关键字，它指代父类的实例（即父类的this对象），子类必须在constructor中调用super方法，否则新建实例会失败，这是因为**子类没有自己的this对象,而是继承了父类的this对象然后对其进行加工，如果不使用super方法，子类就得不到this对象**
    - ES5的继承实质上是先创造子类的实例对象this，然后再将父类的方法添加到this上（Parent.apply(this)）；ES6是先创造父类的实力对象this，所以必须先调用super方法，然后再用子类的构造函数修改this。（**子类的构造函数中只有先调用super之后才可以使用this关键字**）
    - 在浏览器对ES5的实现中，每个对象都有一个 __proto__属性指向对应的构造函数的prototype。Class同时具有prototype和__proto__属性，因此存在两条继承链，
- Map和Set数据结构
    - Map对象保存键值对，任何值都可以作为一个键或者一个值 new Map([iterable])  其中iterable可以是一个数组或者其他iterable（迭代）对象，其元素或为键值对或为两个元素的数组。