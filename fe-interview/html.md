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
<meta charset="utf-8">
<meta http-equiv="expires" content="31 Dec 2008">
<meta name="keywords" content="HTML,ASP,PHP,SQL">
<meta name="viewport", content="width=device-width, user-scalable=no, initial-scale=1.0, maximum-scale=1.0,  minimum-scale=1.0">
```

> #### 页面导入样式时，使用 link 和@import 有什么区别？

1. link 是 XHTML 标签,它不仅可以引入 css 文件，还可以引入网站图标或者设置媒体查询。
   - @import 是 CSS 提供的语法规则，只能用来加载 css。
   - @import 一定要写在除@charset 外的其他任何 CSS 规则之前，如果置于其它位置将会被浏览器忽略。而且，在@import 之后如果存在其它样式，则@import 之后的分号是必须书写，不可省略的。
2. link 引入 css 文件，页面载入同时载入 css 文件，@import 在页面完全载入之后载入 css 文件，在网络较慢情况下一开始会没有 css 样式。
3. link 在浏览器中没有兼容问题。@import 在 css2.1 中提出，低版本浏览器会不支持。
4. link 中的 css 可以被 javascript 获取进而控制 DOM，而@import 不支持。

> #### HTML5 的离线储存 Cookie、Storage、IndexedDB

1. <font color="#f06">Cookie</font>

- 最古老的存储方式为 Cookie，这种存储方式存储内容很有限，只适合做简单信息存储，存储空间大小只有 4K，存储有效时间有限制。
- Cookie 是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上，使得每次的请求数据都会无意义的增大
- 通常，它用于告知服务端两个请求是否来自同一浏览器，如保持用户的登录状态。
- Cookie 使基于无状态的 HTTP 协议记录稳定的状态信息成为了可能。

```js
function getCookieValue(name) {
  if (document.cookie.length > 0) {
    var start = document.cookie.indexOf(name + "=");
    if (start !== -1) {
      start = start + name.length + 1;
      var end = document.cookie.indexOf(";", start);
      if (end === -1) {
        end = document.cookie.length;
      }
      return unescape(document.cookie.substring(start, end));
    }
  }
  return "";
}
function save(dataModel) {
  var value = dataModel.serialize();
  document.cookie = "DataModel=" + escape(value);
  document.cookie = "DataCount=" + dataModel.size();
  console.log(dataModel.size() + " datas are saved");
  return value;
}
function restore(dataModel) {
  var value = getCookieValue("DataModel");
  if (value) {
    dataModel.deserialize(value);
    console.log(getCookieValue("DataCount") + " datas are restored");
    return value;
  }
  return "";
}
function clear() {
  if (getCookieValue("DataModel")) {
    console.log(getCookieValue("DataCount") + " datas are cleared");
    document.cookie = "DataModel=; expires=Thu, 01 Jan 1970 00:00:00 UTC";
    document.cookie = "DataCount=; expires=Thu, 01 Jan 1970 00:00:00 UTC";
  }
}
```

2. <font color="#f06">Storage</font>

   一种是永久存储的 localStorage,一种是会话期间存储的 sessionStorage

- 优点：
  - 存储空间更大,有 5M 大小
  - 在浏览器发送请求是不会带上 web Storage 里的数据
  - 更加友好的 API
  - 可以做永久存储(localStorage).
- 缺点：
  - 随着 web 应用程序的不断发展,5M 的存储大小对于一些大型的 web 应用程序来说有些不够
  - web Storage 只能存储 string 类型的数据.对于 Object 类型的数据只能先用 JSON.stringify()转换一下在存储.

```js
function save(dataModel) {
  var value = dataModel.serialize();
  window.localStorage["DataModel"] = value;
  window.localStorage["DataCount"] = dataModel.size();
  console.log(dataModel.size() + " datas are saved");
  return value;
}
function restore(dataModel) {
  var value = window.localStorage["DataModel"];
  if (value) {
    dataModel.deserialize(value);
    console.log(window.localStorage["DataCount"] + " datas are restored");
    return value;
  }
  return "";
}
function clear() {
  if (window.localStorage["DataModel"]) {
    console.log(window.localStorage["DataCount"] + " datas are cleared");
    delete window.localStorage["DataModel"];
    delete window.localStorage["DataCount"];
  }
}
```

3. <font color="#f06">IndexedDB</font>

IndexedDB 是一种使用浏览器存储大量数据的方法.它创造的数据可以被查询，并且可以离线使用. IndexedDB 对于那些需要存储大量数据，或者是需要离线使用的程序是非常有效的解决方法. --- MDN

[MDN---IndexedDB](!https://developer.mozilla.org/zh-CN/docs/Web/API/IndexedDB_API)  
[HTML5 进阶系列：indexedDB 数据库](!https://juejin.im/post/59013d2c0ce46300614ebe70)

> #### 浏览器是怎么对 HTML5 的离线储存资源进行管理和加载的呢？

- 在线的情况下，浏览器发现 html 头部有 manifest 属性，它会请求 manifest 文件，如果是第一次访问 app，那么浏览器就会根据 manifest 文件的内容下载相应的资源并且进行离线存储。如果已经访问过 app 并且资源已经离线存储了，那么浏览器就会使用离线的资源加载页面，然后浏览器会对比新的 manifest 文件与旧的 manifest 文件，如果文件没有发生改变，就不做任何操作，如果文件改变了，那么就会重新下载文件中的资源并进行离线存储。
- 离线的情况下，浏览器就直接使用离线存储的资源。

> #### iframe 有那些优缺点？

- 优点：
  1. iframe 能够原封不动的把嵌入的网页展现出来。
  2. 如果有多个网页引用 iframe，那么你只需要修改 iframe 的内容，就可以实现调用的每一个页面内容的更改，方便快捷。
  3. 网页如果为了统一风格，头部和版本都是一样的，就可以写成一个页面，用 iframe 来嵌套，可以增加代码的可重用。
  4. 如果遇到加载缓慢的第三方内容如图标和广告，这些问题可以由 iframe 来解决。
- 缺点：
  1. 会产生很多页面，不容易管理。
  2. iframe 框架结构有时会让人感到迷惑，如果框架个数多的话，可能会出现上下、左右滚动条，会分散访问者的注意力，用户体验度差。
  3. 代码复杂，无法被一些搜索引擎索引到，这一点很关键，现在的搜索引擎爬虫还不能很好的处理 iframe 中的内容，所以使用 iframe 会不利于搜索引擎优化。
  4. 很多的移动设备（PDA 手机）无法完全显示框架，设备兼容性差。
  5. iframe 框架页面会增加服务器的 http 请求，对于大型网站是不可取的。  
     分析了这么多，现在基本上都是用 Ajax 来代替 iframe，所以 iframe 已经渐渐的退出了前端开发。

> #### Canvas

`<canvas id="" width="300" height="300"></canvas>` //默认 width 为 300、height 为 150,单位都是 px

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
