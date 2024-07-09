## script标签放head和body有什么区别？

**head中的script:**当浏览器解析HTML文档时，它会首先遇到 `<head>` 中的 `<script>` 标签。因此，这些脚本会在文档的其他部分（包括 `<body>`）之前加载和执行。如果脚本较大或依赖于外部资源（如API调用），这可能会导致页面加载的延迟，因为浏览器必须等待脚本加载和执行完成后才能继续解析和渲染页面的其余部分。在 `<head>` 中的脚本执行时，DOM可能还没有完全加载和解析。因此，如果脚本尝试访问或修改尚未加载的DOM元素，可能会导致错误或不可预期的行为

**body中的script**:将 `<script>` 标签放在 `<body>` 的底部（通常是在关闭的 `</body>` 标签之前）可以确保在脚本执行之前，页面的大部分内容（如DOM元素）已经被加载和解析。这有助于避免由于脚本执行而阻塞页面渲染的问题。将脚本放在 `<body>` 的底部可以确保在脚本执行时，DOM已经完全加载和解析。这使得脚本能够安全地访问和修改页面上的任何DOM元素。

## async和defer属性区别？

![image.png](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/894d84e8a7e0497091a26e43e1a84237~tplv-k3u1fbpfcp-zoom-in-crop-mark:1512:0:0:0.awebp?)

**蓝色线代表网络读取，红色线代表执行时间，这俩都是针对脚本的；绿色线代表 HTML 解析。**

1.async和defer都会在加载和渲染后续文档元素的过程将和 `script.js` 的加载并行进行（异步）

2.async会在script加载完后立即执行脚本，defer会等待文档元素加载完后再执行脚本。

## 页面加载过程中js文件会不会阻塞DOM的构建

js文件在以下的两种情况下会阻塞DOM的构建

- JavaScript文件被放置在head标签内部

- JavaScript代码修改了DOM结构

  js代码执行时，如果对DOM结构进行了修改，那么浏览器需要重排（reflow）和重绘（repaint），这个过程会较为耗时，并且会阻塞DOM和CSSOM的构建。

  以下两种方法可以解决js文件阻塞DOM的构建

- 通过设置 script 标签的 async 、defer 属性避免阻塞DOM和CSSOM的构建

- Web Workers ：Web Workers 是一种运行在后台线程的JavaScript脚本，它不会阻塞DOM和CSSOM的构建，并且可以利用多核CPU提高JavaScript代码执行速度。