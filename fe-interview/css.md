> #### 盒模型

- 标准模型：  
  `box-sizing: content-box;` width = content + padding + border
- IE 模型：  
  `box-sizing: border-box;` width = content

> #### BFC

块格式化上下文（Block Formatting Context，BFC） 是 Web 页面的可视化 CSS 渲染的一部分，是块盒子的布局过程发生的区域，也是浮动元素与其他元素交互的区域。

- 创建 BFC：
  - 根元素(html)
  - 浮动元素（元素的 float 不是 none）
  - 绝对定位元素（元素的 position 为 absolute 或 fixed）
  - 行内块元素（元素的 display 为 inline-block）
  - overflow 值不为 visible 的块元素
  - display 值为 flow-root 的元素
  - contain 值为 layout、content 或 paint 的元素
  - 弹性元素（display 为 flex 或 inline-flex 元素的直接子元素）
  - 网格元素（display 为 grid 或 inline-grid 元素的直接子元素）
  - 多列容器（元素的 column-count 或 column-width 不为 auto，包括 column-count 为 1）
    column-span 为 all 的元素始终会创建一个新的 BFC，即使该元素没有包裹在一个多列容器中（标准变更，Chrome bug）。
  - 表格单元格（元素的 display 为 table-cell，HTML 表格单元格默认为该值）
  - 表格标题（元素的 display 为 table-caption，HTML 表格标题默认为该值）
  - 匿名表格单元格元素（元素的 display 为 table、table-row、 table-row-group、table-header-group、table-footer-group（分别是 HTML table、row、tbody、thead、tfoot 的默认属性）或 inline-table）

> #### CSS 伪类和伪元素有哪些，它们的区别和实际应用

- 伪类：
  :hover、:active、:first-child、:visited 等

- 伪元素：
  :first-line、:first-letter、:after、:before

伪类和伪元素的根本区别在于：

它们`是否创造了新的元素(抽象)`。从我们模仿其意义的角度来看，如果需要添加新元素加以标识的，就是伪元素，反之，如果只需要在既有元素上添加类别的，就是伪类。  
伪元素在一个选择器里只能出现一次，并且只能出现在末尾;
伪类则是像真正的类一样发挥着类的作用，没有数量上的限制，只要不是相互排斥的伪类，也可以同时使用在相同的元素上。

伪类用一个冒号表示 :first-child  
伪元素则使用两个冒号表示 ::first-line

> #### CSS3 新增伪类有那些？

1. `elem:nth-child(n)` 父元素下的第 n 个子元素，并且这个子元素的标签名为 elem，可为 2n+1、odd(单数)、even(双数)
2. `elem:nth-last-child` 同上，倒数计算
3. `elem:last-child` 最后一个子元素
4. `elem:nth-of-type(n)` 父元素下第 n 个 elem 元素，类似 nth-child
5. `elem:first-of-type`和`elem:last-of-type` 父元素下第 1 个/最后 1 个 elem 元素
6. `elem:only-child` 父元素下的有且只有一个子元素
7. `elem:only-of-type` 如果父元素下的子元素有且只有一个 elem 元素，选中该元素
8. `elem:empty` 选中不包含子元素和内容的 elem 标签
9. `:not(selector)` 选择除 selector 元素意外的元素
10. `:enabled` 选择可用的表单元素 `:disabled` 选择禁用的表单元素 `:checked` 选择被选中的表单元素
11. `:after` 在元素内部最前添加内容 `:before` 在元素内部最后添加内容

> #### CSS3 有哪些新特性？

transition、animation、transform、选择器、阴影 box-shadow、圆角 border-radiu、文字超出省略号、渐变、滤镜 filter、flex、grid、

> #### 水平垂直居中

1. absolute + transform

```css
.parent {
  position: relative;
}
.child {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
}
```

2. absolute + 负 margin (已知子元素的宽高)

```css
.parent {
  position: relative;
}
.child {
  width: 100px;
  height: 100px;
  position: absolute;
  top: 50%;
  left: 50%;
  margin-left: -50px;
  margin-top: -50px;
}
```

3. flex

```css
.parent {
  display: flex;
  justify-content: center;
  align-items: center;
}
```

4. inline-block + table-cell

```css
.parent {
  text-align: center;
  display: table-cell;
  vertical-align: middle;
}
.child {
  display: inline-block;
}
```

> #### 请解释一下 CSS3 的 Flexbox（弹性盒布局模型）,以及适用场景？

- `flex-direction`: row | row-reverse | column | column-reverse;
- `flex-wrap`: nowrap | wrap | wrap-reverse;
- `flex-flow`: flex-direction || flex-wrap;
- `justify-content`: flex-start | flex-end | center | space-between(两端对齐，项目之间的间隔都相等。) | space-around(两侧间隔相等,项目之间的间隔比项目与边框的间隔大一倍。);
- `jutify-content`: flex-start | flex-end | center | space-between | space-around ; (主轴)
- `align-items`: flex-start | flex-end | center | baseline | stretch(铺满); (交叉轴)

> #### 选择器优先级

!important > 行内样式 > #id > .class > tag > \* > 继承 > 默认

> #### 清除浮动

清除浮动主要是为了解决，父元素因为子级元素浮动，脱离了文档流，引起的内部高度坍塌为 0 的问题

1. clear

- clear: both

  在浮动元素后使用一个空元素如`<div class="clear"></div>`，并在 CSS 中赋予`.clear{clear:both;}`

