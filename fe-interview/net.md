#### HTTP 状态码

- 1xx: 接受，继续处理
- 200: 成功，并返回数据
- 201: 已创建
- 202: 已接受
- 203: 成为，但未授权
- 204: 成功，无内容
- 205: 成功，重置内容
- 206: 成功，部分内容
- 301: 永久移动，重定向
- 302: 临时移动，可使用原有 URI
- 304: 资源未修改，可使用缓存
- 305: 需代理访问
- 400: 请求语法错误
- 401: 要求身份认证
- 403: 拒绝请求
- 404: 资源不存在
- 500: 服务器错误

#### Cookie / LocalStorage / Session

#### CDN

#### GET / POST / PUT

#### 跨域 CORS / JSONP

> #### 浏览器内核

- IE：Trident
- Chrome：统称为 Chromium 内核，以前是 Webkit，现在是 Blink
- Firefox：Gecko
- Safari：Webkit
- Opera：最初是自己的 Presto 内核，后来是 Webkit，现在是 Blink 内核
- 360/猎豹：IE+Chrome 双内核

> #### 从 url 输入到页面展示

- DNS 解析
- TCP 三次握手
- 发送请求，分析 url，设置请求报文(头，主体)
- 服务器返回请求的文件 (html)
- 浏览器渲染
  - HTML parser --> DOM Tree
    - 标记化算法，进行元素状态的标记
    - dom 树构建
  - CSS parser --> Style Tree
    - 解析 css 代码，生成样式树
  - attachment --> Render Tree
    - 结合 dom 树 与 style 树，生成渲染树
  - layout: 布局
  - GPU painting: 像素绘制页面
