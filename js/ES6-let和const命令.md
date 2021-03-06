## let命令

### 基本用法

`let`命令，用于声明变量，用法类似于`var`，但所声明的变量，**只在`let`命令所在的代码块内有效。**

```js
{
  let a = 10;
  var b = 1;
}

a // ReferenceError: a is not defined.
b // 1
```

`for`循环还有一个特别之处，就是设置循环变量的那部分是一个父作用域，而循环体内部是一个单独的子作用域。

```javascript
for (let i = 0; i < 3; i++) {
  let i = 'abc';
  console.log(i);
}
// abc
// abc
// abc
```

### 不存在变量提升

`let`命令一定要在声明后使用，否则报错。

### 暂时性死区

只要块级作用域内存在`let`命令，它声明的变量就‘绑定’这个区域，不再受外部的影响。

```js
var tmp = 123;

if (true) {
  tmp = 'abc'; // ReferenceError
  let tmp;
}
```

ES6 明确规定，如果区块中存在`let`和`const`命令，这个区块对这些命令声明的变量，从一开始就形成了封闭作用域。凡是在声明之前就使用这些变量，就会报错。

有些“死区”比较隐蔽，不太容易发现。

```javascript
function bar(x = y, y = 2) {
  return [x, y];
}

bar(); // 报错
```

上面代码中，调用`bar`函数之所以报错（某些实现可能不报错），是因为参数`x`默认值等于另一个参数`y`，而此时`y`还没有声明，属于“死区”。如果`y`的默认值是`x`，就不会报错，因为此时`x`已经声明了。

```javascript
function bar(x = 2, y = x) {
  return [x, y];
}
bar(); // [2, 2]
```

另外，下面的代码也会报错，与`var`的行为不同。

```javascript
// 不报错
var x = x;

// 报错
let x = x;
// ReferenceError: x is not defined
```

上面代码报错，也是因为暂时性死区。使用`let`声明变量时，只要变量在还没有声明完成前使用，就会报错。上面这行就属于这个情况，在变量`x`的声明语句还没有执行完成前，就去取`x`的值，导致报错”x 未定义“。

暂时性死区的本质就是，只要一进入当前作用域，所要使用的变量就已经存在了，但是不可获取，只有等到声明变量的那一行代码出现，才可以获取和使用该变量。

###  不允许重复声明

`let`不允许在相同作用域内，重复声明同一个变量。所以，不能在函数内部重新声明参数。

```js
function func(arg) {
  let arg;
}
func() // 报错
```

##  块级作用域

### 为什么需要块级作用域？

* 内层变量可能会覆盖外层变量。
* 用来计数的循环变量泄露为全局变量

### ES6的块级作用域

* `let`实际上为JavaScript新增了块级作用域。

* ES6 允许块级作用域的任意嵌套。

* 内层作用域可以定义外层作用域的同名变量。

### 块级作用域与函数声明

ES5 规定，函数只能在顶层作用域和函数作用域之中声明，不能在块级作用域声明。但是，浏览器没有遵守这个规定，为了兼容以前的旧代码，还是支持在块级作用域之中声明函数，因此上面两种情况实际都能运行，不会报错。

ES6 引入了块级作用域，明确允许在块级作用域之中声明函数。ES6 规定，块级作用域之中，函数声明语句的行为类似于`let`，在块级作用域之外不可引用。

原来，如果改变了块级作用域内声明的函数的处理规则，显然会对老代码产生很大影响。为了减轻因此产生的不兼容问题，ES6在附录B里面规定，浏览器的实现可以不遵守上面的规定，有自己的行为方式。

* 允许在块级作用域内声明函数。
* 函数声明类似于`var`，即会提升到全局作用域或函数作用域的头部。

* 同时，函数声明还会提升到所在的块级作用域的头部。

上面三条规则只对 ES6 的浏览器实现有效，其他环境的实现不用遵守，还是将块级作用域的函数声明当作`let`处理。

## const命令

`const`声明一个只读的常量。一旦声明，常量的值就不能改变。

```javascript
const PI = 3.1415;
PI // 3.1415

PI = 3;
// TypeError: Assignment to constant variable.
```

`const`的作用域与`let`命令相同：只在声明所在的块级作用域内有效。

`const`命令声明的常量也是不提示，同样存在暂时性死区，只能在声明的位置后面使用。

`const`声明的常量，也与`let`一样不可以重复声明。

### 本质

 `const`实际上保证的，并不是变量的值不得改动，而是变量指向的那个内存地址所保存的数据不得改动。对于简单类型的数据（数值、字符串、布尔值），值就保存在变量指向的那个内存地址，因此等同于常量。但对于复合类型的数据（主要是对象和数组），变量指向的内存地址，保存的只是一个指向实际数据的指针，`const`只能保证这个指针是固定的（即总是指向另一个固定的地址），至于它指向的数据结构是不是可变的，就完全不能控制了。因此，将一个对象声明为常量必须非常小心。

```javascript
const foo = {};

// 为 foo 添加一个属性，可以成功
foo.prop = 123;
foo.prop // 123

// 将 foo 指向另一个对象，就会报错
foo = {}; // TypeError: "foo" is read-only
```

如果真的想将对象冻结，应该使用`Object.freeze`方法

```javascript
const foo = Object.freeze({});

// 常规模式时，下面一行不起作用；
// 严格模式时，该行会报错
foo.prop = 123;
```

## ES6声明变量的六种方法

* var
* function
* let
* const
* import
* class

## 顶层对象的属性

顶层对象，在浏览器环境指的是`window`对象。ES5之中，顶层对象的属性与全局变量是等价的。

ES6规定：`var`与`function`命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，`let`,`const`,`class`命令声明的全局变量，不属于顶层对象的属性，也就是说，从ES6开始，全局变量将逐步与顶层对象的属性脱钩。

## globalThis对象

ES2020在语言标准的层面，引入`globalThis`作为顶层对象。也就是说，任何环境下，`globalThis`都是存在的，都可以从它拿到顶层对象，指向全局环境下的`this`。