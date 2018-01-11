# CSS 日常积累

* 消除 safari 中默认的 title 弹出使用 before 伪元素实现

```css
&::before {
    content: '';
    display: block;
}
```
* 在设置posotion为fixed的时候不写top或者left的时候在IE和Safari浏览器中不能正常显示