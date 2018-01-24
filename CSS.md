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