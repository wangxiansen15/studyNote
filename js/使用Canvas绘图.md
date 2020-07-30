# 使用Canvas绘图

## 基本用法

* 必须要先设置width与height（指定可以绘图的区域）

* 出现在标签之中的内容是后备信息，如果浏览器不支持canvas元素，就会显示这些信息

* 也可以通过css为该元素添加样式，如果不添加任何样式或者不绘制任何图形，页面中是看不到该元素的

  举例：

  ```html
  <canvas width="200" height="200" id="drawing">hhhh</canvas>
  ```

* 如果要在这块画布上绘图，需要取得绘图上下文。而取得绘图上下文对象的引用，需要调用getContext()方法并传入上下文的名字。传入‘2d’，就可以取得2D上下文对象
  举例：

  ```js
  var drawing = document.getElementById("drawing");
  
  //确认浏览器支持canvas元素
  if (drawing.getContext) {
     var context = drawing.getContext('2d');
  }
  ```

* 使用toDataURl()方法，可以导出在canvas元素上绘制的元素。这个方法接受一个参数，即图片的MIME类型格式，而且适合用于创建图像的任何上下文。
  举例：

  ```js
  	var drawing = document.getElementById("drawing");
  
      //确认浏览器支持canvas元素
      if (drawing.getContext) {
          //取得图像的URL
          var imgURL = drawing.toDataURL("image/png");
          
          //显示图像
          var image = document.createElement("img");
          image.src = imgURL;
          document.body.appendChild(image);
      }
  ```

## 2D上下文

### 原点坐标

坐标开始于canvas元素的左上角，原点坐标为0，0。

### 填充和描边

fillStyle和strokeStyle。所有涉及描边和填充的操作都会使用这两个样式，直至重新设置这两个值

### 绘制矩形

方法

* fillRect()

  在画布上绘制的矩形会填充指定的颜色。填充的颜色由fillStyle属性指定

* strokenRect()

  在画布上绘制的矩形会使用指定的颜色描边。描边的颜色通过strokeStyle属性指定

* clearRect()

  用于清除画布上的矩形区域，把某一矩形区域变透明

  这三个方法都可以接收四个参数：矩形的X坐标、矩形的Y坐标、矩形的宽度、矩形的高度。单位都是像素。

### 绘制路径

* 首先要调用beginPath()方法，表示要开始绘制新路径

* 方法

  * arc(x, y, radius, startAngle, endAngle, anticlockwise)
    
    画一个以`（x,y）`为圆心的以radius为半径的圆弧（圆），从`startAngle`开始到`endAngle`结束，按照`anticlockwise`给定的方向（默认为顺时针）来生成
    
  * arcTo(x1, y1, x2, y2, radius)
    
    从上一点开始绘制一条弧线，到x2,y2为止，并且以给定半径radius穿过x1，y1
    
  * lineTo(x,y)：从上一个点开始绘制一条直线，到x，y为止
  
  * moveTo(x,y)：将绘图游标移到x，y。不画线
  
* 如果想要绘制一条连接到路径起点的线条，可以调用closePath()

* 如果想用fillStyle填充，可以用fill()方法

* 可以用stroke()方法对路径描边，描边使用的是strokeStyle

* clip()。在路径上创建一个剪切区域

* isPointinPath(x,y)。x，y是否在路径上

### 绘制文本

* fillText()

* strokeText()

  这两个方法都可以接收4个参数：字符文本串，x坐标，y坐标，可选的最大像素宽度。这两个方法以3个属性为基础

  * font
  * textAlign
  * textBaseline：表示文本的基线

### 变化

* rotate(angle)
* scale(caleX,scaleY)
* translate(x,y)

### 绘制图像

drawimage()

参数为(image,x,y)：起点为(x,y)。绘制到画布上的图像大小与原始大小相同

参数为(image,x,y,x1,y2)：多传入的参数代表目标宽度和高度

### 阴影

* shadowCOlor
* shadowOffsetX
* shadowOffsetY
* shadowBlur

### 渐变

* createLinearGradient(起点的x坐标，起点的y坐标，终点的x坐标，终点的y坐标)
* addColorStop()：指定色标。两个参数，色标位置和CSS颜色，色标位置是0-1之间的数字

### 模式

createPattern(image, 'repeat')

第二个参数与background-repeat属性值相同

### 使用图像数据

getImageData(x坐标，y坐标，该区域像素宽度，高度)



