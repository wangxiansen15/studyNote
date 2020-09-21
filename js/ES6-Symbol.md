# Symbol

ES6引入了一种新的原视数据类型Symbol，**表示独一无二的值。**它是JavaScript语言中的第七种数据类型，是一种类似于字符串的数据类型。

## Symbol特点

* Symbol的值是唯一的。用来解决命名冲突的问题
* Symbol值不能与其他数据进行运算
* Symbol定义的对象属性不能使用for-in循环遍历，但是可以使用Reflect.ownKeys来获取对象的所有键名

## 创建Symbol

```js
//创建Symbol
let s = Symbol();
let s2 = Symbol('WWJ');
let s3 = Symbol('WWJ');
s2 === s3;//false
//Symbol.for创建
let s4 = Symbol.for('WWJ');
let s5 = Symbol.for('WWJ');
s4 === s5;//true
```

## Symbol的使用

* 解决命名冲突

  ```js
  let game = {
      name: '俄罗斯方块',
      up: function(){
          console.log('up1');
      },
      down: function(){
          console.log('down1');
      }
  }
  
  let methods = {
      up: Symbol(),
      down: Symbol()
  };
  
  game[methods.up] = function(){
      console.log('up2');
  }
  
  game[methods.down] = function(){
      console.log('down2');
  }
  ```

  ```js
  let youxi = {
  	name: '狼人杀',
  	[Symbol('say')]: function(){
  		console.log('我可以发言');
  	},
  	[Symbol('zibao')]: function(){
  		console.log('我可以自爆');
  	}
  }
  
  console.log(youxi);
  ```

  

