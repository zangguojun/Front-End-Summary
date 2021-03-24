```js
var s={a:1,b:2,c:3}
Object.defineProperty(s,'d',{
    value:4,
    enumerable:false
})
console.log(s)
var s1=Object.create(s);
```

## for in

1. 一般用于遍历对象的可枚举**属性**。**以及对象从构造函数原型中继承的属性**。对于每个不同的属性，语句都会被执行。
2. **不建议使用for in 遍历数组，因为输出的顺序是不固定的。**
3. 如果迭代的对象的变量值是null或者undefined, for in不执行循环体，建议在使用for in循环之前，先检查该对象的值是不是null或者undefined

```js
//此时s1是{}，但是能遍历它的原型中的属性
for(var prop in s1){
    console.log(prop);//a b c
    console.log(s1[prop]);//1 2 3
}
```

## for of

1. for…of 语句在可迭代对象（包括 Array，Map，Set，String，TypedArray，arguments 对象等等）上创建一个迭代循环，调用自定义迭代钩子，并为每个不同属性的值执行语句
2. 需要借助`Object.keys()`或者`Array.keys()，Array.values()，Array.entries()`，前者返回一个`Array`，后者返回`Array Iterator {  }`

```js
for(let prop of Object.keys(s1)){ // 这里的不可以获取到s1的key的
    console.log(prop);
    console.log(s1[prop]);
}
//没有任何输出，因为Object.keys(s1)，只能拿到s1自身的属性，不会拿到继承的属性，所以得到的是空数组


for(let prop of Object.keys(s)){ // 这里的可以获取到s的key的
    console.log(prop);//a b c
    console.log(s[prop]);//1 2 3
}
//没有任何输出，因为Object.keys(s1)得到的是[a,b,c]

```

