# JavaScript代码的执行顺序

```js
showName();
console.log(myname);
var myname = 'wwj'
function showName() {
	console.log("函数被执行");
}
```

## 变量提升

**所谓的变量提升，是指在JavaScript代码在执行过程中，JavaScript引擎把变量的声明部分和函数的声明部分提升到代码开头的“行为”。变量被提升后，会给变量设置默认值undefined。**

## 出现变量提升的原因

**一段JavaScript代码会经过两个阶段；第一个阶段是编译阶段，第二个阶段是执行阶段。**

### 编译阶段

```js
showName();
console.log(myname);
var myname = 'wwj'
function showName() {
	console.log("函数被执行");
}
```

* 第一行和第二行，这两行不是声明操作，所以JavaScript引擎不会做任何处理
* 第三行，由于这行是经过var声明的，因此JavaScript引擎将会在环境对象中创建一个名为myname的属性，并使用undefined对其初始化
* 第四行，JavaScript引擎发现了一个通过function定义的函数，所以它将函数定义存储到堆(HEAP)中，并在环境对象中创建一个showName的属性，然后将该属性值指向堆中函数的位置

### 执行阶段

JavaScript引擎开始执行代码，按照顺序一行一行地执行。

* 当执行到showName函数时，JavaScript引擎会在变量环境对象中查找该函数，由于变量环境对象中存在该函数的引用，所以JavaScript引擎便开始执行该函数
* 接下来打印myname信息，JavaScript引擎继续在变量环境中查找该对象，由于环境变量存在myname变量，并且其值为undefined，所以这时候就输出undefined
* 接下来执行第三行，把‘wwj’赋给变量，赋值后变量环境中的myname属性值发生改变