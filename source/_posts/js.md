---
title: js基础
date: 2022-2-11
categories: JS 
---

# ES6

ES5 只有两种声明变量的方法：`var`命令和`function`命令。ES6 除了添加`let`和`const`命令，还有另外两种声明变量的方法：`import`命令和`class`命令。所以，ES6 一共有 6 种声明变量的方法。

顶层对象，在浏览器环境指的是`window`对象，在 Node 指的是`global`对象。ES5 之中，顶层对象的属性与全局变量是等价的。ES6 为了改变这一点，一方面规定，为了保持兼容性，`var`命令和`function`命令声明的全局变量，依旧是顶层对象的属性；另一方面规定，`let`命令、`const`命令、`class`命令声明的全局变量，不属于顶层对象的属性。也就是说，从 ES6 开始，全局变量将逐步与顶层对象的属性脱钩。

## let、const和var

### 重复声明

var允许重复声明，let、const不允许，重复声明指的是：已经存在的变量或常量，又声明了一遍（**形参也算已经声明的变量**）

### 变量提升

var会提升变量的声明到作用域的顶部，但let和const不会

### 暂时性死区

只要作用域内存在let、const，它们所声明的变量或常量就会自动“绑定”这个区域，不再受外部作用域的影响；var没有暂时性死区

```js
let a=2
function func(){
    console.log(a) //报错，因为在func中定义了变量a，而let没有变量提升，弱func中无定义，则会向上找，a是2
    let a=1
}
func()
```

```js
let a=2
function func(){
    console.log(a) //undefined
    var a=1
}
func()
```

### window对象的属性和方法（全局作用域中）

全局作用域中，var声明的变量，通过function声明的函数，会自动变为window对象的变量，属性或方法，但const和let不会

### 块级作用域

var没有块级作用域，let和const有块级作用域

```js
for(var i=0;i<3；i++){
    console.log(i)
}
console.log(i) //3
```

```js
for(let i=0;i<3；i++){
    console.log(i)
}
console.log(i) //i is not defined
```

## 变量的解构赋值

ES6 允许按照一定模式，从数组和对象中提取值，对变量进行赋值，这被称为解构（Destructuring）。

### 1.数组的解构赋值

```javascript
let [a, b, c] = [1, 2, 3];
```

```javascript
let [head, ...tail] = [1, 2, 3, 4];
head // 1
tail // [2, 3, 4]

let [x, y, ...z] = ['a'];
x // "a"
y // undefined
z // []
```

如果等号的右边不是数组（或者严格地说，不是可遍历的结构），那么将会报错。

```javascript
// 报错
let [foo] = 1;
let [foo] = false;
let [foo] = NaN;
let [foo] = undefined;
let [foo] = null;
let [foo] = {};
```

上面的语句都会报错，因为等号右边的值，要么转为对象以后不具备 Iterator 接口（前五个表达式），要么本身就不具备 Iterator 接口（最后一个表达式）。

如果解构不成功，变量的值就等于`undefined`。

### 2.对象的解构赋值

```javascript
let { foo, bar } = { foo: 'aaa', bar: 'bbb' };
foo // "aaa"
bar // "bbb"
let { foo:fooNew, bar } = { foo: 'aaa', bar: 'bbb' };
fooNew //"aaa"  赋别名
```

对象的解构与数组有一个重要的不同。数组的元素是按次序排列的，变量的取值由它的位置决定；而对象的属性没有次序，变量必须与属性同名，才能取到正确的值。

如果解构不成功，变量的值就等于`undefined`。

注意，对象的解构赋值可以取到继承的属性。

```javascript
const obj1 = {};
const obj2 = { foo: 'bar' };
Object.setPrototypeOf(obj1, obj2);

const { foo } = obj1;
foo // "bar"
```

上面代码中，对象`obj1`的原型对象是`obj2`。`foo`属性不是`obj1`自身的属性，而是继承自`obj2`的属性，解构赋值可以取到这个属性。

注意：

```javascript
// 错误的写法
let x;
{x} = {x: 1};
// SyntaxError: syntax error
```

上面代码的写法会报错，因为 JavaScript 引擎会将`{x}`理解成一个代码块，从而发生语法错误。只有不将大括号写在行首，避免 JavaScript 将其解释为代码块，才能解决这个问题。

```javascript
// 正确的写法
let x;
({x} = {x: 1});
```

还有字符串的解构赋值、函数参数解构赋值、数值和布尔值解构赋值。

## 字符串

ES6 为字符串添加了遍历器接口，使得字符串可以被`for...of`循环遍历。这个遍历器最大的优点是可以识别大于`0xFFFF`的码点（Unicode)，传统的`for`循环无法识别这样的码点。

### 1.模板字符串

如果使用模板字符串表示多行字符串，所有的空格和缩进都会被保留在输出之中。

```javascript
`User ${user.name} is not authorized to do ${action}.`)
```

### 2.模板编译

### 3.标签模板

### 4.方法

#### includes(), startsWith(), endsWith() 

- **includes()**：返回布尔值，表示是否找到了参数字符串。
- **startsWith()**：返回布尔值，表示参数字符串是否在原字符串的头部。
- **endsWith()**：返回布尔值，表示参数字符串是否在原字符串的尾部。

这三个方法都支持第二个参数，表示开始搜索的位置。

```javascript
let s = 'Hello world!';

s.startsWith('world', 6) // true
s.endsWith('Hello', 5) // true
s.includes('Hello', 6) // false
```

上面代码表示，使用第二个参数`n`时，`endsWith`的行为与其他两个方法有所不同。它针对前`n`个字符，而其他两个方法针对从第`n`个位置直到字符串结束。

#### repeat

`repeat`方法返回一个新字符串，表示将原字符串重复`n`次。

```javascript
'x'.repeat(3) // "xxx"
'hello'.repeat(2) // "hellohello"
'na'.repeat(0) // ""
```

参数如果是小数，会被取整。

#### padStart()，padEnd() 

ES2017 引入了字符串补全长度的功能。如果某个字符串不够指定长度，会在头部或尾部补全。`padStart()`用于头部补全，`padEnd()`用于尾部补全。

```javascript
'x'.padStart(5, 'ab') // 'ababx'
'x'.padStart(4, 'ab') // 'abax'

'x'.padEnd(5, 'ab') // 'xabab'
'x'.padEnd(4, 'ab') // 'xaba'
```

上面代码中，`padStart()`和`padEnd()`一共接受两个参数，第一个参数是字符串补全生效的最大长度，第二个参数是用来补全的字符串。

如果原字符串的长度，等于或大于最大长度，则字符串补全不生效，返回原字符串。

```javascript
'xxx'.padStart(2, 'ab') // 'xxx'
'xxx'.padEnd(2, 'ab') // 'xxx'
```

如果用来补全的字符串与原字符串，两者的长度之和超过了最大长度，则会截去超出位数的补全字符串。

```javascript
'abc'.padStart(10, '0123456789')
// '0123456abc'
```

如果省略第二个参数，默认使用空格补全长度。

## Flat

Array.prototype.flat()用于将嵌套的数组“拉平”，变成一维数组。该方法返回一个新数组，对原数据没有影响。

```js
[1, 2, [3, 4]].flat(); //默认只展开一层
[1, 2, [3, [4, 5]]].flat(2);//展开两层
[1, [2, [3]]].flat(Infinity);//不管多少层，都会展开
//如果数组有空位，会跳过
[1, 2, , 4, 5].flat()
// [1, 2, 4, 5]
```

原理是递归

## null是Object?

null作为一个基本数据类型为什么会被typeof运算符识别为Object类型呢？是因为javascript中不同对象在底层都表示为二进制，而javascript中会把二进制的前三位都为0的判断为Object类型，而null的二进制表示全都是0，自然前三位也是0，所以执行typeof时会返回object，null被认为是对一个空对象的引用。

## js中this指针的改变

1.apply： 第一个参数是this指向的新目标，第二个参数接受一个数组，里面是要传递的参数，相当于arguments

