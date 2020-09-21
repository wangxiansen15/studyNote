## 数组的特点

* 数组的每一项可以保存任何类型的数据
* 数组的大小可以动态调整

## 创建数组

* 使用new操作符

  ```js
  var colors = new Array();//参数只有一个数字，指的是长度；不是只有一个数字，这时候得到的是参数
  ```

* 使用数组字面量表示法

  ```js
  var colors = []
  ```

## length

当设置的length比数组长时，数组会变长，未定义的数组内容为undefined

当设置的length比数组短时，数组会变短，删除后面的数组内容

## 检测数组

* instanceof

* instance.constructor === Array

* Array.isArray(instance)

* ```js
  Object.prototype.toString.call(value) == '[object Array]';
  ```



## 方法

### 栈方法

* push()：接收**任意数量**的参数，逐个添加到数组末尾，返回修改后的**数组长度**
* pop()：不接受参数，从数组末尾**移除最后一项**，减少数组的length值，然后返回**移除的项**

```js
var a = ['a', 2, 'www'];
a.push('c');//4,返回数组长度
console.log(a);//["a", 2, "www", "c"]
a.pop();//c
```



### 队列方法

shift()：移除数组中的第一个项并且返回该项，同时数组长度减一

unshift()：数组前端添加任意个项返回新数组的长度

```js
var a = [1,2,3,4,5];
a.shift();//1
a.unshift('a','b','c');//7
a;//["a", "b", "c", 2, 3, 4, 5]
```



### 重排序方法

reverse()：反转数组项的顺序

```js
a;//["a", "b", "c", 2, 3, 4, 5]
a.reverse();
a;//[5, 4, 3, 2, "c", "b", "a"]
```



sort()：按升序排序数组项，调用每个数组项的toString()方法，然后比较得到的字符串（ascii码）；可以接收一个比较函数为参数，比较函数接受两个参数，如果第一个应该位于第二个之前，返回负数；如果两个参数相等，返回0；如果第一个参数应该位于第二个之后，返回一个正数。

```js
 var values = [0, 1, 5, 10, 15];
values.sort();
console.log(values);//[0, 1, 10, 15, 5]

function compare(value1, value2){
    return value1 >= value2 ? 1 : -1;
}
values.sort(compare);
console.log(values);//[0, 1, 5, 10, 15]
```



### 操作方法

* join(separator)：用于把数组中的所有元素放入一个字符串。元素是通过指定的分隔符进行分隔的，默认以逗号分割，返回一个字符串

  ```js
  var values = [0, 1, 5, 10, 15];
  var b = values.join();
  console.log(b);//0,1,5,10,15
  b = values.join('asd');//"0asd1asd5asd10asd15"
  ```

  

* concat()：会创建一个副本，然后将接收到的参数添加到副本的末尾，然后返回新创建的数组

  ```js
  var a = [1,2,3,4];
  var b = a.concat(5,6,7,8);
  console.log(b,a);// [1, 2, 3, 4, 5, 6, 7, 8] [1, 2, 3, 4]
  ```

  

* slice()：接收一或两个参数，要返回项的起始和结束位置。只有一个参数，从开始到结尾；如果有两个参数，返回起始和结束位置之前的项但不包括结束位置的项。不会影响原始数组。如果有一个负数，使用数组长度加上该数；如果结束位置小于起始位置，返回空数组。创建了一个新数组

  ```js
  var c = b.slice(1);
  console.log(b,c);//[1, 2, 3, 4, 5, 6, 7, 8] [2, 3, 4, 5, 6, 7, 8]
  c = b.slice(1,3);
  console.log(b,c);//[1, 2, 3, 4, 5, 6, 7, 8] [2, 3]
  ```

  

* splice()：
  * 删除：可以删除任意数量的项，两个参数：要删除的第一项的位置和要删除的项数
  * 插入：向指定位置插入任意数量的项：三个参数：起始位置、0（要删除的项）、要插入的项（可以传入任意多个项）
  * 替换：可以向指定位置插入任意数量的项，同时删除任意数量的项（结合前两个来看）



### 位置方法

indexOf()和lastIndexOf()：接收两个参数，要查找的项和表示查找起点位置的索引。indexOf从前往后，lastIndexOf从后往前。没找到返回-1.全等匹配。

```js
b;//[1, 2, 3, 4, 5, 6, 7, 8]
b.indexOf(2,0);//1
b.lastIndexOf(2,7);//1
```



### 迭代方法

5个迭代方法。

每个方法接收两个参数：每一项运行的函数和(可选的)运行该函数的作用域对象-影响this的值。传入的函数会接收三个参数，数组项的值，该项在数组中的位置和数组对象本身。但函数返回值不同

* every()：对数组每一项运行给定函数，该函数每一项都返回true，则返回true

  ```js
  var number = [1 ,2,3,4,5,6,7];
  var res = number.every(function(item, index, array){
      return (item > 2)
  });
  console.log(res);//false
  ```

  

* filiter()：对数组每一项运行给定函数，返回true的项组成的数组

  ```js
  var a = [1,2,3,4,5,6,7];
  var b = a.filter(function(item, index, array) {
      return (item > 3);
  });
  console.log(b);// [4, 5, 6, 7]
  ```

  

* forEach()：对数组每一项运行给定函数，没有返回值，相当于for循环

  ```js
  var number = [1,2,3,4,5,6,7];
  number.forEach(function(item, index, array) {
      array[index] += 1;
  });
  console.log(number);// [2, 3, 4, 5, 6, 7, 8]
  ```

  

* map()：对数组每一项运行给定函数，返回每次函数调用的结果组成的数组

  ```js
  var number = [1,2,3,4,5,6,7];
  var res = number.map(function(item, index, array) {
      return item*2;
  });
  console.log(res);//[2, 4, 6, 8, 10, 12, 14]
  ```

  

* some()：对数组每一项运行给定函数，如果对任意项返回true则返回true

  ```js
  var number = [1 ,2,3,4,5,6,7];
  var res = number.some(function(item, index, array){
      return (item > 2)
  });
  console.log(res);//true
  ```

  

### 归并方法

reduce()和reduceRight()迭代数组的所有项，构建一个最终返回的值。reduce从第一项到最后一项；reducerRight从最后一项到第一项

接收两个参数：每一项调用的函数和（可选）作为归并基础的初始值。函数接收四个参数：前一个值、当前值、项的索引、数组对象。这个函数返回的任何值都会作为第一个参数自动传给下一项。第一次迭代发生在第二项上。



