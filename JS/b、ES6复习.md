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

> 包括 默认解构、省略解构

```javascript
// 对象解构
let obj1 = {name:'buchiyu',age:21}
let {name:myName,age:myAge,sex:mySex='男'} = obj1 
console.log(myName,myAge,mySex);
//默认解构：sex=‘男’
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













