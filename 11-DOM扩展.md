# DOM扩展

## 主要内容

* **Selectors API**
* **HTML5**

## Selectors API Level 1
### `querySelector()`

接收一个css选择符，返回与该模式匹配的第一个元素，如果没有找到匹配的元素返回null。

范围：通过Document类型调用该方法，会在文档元素的范围内查找匹配的元素；而通过Element类型调用时，只会在该元素后代的范围内查找匹配的元素

举例

```js
document.querySelector("body");//取得body元素
document.querySelector("#myDiv");//取得ID为"myDiv"的元素
document.querySelector(".selected");//取得类为"selected"的第一个元素
document.querySelector("img.selected");//取得类为"selected"的第一个图像元素
```

### `querySelectorAll()`

接收一个css选择符，返回所有匹配的元素（一个NodeList的实例）

## Selectors API Level 2

### `matchesSelector()`

接收一个css选择符，如果调用元素与该选择符匹配，返回`true`;否则`false`。

## HTML5

### `getElementsByClassName()`

接收一个参数，即一个包含一或者多个类名的字符串，返回带有指定类的所有元素的NodeList。传入多个类名时，类名的先后顺序不重要

只有调用元素子树上的元素才会返回，在元素调用该方法就只会返回后代元素中匹配的元素

### `classList`

获取：`[]`或者`item()`

方法：

*  `add(value)`：将给定的字符串添加到列表中；如果已经存在就不添加了
* `contains(value)`：表示列表是否存在给定的值，存在返回`true`，不存在返回`false`
* `remove(value)`： 从列表删除给定的字符串
* `toggle(value)`：如果列表中已经存在给定的值，删除它；如果列表中没有给定的值，添加它。

举例：

```js
div.classList.remove("disabled");//删除“disabled”类
div.classList.add("current");//添加"current"类
div.classList.toggle("user");//切换"user"类
```

### 焦点管理

 #### `document.activeElement`

这个属性始终会引用DOM中当前获得了焦点的元素。

元素获得焦点的方式有页面加载、用户输入（通常是通过按tab）和在代码中调用`focus()`方法

举例：

```js
var  button = document.getElementById("myButton");
button.focus();
document.activeElement === button//true
```

文档刚加载完毕，默认保存body；文档加载期间，保存`null`.

#### `document.hasFocus()`

用于确定文档是否获得了焦点

### HTMLDocument的变化

#### readyState属性

document.readState 。有两个可能的值。loading：正在加载文档；complete：已经加载完文档。

#### 字符集属性

document.charset默认`utf-16`，可以通过`<meta>`元素或者直接设置charset属性修改这个值

document.defaultCharset,根据浏览器和操作系统的设置，当前文档默认的字符集应该是什么

#### 自定义数据属性

HTML5规定可以为元素添加非标准的属性，但要加前缀`data-`,为元素提供与渲染无关的信息，或者提供语义信息。

添加了自定义属性之后，可以使用`dataset`属性去访问

举例：

```js
<button id="lx" data-id="lianxi">练习</button>
console.log(a.dataset.id);//lianxi
```

#### 插入标记

* innerHTML属性

  返回调用元素的所有子节点（包括元素、注释和文本节点）对应的HTML标记。在写模式下，innerHTML会根据指定的值创建新的DOM树，然后用这个DOM树替换调用元素原先的所有子节点

  注意

  * 插入`<script>`元素不会执行其中的脚本，IE8之前例外
  * 支持插入`<style>`元素

* outerHTML属性

  读模式，返回调用它的元素及所有子节点的HTML标签。

  写模式，根据指定的HTML字符串创建新的DOM子树，然后用这个子树完全替换调用元素

* insertAdjacentHEML()方法

  两个参数，插入的位置和要插入的HTML文档

  第一个参数

  * "beforebegin"，在当前元素之前插入一个紧邻的同辈元素
  * "afterbegin"，在当前元素之下插入一个新的子元素或在第一个子元素之前插入新的子元素
  * "beforend"，在当前元素之下插入一个新的子元素或在最后一个子元素之后再插入新的子元素
  * "afterend"，在当前元素之后插入一个紧邻的同辈元素

#### 内存与性能问题

在如果在替换html中的元素中带有事件处理程序或引用了一个js对象，当元素从文档树中删除后，元素与事件处理函数（或js对象）之间的绑定关系在内存中没有一并被删除。导致页面占用的内存数量就会明显增加。所以在使用插入标记方法时，最好手动删除要被替换的元素的所有事件处理程序和js对象属性

要将innerHTML与outerHTML的次数控制在合理的范围















