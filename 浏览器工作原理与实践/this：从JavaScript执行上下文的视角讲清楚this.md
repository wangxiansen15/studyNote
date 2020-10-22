# this：从JavaScript执行上下文的视角讲清楚this

首先要清楚**this是和执行上下文绑定的**，也就是说每个执行上下文中都有一个this。

执行上下文的三种类型

* 全局执行上下文
* 函数执行上下文
* eval执行上下文

所以对应的this也只有这三种——**全局执行上下文中的this、函数中的this和eval中的this**。

## 全局执行上下文中的this

在控制台中输入console.log(this)来打印出来全局执行上下文中的this，最终输出的是window对象。所以可以得出这样一个结论：**全局执行上下文中的this是指向window对象的。**这也是this和作用域链的唯一交点，作用域链的最底端包含了window对象，全局执行上下文中的this也是指向window对象。

## 函数执行上下文中的this

```js
function foo(){
	console.log(this);
}
foo();
```

执行这段代码，打印出来的也是window对象，这说明在默认情况下调用一个函数，其执行上下文中的this也是指向window对象的。下面有三种方式来设置函数执行上下文中的this值指向其他对象。

* ### 通过函数的call方法设置

  可以通过**call**方法来设置函数执行上下文的this指向

  ```js
  let bar = {
  	myName: "极客邦",
  	test1: 1
  };
  function foo(){
  	this.myName = "极客时间";
  }
  foo.call(bar);
  console.log(bar);//{myName: "极客时间", test1: 1}
  console.log(myName);//undeclared
  ```

  执行这段代码，你会发现foo函数内部的this已经指向了bar对象

  其实除了call方法，你还可以使用**bind**和**apply**方法来设置函数执行上下文中的this

* ### 通过对象调用方法设置

  要改变函数执行上下文中的this指向，除了通过函数的call方法来实现外，还可以通过对象调用的方式

  ```js
  var myObj = {
  	name: "极客时间",
  	showThis: function(){
  		console.log(this);
  	}
  }
  myObj.showThis();//{name: "极客时间", showThis: ƒ}
  ```

  结论：**使用对象来调用其内部的一个方法，该方法的this是指向对象本身的。**

  其实也可以认为JavaScript引擎在执行myObject.showThis()时，将其转化为了：`myObj.showThis.call(myObj)`

  ```js
  var myObj = {
  	name: "极客时间",
  	showThis: function(){
  		console.log(this);
  	}
  }
  var foo = myObj.showThis
  foo();//window
  ```

  执行这段代码，你会发现this又指向了全局window对象

  两个结论：

  * **在全局环境中调用一个函数，函数内部的this指向的是全局变量window。**
  * **通过一个对象来调用其内部的一个方法，该方法的执行上下文中的this指向对象本身。**

* ### 通过构造函数中设置

  ```js
  function CreateObj(){
  	this.name = "极客时间";
  }
  var myObj = new CreateObj();
  ```

  其实，当执行`new CreateObj()`的时候，JavaScript引擎做了如下四件事：

  * 首先创建了一个空对象tempObj；
  * 接着调用`CreateObj.call`方法，并将tempObj作为call方法的参数，这样当CreateObj的执行上下文创建时，它的this就指向了tempObj对象；
  * 然后执行`CreateObj`函数，此时的CreateObj函数的执行上下文中的this指向tempObj对象；
  * 最后返回tempObj对象。

  为了直观理解，用以下代码来演示：

  ```js
  var tempObj = {};
  CreateObj.call(tempObj);
  return tempObj;
  ```

  这样就通过new关键字构建好了一个新对象，并且构造函数中的this其实就是新对象本身。

## this的设计缺陷以及应对方法

### 1.嵌套函数中的this不会从外层函数中继承

```js
var myObj = {
	name: "极客时间",
	showThis: function(){
		console.log(this);//{name: "极客时间", showThis: ƒ}
		function bar(){
			console.log(this);//window
		}
        bar();
	}
}
myObj.showThis();
```

按照直觉可能以为bar中的this应该和其外层showThis函数中的this是一致的，都是指向myObj的。但实际情况并非如此，**函数bar中的this指向的是全局window对象，而函数showThis中的this指向的是myObj对象**。

可以通过一个小技巧来解决这个问题，比如在showThis函数中**声明一个变量self用来保存this**，然后在bar函数中使用self，代码如下：

```js
var myObj = {
	name: "极客时间",
	showThis: function(){
		console.log(this);//{name: "极客时间", showThis: ƒ}
		var self = this;
		function bar(){
			console.log(self);//{name: "极客时间", showThis: ƒ}
		}
        bar();
	}
}
myObj.showThis();
```

**也可以使用ES6中的箭头函数来解决这个问题**

```js
var myObj = {
	name: "极客时间",
	showThis: function(){
		console.log(this);//{name: "极客时间", showThis: ƒ}
		var bar = () => {
			console.log(this);//{name: "极客时间", showThis: ƒ}
		}
        bar();
	}
}
myObj.showThis();
```

你会发现箭头函数bar里面的this是指向myObj对象的。这是因为ES6中的箭头函数并不会创建其自身的执行上下文，所以箭头函数中的this取决于它的外部函数。

### 2.普通函数中的this默认指向全局对象window

这个问题可以通过设置JavaScript的“严格模式”来解决。在严格模式下，默认执行一个函数，其函数的执行上下文中的this值是undefined。