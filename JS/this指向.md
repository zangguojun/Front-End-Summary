## 一、



首先必须要说的是，**this的指向在函数定义的时候是确定不了的，只有函数执行的时候才能确定this到底指向谁**，**实际上this的最终指向的是那个调用它的对象**，这句话有些问题，后面会解释为什么会有问题，虽然网上大部分的文章都是这样说的，虽然在很多情况下那样去理解不会出什么问题，但是实际上那样理解是不准确的，所以在你理解this的时候会有种琢磨不透的感觉**）**，那么接下来我会深入的探讨这个问题。

----

### 例1

```js
function a(){
    var user = "追梦子";
    console.log(this.user); //undefined
    console.log(this); //Window
}
//a();
//window.a();
```

> 按照我们上面说的this最终指向的是调用它的对象，这里的函数a实际是被Window对象所点出来的，下面的代码就可以证明。

### 例2

```js
var o = {
    user:"追梦子",
    fn:function(){
        console.log(this.user);  //追梦子
    }
}
o.fn();
```

> 这里的this指向的是对象o，因为你调用这个fn是通过o.fn()执行的，那自然指向就是对象o，这里再次强调一点，this的指向在函数创建的时候是决定不了的，在调用的时候才能决定，谁调用的就指向谁，一定要搞清楚这个。

### 例3

```js
var o = {
    user:"追梦子",
    fn:function(){
        console.log(this.user); //追梦子
    }
}
window.o.fn();
```

> 这段代码和上面的那段代码几乎是一样的，但是这里的this为什么不是指向window，如果按照上面的理论，最终this指向的是调用它的对象，这里先说个而外话，window是js中的全局对象，我们创建的变量实际上是给window添加属性，所以这里可以用window点o对象。

##### 这里先不解释为什么上面的那段代码this为什么没有指向window，我们再来看一段代码。

### 例3.1

```js
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //12
        }
    }
}
o.b.fn();
```

> 这里同样也是对象o点出来的，但是同样this并没有执行它，那你肯定会说我一开始说的那些不就都是错误的吗？其实也不是，只是一开始说的不准确，接下来我将补充一句话，我相信你就可以彻底的理解this的指向的问题。

+ 情况1：**如果一个函数中有this，但是它没有被上一级的对象所调用，那么this指向的就是window**，这里需要说明的是在js的严格版中this指向的不是window，但是我们这里不探讨严格版的问题，你想了解可以自行上网查找。

+ 情况2：**如果一个函数中有this，这个函数有被上一级的对象所调用，那么this指向的就是上一级的对象**。

+ 情况3：如果一个函数中有this，**这个函数中包含多个对象，尽管这个函数是被最外层的对象所调用，this指向的也只是它上一级的对象，**

> 例子3可以证明，如果不相信，那么接下来我们继续看几个例子。

### 例3.2

```js
var o = {
    a:10,
    b:{
        // a:12,
        fn:function(){
            console.log(this.a); //undefined
        }
    }
}
o.b.fn();
```

> 尽管对象b中没有属性a，这个this指向的也是对象b，**因为this只会指向它的上一级对象，不管这个对象中有没有this要的东西**。

### 例4

```js
var o = {
    a:10,
    b:{
        a:12,
        fn:function(){
            console.log(this.a); //undefined
            console.log(this); //window
        }
    }
}
var j = o.b.fn;
j();
```

> 这里this指向的是window，是不是有些蒙了？其实是因为你没有理解一句话，这句话同样至关重要。
>
> **this永远指向的是最后调用它的对象，也就是看它执行的时候是谁调用的**，例4中虽然函数fn是被对象b所引用，但是在将fn赋值给变量j的时候并没有执行，所以最终指向的是window，这和例3是不一样的，例3是直接执行了fn。
>
> this讲来讲去其实就是那么一回事，只不过在不同的情况下指向的会有些不同，上面的总结每个地方都有些小错误，也不能说是错误，而是在不同环境下情况就会有不同，所以我也没有办法一次解释清楚，只能你慢慢地的去体会。

### 构造函数版this：

