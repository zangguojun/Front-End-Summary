### 实现 let

> let 定义一个仅作用于该代码块的变量，我们用闭包的形式来定义一个不被污染的变量

在 es6 出现以前我们一般使用**无限接近闭包的形式**或者**立即执行函数的形式**来定义不会被污染的变量。我们这也可以做类似的操作。

```js
(function () {
    var a = 1;
    console.log(a); // 1
})();
console.log(a); // a is not defined
```



### **实现 const**

> 由于 ES5 环境没有 block 的概念，**所以是无法百分百实现 const**，只能是**挂载到某个对象下**，要么是全局的 window，要么就是自定义一个 object 来当容器

```js
function _const(key, value) {
    window[key] = value;
    Object.defineProperty(window, key, {
        enumerable: false,
        configurable: false,
        get: function () {
            return value;
        },
        set: function (newValue) {
            if (newValue !== value) {
                throw TypeError("只读变量，不可修改");
            } else {
                return value;
            }
        },
    });
}
```

或者使用ES5的Object.freeze();

```js
var f = Object.freeze({'name':'admin'});
f.name = 'hello'; // 严格模式下是会报错的
f.name; // 打印出admin ,值没有被改变
```