```js
var obj = {
  name : 'tom',
}
function say(arr){
    console.log(this) // {name: 'tom'}
    console.log(arr) // one
}
say.apply(obj,['one']);
```

apply还有一个作用是，取数组中最大或最小值

```js
let arr=[1,2,3,4,5]
let max=Math.max.apply(Math,arr) // 5
let min=Math.min.apply(Math,arr) // 1
// math中求大小值
Math.max(1,2,3,4,5)
Math.min(1,2,3,4,5)
```

2.call： 第一个参数是this指向的新目标，第二个级以后的参数是要传递的参数，以逗号隔开

```js
say.call(obj,'one');
// this: {name: 'tom'}
// arr: one
```

3.bind： bind()方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。

```js
window.color='red';
var o={color:'blue'};
 
function sayColor(){ //原函数
console.log(this.color);
}
var objectSaycolor=sayColor.bind(o);
//var objectSaycolor=sayColor.bind(); //red
objectSaycolor();//blue
```

区别： apply和call会调用函数，并且改变函数内部指向；bind不会调用函数，会创建新函数

应用场景： call做继承；apply跟数组有关，做数组中最大最小值；bind不调用函数，却要改变函数的this指向，例如修改定时器中的this

```js
//vue中使用定时器
setTimeout(function() {
  console.log(this.total); 
  console.log(this);
}.bind(this), 300);//this始终指向window对象，需要通过bind来修改指向
// 或者使用箭头函数，默认this指向的是当前箭头函数定义位置的this，无法改变
setTimeout(() => {
  console.log(this.total);
  console.log(this);
}, 300);
```

## js判断数据类型的几种方法？

### 1.typeof 

适合判断基础数据类型，判断引用类型的话基本上返回的都是Object;

### 2.instanceof

instanceof检测的是原型，A instanceof B，如果A是B的实例，则返回true,否则返回false

```
{} instanceof Object
[] instanceof Array
```

### 3.Object.prototype.toString

toString是Object原型对象上的一个方法，该方法默认返回其调用者的具体类型，更严格的讲，是 toString运行时this指向的对象类型, 返回的类型格式为[object,xxx],xxx是具体的数据类型

```js
Object.prototype.toString.call(1); //[object Number]
Object.prototype.toString.call([]); //[object Array]
```

## js判断对象中是否有某属性

### 1.[]或.

通过点或者方括号可以获取对象的属性值，如果对象上不存在该属性，则会返回undefined。

```js
let obj ={name:'tom'}
obj.name //tom
obj[name] //tom
obj.age //undefined
```

不能用在x的属性值存在，但可能为 undefined的场景

### 2.in运算符

如果指定的属性在指定的对象或其原型链中，则in 运算符返回true。

```js
name in obj //true
age in obj //false
```

这种方式的局限性就是无法区分自身和原型链上的属性，在只需要判断自身属性是否存在时，这种方式就不适用了。这时需要hasOwnProperty()

### 3.hasOwnProperty()

```js
obj.hasOwnProperty('name') //true 自身属性
obj.hasOwnProperty('age') //false 不存在
obj.hasOwnProperty('toString') //false 原型链上属性
```

注意：for in循环会遍历原型链上的属性，为了解决此问题，可以通过hasOwnProperty判断是否是自身属性，再去操作。

## 删除对象中的某一属性

```js
var obj={
	name: 'zhagnsan',
	age: 19 
}
delete obj.name //true
typeof obj.name //undefined
```

## js判断数组中是否包含某个值

### 1.indexOf.(item,index)

此方法判断数组中是否存在某个值，如果存在，则返回数组元素的下标，否则返回-1。查找字符串最后出现的位置，使用 lastIndexOf() 方法。

```js
var fruits = ["Banana", "Orange", "Apple", "Mango"];
var a = fruits.indexOf("Apple"); // 2
//以上输出结果意味着 "Apple" 元素位于数组中的第 3 个位置。
var fruits=["Banana","Orange","Apple","Mango","Banana","Orange","Apple"];
var a = fruits.indexOf("Apple",4); //6
//以上输出结果意味在数组的第四个位置开始检索：
```

### 2.includes.(item,index)

此方法判断数组中是否存在某个值，如果存在返回true，否则返回false

```js
[1, 2, 3].includes(2); //true
[1, 2, 3].includes(4); //false
[1, 2, 3].includes(3, 3); //false
```

### 3.find()

返回数组中满足条件的第一个元素的值，如果没有，返回undefined

**注意**: find() 对于空数组，函数是不会执行的。

**注意:**  find() 并没有改变数组的原始值。

```js
[1, 5, 10, 15].find(function(value, index, arr) {
	return value > 9;
}) //10
```

### 4.findeIndex()

返回数组中满足条件的第一个元素的下标，如果没有找到，返回`-1`

注意点与find相同

```js
var ages = [3, 10, 18, 20];
ages.findIndex(function(value,index,arr){
	return value>=18;
}) //2
```

### 5.filter()

filter() 方法创建一个新的数组，新数组中的元素是通过检查指定数组中符合条件的所有元素。

**注意：**  filter() 不会对空数组进行检测。

**注意： ** filter() 不会改变原始数组。

```js
var ages = [3, 10, 18, 20];
const agesNew = ages.filter(function(value,index){
	return value>=18;
}) //[18,20]
```

### 6.some()

如果有一个元素满足条件，则表达式返回true , 剩余的元素不会再执行检测。如果没有满足条件的元素，则返回false。

**注意：** some() 不会对空数组进行检测。

**注意：** some() 不会改变原始数组。

```js
var ages = [3, 10, 18, 20];
const agesNew = ages.some(function(value,index){
	return value>=18;
}) //true
```

## js数组中删除某一个元素

### 1.delete方法

```js
var arr = ['a','b','c','d']; 
delete arr[1]; 
console.log(arr); //["a", undefined, "c", "d"] 数组长度不变，有一项为undefined
```

### 2.splice方法

```js
//删除起始下标为1，长度为1的一个值(len设置1，如果为0，则数组不变)
var arr = ['a','b','c','d'];
arr.splice(1,1);
console.log(arr); //['a','c','d']; 
rr.splice(1,1,f);
console.log(arr); //['a','f','d'];
```

```js
//删除指定元素而非指定位置
Array.prototype.remove = function(val) {
      var index = this.indexOf(val);
      if (index > -1) {
        this.splice(index, 1);
      }
};
var arr = ['a','b','c','d'];
arr.remove('a');
console.log(arr); //['b','c','d']; 
```

## 数组中常用的方法

1.push： 在数组的末尾添加一个或多个元素，并返回数组的新长度

2.pop： 删除数组末尾的元素，并返回数组的新长度。

3.unshift： 在数组的头部插入元素，,并返回数组的新长度

4.shift： 删除数组的头部元素，并返回删除的元素

5.concat： 数组和数组（或元素）的合并，返回新的数组，原数组不会改变

6.splice： 在数组中添加删除或替换元素，在任意位置添加或删除元素，返回删除或被替换的值，如果没有被删除或替换则返回空数组，splice()方法会修改原数组的值

```js
let arr=[1,2,3,4,5]；
let num1=arr.splice(1)
console.log(num1;arr)//num1=[2,3,4,5];arr=[1];

let num1=arr.splice(2,3)//删除从索引值2开始的3个元素
console.log(num1;arr);// num1=[3,4,5],arr=[1,2]

let num2=arr.splice(2,1,6,7,8);//从索引值2开始替换掉1个元素，并且插入6,7,8
//如果第二个值为0，则不替换，直接插入6,7,8;
console.log(num2;arr);//被替换的值num2=[3]; arr=[1,2,6,7,8,4,5]
```

7.slice： 截取复制数组指定位置的内容，slice(开始位置，结束位置)；第二个参数不写默认到尾部，只能从前往后截取；返回的值为截取到的内容形成的新数组；slice()方法不会更改到原数组的值