```js
function Fn(){
    this.user = "追梦子";
}
var a = new Fn();
console.log(a.user); //追梦子
```

> 这里之所以对象a可以点出函数Fn里面的user是因为**new关键字可以改变this的指向**，将这个this指向对象a，为什么我说a是对象，因为用了new关键字就是创建一个对象实例，理解这句话可以想想我们的例子3，**我们这里用变量a创建了一个Fn的实例**（相当于复制了一份Fn到对象a里面），此时仅仅只是创建，并没有执行，**而调用这个函数Fn的是对象a**，**那么this指向的自然是对象a**，那么为什么对象a中会有user，**因为你已经复制了一份Fn函数到对象a中**，**用了new关键字就等同于复制了一份**。

### 更新一个小问题当this碰到return时

```js
function fn()  
{  
    this.user = '追梦子';  
    return {};  
}
var a = new fn;  
console.log(a.user); //undefined
```

##### 再看一个

```js
function fn()  
{  
    this.user = '追梦子';  
    return function(){};
}
var a = new fn;  
console.log(a.user); //undefined
```

##### 再来

```js
function fn()  
{  
    this.user = '追梦子';  
    return 1;
}
var a = new fn;  
console.log(a.user); //追梦子
```

```js
function fn()  
{  
    this.user = '追梦子';  
    return undefined;
}
var a = new fn;  
console.log(a.user); //追梦子
```

什么意思呢？

> 如果返回值是一个对象，那么this指向的就是那个返回的对象，如果返回值不是一个对象那么this还是指向函数的实例。

```js
function fn()  
{  
    this.user = '追梦子';  
    return undefined;
}
var a = new fn;  
console.log(a); //fn {user: "追梦子"}
```

> 还有一点就是虽然null也是对象，但是在这里this还是指向那个函数的实例，因为null比较特殊。

```js
function fn()  
{  
    this.user = '追梦子';  
    return null;
}
var a = new fn;  
console.log(a.user); //追梦子
```

**知识点补充：**

+ **在严格版中的默认的this不再是window，而是undefined。**

+ new操作符会改变函数this的指向问题，虽然我们上面讲解过了，但是并没有深入的讨论这个问题，网上也很少说，所以在这里有必要说一下。

```js
function fn(){
    this.num = 1;
}
var a = new fn();
console.log(a.num); //1
```

> 为什么this会指向a？首先new关键字会创建一个空的对象，然后会自动调用一个函数apply方法，将this指向这个空对象，这样的话函数内部的this就会被这个空的对象替代。

> 注意: 当你new一个空对象的时候,js内部的实现并不一定是用的apply方法来改变this指向的,这里我只是打个比方而已.

```js
if (this === 动态的\可改变的) return true;
```

## 二、

按照this指针的优先级，列出下面常会遇到的四种情况，从上到下依次是优先级从高到低（后面会详细比较优先级）。

+ 函数是和new一起被调用的吗（new绑定）？如果是，this就是新构建的对象。
  `var bar = new foo()`

+ 函数是用call或apply被调用（明确绑定），甚至是隐藏在bind 硬绑定 之中吗？如果是，this就是明确指定的对象。
  `var bar = foo.call( obj2 )`

+ 函数是用环境对象（也称为拥有者或容器对象）被调用的吗（隐含绑定）？如果是，this就是那个环境对象。
  `var bar = obj1.foo()`

+ 否则，使用默认的this（默认绑定）。如果在strict mode下，就是undefined，否则是global对象。

   `var bar = foo()`

> 以上，就是理解对于普通的函数调用来说的this绑定规则所需的全部。是的···几乎是全部。



+ call和apply功能几乎一致,唯一的区别就是传参的方式！！
+ **call和apply的第一个参数如果为null或者undefined,那么this指向window**
+ **call和apply的第一个参数如果为值类型的数据，那么会将这个值类型的数据，转换成其对应的引用类型的数据，然后再将this指向这个引用类型的数据**
+ call和apply立即执行这个函数，bind方法可以让对应的函数想什么时候调就什么时候调用，并且可以将参数在执行的时候添加，这是它们的区别，根据自己的实际情况来选择使用。
+ **当参数的个数确定的情况下可以使用call;当参数的个数不确定的情况下可以使用apply**



