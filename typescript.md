# Typescript

## 问题

* document is null 的问题在 tsconfig 配置中加入 lib：dom

## 基础知识

### 数据类型

#### boolean、number、string、array 同 js 数据类型

#### 元组类型：允许表示一个已知元素数量和类型的数组，各元素的类型不必相同。

    * 当访问一个越界元素时会使用 <a href="#lianheleixing">联合类型</a>代替

```ts
let x: [string, number];
x = ['hello', 10]; // ok

x = [10, 'hello']; // error
```

#### 枚举类型 enum

* enum 类型是对 javascript 标准数据类型的一个补充。

```ts
enum Color {
    Red,
    Green,
    Blue
}
let c: Color = Color.Green;
alert(c); // 1
// 默认情况下，从0开始为元素编号。 你也可以手动的指定成员的数值。 例如，我们将上面的例子改成从 1开始编号：
enum Color {
    Red = 1,
    Green,
    Blue
}
alert(c); // 2
// 全部都采用手动赋值
enum Color {
    Red = 1,
    Green = 2,
    Blue = 4
}
let c: Color = Color.Green;
```

* 枚举类型提供一个便利是你可以由枚举的值得到他的名字。

```ts
enum Color {
    Red = 1,
    Green,
    Blue
}
let colorName: string = Color[2];

alert(colorName); // 显示'Green'因为上面代码里它的值是2
```

*

#### 交叉类型

* 交叉类型是将多个类型合并为一个类型；我们大多是在混入（mixins）或其它不适合典型面向对象模型的地方看到交叉类型的使用。Person & Serializable & Loggable 同时是 Person 和 Serializable 和 Loggable。 就是说这个类型的对象同时拥有了这三种类型的成员

#### <h4 id="lianheleixing">联合类型</h4>

* 联合类型表示一个值可以是几种类型之一。 我们用竖线（ |）分隔每个类型，所以 number | string | boolean 表示一个值可以是 number， string，或 boolean。

```ts
interface Bird {
    fly();
    layEggs();
}

interface Fish {
    swim();
    layEggs();
}

function getSmallPet(): Fish | Bird {
    // ...
}

let pet = getSmallPet();
pet.layEggs(); // okay
pet.swim(); // errors
```

#### 交叉类型  与联合类型的  异同
