> #### 浏览器的渲染过程

1. 解析 HTML，生成 DOM 树，解析 CSS，生成 CSSOM 树
2. 将 DOM 树和 CSSOM 树结合，生成渲染树(Render Tree)
3. Layout(回流):根据生成的渲染树，进行回流(Layout)，得到节点的几何信息（位置，大小）
4. Painting(重绘):根据渲染树以及回流得到的几何信息，得到节点的绝对像素
5. Display:将像素发送给 GPU，展示在页面上。（这一步其实还有很多内容，比如会在 GPU 将多个合成层合并为同一个层，并展示在页面中。而 css3 硬件加速的原理则是新建合成层，这里我们不展开，之后有机会会写一篇博客）

> #### 检测浏览器版本版本有哪些方式？

> #### 重绘与回流

当元素的样式发生变化时，浏览器需要触发更新，重新绘制元素。这个过程中，有两种类型的操作，即重绘与回流。

- <b>重绘(repaint)</b>: 当元素样式的改变不影响布局时，浏览器将使用重绘对元素进行更新，此时由于只需要 UI 层面的重新像素绘制，因此 损耗较少

- <b>回流(reflow)</b>: 当元素的尺寸、结构或触发某些属性时，浏览器会重新渲染页面，称为回流。此时，浏览器需要重新经过计算，计算后还需要重新页面布局，因此是较重的操作。会触发回流的操作:

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

1.  我们可以将对元素多次的属性变化改成一次例如直接通过添加 class 的方式来改变样式：

    ```js
    // 该写法将触发三次回流（不考虑浏览器优化的情况，即使浏览器优化的情况）
    const el = document.getElementById("test");
    el.style.padding = "1px";
    el.style.borderLeft = "1px";
    el.style.borderRight = "1px";

    // 该写法只会触发一次视图回流
    const el = document.getElementById("test");
    el.style.cssText += "border-left: 1px; border-right: 1px; padding: 1px;";

    // 或者
    const el = document.getElementById("test");
    el.classList.add("test"); // 该样式如上添加的样式
    ```

2.  将需要样式修改的元素如动画元素等，使用 position:absolute 等方法，让其脱离文档流，这样该元素的样式改变了就不会影响其他的 dom 结构

3.  将需要复杂操作的元素现在内存中进行操作，不去渲染它，在完成操作以后再去渲染：

    ```js
    const el = document.createElement("div");
    el.style.padding = "1px";
    el.style.borderLeft = "1px";
    el.style.borderRight = "1px";
    // 在对元素完成相关操作后再渲染
    element.appendChild(div);
    ```

4.  需要循环插入多条数据时，应该生成代码片段后再一次性插入，不要每次循环都插入一条数据。

5.  我们知道 render-tree 只会处理 display 不为 none 的 dom 节点，因此如果要对一个 dom 节点做复杂操作可以先把该节点的 display 设置为 none 然后在进行相关操作，最后再将该节点设置为可见，这样只会触发两次的回流。

6.  如上我们知道，当使用一些 js 方法的时候会强制触发回流，比如获取元素的 width 属性，此时我们可以将改属性缓存起来，而不是每次都去直接获取触发回流：

    ```js
    // 此时将触发10次回流
    for (let i = 0; i < 10; i++) {
      list[i].style.width = box.offsetWidth + "px";
    }
    // 此时只触发一次
    let width = box.offsetWidth;
    for (let i = 0; i < 10; i++) {
      list[i].style.width = width + "px";
    }
    ```

- 使用 css3 的 GPU 硬件加速，可以让 transform、opacity、filters 这些动画不会引起回流重绘
