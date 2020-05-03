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

> #### CSS3 新增伪类有那些？

> #### CSS3 有哪些新特性？

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

> #### margin 和 padding 分别适合什么场景使用？

> #### ::before 和 :after 中双冒号和单冒号 有什么区别？解释一下这 2 个伪元素的作用。

> #### 让页面里的字体变清晰，变细用 CSS 怎么做？（-webkit-font-smoothing: antialiased;）

> #### rem 布局的优缺点

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