- :after 伪元素

  ```css
  .clearfix:after {
    content: "";
    display: block;
    height: 0;
    clear: both;
    visibility: hidden;
  }
  .clearfix {
    zoom: 1; /* IE 兼容 */
  }
  ```

2. 创建 BFC

- overflow: hiden | auto
- display: inline-block
- position 等

> #### 消除图片底部间隙的方法

- 图片块状化 - 无基线对齐：`img { display: block; }`
- 图片底线对齐：`img { vertical-align: bottom; }`
- 行高足够小 - 基线位置上移：`.box { line-height: 0; }`

> #### 动画

- transition 过渡动画  
  `transition: <property> <duration> <timing-function> <delay>;`
  - 伪类触发：:hover : focus :checked :active、
  - js 触发
- animation 关键帧动画 @keyframes

  `animation: name | duration | timing-function | delay(动画开始之前的延迟) | iteration-count(动画播放的次数) | direction(是否轮流反向播放动画);`

  ```css
  animation: name 5s infinite;
  @keyframes name {
    from {
      background: red;
    }
    to {
      background: yellow;
    }
  }
  ```

> #### 背景 background

- `background-color` 最常用的属性，默认不继承（background 的所有属性都默认不继承），初始值为 transparent；有时候使用默认继承可以实现一些好玩的效果，比如倒影；
- `backgroundo-image` 背景图片，可以逗号分割设置多个，可以是图片 url 或者渐变色；
- `background-clip` 背景剪裁，可以逗号分割设置多个，值可以为 broder-box（初始值）、padding-box、content-box、text（新，将背景被文字剪裁）；
- `background-origin` 设置背景起始点的相对区域，搭配 background-position 使用，可以逗号分割设置多个，值可以是 border-box、padding-box（初始值）、content-box；
- `background-position` 设置背景的起始点，可以逗号分割设置多个，值可以是 10px 20px 、center center 、left 10px bottom 20px 等等，非常灵活；
- `background-size` 设置背景的大小，可以逗号分割设置多个，值可以是数字值如 30px 40px、auto auto（初始值）、conver、contain；background-repeat: repeat 就是根据这个尺寸大小来重复的。
- `background-repeat` 设置背景的重复方式，初始值为 repeat，常使用值的还有 no-repeat；
- `background-attachment` 设置背景图像的位置是在视口内固定，还是随着包含它的区块滚动。可以逗号分割设置多个，值有 scroll（初始值）、local、fixed。

简写：`background: [background-color] [background-image] [background-repeat] [background-attachment] [background-position] / [ background-size][background-origin] [background-clip];`  
background-size 只能紧接着 background-position 出现，以 / 分割，如："center / 80%"。

> #### 让页面里的字体变清晰，变细用 CSS 怎么做？

    -webkit-font-smoothing: antialiased;

> #### 响应式布局的常用解决方案对比

- px 和视口

  `<meta id="viewport" name="viewport" content="width=device-width; initial-scale=1.0; maximum-scale=1; user-scalable=no;">`

- 媒体查询

  ```css
  @media screen and (max-width: 960px) {
    body {
      background-color: #ff6699;
    }
  }
  ```

- 百分比

  ```css
  .trangle {
    height: 0;
    width: 100%;
    padding-top: 75%;
  }
  ```

- rem

  rem 单位都是相对于根元素 html 的 font-size 来决定大小的,根元素的 font-size 相当于提供了一个基准，当页面的 size 发生变化时，只需要改变 font-size 的值，那么以 rem 为固定单位的元素的大小也会发生响应的变化。
  因此，如果通过 rem 来实现响应式的布局，只需要根据视图容器的大小，动态的改变 font-size 即可。

  ```js
  function refreshRem() {
    var docEl = doc.documentElement;
    var width = docEl.getBoundingClientRect().width;
    var rem = width / 10;
    docEl.style.fontSize = rem + "px";
    flexible.rem = win.rem = rem;
  }
  win.addEventListener("resize", refreshRem);
  ```

  上述代码中将视图容器分为 10 份，font-size 用十分之一的宽度来表示，最后在 header 标签中执行这段代码，就可以动态定义 font-size 的大小，从而 1rem 在不同的视觉容器中表示不同的大小，用 rem 固定单位可以实现不同容器内布局的自适应。

- vw/vh

  css3 中引入了一个新的单位 vw/vh，与视图窗口有关，vw 表示相对于视图窗口的宽度，vh 表示相对于视图窗口高度，除了 vw 和 vh 外，还有 vmin 和 vmax 两个相关的单位。

[参考出处](!https://juejin.im/post/5b39905351882574c72f2808#heading-17)

> #### css reset 和 normalize.css 有什么区别

- reset 相对「暴力」，不管你有没有用，统统重置成一样的效果，且影响的范围很大，讲求跨浏览器的一致性。
- normalize 相对「平和」，注重通用的方案，重置掉该重置的样式，保留有用的 user agent 样式

> #### CSS 预处理器(Sass/Less)

CSS 预处理器的原理: 是将类 CSS 语言通过 Webpack 编译 转成浏览器可读的真正 CSS。在这层编译之上，便可以赋予 CSS 更多更强大的功能，常用功能:

- 嵌套
- 变量
- 循环语句
- 条件语句
- 自动前缀
- 单位转换
- mixin 复用

```

```
