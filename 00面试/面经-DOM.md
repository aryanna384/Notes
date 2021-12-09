

## 页面生命周期
https://zh.javascript.info/onload-ondomcontentloaded#windowonunload
HTML 页面的生命周期包含三个重要事件：

1. DOMContentLoaded —— 浏览器已完全加载 HTML，并构建了 DOM 树，但像 <img> 和样式表之类的外部资源可能尚未加载完成。
2. load —— 浏览器不仅加载完成了 HTML，还加载完成了所有外部资源：图片，样式等。
3. beforeunload/unload —— 当用户正在离开页面时。


`DOMContentLoaded`事件 —— DOM 已经就绪，因此处理程序可以查找 DOM 节点，并初始化接口。
`load` 事件 —— 外部资源已加载完成，样式已被应用，图片大小也已知了。
`beforeunload` 事件 —— 用户正在离开：我们可以检查用户是否保存了更改，并询问他是否真的要离开。
`unload` 事件 —— 用户几乎已经离开了，但是我们仍然可以启动一些操作，例如发送统计数据。

```html
<body>
  <!-- 
    Library loaded, inline script executed
    DOM is ready
    Image size：0*0
    图片完全加载完成之后
    Page loaded
    Image size：177*160
    在关闭页面之前会chrome 会弹出默认对话框：系统可能不会保存您所做的更改
   -->

  <img id="img" src="https://en.js.cx/clipart/train.gif?speed=1&cache=0">
  <script>
    function ready() {
      alert('DOM is ready');
      // 图片目前尚未加载完成（除非已经被缓存），所以图片的大小为 0x0
      alert(`Image size: ${img.offsetWidth}x${img.offsetHeight}`);
    }
    window.onload = function () {
      alert('Page loaded');
      // 此时图片已经加载完成
      alert(`Image size: ${img.offsetWidth}x${img.offsetHeight}`);
    };
    document.addEventListener("DOMContentLoaded", ready);
    window.addEventListener("unload", function () {
      alert('on unloading ')
    });
    window.onbeforeunload = function () {
      return false;
    };

  </script>
  <script>
    alert("Library loaded, inline script executed");
  </script>
</body>
```