```js
let copyArr=arr.slice(); // slice()或者slice(0)都可以复制数组；
let arr=[1,2,3,4,5]；
let newArr=arr.slice(1,3);//截取索引1到索引3(不包括3)的值;
console.log(newArr,arr)；//newArr=[2,3];arr=[1,2,3,4,5];
```

8.join： 指定字符连接字符串，默认用逗号连接

9.sort： sort()将数组进行排序(升序)，返回新数组，原数组也会改变;

不传递参数时，有时会失效，原因是：js数组的sort方法总会以第一个字符的ASCII值来进行比较排序，也就是按字母顺序对数组中的元素进行排序。

如果想按照其他标准进行排序，就需要提供比较函数，该函数要比较两个值，然后返回一个用于说明这两个值的相对顺序的数字。比较函数应该具有两个参数 a 和 b，其返回值如下：

若 a 小于 b，在排序后的数组中 a 应该出现在 b 之前，则返回一个小于 0 的值。
若 a 等于 b，则返回 0。
若 a 大于 b，则返回一个大于 0 的值。

a-b输出从小到大排序，b-a输出从大到小排序

10.reverse： 可以将数组进行倒序，并返回新数组，原数组也会随之改变;

## 生成1-10的连续整数的数组

```js
// 用new Array生成一个长度是10的空数组
// 通过.key 返回带有数组下标索引的的 Array Iterator 对象
// Array.from 方法对一个类似数组（伪数组）或可迭代对象创建一个新的数组实例
// slice(0)只有一个参数，截取从0到数组末尾的数值
Array.from(new Array(9 + 1).keys()).slice(0)
```

## 迭代器和生成器

### 1.迭代器


​        迭代器是一种特殊对象，它具有一些专门为迭代过程设计的专有接口，所有的迭代器对象都有一个next()方法，每次调用都返回一个结果对象。结果对象有两个属性： 一个是value，表示下一个将要返回的值；另一个是done，它是一个布尔类型的值，当没有更多可返回数据时返回true。迭代器还会保存一个内部指针，用来指向当前集合中值的位置，每调用一次next()方法，都会返回下一个可用的值

　　如果在最后一个值返回后再调用next()方法，那么返回的对象中属性done的值为true，属性value则包含迭代器最终返回的值，这个返回值不是数据集的一部分，它与函数的返回值类似，是函数调用过程中最后一次给调用者传递信息的方法，如果没有相关数据则返回undefined

迭代数据结构中的内容，一般用在数组、set/Map集合、类似数组对象、字符串，主要供for...of使用

for...in只能遍历key值，还会遍历出原型的循环；而for...of可以遍历key,value

```js
//Symbol.iterator表示唯一的迭代器，诸如对象，一般不需要有顺序，所以对象本身没有迭代器 
// 数组
let arr = [11,22,33,44]
console.log(arr);
let iterator = arr[Symbol.iterator]()
console.log(iterator.next());
// set集合
let set = new Set([1,2,3,4])
let setIt=set[Symbol.iterator]()
console.log(setIt.next());
// Map集合
let map = new Map([["id",1],["name","zhangsan"]])
console.log(map);
// 字符串
let str="HelloWorld"
let strIt=str[Symbol.iterator]()
console.log(strIt.next());
```

### 2.生成器

生成器是一种返回迭代器的函数，通过function关键字后的星号(*)来表示，函数中会用到新的关键字yield。星号可以紧挨着function关键字，也可以在中间添加一个空格

#### 可解决异步（同时解决异步的还有promise和async）

```js
function * gen(){
  console.log('生成器函数进入了');
  yield console.log(1);
  yield console.log(2)
  yield console.log(3)
  yield console.log(4)
  yield console.log(5)
  return 5
}
let it=this.gen()
it.next() //1
it.next() //2
```

#### 异步加载实例

```js
// 延时加载任务的作用
function preLoad() {
  console.log('显示进度动画')
},
function loadingData() {
  let x = 0
  let id = setInterval(() => {
    console.log('加载数据中。。。' + ++x)
    if (x == 20) {
      clearInterval(id)
      it.next({ msg: 200 })
    }
  }, 100)
},
function loaded() {
  console.log('数据加载完毕，关闭动画')
},
function *loadData() {
  preLoad()
  let res = yield loadingData()
  if (res.msg == 200) {
    loaded()
  }
},
 let it = loadData()
 it.next() //显示进度动画  加载数据中。。。     数据加载完毕，关闭动画
```

#### yield返回值和传参

```js
function *test() {
  let num = 10
  let y = yield console.log(num + 10)
  console.log('genY:' + y)
  let z = yield console.log(y + 10)
  console.log('genz:' + z)
  return z
}
let it1 = test()
it1.next() //20   genY:undefined  因为程序从右向左执行，一个yield只能执行等号右部分，没有赋值，下一个next才是给等号左侧的赋值
it1.next(10) //20   genY:10    20
it1.next(30) //20   genY:10    20  genz:30
```

#### 生成器函数调生成器函数

```js
function *test1(){
      yield *test()
}
```

## **forEach不支持break**

forEach并不支持break操作，使用break会导致报错。forEach跳出循环结合try catch操作。

```js
let arr = [1, 2, 3, 4]；
arr.forEach((self,index) => {
    console.log(self);
    if (self === 2) {
        break; //报错
    };
});
```

forEach中使用return不会报错，但rerutn并不会生效。如果我们真的要用return返回某个值，那就只能将return操作放在函数中，而不是forEach循环中，像这样：

```js
function find(array, num) {
    let _index;
    array.forEach((self, index) => {
        if (self === num) {
            _index = index;
            //return index; 无效
        };
    });
    return _index;
};
```

注意：

```js
let arr = [1, 2];
arr.forEach((item, index) => {
    arr.splice(index, 1);
    console.log(1); //输出几次？
});
console.log(arr) //?
```

代码其实只会执行一次，数组也不会被删除干净，这是因为forEach在遍历跑完回调函数后，会隐性让index自增。当第一次遍历结束，此时数组为[2]而index变成了1，此时数组最大索引只是0，不满足条件，所以跳出了循环。

| 方法名   | break      | return       | continue     | 有无返回值 |
| -------- | ---------- | ------------ | ------------ | ---------- |
| for      | 跳出循环体 | 报错         | 结束本次循环 | 无         |
| forEach  | 报错       | 结束本次循环 | 报错         | 无         |
| map      | 报错       | 结束本次循环 | 报错         | 有         |
| for...in | 跳出循环体 | 报错         | 结束本次循环 | 无         |
| for..of  | 跳出循环体 | 报错         | 结束本次循环 | 无         |

## new对象发生了什么？

```
function Animate(name){
      this.name = name;
}
Animate.prototype.dance = function(){
      console.log(this.name + "在跳舞！");
}
var dog = new Animate('小白');
dog.dance();
```

1.创建了一个空对象obj{}，克隆一个 js 的 Object.prototype 对象dog

2.将Animate中的this关键字指向obj(dog)

3.将Animate的prototype原型指向obj原型，dog.proto=Animate.prototype，这样obj就拥有了Animate中的方法.

## 原型链

```js
class Person{
    constructor(x,y){
        this.x=x;
        this.y=y;
    }
}
let p=new Person(1,2)
console.log(p.__proto__===Person.prototype)  //true  _proto_是对象实例的隐式原型
console.log(p.z) //undefined
p.__proto__.z=5
console.log(p.z) // 5 加在了p的原型上
p.f=6
console.log(p.f) // 6 加在了p的这个对象上
//constructor指向的就是构造函数本身
console(Person==Person.prototype.constructor) //true
console.log(Person==p.__proto__.constructor) //true
```

{% asset_img image-20221102170434564.png 这是一张图片 %}

## 手写深拷贝

该函数对简单数据类型和引用数据类型都能实现深拷贝

