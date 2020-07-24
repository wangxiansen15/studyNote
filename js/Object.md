### 对象与基本类型的不同

* 对象有属性
* 对象有方法
* 对象可以改变（改变对象的属性）

### 只有对象有方法

​	数组，字符串数字里面的方法都是先转化为常用对象，然后调用常用对象的方法

### 对象的分类

* 内部对象（17）： js自带的对象
  * 错误对象：抛出错误的对象
  * 常用对象： bool str number ary date fun obj 正则表达式
  * 内置对象：math，global, json
* 环境对象
* 自定义对象

### Object()

​	把其他属性转换为对象

​	Object(null||undefined)   转换为一个空对象

 ### 对象转化为原始数据类型

* Bool： 当你把bool对象转化为原始数据类型的时候，都是true
  * false -> 对象 -> true
  * Boolean(new Object(false)) == true
* Str: 先调用toString() ， 不能得到结果调用 valueOf()
* number： 先调用valueof 然后调用ttostring

### 每个对象都有的方法

* toString():让对象用字符串表示出来
  * ary:  把数组里的每一个元素都用字符串表示，中间拿，分割
  * fun：当前函数的源代码
  * date：日期和时期的字符串表示
  * 正则表达式：直接转换为字符串
* valueOf():如果有原始值直接返回原始值，没有原始值，返回对象本身，date不一样，返回一长串字符串

### 如何创建对象

* 对象直接量： 键最好加'',防止关键字

  ```
  var person = {
  	aa:'0231'
  }
  ```

* new Object()

* Object.create()

###  查询对象

* .

  * ```js
    var obj = {
        name: "cedric"
    }
    
    console.log(obj.name); // cedric
    ```

    

* []

  * ```js
    var obj = {
        name: "cedric"
    }
    
    console.log(obj["name"]); // cedric
    ```

    

  当解释器遇到.的时候会去计算.前面的东西是不是null||undefined，如果是直接报错，不是的话继续，看前面 的是不是对象，如果不是转换为对象，之后看是不是.，如果是.,直接把.后面的名字对应的值返回回来，如果是[]，首先将[]里面的东西进行计算，结果转化为字符串，然后字符串所对应的值返回;如果属性不存在返回undefined
  
  ### function属于object
  
  