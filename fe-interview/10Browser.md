> #### 浏览器的渲染过程

1. 解析 HTML，生成 DOM 树，解析 CSS，生成 CSSOM 树
2. 将 DOM 树和 CSSOM 树结合，生成渲染树(Render Tree)
3. Layout(回流):根据生成的渲染树，进行回流(Layout)，得到节点的几何信息（位置，大小）
4. Painting(重绘):根据渲染树以及回流得到的几何信息，得到节点的绝对像素
5. Display:将像素发送给 GPU，展示在页面上。（这一步其实还有很多内容，比如会在 GPU 将多个合成层合并为同一个层，并展示在页面中。而 css3 硬件加速的原理则是新建合成层，这里我们不展开，之后有机会会写一篇博客）

> #### 检测浏览器版本版本有哪些方式？

> #### 重绘与回流

当元素的样式发生变化时，浏览器需要触发更新，重新绘制元素。这个过程中，有两种类型的操作，即重绘与回流。

- **重绘(repaint)**: 当元素样式的改变不影响布局时，浏览器将使用重绘对元素进行更新，此时由于只需要 UI 层面的重新像素绘制，因此 损耗较少

- **回流(reflow)**: 当元素的尺寸、结构或触发某些属性时，浏览器会重新渲染页面，称为回流。此时，浏览器需要重新经过计算，计算后还需要重新页面布局，因此是较重的操作。会触发回流的操作:

  - 页面首次加载(无法避免)
  - 添加或者删除可见的 dom 元素
  - 元素的位置发生了变化（未脱离文档流）
  - 元素的尺寸发生变化（包括 margin，padding，border）
  - 元素内容发生变化，比如文本节点改变，或者 img 的 src 属性改变
  - 浏览器窗口大小改变
  - 激活 CSS 伪类（例如：:hover）
  - 查询某些属性或调用某些方法
    - clientWidth、clientHeight、clientTop、clientLeft
    - offsetWidth、offsetHeight、offsetTop、offsetLeft
    - scrollWidth、scrollHeight、scrollTop、scrollLeft
    - getComputedStyle()
    - getBoundingClientRect()
    - scrollTo()

`回流一定会引起重绘，而重绘则不一定会触发回流`（主要看是否影响其他 dom 结构）。重绘的开销较小，回流的代价较高。

- 减少回流和重绘的

  - CSS

    - 避免使用 table 布局。
    - 尽可能在 DOM 树的最末端改变 class。
    - 避免设置多层内联样式。
    - 将动画效果应用到 position 属性为 absolute 或 fixed 的元素上。
    - 避免使用 CSS 表达式(例如：calc())。

  - JavaScript

    - 避免频繁操作样式，最好一次性重写 style 属性，或者将样式列表定义为 class 并一次性更改 class 属性。
    - 避免频繁操作 DOM，创建一个 documentFragment，在它上面应用所有 DOM 操作，最后再把它添加到文档中。
    - 也可以先为元素设置 display: none，操作结束后再把它显示出来。因为在 display 属性为 none 的元素上进行的 DOM 操作不会引发回流和重绘。
    - 避免频繁读取会引发回流/重绘的属性，如果确实需要多次使用，就用一个变量缓存起来。
    - 对具有复杂动画的元素使用绝对定位，使它脱离文档流，否则会引起父元素及后续元素频繁回流。

> #### localStorage 与 sessionStorage 与 cookie 的区别总结

- 共同点: 都保存在浏览器端, 且同源
- localStorage 与 sessionStorage 统称 webStorage,保存在浏览器,不参与服务器通信,大小为 5M
- 生命周期不同: localStorage 永久保存, sessionStorage 当前会话, 都可手动清除
- 作用域不同: 不同浏览器不共享 local 和 session, 不同会话不共享 session
- Cookie: 设置的过期时间前一直有效, 大小 4K.有个数限制, 各浏览器不同, 一般为 20 个.携带在 HTTP 头中, 过多会有性能问题.可自己封装, 也可用原生

> #### 浏览器的缓存机制

- 首先通过 `Cache-Control` 验证强缓存是否可用

  - 如果强缓存可用，直接使用
  - 否则进入协商缓存，即发送 HTTP 请求，服务器通过请求头中的 `If-Modified-Since` 或者 `If-None-Match` 字段检查资源是否更新
    - 若资源更新，返回资源和 `200` 状态码
    - 否则，返回 `304`，告诉浏览器直接从缓存获取资源

> #### 介绍一下浏览器缓存位置和优先级，以及不同缓存间的差别

1. Service Worker
   和 Web Worker 类似，是独立的线程，我们可以在这个线程中缓存文件，在主线程需要的时候读取这里的文件，Service Worker 使我们可以自由选择缓存哪些文件以及文件的匹配、读取规则，并且缓存是持续性的
2. Memory Cache
   即内存缓存，内存缓存不是持续性的，缓存会随着进程释放而释放
3. Disk Cache
   即硬盘缓存，相较于内存缓存，硬盘缓存的持续性和容量更优，它会根据 HTTP header 的字段判断哪些资源需要缓存
4. Push Cache
   即推送缓存，是 HTTP/2 的内容，目前应用较少
5. 以上缓存都没命中就会进行网络请求

> #### 原生 jsonp 跨域（只支持 GET 请求）

```html
<script>
  var script = document.createElement("script");
  script.type = "text/javascript";

  // 传参并指定回调执行函数为jsonp
  script.src = "https://api.asilu.com/geo?a=1&callback=jsonp"; //这个是获取当前经纬度的接口
  document.head.appendChild(script); //创建并添加script标签到<head>下

  // 回调执行函数
  function jsonp(res) {
    alert(JSON.stringify(res));
  }
</script>
```