```js
function copyObj(obj){
        var cloneObj;
        //当输入数据为简单数据类型时直接复制
        if(obj&&typeof obj!=='object'){cloneObj=obj;}
        //当输入数据为对象或数组时
        else if(obj&&typeof obj==='object'){
            //检测输入数据是数组还是对象
            cloneObj=Array.isArray(obj)?[]:{};
            for(let key in obj){
                if(obj.hasOwnProperty(key)){
                    if(obj[key]&&typeof obj[key]==='object') {
                        //若当前元素类型为对象时，递归调用
                        cloneObj[key] = copyObj(obj[key]);
                    }
                    //若当前元素类型为基本数据类型
                    else{cloneObj[key]=obj[key];}
                }

            }
        }
        return cloneObj;

  }
```

## JSON.parse(JSON.stringfy())

#### 弊端： 

用法简单，然而使用这种方法会有一些隐藏的坑：因为在序列化JavaScript对象时，所有函数和原型成员会被有意忽略。
通俗点说，`JSON.parse(JSON.stringfy(X))`，其中X只能是`Number`, `String`, `Boolean`, `Array`, 扁平对象，即那些能够被 JSON 直接表示的数据结构。

## 类数组对象arguments和数组对象

相同点： 

- 都可用下标访问每个元素
- 都有length属性

不同点： 

- 数组对象的类型是Array，类数组对象的类型是Object；

- 类数组对象不能直接调用数组API；

- 数组遍历可以用for in和for循环，类数组只能用for循环遍历；

类数组对象转换为数组对象的方法

### 1.Array**.**prototype**.**slice**.**call ( arguments )（ES5写法）

```
let newArray = Array.prototype.slice.call(array); //array是类数组对象
```

### 2.Array.from（ES6写法）

###### 注意： 

1.类数组对象必须具有length属性，用于指定数组的长度。如果没有length属性，那么转换后的数组是一个空数组。

2.类数组对象的属性名必须为数值型或字符串型的数字（该类数组对象的属性名可以加引号，也可以不加引号）

```js
let array = {
    0: 'name', 
    1: 'age',
    2: 'sex',　　　　//注意这些属性名的类型  是数值型
    3: ['user1','user2','user3'],
    'length': 4     //注意这里的length
}
let arr = Array.from(array )
console.log(arr) // ['name','age','sex',['user1','user2','user3']]
```

###### 其他功能

1.将Set解构的数据转换为数组

```js
let arr = [1,2,3,4,5,6,7,8,9]
let set = new Set(arr)
console.log(Array.from(set))  // [1,2,3,4,5,6,7,8,9]
```

2.Array.from还可以接受第二个参数，作用类似于数组的map方法，用来对每个元素进行处理，将处理后的值放入返回的数组

```js
let arr = [1,2,3,4,5,6,7,8,9]
let set = new Set(arr)
console.log(Array.from(set, item => item + 1)) // [2,3,4,5,6,7,8,9,10]
```

3.将字符串转换为数组

```js
let  str = 'hello world!';
console.log(Array.from(str)) // ["h", "e", "l", "l", "o", " ", "w", "o", "r", "l", "d", "!"]
```

### 3.扩展运算符

```
[...argument]
```

## 节流和防抖

### 防抖

在函数被触发的n秒内，如果又被触发，就会重新计时；

```js
const inp = document.querySelector('input');
inp.oninput = debounce(function() {
    // console.log(this);
    console.log(this.value)
}, 500);

function debounce(fn, delay) {
    let t = null;
    return function() {
        if (t !== null) {
            clearTimeout(t)
        }
        t = setTimeout(() => {
            fn.call(this);
        }, delay)
    }
}
```

应用场景： 

- 在input框中输入搜索内容的时候，浏览器不会马上就去执行，
- 手机号、邮箱输入验证；
- 窗口大小resize，只需要调整完成后，计算窗口大小，防止重复渲染

### 节流

每隔一段时间，只执行一次函数

```js
window.onscroll = throttle(function() {
        console.log(123);
    }, 500)

    function throttle(fn, delay) {
        let flag = true;
        return function() {
            if (flag) {
                setTimeout(() => {
                    fn.call(this);
                    flag = true;
                }, delay)
            }
            flag = false;
        }
    }
```

应用场景：

- 滚动加载，加载更多或者滚到底部监听
- 频繁点击按钮

## 闭包

http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html

```js
function f1(){
    var n=999;
    function f2(){
      alert(n);
    }
    return f2;
}
var result=f1();
result(); // 999
//f2函数就是闭包
```

闭包就是能够读取其他函数内部变量的函数。由于在Javascript语言中，只有函数内部的子函数才能读取局部变量，因此可以把闭包简单理解成"定义在一个函数内部的函数"。所以，在本质上，闭包就是将函数内部和函数外部连接起来的一座桥梁。

**闭包用途**

闭包可以用在许多地方。它的最大用处有两个，一个是前面提到的可以读取函数内部的变量，另一个就是让这些变量的值始终保持在内存中。

**使用闭包的注意点**

1）由于闭包会使得函数中的变量都被保存在内存中，内存消耗很大，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致内存泄露。解决方法是，在退出函数之前，将不使用的局部变量全部删除。

2）闭包会在父函数外部，改变父函数内部变量的值。所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），把内部变量当作它的私有属性（private value），这时一定要小心，不要随便改变父函数内部变量的值。

## 在浏览器中输入URL后发生的过程

整体流程： 

　　1. DNS域名解析

　　2. 建立TCP连接

　　3. 发送HTTP请求

　　4. 服务器处理请求

　　5. 返回响应结果

　　6. 关闭TCP连接

　　7. 浏览器解析HTML

　　8. 浏览器布局渲染

使用的协议： 

　　DNS、TCP、IP、OSPF(IP数据包在路由器中，路由选择协议)、ARP、HTTP

详细： 

　　1. **DNS域名解析**

　　　　在浏览器输入网址，其实就是要向服务器请求我们想要的页面内容，所以浏览器首先要确认的是域名所对应的服务器在哪里。

　　　　将域名解析成对应的服务器IP地址这项工作，是由DNS服务器来完成的。

　　　　客户端收到域名地址后，首先去找本地的hosts文件，检查在该文件中是否有相应的域名、IP对应关系，如果有，则向其IP地址发送请求，如果没有，再去找DNS服务器。

　　2. **建立TCP连接**

　　　　三次握手： 请求连接（SYN数据包），确认信息（SYN/ACK数据包），握手结束（ACK数据包）

　　3. **发起http请求**

　　　　与服务器建立了连接后，就可以向服务器发起请求了。

　　4. **服务器处理请求**

　　　　服务器端收到请求后的由web服务器（准确说应该是http服务器）处理请求。

　　　　web服务器解析用户请求，知道了需要调度哪些资源文件，再通过相应的这些资源文件处理用户请求和参数，并调用数据库信息，最后将结果通过web服务器返回给浏览器客户端。

　　5. **返回响应结果**

　　　　在http里，有请求就会有响应，哪怕是错误信息。

　　　　在响应结果中都会有个一个http状态码，如200、301、404、500等。通过这个状态码可以知道服务器端的处理是否正常，并能了解具体的错误。

　　6. **关闭TCP连接**

　　　　为了避免服务器与客户端双方的资源占用和损耗，当双方没有请求或响应传递时，任意一方都可以发起关闭请求。四次挥手。

　　7. **浏览器解析HTML**

　　　　浏览器需要加载解析的不仅仅是HTML，还包括CSS、JS。以及还要加载图片、视频等其他媒体资源。

　　　　浏览器通过解析HTML，生成DOM树，解析CSS，生成CSS规则树，然后通过DOM树和CSS规则树生成渲染树。

　　　　渲染树与DOM树不同，渲染树中并没有head、display为none等不必显示的节点。

　　8. **浏览器布局渲染**

　　　　根据渲染树布局，计算CSS样式，即每个节点在页面中的大小和位置等几何信息。

　　　　HTML默认是流式布局的，CSS和js会打破这种布局，改变DOM的外观样式以及大小和位置。

## 页面渲染

浏览器渲染页面的一般过程： 

