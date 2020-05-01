### HTML

> #### HTML 语义化

- 根据内容的结构化（内容语义化），选择合适的标签（代码语义化）便于开发者阅读和写出更优雅的代码的同时让浏览器的爬虫和机器很好地解析。
- 尽可能少的使用无语义的标签 div 和 span；
- 不要使用纯样式标签，如：b、font、u 等，改用 css 设置。

> #### HTML5 新增标签

- 语义化：`<header><footer><main><nav><article><aside><progress><dialog>`
- 图像：`<canvas><svg>`
- 媒体类型：`<audio><video>`

> #### meta 标签

`<meta>`元素可以定义文档的各种元数据，提供各种文档信息，通俗点说就是可以理解为提供了关于网站的各种信息。html 文档中可以包含多个`<meta>`元素，每个`<meta>`元素只能用于一种用途，如果想定义多个文档信息，则需要在 head 标签中添加多个 meta 元素。

- meta 元素定义的元数据的类型包括以下几种：
  - 如果设置了 name 属性，meta 元素提供的是文档级别（document-level）的元数据，应用于整个页面。
  - 如果设置了 http-equiv 属性，meta 元素则是编译指令，提供的信息与类似命名的 HTTP 头部相同。
  - 如果设置了 charset 属性，meta 元素是一个字符集声明，告诉文档使用哪种字符编码。
  - 如果设置了 itemprop 属性，meta 元素提供用户定义的元数据。

```
<meta
  name="viewport",
  content="width=device-width,
  user-scalable=no,
  initial-scale=1.0,
  maximum-scale=1.0,
  minimum-scale=1.0">
```

> #### Canvas

`<canvas id="" width="300" height="300"></canvas> //默认 width 为 300、height 为 150,单位都是 px`

- Canvas API 提供了一个通过 JavaScript 和 HTML 的`<canvas>`元素来绘制图形的方式。它可以用于动画、游戏画面、数据可视化、图片编辑以及实时视频处理等方面
- Canvas API 主要聚焦 2D 图形。而同样使用`<canvas>`元素的 WebGL API 则用于绘制硬件加速的 2D 和 3D 图形

> #### svg 和 canvas 各自的优缺点？

都是有效的图形工具，对于数据较小的情况下，都有很高的性能，使用 JavaScript 和 HTML，都遵守 W3C 标准。

- svg 优点：
  - 矢量图，不依赖于像素，无限放大后不会失真。
  - 以 dom 的形式表示，事件绑定由浏览器直接分发到节点上。
- svg 缺点：
  - dom 形式，涉及到动画时候需要更新 dom，性能较低。
- canvas 优点：
  - 定制型更强，可以绘制绘制自己想要的东西。
  - 非 dom 结构形式，用 JavaScript 进行绘制，涉及到动画性能较高。
- canvas 缺点：
  - 事件分发由 canvas 处理，绘制的内容的事件需要自己做处理。
  - 依赖于像素，无法高效保真，画布较大时候性能较低。

> #### 浏览器内核

- IE：Trident
- Chrome：统称为 Chromium 内核，以前是 Webkit，现在是 Blink
- Firefox：Gecko
- Safari：Webkit
- Opera：最初是自己的 Presto 内核，后来是 Webkit，现在是 Blink 内核
- 360/猎豹：IE+Chrome 双内核