### 箭头函数this指向注意事项

箭头函数体内的`this`对象，如果包裹在函数中就是函数调用时所在的对象，如果放在全局中就是指全局对象window。并且固定不会更改。换句话说**内部的`this`就是外层代码块的`this`**

下面是对比分析普通函数和箭头函数中this区别

```js
// 普通函数
function foo() {
  setTimeout(function() {
    console.log('id:', this.id);
  });
}
 
var id = 21;
foo.call({ id: 42 }); //21
```

```js
// 箭头函数
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}
 
var id = 21;
foo.call({ id: 42 }); //42
// 上面的匿名函数定义时所在的执行环境就是foo函数，所以匿名函数内部的this执向始终会和foo函数的this执向保持一致，不会更改，如同下面的这个案例
function foo() {
  setTimeout(() => {
    console.log('id:', this.id);
  }, 100);
}
 
var id = 21;
foo(); //21（没有用call）
```

```js
function foo() {
  console.log(this.a)
}
var a = 1

foo()

const obj = {
  a: 2,
  foo: foo
}

obj.foo()

const c = new foo()
```

接下来我们一个个分析上面几个场景

- 对于直接调用`foo`来说，不管`foo`函数被放在了什么地方，`this`一定是`window`
- 对于`obj.foo()`来说，我们只需要记住，谁调用了函数，谁就是`this`，所以在这个场景下`foo`函数中的`this`就是`obj`对象
- 对于`new`的方式来说，`this`被永远绑定在了`c`上面，不会被任何方式改变`this`

说完了以上几种情况，其实很多代码中的`this`应该就没什么问题了，下面让我们看看箭头函数中的`this`

```js
function a() {
  return () => {
    return () => {
      console.log(this)
    }
  }
}
console.log(a()()())
```

首先箭头函数其实是没有`this`的，箭头函数中的`this`只取决包裹箭头函数的第一个普通函数的`this`。在这个例子中，因为包裹箭头函数的第一个普通函数是`a`，所以此时的`this`是`window`。另外对箭头函数使用`bind`这类函数是无效的。

最后种情况也就是`bind`这些改变上下文的 API 了，对于这些函数来说，`this`取决于第一个参数，如果第一个参数为空，那么就是`window`。

```js
        this.x = 9;
        var module = {
            x: 81,
            getX: function () { return this.x; }
        };

        module.getX();

        var retrieveX = module.getX;
        retrieveX();

        var boundGetX = retrieveX.bind(module);
        boundGetX();
```

`retrieveX`只是`getX`函数的引用，也就是只是`getX`的一个指针（`getX`的另一个指针是`module.getX`），所以`retrieveX`还是指向`getX`函数本身的

和上面类似的案例，下面的func只是函数引用，所以即使在函数内部，还是执行的函数本身，**不受词法作用域限制**（箭头函数则受限制）

```js
function foo() {
    console.log( this.a );
}
 
var a = 2;
var o = { a: 3, foo: foo };
var p = { a: 4 };
 
o.foo(); // 3
(p.foo = o.foo)(); // 2
```

确实，许多包中的函数，和许多在JavaScript语言以及宿主环境中的内建函数，都提供一个可选参数，通常称为“环境（context）”，这种设计作为一种替代方案来确保你的回调函数使用特定的this而不必非得使用bind(..)。

举例来说：

```js
function foo(el) {
    console.log( el, this.id );
}
 
var obj = {
    id: "awesome"
};
 
// 使用`obj`作为`this`来调用`foo(..)`
[1, 2, 3].forEach( foo, obj ); 
// 1 awesome  2 awesome  3 awesome
```

明确绑定的优先权要高于隐含绑定

```js
function foo() {
    console.log( this.a );
}
 
var obj1 = {
    a: 2,
    foo: foo
};
 
var obj2 = {
    a: 3,
    foo: foo
};
 
obj1.foo(); // 2
obj2.foo(); // 3
 
obj1.foo.call( obj2 ); // 3
obj2.foo.call( obj1 ); // 2
```