1.浏览器解析html源码，然后创建一个 DOM树。并行请求 css/image/js在DOM树中，每一个HTML标签都有一个对应的节点，并且每一个文本也都会有一个对应的文本节点。DOM树的根节点就是 documentElement，对应的是html标签。

2.浏览器解析CSS代码，计算出最终的样式数据。构建CSSOM树。对CSS代码中非法的语法它会直接忽略掉。解析CSS的时候会按照如下顺序来定义优先级：浏览器默认设置 < 用户设置 < 外链样式 < 内联样式 < html中的style。

3.DOM Tree + CSSOM --> 渲染树（rendering tree）。渲染树和DOM树有点像，但是是有区别的。

DOM树完全和html标签一一对应，但是渲染树会忽略掉不需要渲染的元素，比如head、display:none的元素等。而且一大段文本中的每一个行在渲染树中都是独立的一个节点。渲染树中的每一个节点都存储有对应的css属性。

4.一旦渲染树创建好了，浏览器就可以根据渲染树直接把页面绘制到屏幕上。

以上四个步骤并不是一次性顺序完成的。如果DOM或者CSSOM被修改，以上过程会被重复执行。实际上，CSS和JavaScript往往会多次修改DOM或者CSSOM。

## js阻塞页面渲染

虽然js都会阻塞DOM解析，但是浏览器对于内联script和外联script的渲染过程还是有一点点不同。内联js会阻塞DOM解析和渲染，直到js执行完成后，页面才会被渲染出来。外联js也会阻塞DOM解析和渲染，但是如果在外联script标签之前已经有DOM元素生成，则浏览器会优先渲染一次。我想这是因为浏览器不知道脚本的内容，因而碰到脚本时，只好先渲染页面，确保脚本能获取到最新的`DOM`元素信息，尽管脚本可能不需要这些信息。

https://www.cnblogs.com/FHC1994/p/13162696.html

### 不阻塞浏览器渲染文档的方法

script标签有两个属性，一个是defer（翻译为延迟），二是asyn（翻译为异步，没错，和我们的常见的ajax和axios的异步是一个意思），这两个属性都可以改变脚本的加载执行方式

延迟： 延迟是指当浏览器解析到script脚本时，会继续向下载入和解析文档，等到文档载入解析完成且可以操作文档时，才开始执行脚本。仅仅IE浏览器支持defer属性，而async属性则被ie10+和其他现代浏览器支持。

异步： 异步是指当浏览器解析到script脚本时，会立刻下载和执行脚本，但同时浏览器解析器也会继续向下解析渲染文档，不会造成阻塞的现象。这使得脚本可以尽快的被载入执行，这很想我们前端调用post异步接口，并不影响文档的正常解析渲染。HTML5中的新属性，仅适用于外部脚本

**注意**： 使用defer属性的脚本，当有多个的时候，这些脚本会按照他们在文档中排列的顺序载入执行。而使用asyn属性的脚本，当有多个的时候，会顺序开始触发载入，但谁先完成载入就谁先执行，这就是说，他们的执行顺序可能是无序不确定的。在有的时候知道这点很重要，因为这可能涉及一些有强制顺序的逻辑处理。

- 一个script标签同时使用了async和defer，则执行async，忽略defer。

## 跨域

