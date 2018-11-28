# Flexible 弹性布局解决方案

## 像素概念

### dpr

devicepixelradio 是设备上物理像素和设备独立像素的比例
- 设备物理像素：设备能控制的显示的最小单位，可以把这些像素看作是显示器上一个个的点，可以理解为硬件像素
- 设备独立像素：也叫密度无关像素，可以理解为逻辑像素，由相关系统转换位物理像素
- css 像素：是一个编程概念，可以认为是计算机坐标系统中的一个点，我们在 css 中设置的 px 像素是抽象，实际中不存在的。

---

### 计算

在 pc 和移动端的都是通过 screen.width/height 两个属性值来获取设备独立像素的（css 像素），而不是设备的分辨率，因为设备的屏幕分辨率对于 web 开发来说是完全透明的，无法通过代码来获取的。

### 设备物理像素和设备独立像素的关系

在一定条件下两者是可以相等的，在 PC 下页面没被缩放或者放的时候，一个物理像素＝一个设备独立像素。但是在移动端多数情况下两个值是不想等的。这个是为了提高画纸的清晰度，涉及到一个叫 PPI 的概念。

#### PPI

-   ppi(pixel per inch)就是每英寸内有几个像素点。 这个像素点指的是设备像素点
-   计算公式:<br>
    ![img](http://yunkus.com/wp-content/uploads/2016/07/physical-pixel-device-independent-pixels-1.jpg)

### 设备像素比

-   设备像素比 ＝ 设备像素／设备独立像素 // 在某一方向上的比值
-   可以通过 window.devicePixelRadio 来获取设备中的像素比；
-   设备像素比可以告诉我们一个设备光点对应多少个像素点，就是对应的 css 像素。
-   retina 视网膜屏：就是 PPI 值超过 300 的叫超高密度屏幕
-   在移动端页面开发的时候经常会遇到不同手机的图片，文字， 线条大小会不一样，就是因为不同设备的设备像素比不一样

## viewport 概念

-   viewport 最早由苹果引入，移动设备上的 viewport 就是设备屏幕上能用来显示我们网页的那部分区域。手机浏览器是把页面放在一个很小的窗口（viewport）中，通常这个虚拟的窗口比屏幕宽，这样就不用把每个网页都挤到很小的窗口中，移动版的 safari 引进了 viewport 这个 meta 标签，让网页开发者来控制 viewport 的大小和缩放。一般是 980px 和 1024px
-   viewport 分为 layout viewport 和 visual viewport、ideal viewport
    -   layout viewport 这个窗口的分辨率接近 pc 显示器，Apple 的定义是 980px，这就很好地解决了早期的页面在手机上显示的问题
    -   visual viewport 承载 layout viewport，可以认为是手持设备物理屏幕的可视区域，称之为视觉窗口。开发者无法对它进行任何设置或者修改。
        -   常见的几种 visual viewport 大小： iPhone4~iPhone5S: 320\*480px iPhone6~iPhone6S: 375\*627px iPhone6 Plus~iPhone6S Plus: 414\*736px
        -   早期移动前端工程师常见到 640px 的设计稿，原因就是 UI 工程师以物理屏幕宽度为 320px 的 iPhone4-iPhone5S 作为参照设计；当然，现在你还可能会见到 750px 和 1242px 尺寸的设计稿，原因当然是 iPhone6 的出现。当然，为了更好的适配移动端或者只为移动端设计的应用，单有布局视口和视觉视口还是不够的。
    -   ideal viewport 完美视窗，主要通过 viewport meta 标签的设置来达到。
        -   width，height，initial－scale（设置页面初始按照某个比例放大或者缩小再呈现给用户。initial－scale ＝ 1 设置，即可得到完美窗口），maximun－scale（页面放大倍数），minimum-scale（页面缩小倍数），user-scale(是否允许用户进行放大和缩小)所有的 scale 都是相对于 ideal viewport 的
-   viewport 是为了解决无视设备分辨率，直接通过 dpi，在物理尺寸和浏览器之间重设分辨率，这个分辨率和设备分辨率无关。

## 小结

-   在 css 中我们使用 px 做单位的话，在桌面浏览器中 css 的 1 个像素往往就是对应电脑屏幕的 1 个物理像素。但是实际上，css 中的像素只是一个抽象的单位，在不同的设备或不同的环境中，css 中 1px 所代表的设备物理像素是不同的。这个问题在桌面端的浏览器中无需考虑，但是在移动端，必须区分。早期的移动设备屏幕像素密度比较低，比如 iphone3，它的分辨率是 320\*480，一个 css 像素就是屏幕物理像素。从 iphone4 开始，苹果公司推出了 retina 屏幕，分辨率提高了一倍，但是屏幕尺寸没有变化，这就意味着同样大小的屏幕上，像素却多了一倍，这是一个 css 像素等于两个物理像素。
-   还有一个因素会引起 css 中 px 的变化，用户缩放的问题。用户吧页面放大一倍，那么 css 中的 1px 所代表的物理像素也会增加一倍，缩小的话，css 中 1px 所代表的物理像素也会减少一倍。
-   在移动端浏览器中以及某些桌面浏览器中，window 中有一个 devicePixelRadio 属性，它的官方定义为：设备物理像素和设备独立像素的比例 ，即 devicePixelRadio ＝ 物理像素／独立像素。css 中的 px 就可看作是设备独立像素，所以我们可以通过 devicePixelRadio 知道设备上一个 css 像素代表多少个物理像素。
-   initial－sacle 的默认值。这个个默认值不是 1，因为当默认值为 1 的时候，layout viewport 被视为 ideal viewport 的宽度，各个浏览器厂商的 layout viewport 的宽度为 980 或者 1024，没有一开始就是 ideal viewport 的，所以 initial－scale 的默认值一定不是 1。安卓上的 initical－scale 默认值没办法拿到，或者干脆就没有默认值，一定要显式的定义出来才会有用。
-   在 iPhone 和 iPad 上无论 layout viewport 设置为多少，而又没有指定初始缩放值的话，那么设备会自动计算这个值，以保证当前 layout viewport 宽度在缩放后就是浏览器可视区域的宽度，也就是不会出现横向滚动条。比如在 iphone 上，我们不设置任何 viewport meta 标签，此时 layout viewport 的宽度为 980px，但是我们并没有发现横向滚动条，浏览器默认把页面缩小了，此时的缩放值＝ ideal viewport 宽度 ／ visual viewport 宽度。
-   viewport 的 width 没设置的话，默认是 980px。但如果设置了 initial-scale，viewport=device-width/scale；同时还设置了 width 和 initial-scale，则会取 min-width，即应用这两个较小的值。

## 移动端设计方案

### 旧式方案

###flexible 方案###

```js
var metaEl = doc.createElement('meta');
var scale = isRetina ? 0.5 : 1;
metaEl.setAttribute('name', 'viewport');
metaEl.setAttribute('content', 'initial-scale=' + scale + ', maximum-scale=' + scale + ', minimum-scale=' + scale + ', user-scalable=no');
if (docEl.firstElementChild) {
    document.documentElement.firstElementChild.appendChild(metaEl);
} else {
    var wrap = doc.createElement('div');
    wrap.appendChild(metaEl);
    documen.write(wrap.innerHTML);
}
```

-   动态改写<meta>标签
-   给 html>元素添加 data-dpr 属性，并且动态改写 data-dpr 的值
-   给<html>元素添加 font-size 属性，并且动态改写 font-size 的值

```css
    @function px2em($px, $base-font-size: 16px) { @if (unitless($px)) { @warn "Assuming #{$px} to be in pixels, attempting to convert it into pixels for you"; @return px2em($px + 0px); // That may fail. } @else if (unit($px) == em) { @return $px; } @return ($px / $base-font-size) * 1em; }
```

### 改进方案
#### vw\vh方案 ####
- vh\vw是基于视窗的长度单位，这里的视窗值得是可视区域，就是window.innerWidth/window.innerHeight
-   vw：是Viewport's width的简写,1vw等于window.innerWidth的1%
    vh：和vw类似，是Viewport's height的简写，1vh等于window.innerHeihgt的1%
    vmin：vmin的值是当前vw和vh中较小的值
    vmax：vmax的值是当前vw和vh中较大的值
  ![img](https://www.w3cplus.com/sites/default/files/blogs/2017/1707/vw-layout-5.png)
- 实际上我们收到的设计稿都是750px的，那么实际上就是100vw＝750px，1vw＝7.5px；这个计算是可以通过postcss的插件postcss－px－to－viewport计算然后直接在css中写入px
- 用途： 
    - 容器适配
    - 文本适配
    - 大于1px的边框、圆角、阴影
    - 内边距和外边距
- 对于不支持的vw的机型，采取降级处理
    - CSS Houdini：通过CSS Houdini针对vw做处理，调用CSS Typed OM Level1 提供的CSSUnitValue API。
    - CSS Polyfill：通过相应的Polyfill做相应的处理，目前针对于vw单位的Polyfill主要有：vminpoly、Viewport Units Buggyfill、vunits.js和Modernizr。
- 缺陷和不足
    - 当使用vw做单位时容易出现整体宽度超过100vw的情况。
    - px转成vw单位会有无法完全整除的情况
### 实验方案 ### 
- 鉴于vw的兼容性问题结合vw＋rem＋flex的方案，以750px设计稿为例，需要借助sass语法
- 字体不用rem或者vw避免出现15px或者13px这种奇数情况，因为字体的点阵都是16px和24px