## 三、

- 1.以函数的形式（包括普通函数、定时器函数、立即执行函数）调用时，this 的指向永远都是 window。比如`fun();`相当于`window.fun();`
- 2.以方法的形式调用时，this 指向调用方法的那个对象
- 3.以构造函数的形式调用时，this 指向实例对象
- 4.以事件绑定函数的形式调用时，this 指向**绑定事件的对象**
- 5.使用 call 和 apply 调用时，this 指向指定的那个对象

```js
function fun() {
    console.log(this);
    console.log(this.name);
}

var obj1 = {
    name: 'smyh',
    sayName: fun,
};

var obj2 = {
    name: 'vae',
    sayName: fun,
};

var name = '全局的name属性';

//以函数形式调用，this是window
fun(); //可以理解成 window.fun()
```



```js
function fun() {
    console.log(this);
    console.log(this.name);
}

var obj1 = {
    name: 'smyh',
    sayName: fun,
};

var obj2 = {
    name: 'vae',
    sayName: fun,
};

var name = '全局的name属性';

//以方法的形式调用，this是调用方法的对象
obj2.sayName();
```

### 四

**（1）this是js的一个关键字，指定一个对象，然后替代this；函数中的this指向行为发生的主体，函数外的this都指向window，没有意义**

**（2）函数内的this跟函数在什么环境下定义没有关系，意思就是跟他的作用域没有关系，而只跟自己执行时候的主体有关**

**（3）主体：就是看它前面有没有点，有点.就是点前面的对象，没有点.就指向window**

**（4）自执行函数的时候，里面的this都指向window**

**（5）给元素中的某一个事件绑定方法，当事件触发时，执行绑定的方法，this就指向当前对象**

***当函数套函数时，不要管它怎么定义，而看它执行时候的主体**

![img](https://img2018.cnblogs.com/blog/1483877/201810/1483877-20181031110632455-692940764.png)

**因为执行test()时，前面没有 . 所以，test里的this就指向window，因为x也是window下的变量，所以x就是12**

![img](https://img2018.cnblogs.com/blog/1483877/201810/1483877-20181031111140976-1067069166.png)

**因为执行obj.ss()时，ss就相当于把test这个函数赋给了ss，则ss就是一个函数，然后执行obj下的ss，obj下的x为45，则obj()就是45；**

![img](https://img2018.cnblogs.com/blog/1483877/201810/1483877-20181031111746739-1073276736.png)

**执行obj.ss()，有两个语句，第一个输出 this指向obj，第二个test()，我们就简单理解为，不要看它在谁的环境下，就看它前面有没有 . ，没有则是window下的，所以执行test()，this.x就指向window下的x，则为12**

```js
var val = 1;
var obj = {
    val:2,
    dbl:function(){
        var val = 45
        console.log(this)
        this.val *= 2
        console.log(val)
        console.log(this.val)
    }
}
var ss = obj.dbl
ss()
```

**ss被赋值为obj.dbl obj.dbl就是他在obj下的值，则那个函数，因为赋值给了ss，ss在window下执行的，所以在这个代码块中，第一个输出的this指window，第二个输出的val是当前下面的val，45；最后一句是this下面的，即window，1\*2=2.**

![img](https://img2018.cnblogs.com/blog/1483896/201810/1483896-20181031203409235-1532912183.png)

第一个是执行fun()  首先你要明白前面把a.say他的函数体 赋值给了fun 所以答案是222

第二个执行a.say()  say归属a 所以答案就是a里面的 111

第三个执行b.say(a.say)  但要注意这里有实参形参 可以直接理解为 fun = a.say 然后返回来在这个匿名函数里面执行这个fun（）

此时的fun就是上面的a.say  但你要明白此时的fun（）前面是没有“.”的他的归属是window 所以答案就是222

第四个执行b.say() 在这条上面吧a.say赋值给了b.say所以此时的b.say就是a.say了,所以答案是333



















