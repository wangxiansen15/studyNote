# UI事件

UI事件指的是那些不一定与用户操作有关的事件。

* ~~DOMActivate：表示元素以及被用户操作（通过鼠标或键盘）激活。已被废弃~~
* load：当页面完全加载后在window上面触发，当所有框架都加载完毕时在框架集上触发，当图像加载完毕时在<img>元素上面触发，或者当嵌入的内容加载完毕时在<object>元素上面触发
* unload：当页面完全卸载后在window上面触发，当所有框架都卸载后在框架集上面触发，或者当嵌入的内容卸载完毕后在<object>元素上面触发
* abort：在用户停止下载过程时，如果嵌入的内容没有加载完，则在<object>元素上面出发。
* error：当发生js错误时在window上面出发，当无法加载图象时在<img>元素上面触发，当无法加载嵌入内容时在<object>元素上面触发。或者当有一或多个框架无法加载时在框架集上面触发
* select：当用户选择文本框（<input>或<texterea>）中的一或多个字符时触发
* resize：当窗口或框架的大小变化时在window或框架上面触发
* scroll：当用户滚动带滚动条的元素中的内容时，就在该元素上面触发

## load事件

当页面完全加载后（包括所有图像、js文件、CSS文件等外部资源），就会触发window上面的load事件

两种方式定义onload事件处理程序

* 通过js来指定事件处理程序的方式

  ```js
  	Event.addHandler("window","load",function(event) {
  		alert("Loaded");
  	})
  ```

* 为<body>元素添加一个onload特性

  ```html
  <body onload = "alert("Loaded")">
  </body>
  ```



图像也可以触发load事件，无论是在DOM中的图像元素还是HTML中的图像元素。因此可以在HTML中为任何图像指定onload事件处理函数，当然也可以使用js来实现

## unload事件

在文档被完全卸载后触发。只要用户从一个页面切换到了一个页面，就会发送unload事件。

利用这个事件最多的情况时清除引用，以避免内存泄漏，也有两种指定onunload事件处理程序的方式

* js

  ```js
  EventUtil.addHandler(window, "unload", function(event) {
  	alert("Unloaded");
  })
  ```

* 为body添加一个特性

  ```html
  <body onunload = "alert("Unloaded")"></body>
  ```

## resize事件

当浏览器窗口被调整到一个新的高度或宽度时吗就会触发resize事件。这个事件在window（窗口）上面触发，因此可以通过js或者<body>元素中的onresize特性来指定事件处理程序。推荐使用js方式

```js
EvebtUtil.addHandler(window, "resize", function(event) {
	alert("resized");
})
```

## scroll事件

虽然scroll事件是在window对象上发生的，但它实际表示的则是页面中响应元素的变化。

```js
EventUtil.addHandler(window, "scroll", function(event){
	alert(document.body.scrollTop);
})
```

滚动期间重复被触发，所以有必要保持事件处理程序的代码简单