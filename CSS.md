# CSS 日常积累

* 消除 safari 中默认的 title 弹出使用 before 伪元素实现

```css
&::before {
    content: '';
    display: block;
}
```
* 在设置posotion为fixed的时候不写top或者left的时候在IE和Safari浏览器中不能正常显示
* will-change属性告知浏览器哪些属性会发生变化，让浏览器在元素属性真正发生变化之前提前做好对应的优化准备工作，这种优化可以将一部分复杂的计算工作提前准备好，使页面的反应更为快速灵敏。
* z-index解析
    *  当兄弟元素之间的z-index为正值时，跟渲染顺序有关，假设有纵向a，b两个元素，无论a元素的z－index有多大，都不可能覆盖b元素；a元素的子元素也不能通过设置z-index(a的children的z-index为1000)盖住b元素（b元素的z-index为1）；
    *  当兄弟元素之间的z-index都为负值时，其中a的overflow设置为visible（这里是关键，当a的overflow设置为visible的时候，溢出的内容会有一个显示较高优先级）a，b的子元素的z-index都设置为1000,此时a的子元素会盖住整个b元素，即使b的子元素z-index设置为10000也依然会被a的子元素盖住;
    *  当设置position为absolute时，z-index越大越靠上，与渲染顺序无关；z-index相等时，依据渲染顺序判断
    *  如果一个元素为absolute一个元素为正常元素，那么需要将正常元素的z-index设置为正值，浮动元素的z-index设置为负值才能实现覆盖
    *  总的来说想让元素之间相互遮挡，应该让一个元素的z-index为负值，一个为正值最为有效
    *  需要探究z-index与position之间的相互影响

* sass的import机制
    - 如果有sass或者scss文件引入，但是不希望被编译为一个css文件，那么这时可以在文件名前面加一个下划线，就能避免被编译。然后可以像往常一样引入文件，还可以省略掉文件名前面的下划线。 
    ```scss
    @import "colors";
    // 等于引入了_colors.scss文件
    ```
    - @import 可以引入多个文件
    ```scss
    @import "rounded-corners", "text-shadow";
    // 等于引入了rounded-corners 和 text-shadow 两个文件。
    ```
    > 在同一个目录不能同时存在带下划线和不带下划线的同名文件。
