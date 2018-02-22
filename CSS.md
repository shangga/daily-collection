# CSS 日常积累

* 消除 safari 中默认的 title 弹出使用 before 伪元素实现

```css
&::before {
    content: '';
    display: block;
}
```

* 在设置 posotion 为 fixed 的时候不写 top 或者 left 的时候在 IE 和 Safari 浏览器中不能正常显示
* will-change 属性告知浏览器哪些属性会发生变化，让浏览器在元素属性真正发生变化之前提前做好对应的优化准备工作，这种优化可以将一部分复杂的计算工作提前准备好， 使页面的反应更为快速灵敏。
* z-index 解析

    * 当兄弟元素之间的 z-index 为正值时，跟渲染顺序有关，假设有纵向 a，b 两个元素，无论 a 元素的 z－index 有多大，都不可能覆盖 b 元素；a 元素的子元素也不能通过设置 z-index(a 的 children 的 z-index 为 1000)盖住 b 元素（b 元素的 z-index 为 1）；
    * 当兄弟元素之间的 z-index 都为负值时，其中 a 的 overflow 设置为 visible（这里是关键，当 a 的 overflow 设置为 visible 的时候，溢出的内容会有一个显示较高优先级）a，b 的子元素的 z-index 都设置为 1000,此时 a 的子元素会盖住整个 b 元素，即使 b 的子元素 z-index 设置为 10000 也依然会被 a 的子元素盖住;
    * 当设置 position 为 absolute 时，z-index 越大越靠上，与渲染顺序无关；z-index 相等时，依据渲染顺序判断
    * 如果一个元素为 absolute 一个元素为正常元素，那么需要将正常元素的 z-index 设置为正值，浮动元素的 z-index 设置为负值才能实现覆盖
    * 总的来说想让元素之间相互遮挡，应该让一个元素的 z-index 为负值，一个为正值最为有效
    * 需要探究 z-index 与 position 之间的相互影响

* sass 的 import 机制
    * 默认情况下，它会寻找 Sass 文件并直接引入， 但是，在少数几种情况下，它会被编译成 CSS 的 @import 规则：
        > * 如果文件的扩展名是 .css。
        > * 如果文件名以 http:// 开头。
        > * 如果文件名是 url()。
        > * 如果 @import 包含了任何媒体查询（media queries）。
    * 如果没有扩展名， Sass 将试着找出具有 .scss 或 .sass 扩展名的同名文件并将其引入。
    * 如果有 sass 或者 scss 文件引入，但是不希望被编译为一个 css 文件，那么这时可以在文件名前面加一个下划线，就能避免被编译。然后可以像往常一样引入文件，还可以省略掉文件名前面的下划线。

    ```scss
    @import 'colors';
    // 等于引入了_colors.scss文件
    ```
    * @import 可以引入多个文件

    ```scss
    @import 'rounded-corners', 'text-shadow';
    // 等于引入了rounded-corners 和 text-shadow 两个文件。
    ```
    > 在同一个目录不能同时存在带下划线和不带下划线的同名文件。

