# CSS 日常积累

* 消除 safari 中默认的 title 弹出使用 before 伪元素实现

```css
&::before {
    content: '';
    display: block;
}
```
