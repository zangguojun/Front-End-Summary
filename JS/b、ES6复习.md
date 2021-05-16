### ES5如何写出块作用域

ES5只有**全局作用域与函数作用域**，如果需要实现*块级作用域*，代码如下：

```javascript
;(function(){
    
})()
```

> 使用的是`闭包 + 自执行函数`的方式
>
> ES6之后，代码如下：
>
> ```javascript
> {
>  let a
> }
> ```
>

#### 一、var 与 let 区别
>
> ```javascript
>var a = 2
> {
> console.log(a); // 2
> }
> ```
> 
> ```javascript
>let a = 2
> {
> console.log(a) // 2
> }
>  ```
> 
> ------
>
> ------
>
> ```javascript
>var a = 2
> {
> console.log(a); // 2
> var a = 1
> }
> // var并没有块作用域，所以此时的{}无用
> ```
> 
> ```javascript
>var a = 2
> {
> console.log(a); //ReferenceError: a is not defined
> let a = 1
> }
> //let拥有块作用域，先在块作用域中找a，因为let 声明了a，所以和外面var申明的a没关系，但是因为let没有变量提升，所以报错：a没有定义
> ```
> 
> ```javascript
>let a = 2
> {
> console.log(a); //ReferenceError: a is not defined
> let a = 1
> }
> //同上，因为块级作用域里面就有a的定义，所以和块级作用域之外的定义没有关系
> ```
> 
> ```javascript
>let a = 2
> {
> console.log(a);
> var a = 1 //SyntaxError: Identifier 'a' has already been declared
> }
> //因为let定义的是全局作用域下的a，而var没有块作用域，虽然再{}之中，但是其实也是全局作用域下的a，所以报错：a已经被定义
> ```
> 

#### 二、for循环

>```javascript
> for (var i = 0; i < 3; i++) {
>setTimeout(() => {
> console.log(i);
> }, 1000);
> }
> // 此时输出 3 3 3
> ```
> 

##### 解决办法① -- 自执行函数

>```javascript
> for (var i = 0; i < 3; i++) {
>(function(i){
> setTimeout(() => {
>   console.log(i);
> }, 1000);
> })(i)
> }
> ```
>

##### 解决办法② -- function

>```javascript
> function loop (i) {
>setTimeout(() => {
> console.log(i);
> }, 1000);
> }
> 
> for (var i = 0; i < 3; i++) {
> loop(i)
>}
> ```
> 

##### 解决办法③ -- let

>```javascript
> for (let i = 0; i < 3; i++) {
>setTimeout(() => {
> console.log(i);
> }, 1000);
> }
> ```



### 对象和数组解构

> 包括 默认解构、省略解构、连续解构赋值

```javascript
// 对象解构
let obj1 = {
  name:'buchiyu',
  age:{
    data:{
      val:21
    }
  }
}
let {name:myName,age:{data:{val:myAge}},sex:mySex='男'} = obj1 
console.log(myName,myAge,mySex);
//默认解构：sex=‘男’
//连续解构赋值:age:{data:{val:myAge}}，最后使用myAge，但是此时的age，data以及val并没有被解构，即它们并不能使用

// ----------------------------------------------------
// 数组解构
let arr1 = [{name:'buchiyu',age:21},[1,2],3]
let [{name:myName,age:myAge},[,y],z,k=4] = arr1 
console.log(myName,myAge,y,z,k);
//默认解构：k=4
//省略解构：[,y] 中的 1
```



### 模板字符串的实现

```js
let name = {val:'buchiyu'}
let age = [21]
let desc = "${name.val}今年${age[0]}岁了！"
console.log(replace(desc));

function replace(desc) {
  return desc.replace(/\$\{([^}]+)\}/g,(matched,key) => {
    return eval(key)
  })
}
```

#### 带标签的模板字符串

```js
function desc (strings,...rest) {
  console.log(strings);
  console.log(rest);
  let result = ''
  // strings数组会比rest数组多一个元素，所以以rest的数组遍历
  for (let i = 0; i < rest.length; i++) {
    result += strings[i] + rest[i]
  }
  // 加上string的最后一个多出的元素
  result += strings[strings.length - 1]
  return result
}

let name = 'buchiyu'
let age = 21
let str = desc`我是 ${name} 今年 ${age} 岁了 `
console.log(str);
```

### `padStart`、`padEnd`使用

```js
let t = new Date()
const y = t.getFullYear()
const m = (t.getMonth() + '').padStart(2,'0')
const d = (t.getDate() + '').padStart(2,'0')
const hh = (t.getHours() + '').padStart(2,'0')
const mm = (t.getMinutes() + '').padStart(2,'0')
const ss = (t.getSeconds() + '').padStart(2,'0')
const ms = (t.getMilliseconds() + '').padStart(3,'0')
console.log(`当前时间为：${y}年${m}月${d}日${hh}时${mm}分${ss}秒${ms}毫秒`)
```

### 函数默认参数

```js
function ajax (url = new Error('url不能为空'), method='GET', dataType='json') {
  console.log(url);
  console.log(method);
  console.log(dataType);
}
ajax('http://www.baidu.com')
```

#### 函数默认参数的实现方式

```js
function ajax () {
  var url = arguments.length > 0 && 
  arguments[0] !== undefined ? 
  arguments[0] : new Error('url不能为空')

  var method = arguments.length > 1 &&
  arguments[1] !== undefined ?
  arguments[1] : 'GET'

  var dataType = arguments.length > 2 &&
  arguments[2] !== undefined ?
  arguments[2] : 'json'

  console.log(url);
  console.log(method);
  console.log(dataType);
}
ajax('http://www.baidu.com')
```