跨域，指的是浏览器不能执行其他网站的脚本。它是由浏览器的同源策略造成的，是浏览器对[JavaScript](http://lib.csdn.net/base/javascript)施加的安全限制。所谓同源是指，域名，协议，端口均相同（域名和域名对应的IP地址也是跨域）。

1.jsonp解决跨域

首先，我们假定向后端请求的地址是’https://www.baidu.com/s?wd=jsonp&cb=show’,并且知道(或约定)后端返回的数据是’show({wd: “jsonp”,p: false,s:     [“jsonp”,“jsonp跨域”,“jsonp实现”]})’

- 那么我们就需要定义一个show方法

```html
<script type="text/javascript">
  function show(data){
   console.log(data);//这就是我们需要的数据
  }

 </script>
```

- 然后，我们要准备script标签，并且src为上述假定的url,并且按需插入这个script

```html
 <script type="text/javascript">
  function show(data){
   console.log(data);//这就是我们需要的数据
  }

  let srcipt = document.createElement('script');
  script.src = 'https://www.baidu.com/s?wd=jsonp&cb=show';
  document.body.append(script);//一插入即请求，请求就相当于执行了show这个函数，那么show函数是上面定义的，所以就得到了data
 </script>文出处链接及本声明。
原文链接：https://blog.csdn.net/qq_17175013/article/details/88984206
```

2.localStorage会跟cookie一样受到跨域的限制，会被document.domain影响

```html
document.domain用来得到当前网页的域名。
比如在百度（https://www.baidu.com）页面控制台中输入：
alert(document.domain);              //"www.baidu.com"

我们也可以给document.domain属性赋值，不过是有限制的，你只能赋成当前的域名或者一级域名。
比如：
alert(document.domain = "baidu.com");     //"baidu.com"
alert(document.domain = "www.baidu.com"); //"www.baidu.com"
上面的赋值都是成功的，因为www.baidu.com是当前的域名，而baidu.com是一级域名。

但是下面的赋值就会出来"参数无效"的错误：
比如：
alert(document.domain = "qq.com");     //参数无效  报错
alert(document.domain = "www.qq.com"); //参数无效  报错
因为qq.com与baidu.com的一级域名不相同，所以会有错误出现。
这是为了防止有人恶意修改document.domain来实现跨域偷取数据。

利用document.domain 实现跨域
前提条件：这两个域名必须属于同一个一级域名!而且所用的协议，端口都要一致，否则无法利用document.domain进行跨域。
例如：
news.baidu.com下的news.html页面：
<script>
    document.domain = 'baidu.com';
    var ifr = document.createElement('iframe');
    ifr.src = 'map.baidu.com/map.html';
    ifr.style.display = 'none';
    document.body.appendChild(ifr);
    ifr.onload = function(){
        var doc = ifr.contentDocument || ifr.contentWindow.document;
        // 这里可以操作map.baidu.com下的map.html页面
        var oUl = doc.getElementById('ul1');
        alert(oUl.innerHTML);
        ifr.onload = null;
    };
</script>

map.baidu.com下的map.html页面：
<ul id="ul1">我是map.baidu.com中的ul</ul>
<script>
    document.domain = 'baidu.com';
</script>
```

3.跨域存取localstorage

采用的是postMessage和iframe相结合的方法

https://www.jianshu.com/p/e86d92aeae69

4.CORS(跨域资源共享)是一种新标准，支持同源通信，也支持跨域通信。fetch是实现CORS通信的

```js
//解决跨域带上cookies
1.后端添加@CrossOrigin注解
2.前端设置允许发送cookie
$.ajax({
    url: url,
    type: 'post',
    data: JSON.stringify(obj),
    //加上 xhrFields及crossDomain
    xhrFields: { 
        withCredentials: true//允许带上cookies
    },
    crossDomain: true,

    success:function(res){
    },
    error:function(){
    }
});

```

## 如何计算一个html页面有多少种标签

这道题看似简单，但是是一个很有价值的一道题目。它包含了很多重要的知识：

- 如何获取所有DOM节点
- 伪数组如何转为数组
- 去重

那么针对这道题目我们来解答一下，就拿当前页面为例吧。

解答
1.获取所有的DOM节点。

```js
document.querySelectorAll('*')
```

此时得到的是一个NodeList集合，我们需要将其转化为数组，然后对其筛选。

2.转化为数组

```
[...document.querySelectorAll('*')]
```

一个拓展运算符就轻松搞定。

3.获取数组每个元素的标签名

```
[...document.querySelectorAll('*')].map(ele => ele.tagName)
```

使用一个map方法，将我们需要的结果映射到一个新数组。

4.去重

```
new Set([...document.querySelectorAll('*')].map(ele=> ele.tagName)).size
```


我们使用ES6中的Set对象，把数组作为构造函数的参数，就实现了去重，再使用Set对象的size方法就可以得到有多少种HTML元素了。

## **sessionStorage、localStorage和cookie**

共同点： 都是保存在浏览器端、且同源的 
区别：  
1、cookie数据始终在同源的http请求中携带（即使不需要），即cookie在浏览器和服务器间来回传递，而sessionStorage和localStorage不会自动把数据发送给服务器，仅在本地保存。cookie数据还有路径（path）的概念，可以限制cookie只属于某个路径下 
2、存储大小限制也不同，cookie数据不能超过4K，同时因为每次http请求都会携带cookie、所以cookie只适合保存很小的数据，如会话标识。sessionStorage和localStorage虽然也有存储大小的限制，但比cookie大得多，可以达到5M或更大 
3、数据有效期不同，sessionStorage： 仅在当前浏览器窗口关闭之前有效；localStorage： 始终有效，窗口或浏览器关闭也一直保存，因此用作持久数据；cookie： 只在设置的cookie过期时间之前有效，即使窗口关闭或浏览器关闭 ，默认是在浏览器关闭后失效。
4、作用域不同，sessionStorage不在不同的浏览器窗口中共享，即使是同一个页面；localstorage在所有同源窗口中都是共享的；cookie也是在所有同源窗口中都是共享的 

## BFC块级格式化上下文

它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。

## 修改Vuex中的state

直接修改state时，store中的state能够改变，并且是响应式的，并没有报错。跟commit提交mutation的方式没啥区别。

开启严格模式，仅需在创建 store 的时候传入 strict: true；在严格模式下，无论何时发生了状态变更且不是由 mutation 函数引起的，将会抛出错误。这能保证所有的状态变更都能被调试工具跟踪到。

于是，将vuex设置成了严格模式。直接修改state发现控制台确实是报出了错误，但是state修改成功，并且依然是响应式的。错误提示：

```
Do not mutate vuex store state outside mutation handlers.
```

通过commit 提交 mutation 的方式来修改 state 时，vue的调试工具能够记录每一次state的变化，这样方便调试。但是如果是直接修改state，则没有这个记录。

## 事件模型

三种事件模型 ： 原始事件模型（DOM0），DOM2事件模型，IE事件模型；

#### 不同点： 

1.事件程序的注册

2.事件传播的过程

### 一、原始事件模型

1.特点： 

事件类型上面有ON（onclick）

没有事件的传播(事件一旦发生就立刻调用事件句柄)

一个DOM对象只能注册一个类型的事件，如果注册了两个，则会发生覆盖，只执行后一个事件；

2.注册

```html
1.将JS代码作为HTML的性质（也就是直接在标签中将HTML元素的性质设置为一段代码）
<input type="button" value="Press me" onclick="alert('thanks');" /> 
2.将事件处理程序作为js对象的属性
<input type="button" id="mybtn">

let btn = document.getElementById('mybtn');
btn.onclick=function(event){
    console.log('啦啦啦')
}
```

3.解除

```
document.getElementById('mybtn').onclick = null;
```

4.阻止默认动作：事件处理程序可以通过返回false来阻止浏览器的默认行为

### 二、Dom2事件模型(IE8以下不支持)

1.特点： 有一个事件传播的过程

事件捕获： 事件由document对象一直向下捕捉到目标元素

事件执行： 目标对象的事件处理程序执行

事件冒泡： 事件从目标元素上升到document

所有事件类型都会经历第一阶段，有的事件不会经历第三阶段

2.特点： 一个dom对象可以注册多个相同类型的事件，不会发生事件的覆盖，会依次的执行各个事件函数。

3.注册： [object].addEventListener('事件名称'，方法名（函数），事件模型（true/false）)

4.解除绑定： [object].removeEventListener('事件名称'，方法名（函数），事件模型（true/false）)

 **true/false决定在那个阶段调用函数 **   true: 捕获               false: 冒泡

5.停止传播： event.stopPropagation()

```js
var click = document.getElementById('inner');
var clickouter = document.getElementById('outer');
click.addEventListener('click',function(event){
    alert('inner show');
    event.stopPropagation();
},false);
clickouter.addEventListener('click',function(){
    alert('outer show');
},false);
```

阻止默认动作： event.preventDefault()

 **由于事件捕获阶段没有可以阻止事件的函数，所以一般都是设置为事件冒泡。**

###  三、IE事件模型

1.特点： Event对象不是事件处理程序的函数参数，而是window的全局变量

2.事件传播过程只有冒泡阶段

3.注册： [object].attachEvent("onclick",click1)

4.解除： [object].detachEvent("onclick",click1)

5.停止传播： window.ecent.cancelBubble=true;

## 回流和重绘

重绘(repaint)： 当**元素样式的改变不影响页面布局时**，比如元素的**颜色**，浏览器将对元素进行的更新，称之为重绘。

回流(reflow)： 也叫做**重排**。当**元素的尺寸或者位置发生了变化**，就需要重新计算渲染树，这就是回流，比如元素的**宽高**、**位置**，浏览器会重新渲染页面，称为回流，又叫重排（layout）。

**关系： 回流必定会触发重绘，重绘不一定会触发回流。重绘的开销较小，回流的代价较高**。

https://blog.csdn.net/yiyueqinghui/article/details/107233565

## js事件循环

事件循环(event-loop)： 先同步再异步，异步中先微任务，在宏任务

宏任务： setTimeout，setInterval

微任务： Promise.then/catch，process.nextTick

https://www.cnblogs.com/hyns/p/12392249.html

## js中this指针的改变

1.apply： 第一个参数是this指向的新目标，第二个参数接受一个数组，里面是要传递的参数，相当于arguments

```js
var obj = {
  name : 'tom',
  say : function(){
    console.log(this.name);
  }
}
say.spply(obj,['one','two']);//tom one two  效果一样
```

2.call： 第一个参数是this指向的新目标，第二个级以后的参数是要传递的参数，以逗号隔开

```js
say.call(obj,'one','two');//tom one two
```

3.bind： bind()方法会创建一个新函数，称为绑定函数，当调用这个绑定函数时，绑定函数会以创建它时传入 bind()方法的第一个参数作为 this，传入 bind() 方法的第二个以及以后的参数加上绑定函数运行时本身的参数按照顺序作为原函数的参数来调用原函数。

```js
window.color='red';
var o={color:'blue'};
 
function sayColor(){
console.log(this.color);
}
var objectSaycolor=sayColor.bind(o);
//var objectSaycolor=sayColor.bind();
objectSaycolor();//blue
```

## promise.all某一项执行失败

promise.all该方法是用于将多个promise实例，包装成一个新的promise实例，适用于多个异步任务都完成之后才执行某个回调。

```js
let p1 = new Promise((resolve, reject) => {
  resolve('成功了')
})

let p2 = new Promise((resolve, reject) => {
  resolve('success')
})

let p3 = Promise.reject('失败')

Promise.all([p1, p2]).then((result) => {
  console.log(result)               //['成功了', 'success']
}).catch((error) => {
  console.log(error)
})

Promise.all([p1,p3,p2]).then((result) => {
  console.log(result)
}).catch((error) => {
  console.log(error)      // 失败了，打出 '失败'
})
const p = Promise.all([p1,p3,p2])
```

（1）只有p1、p2、p3的状态都变成fulfilled，p的状态才会变成fulfilled，此时p1、p2、p3的返回值组成一个数组，传递给p的回调函数。

（2）只要p1、p2、p3之中有一个被rejected，p的状态就变成rejected，此时第一个被reject的实例的返回值，会传递给p的回调函数。

## async/await和promise的区别

promise是ES6为实现异步回调，async/await是ES7的语法，写起来更像是同步代码，看起来比较简洁。async的本质是promise，async函数返回的是一个promise对象，那么async也可以通过.then调用，而async里return的值将会被当作参数传递给then回调。如果没有return语句，参数会赋值为undefined。

```js
async function getAsyncOreder(){
    console.log(1)
    console.log(2)
    return 'then函数的参数'
}
getAsyncOreder().then(res=>{
    console.log(res)
})
console.log(3)
打印顺序：1  2  3  then函数的参数
```

await： 相当于promise中的then

```js
function asyncTime(x){
    return new Promise((resolve)=>{
        setTimeout(()=>{
            resolve(x)
        },3000)
    })
}
async function asyncTest(){
    console.log(3)
    let result = await asyncTime(4)
    console.log(result)
    return '执行完成'
}
console.log(1)
asyncTest()
console.log(2)
打印顺序：1  3  2  4
```

await会阻塞后边代码的执行，函数遇到await会中断async的执行，也就是说asyncTest的控制权交给了asyncTime，所以代码的作用域从asyncTest跳到了全局作用域window，所以执行顺序变成了 2   4。

async函数是Generator 函数的语法糖 ，async函数就是将 Generator 函数的星号（*）替换成async，将yield替换成await；进一步说就是async函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而await命令就是内部then命令的语法糖。

## 模态框拖拽

js鼠标移动事件：onmousemove

js鼠标点击：onmousedown

js鼠标松开：onmouseup

js鼠标移出：onmouseout

## 继承方式

实现继承，要先有一个父类，如

```js
// 定义一个动物类
function Animal (name) {
  // 属性
  this.name = name || 'Animal';
  // 实例方法
  this.sleep = function(){
    console.log(this.name + '正在睡觉！');
  }
}
// 原型方法
Animal.prototype.eat = function(food) {
  console.log(this.name + '正在吃：' + food);
};
```

### 1.原型链继承

核心：将父类的实例作为子类的原型

```js
function Cat(){
    age:2
}
Cat.prototype.name = 'cat';
Cat.prototype = new Animal(); //Cat通过prototype指向Animal函数
Cat.prototype.constructor = Cat

//　Test Code
var cat = new Cat();
console.log(cat.name); //Animal
console.log(cat.eat('fish')); //Animal正在吃：fish
console.log(cat.sleep()); //Animal正在睡觉！
console.log(cat.age); //undefined
console.log(cat instanceof Animal); //true 
console.log(cat instanceof Cat); //true

//改造后
//必须将要为子类新增的属性和方法放在new Animal()后
Cat.prototype = new Animal(); //Cat通过prototype指向Animal函数
Cat.prototype.constructor = Cat
Cat.prototype.name = 'cat';
Cat.prototype.age = 2;

var cat = new Cat();
console.log(cat.name); //cat
console.log(cat.eat('fish')); //cat正在吃：fish
console.log(cat.sleep()); //cat正在睡觉！
console.log(cat.age); //2
```

特点：

1. 非常纯粹的继承关系，实例是子类的实例，也是父类的实例
2. 父类新增原型方法/原型属性，子类都能访问到
3. 简单，易于实现

缺点：

1. 要想为子类新增属性和方法，必须要在`new Animal()`这样的语句之后执行，不能放到构造器中
2. 无法实现多继承
3. 来自原型对象的所有属性被所有实例共享
4. 创建子类实例时，无法向父类构造函数传参

### 2.构造继承

构造函数继承的基本思想就是利用call或者apply把父类中通过this指定的属性和方法复制（借用）到子类创建的实例中。因为this对象是在运行时基于函数的执行环境绑定的。在全局中，this等于window，而当函数被作为某个对象的方法调用时，this等于那个对象。call 、apply方法可以用来代替另一个对象调用一个方法。

创建了一个新的实例对象，并且执行SubType里面的代码，而SubType里面用call调用了SuperTyep，也就是说把this指向改成了指向新的实例，所以就会把SuperType里面的this相关属性和方法赋值到新的实例上，而不是赋值到SupType上面。所有实例中就拥有了父类定义的这些this的属性和方法。

```js
function SuperType(){
 this.colors = ["red", "blue", "green"];
}
function SubType(){
  //继承了SuperType
 SuperType.call(this);
}
 
var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"
var instance2 = new SubType();
alert(instance2.colors); //"red,blue,green" 
```

### 3.组合继承

组合继承有时候也叫伪经典继承，指的是将[原型链](https://so.csdn.net/so/search?q=原型链&spm=1001.2101.3001.7020)和借用构造函数技术组合到一块，从而发挥二者之长的一种继承模式，其背后的思路是使用原型链实现对原型属性和方法的继承，而通过借用构造函数来实现对实例属性的继承。这样既通过在原型上定义方法实现了函数复用，又能保证每个实例都有它的自己的属性。

```js
function SuperType(name){
 this.name=name;
 this.colors=["red","blue","green"];
}
SuperType.prototype.sayName=function(){
 console.log(this.name);
}
function SubType(name,age){
 SuperType.call(this,name);
 this.age=age;
}
SubType.prototype=new SuperType(); //原型上继承了SuperType的本身属性和原型
SubType.prototype.constructor=SubType;
SubType.prototype.sayAge=function(){
 console.log(this.age);
}
var instance1=new SubType("zxf",24);
instance1.colors.push("black");
console.log(instance1.colors);//["red","blue","green","black"]
instance1.sayName();//"zxf"
instance1.sayAge();//24
var instance2=new SubType("jay",36);
console.log(instance2.colors);//["red","blue","green"]
instance2.sayName();//"jay"
instance2.sayAge();//36
```

**基本思想**：supertype构造函数定义了两个属性，name和colors。supertype的原型定义了一个方法sayname()。subtype构造函数调用supertype时传入了name参数，紧接着又定义了它自己的属性age。然后将supertype的实例赋值给subtype的原型，然后又在该新原型上定义了方法sayage()。这样一来，就可以让两个不同的subtype实例既可以拥有属性--包括colors属性，又可以使用相同的方法。
**缺点**：调用了两次supertype构造函数，一次在赋值subtype的原型时，一次在实例化子类时call调用，这次调用会屏蔽原型中的两个同名属性。

### 4.原型式继承

原型继承并没有使用严格意义上的构造函数，是通过借助原型基于已有的对象创建的新对象，同时还不必创建自定义类型。使用原型继承的思路：

```js
// 在Object函数的内部先创建了一个临时性的构造函数，然后将传入的对象作为这个构造函数的原型，最后返回了这个临时类型的一个新实例
function Object(o){
    function F(){}
    F.prototype=o
    return new F()
}
```

```js
//代码1：
var person={
name:"Tom",
friends:["111","222","333"]
}
 
var anotherPerson=Object(person);
anotherPerson.name="Greg";
anotherPerson.friends.push("aaa");
 
var yetAnotherPerson=Object(person);
yetAnotherPerson.name="Bob";
yetAnotherPerson.friends.push("000");
 
alert(person.friends); //111,222,333,aaa,000
```

克洛克福德主张这种原型式继承，要求你必须有一个对象可以作为另一个对象的基础。在代码1中，可以作为另一个对象的基础的是person对象，于是我们可以把它传入到object()函数中，然后该函数就会返回一个新对象

#### **Object.create( )**

ECMAScript5新增Object.create( )方法规范了原型式继承。该方法接收了两个参数：一个用作新对象原型的对象和(可选)一个为新对象定义额外属性的对象。在传入一个参数的情况下Object.create( )和object( )方法的行为相同。如果传入两个参数，则Object.create( )的第二个参数与Object.defineProperties( )方法的第二个参数的格式相同：每个属性都是通过自己的描述符定义的。

```js
var anotherPerson=Object(person);   =>   var anotherPerson=Object.create(person);
```

### 5.寄生式继承

寄生式继承就是创建一个仅用于封装继承过程的函数，该函数在内部以某种方式来增强对象，后再像真的是它做了所有工作一样返回对象。

```js
function createAnother(o){
    var clone = Object(o) //通过调用上边说的Object函数创建新的对象
    clone.sayHi=function(){
    //以某种方式来增强对象
    alert('hi')
    }
    return clone
}
```

createAnother()函数接收了一个参数，也就是将要作为新对象基础的对象。然后，把这个对象（original）传递给 object()函数，将返回的结果赋值给 clone。再为 clone 对象 添加一个新方法 sayHi()，后返回 clone 对象。可以像下面这样来使用 createAnother()函数：

```js
var person = {     
name: "Nicholas",     
friends: ["Shelby", "Court", "Van"] 
}; 
 
var anotherPerson = createAnother(person); 
anotherPerson.sayHi(); //"hi" 
```

### 6.寄生组合继承

所谓寄生组合式继承，即通过借用[构造函数](https://so.csdn.net/so/search?q=构造函数&spm=1001.2101.3001.7020)来继承属性，通过原型链的混成形式来继承方法。其背后的基本思路是：不必为了指定子类型的原型而调用超类型的构造函数，我们所需要的无非就是超类型原型的一个副本而已。本质上，就是使用寄生式继承来继承超类型的原型，然后再将结果指定给子类型的原型。

一、定义父类

```js
function Person(name){
    this.category = 'human';
    this.legNum = 2;
    this.name = name;
}
 
Person.prototype.sayHello = function(){
    console.log('Hi,i am ' + this.name);
}
```

二、定义继承方法

```js
function inherit(subType,superType){
    //在new inheritFn 的时候将构造函数指向子类
    function inheritFn(){this.constructor = subType}
    inheritFn.prototype = superType.prototype;  // 不写的话subType.prototype指向的是inheritFn
    //将子类的原型指向父类原型的一个副本
    subType.prototype = new inheritFn();
}
```

三、定义子类并实现继承

```js
//定义子类构造函数Pan
function Pan(name,age){
    Person.call(this,name);  //借用构造函数
    this.age = age;
}
 
//将子类Pan的原型指向父类Person原型的一个副本
//注意：要执行该动作后才能在Pan的prototype上定义方法，否则没用
inherit(Pan,Person); 
 
Pan.prototype.sayAge = function(){
    console.log(this.age);
}
 
//定义子类构造函数Duan
function Duan(name,hairColor){
    Person.call(this,name);
    this.hairColor = hairColor;
}
 
inherit(Duan,Person);
 
Duan.prototype.showHairColor = function(){
    console.log(this.hairColor);
}
```

四、结果输出

```js
//Pan的实例
var pan = new Pan('panfengshan',27);
console.log(pan.name); //panfengshan
console.log(pan.age); //27
console.log(pan.category); //human
console.log(pan.legNum); //2
 
pan.sayHello(); //Hi,i am panfengshan
pan.sayAge(); //27
 
//Duan的实例
var duan = new Duan('duanyanan','black');
console.log(duan.name); //duanyanan
console.log(duan.hairColor); //black
console.log(pan.category); //human
console.log(pan.legNum); //2
 
duan.sayHello(); //Hi,i am duanyanan
duan.showHairColor(); //black
```

寄生组合式继承的高效体现在它只调用了一次Person构造函数，并且因此避免了在Pan.prototype上面创建不必要的、多余属性。与此同时，[原型链](https://so.csdn.net/so/search?q=原型链&spm=1001.2101.3001.7020)还能保持不变；因此，还能正常使用instanceof和isPropertyOf()。开发人员普遍认为寄生组合式继承是引用类型最理想的继承范式。

### 7.ES6 class类继承

ES6引入了Class语法糖，使得Js继承更像面向对象语言的写法。Class可以通过extends关键字来实现继承，这比ES5的通过修改原型链继承，要清晰和方便很多。

```js
class Father{
}
class Son extends Father{
}
//由于两个类里边什么都没写，相当于Son是复制了Father，两个类完全一样
```

子类必须在constructor中调用super()，否则新建实例会报错，这是因为子类没有自己的this对象，而是继承父类的this对象，然后对其加工，如果不调用super()，子类就得不到this对象。

```js
class Father{
}
class Son extends Father{
    constructor(){}
}
let s=new Son()
//referenceError : this is not defined
```

ES5的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上（Parent.apply(this)）,ES6的继承机制完全不同，实质是先创造父类的实例对象this（所以必须先调用super方法）,然后再用子类的构造函数修改this；
如果子类没有定义constructor方法，这个方法会默认添加，也就是说，不管有没有显式定义，任何一个子类都有constructor方法。constructor中的属性和方法是实例对象上的，其外的是类原型上的。

```js
class Father {
   constructor (x,y) {
      this.x= x;
      this.y = y;
    }
}  
class Son extends Father {
   constructor (x, y, color) {
       this.color =color ;//ReferenceError : this is not defined
       super(x,y);
       this.color = color;//正确
      }

}
```

**super关键字**

既可以当函数使用，也可以当对象使用

（1）当对函数使用，代表的是父类的构造函数，ES6要求，子类的构造函数必须执行一个super()函数。super虽然代表了父类的构造函数，但是返回的是子类Son的实例，即super内部的this指向 的是Son，因此super()在这里相当于Father.constructor.call(this)。作为函数使用时，super()只能用在子类的构造器中，在其它地方会报错。

```js
class A {
     constructor (){
        console.log(new.target.name);
      }
 }
class B extends A {
   constructor () {
      super();
     }
 }
 new A()//A
 new B()//B
//new.target指向当前正在执行的函数，在super()执行时，他指向的是子类B的构造函数，而不是父类A的构造函数，super()内部的this指向的是B；
```

（2）作为对象使用时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类

**普通方法**

```js
// super指向父类原型
//定义在了父类的原型上的方法
class Father{
    getName(){
        return 'Bob'
    }
}
class Son extends Father{
    constructor(){
        super()
        console.log(super.getName()) //Bob
    }
}
let s =new Son()
//定义在父类实例（非原型上）的属性和方法是无法访问的
class Father{
    constructor(){
        this.p=2
    }
}
class Son extends Father{
    get m(){
        return super.p
    }
}
let s = new Son();
s.m  //undefined
//定义在原型的变量
class Father{
}
Father.prototype.p=2
class Son extends Father{
    get m(){
        return super.p
    }
}
let s = new Son();
s.m  //2
```

ES6 规定，通过super调用父类的方法时，super会绑定子类的this

```js
class Father {
   constructor () {
      this.x =1;//这个this指向的是Father对象的实例
   }
   print () {
      console.log(this.x);
   }
}
class Son extends Father {
   constructor () {
       super();
       this.x = 2;//这个this指向的是Son对象的实例
   }
     m() {
      super.print();   
     }
}
let s = new Son();
s.m(); //2
```

super.print()虽然调用的是Father.prototype.print()，但是Father.prototype.print()会绑定子类Son的this，导致输出的是2，而不是1，也就是说，实际上执行的是 super.print.call(this)。

**静态方法**

```js
class Parent {
    //使用static声明的方法是在 Parent实例对象的原型的constructor上
      static myMethod (msg) {
           console.log("static",msg);
        }
    //普通方法声明的是在 Parent实例对象的原型
      myMethod (msg) {
          console.log("instance" ,msg);
        }
}
let p=new Parent()
console.log(Parent.myMethod(1)) // static 1
console.log(p.__proto__.constructor.myMethod(1)) // static 1
console.log(p.__proto__.myMethod(1)) // instance 1

class Child extends Parent {
     static myMethod(msg) {
          super.myMethod(msg);
     }
      myMethod (msg) {
          super.myMethod(msg);
      }
 }
Child.myMethod(1);
//static 1
var child = new Child();
child.myMethod(2);
//instance 2
```

super在静态方法之中指向父类，在普通方法之中指向父类的原型对象。使用super的时候，必须显式指定是作为函数、还是作为对象使用，否则会报错。

**类的prototype属性和proto属性**

大多数浏览器的ES5实现之中，每一个对象都有**proto**属性，指向对应的构造函数的prototype属性，class作为构造函数的[语法糖](https://so.csdn.net/so/search?q=语法糖&spm=1001.2101.3001.7020)，同时有prototype属性和**proto**属性，因此同时存在两条继承链；
（1）子类的**proto**属性，表示构造函数的继承，总是指向父类；
（2）子类prototype属性的**proto**属性，表示方法的继承，总是指向父类的prototype属性；

