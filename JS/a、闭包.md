## 自由变量

自由变量：是指在**函数**中使用的变量即不是函数参数`arguments`也不是函数内部声明的变量，那这个变量就是**自由变量**。

```js
function outer(){
    var public = '前端自学驿站：';
    function inner(name){
        // 这个函数中用到了两个变量
        //public: 
        	//不是函数自己的参数
        	//也不是函数内部定义的变量
        //name: 
        	//函数自己的参数
        console.log(public + name);  //前端自学驿站：北歌
    }
    return inner('北歌'); // 返回inner函数执行的返回值
}
outer();
```

上面这段代码中中`public`不是自己的参数也不是函数内部声明的变量，那它就是一个**自由变量**



所以说上面的代码形成了闭包，让我们来看看它是不是符合上面对于闭包的定义

- 《你不知道的javaScript》：**函数创建和函数执行不在同一个作用域下就会形成闭包。**
-  MDN：**闭包是指那些能够访问自由变量的函数。**
- 《JS高级程序设计-第3版》：**闭包是指有权访问另外一个函数作用域中的变量的函数**

对照上面的代码我们不难发现上面三个条件都符合

按照的我理解闭包得符合两个条件：

1、**函数执行完毕，函数内部创建的东西被函数外部引用了，形成不销毁的栈内存**

2、**在代码中引用了自由变量**

## 闭包的用途

+ 隐藏变量，避免全局污染 
+ 可以读取函数内部的变量

+ 有利于代码封装。

## 使用闭包的注意

1）由于闭包会使得函数中的变量都被保存在内存中，**内存消耗很大**，所以不能滥用闭包，否则会造成网页的性能问题，在IE中可能导致**内存泄露**。解决方法是，在退出函数之前，**将不使用的局部变量全部删除**。

2）**闭包会在父函数外部，改变父函数内部变量的值。**所以，如果你把父函数当作对象（object）使用，把闭包当作它的公用方法（Public Method），**把内部变量当作它的私有属性**（private value），这时一定要小心，不要随便改变父函数内部变量的值。



## 闭包面试题

```js
function fun(n, o) {
    console.log(o);
    return {
        fun: function (m) {
            return fun(m, n);
        }
    };
}

var a = fun(0); // ?
a.fun(1); // ?
a.fun(2); // ?
a.fun(3); // ?
var b = fun(0).fun(1).fun(2).fun(3); // ?
var c = fun(0).fun(1); // ?
c.fun(2); // ?
c.fun(3); // ?
```

```js
function fun(n, o) {
    /**
    * 第一次执行fun
    * n: 0  
    * o: undefined
    */

    /**
    * 第二次执行：a.fun(1)中调用的fun(1, 0)
    * n：1
    * o: 0
    */

    /**
    * 第三次执行：a.fun(2)中调用的fun(2, 0)
    * n：2
    * o: 0
    */

    /**
    * 第四次执行：a.fun(3)中调用的fun(3, 0)
    * n：3
    * o: 0
    */

console.log(o);
return {
    fun: function (m) {
        /**
       * 第一次调用: m: 1
       * fun(1, n是自由变量用的是上层作用域中的0)
       */

        /**
       * 第二次调用: m: 2
       * fun(2, n是自由变量用的是上层作用域中的0)
       */

        /**
       * 第二次调用: m: 3
       * fun(3, n是自由变量用的是上层作用域中的0)
       */
        return fun(m, n);
    }
};
}

var a = fun(0); // undefined 执行返回了一个对象被a引用了，形成闭包fun中的变量没有被销毁
a.fun(1); // 0
a.fun(2); // 0
a.fun(3); // 0
```



```js
function fun(n, o) {
        /**
       * 第一次执行：fun(0)
       * n: 0  
       * o: undefined
       */

        /**
       * 第二次执行：对象.fun(1)调用的fun(1, 0)
       * n: 1  
       * o: 0
       */

        /**
       * 第三次执行：对象.fun(2)调用的fun(2, 1)
       * n: 2
       * o: 1
       */

        /**
       * 第四次次执行：对象.fun(3)调用的fun(3, 2)
       * n: 3
       * o: 2
       */
    console.log(o);
    return {
        fun: function (m) {
            /**
           * 第一次调用：对象.fun(1)
           * m: 1
           * fun(1, n是用的是上层作用域中的0)
           */

            /**
           * 第二次调用：对象.fun(2)
           * m: 2
           * fun(2, n是用的是上层作用域中的1)
           */

            /**
           * 第二次调用：对象.fun(3)
           * m: 3
           * fun(3, n是用的是上层作用域中的2)
           */
            return fun(m, n);
        }
    };
}

// 第一次执行fun(0)返回的是对象，调用对象的中的fun方法
var b = fun(0).fun(1).fun(2).fun(3); 
// undefined, 0, 1, 2

// 能看懂上面的话剩下的这些应该就能自己分析出来了，答案也不给了分析好后直接控制台输出
var c = fun(0).fun(1); // ?
c.fun(2); // ?
c.fun(3); // ?
// undefined,0,1,1
```

```js
var name = "The Window";
var object = {
    name : "My Object",
    getNameFunc : function(){
        return function(){
            return this.name;
        };
    }
};

alert(object.getNameFunc()());
```

```js
var name = "The Window";
var object = {
    name : "My Object",
    getNameFunc : function(){
        var that = this;
        return function(){
            return that.name;
        };
    }
};

alert(object.getNameFunc()());
```

## 闭包应用场景

#### setTimeout传参

```js
//原生的setTimeout传递的第一个函数不能带参数
setTimeout(function(param){
    alert(param)
},1000)

//通过闭包可以实现传参效果
function func(param){
    return function(){
        alert(param)
    }
}
var f1 = func(1);
setTimeout(f1,1000);
```

#### 封装函数（回调）

```js
function changeSize(size){
    return function(){
        document.body.style.fontSize = size + 'px';
    };
}
document.getElementById('size-12')
    .onclick = changeSize(12);
```

#### 封装变量



参考：

https://juejin.cn/post/6844903470839906311

https://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html