### `apply`的妙用

>  原本Math的max函数只能传递连续的参数而非数组，但是再apply的作用下，可以间接向`Math.max()`函数传递数组

```js
let arr = [1,2,3,4]
let max = Math.max.apply(Math,arr)
console.log(max);
```

> 但是，这里可以使用更简单的



### 对象中函数的特殊申明方式

```js
let obj = {
  name:'buchiyu',
  getName(){
    return this.name
  }
}
console.log(obj.getName());
```



### `this`的实现

```js
let obj = {
  name:'buchiyu',
  getName:function () {
    setTimeout(() => {
      console.log(this.name);
    }, 1000);
  }
}
obj.getName()
```

```js
let obj = {
  name:'buchiyu',
  getName:function () {
    const _this = this
    setTimeout(() => {
      console.log(_this.name);
    }, 1000);
  }
}
obj.getName()
```

### `reduce`的实现

```js
Array.prototype.reduce = function (reducer,initVal) {
  let initIndex = 0
  if (initVal === undefined) {
    initVal = this[0]
    initIndex = 1
  }
  for (let i = initIndex; i < this.length; i++) {
    initVal = reducer(initVal,this[i],i,this)
  }
  return initVal
}
```

```js
let arr = [1,2,3,4]
let sum = arr.reduce((pre,cur,index,array) => {
  // console.log(index,array);
  return pre + cur
},0)
console.log(sum);
let sum2 = arr.reduce((pre,cur,index,array) => {
  return pre + cur
})
console.log(sum2);
```

#### reduceRight实现

```js
Array.prototype.reduceRight = function (reducer,initVal) {
  let n = this.length
  let initIndex = n - 1
  if (initVal === undefined) {
    initVal = this[n - 1]
    initIndex = n - 2
  }
  for (let i = initIndex; i >= 0; i--) {
    initVal = reducer(initVal,this[i],i,this)
  }
  return initVal
}
```

```js
let arr = [1,2,3,4]
let sum = arr.reduceRight((pre,cur,index,array) => {
  // console.log(index,array);
  return pre + cur
},0)
console.log(sum);
let sum2 = arr.reduceRight((pre,cur,index,array) => {
  return pre + cur
})
console.log(sum2);
```

### `filter`的实现

```js
Array.prototype.filter = function (fn) {
  const result = []
  for (let i = 0; i < this.length; i++) {
    if (fn(this[i], i, this)) result.push(this[i])
  }
  return result
}
```

```js
let arr = [1,2,3,4,5]
let newArr = arr.filter((cur,index,array) => cur >= 3)
console.log(newArr);
```

### `map`的实现

```js
let arr = [1,2,3,4,5]
Array.prototype.map = function (fn) {
  const result = []
  for (let i = 0; i < this.length; i++) {
    result.push(fn(this[i], i, this))
  }
  return result
}
let newArr = arr.map((cur) => cur + 10)
console.log(newArr);
```

### `find`的实现

```js
Array.prototype.find = function (fn) {
  for (let i = 0; i < this.length; i++) {
    if (fn(this[i], i, this)) {
      return this[i]
    }
  }
}
```

```js
let arr = [1,2,3,4,5]
let newArr = arr.find((cur, index, array) => index === 3)
console.log(newArr);
```

### `findIndex`的实现

```js
Array.prototype.findIndex = function (fn) {
  for (let i = 0; i < this.length; i++) {
    if (fn(this[i], i, this)) {
      return i
    }
  }
}
```

```js
let arr = [1,2,3,4,5]
let newArr = arr.findIndex((cur, index, array) => cur === 3)
console.log(newArr);
```

### `some`的实现

```js
Array.prototype.some = function (fn) {
  for (let i = 0; i < this.length; i++) {
    if (fn(this[i], i, this)) {
      return true
    }
  }
  return false
}
```

```js
let arr = [1,2,3,4,5]
let newArr = arr.some((cur, index, array) => index > 5)
console.log(newArr);
```

### `every`的实现

```js
Array.prototype.every = function (fn) {
  for (let i = 0; i < this.length; i++) {
    if (!fn(this[i], i, this)) {
      return false
    }
  }
  return true
}
```

```js
let arr = [1,2,3,4,5]
let newArr = arr.every((cur, index, array) => index < 5)
console.log(newArr);
```

### 类数组遍历

#### 第一种：`Array.prototype.slice.call`

```js
function sum (){
  let arr = Array.prototype.slice.call(arguments)
  let s = 0
  arr.forEach(i => {
    s += i
  });
  return s
}
let s = sum(1,2,3,4)
console.log(s);
```

#### 第二种：`Array.prototype.forEach.call`

```js
function sum (){
  let s = 0
  Array.prototype.forEach.call(arguments, i => {
    s += i
  })
  return s
}
let s = sum(1,2,3,4)
console.log(s);
```

#### 第三种：`Array.from`

```js
function sum (){
  let s = 0
  Array.from(arguments).forEach(i => {
    s += i
  })
  return s
}
let s = sum(1,2,3,4)
console.log(s);
```

#### `Array.of`的使用

```js
let arr1 = Array(3)
let arr2 = Array.of(3)
console.log(arr1);
console.log(arr2);
```

### 对象短命名

```js
let name = 'buchiyu'
let age = 21
let obj = {name,age}
console.log(obj);
```

