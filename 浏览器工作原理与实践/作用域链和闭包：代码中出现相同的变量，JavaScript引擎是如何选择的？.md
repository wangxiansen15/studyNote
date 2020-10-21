# 作用域链和闭包：代码中出现相同的变量，JavaScript引擎是如何选择的？

```js
function bar(){
	console.log(myname);//全局
}
function foo(){
	var myname = "foo";
	bar();
}
var myname = "quanju";
foo();
```

注意：**我的第一反应是按照调用栈的顺序来查找变量，即先在bar的执行上下文中找myname变量没找到；接着去foo的执行上下文中去找，找到了myname变量即“foo”，但结果却是“全局”。**

## 作用域链

每个执行上下文的变量环境中，都包含了一个外部引用，用来指向外部的执行上下文，我们把这个外部引用称为**outer**。

当一段代码使用了一个变量时，JavaScript引擎首先会在“当前的执行上下文”中查找该变量，如果在当前的变量环境中没有查找到，那么JavaScript引擎会继续在outer所指向的执行上下文中查找。我们把这个查找的链条称为**作用域链**。

## 词法作用域

**在JavaScript执行过程中，其作用域链是由词法作用域来决定的。**

**词法作用域就是指作用域是由代码中函数声明的位置来决定的，所以词法作用域是静态的作用域，通过它就能够预测代码在执行过程中如何查找标识符。**

**词法作用域是代码阶段就决定好的，和函数是怎么调用的没有关系。**

## 块级作用域的变量查找

```js
function bar() {
    var myName = "极客世界";
    let test1 = 100;
    if (1) {
        let myName = "Chrome浏览器";
        console.log(test);//1
    }
}
function foo() 
{
    var myName = "极客邦";
    let test = 2;
    {
        let test = 3;
        bar();
    }
}
var myName = "极客时间";
let myAge = 10;
let test = 1;
foo();
```

分析一下过程，首先在bar函数的执行上下文查找，根据词法环境，从词法环境的栈顶开始向下搜索，没有找到，然后在bar执行上下文中的变量环境中寻找，没有找到；然后根据词法作用域的规则（作用域是由代码中函数声明的位置来决定的）沿着作用域链到了全局函数的执行环境，首先在全局函数的词法环境中寻找，找到了test为1。

## 闭包

```js
function foo() {
	var myName = "极客时间";
	let test1 = 1;
	const test2 = 2;
	var innerBar = {
		getName:function(){
			console.log(test1);
			return myName;
		},
		setName:function(newName){
			myName = newName;
		}
	}
	return innerBar;
}
var bar = foo();
bar.setName("极客邦");
bar.getName();
console.log(bar.getName());//1 1 "极客邦"
```

从上面的代码可以看出，innerBar是一个对象，包含了getName和setName的两个方法（通常我们把对象内部的函数称为方法）。可以看到这两个方法都是在函数内部定义的，并且这两个方法内部都使用了myName和test1两个变量。

**根据词法作用域的规则，内部函数getName和setName总是可以访问它们的外部函数，**所以当innerBar对象返回给全局变量bar时，虽然foo函数已经执行结束，但是getName和setName函数依然可以使用foo函数中的变量myName和test1。

虽然在foo函数执行完毕后，其执行上下文从栈顶弹出了，但是由于myName和test1这两个变量依然保存在内存中。这像极了这两个方法背的一个专属背包，无论在哪里调用了setName和getName方法，它们都背着这个foo函数的专属背包。

之所以是专属背包，是因为除了这两个方法，其他任何地方都是无法访问该背包的，我们就可以把这个背包称为foo函数的**闭包**。

### 闭包定义

**在JavaScript中，根据词法作用域，内部函数总是可以访问其外部函数中声明的变量，当通过调用一个外部函数返回一个内部函数后，即使该外部函数已经执行结束了，但是内部函数引用外部函数的变量依然保存在内存中，我们把这些变量的集合称为闭包。比如外部函数是foo，那么这些变量的集合就称为foo函数的闭包。**

当执行到bar.setName方法中的myName=“极客邦”这句代码时，JavaScript引擎会沿着“当前执行上下文- >foo函数闭包->全局执行上下文”的顺序来查找myName变量。

## 闭包是怎么回收的

通常，如果引用闭包的函数是一个全局变量，那么闭包会一直存在知道页面关闭；但如果这个闭包以后不再使用的话，就会造成内存泄露。

如果引用闭包的函数是一个局部变量，等函数销毁之后，在下次JavaScript引擎执行垃圾回收时，判断闭包这块内容已经不再被使用了，那么JavaScript引擎的垃圾回收器就会回收这块内存。

所以在使用闭包的时候，尽量注意一个原则：**如果该闭包会一直使用，那么它可以作为全局变量而存在；但如果使用频率不高，而且占用内存又比较大的话，那就尽量让它成为一个局部变量**。



