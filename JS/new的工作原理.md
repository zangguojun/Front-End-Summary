# new操作符的工作原理

要创建Person的新实例，必须使用new操作符。以这种方式调用构造函数实际上会经历以下4个步骤：
       （1）创建一个新对象；
       （2）将构造函数的作用域赋给新对象（因此this就指向了这个对象）；
       （3）执行构造函数中的代码（为这个新对象添加属性）；
       （4）返回新对象。

那么我们就按照书上的步骤来模拟以下new操作符，首先构造函数如下：

```js
function Person (name) {
    this.name = name;
}
```

接下来我们就封装一个函数来模拟new操作符：

```js
function newFunc (name) {
    var o = {};
    Person.call(o, name);
    return o;
};
```

看起来步骤是和上面四条相吻合的，那赶快试验一下！

![img](https://pic4.zhimg.com/50/v2-ebaef1da4ab14824e78ada3a8520745e_hd.jpg?source=1940ef5c)![img](https://pic4.zhimg.com/80/v2-ebaef1da4ab14824e78ada3a8520745e_720w.jpg?source=1940ef5c)

这是我们使用new操作符创建的对象，没有任何问题。

![img](https://pic1.zhimg.com/50/v2-3338735823f722b44ea1f7589496091d_hd.jpg?source=1940ef5c)![img](https://pic1.zhimg.com/80/v2-3338735823f722b44ea1f7589496091d_720w.jpg?source=1940ef5c)

而我们使用newFunc来模拟new操作符创建的，就出了些问题。

那就是在检验对象时person2不是Person的实例。

可是我们的确是按照书上的4个步骤来进行的，那么哪里出了问题呢？

啊哈，再往后看到**原型模式**这一小节我们就明白了，我们缺少了一步绑定构造函数的原型。

修改后的newFunc函数如下：

```js
function newFunc (name) {
    var o = {};
    o.__proto__ = Person.prototype;//绑定Person的原型
    Person.call(o, name);
    return o;
}
```

这时我们再来尝试一下，结果就很清楚了。

![img](https://pic1.zhimg.com/50/v2-a9022aa61b75b4202bdb66b1e03aa7ab_hd.jpg?source=1940ef5c)![img](https://pic1.zhimg.com/80/v2-a9022aa61b75b4202bdb66b1e03aa7ab_720w.jpg?source=1940ef5c)

如果再将这个函数抽象一下，可以传入构造函数：

```js
function newFunc (constructor){
    var o = {};
    o.__proto__ = constructor.prototype;
    constructor.apply(o, Array.prototype.slice.call(arguments, 1));
    return o;
}
```

这样就是实现了更加通用的newFunc函数。使用时候传入构造函数和参数就可以。

```js
var person1 = newFunc(Person, 'MeloGuo', 21)
```

# 二

**用new调用一个函数发生了这些事：**
（1）新建一个对象

```js
instance=new Object();
```

（2）设置原型链

```js
instance.__proto__=F.prototype;
```

（3）**让F中的this指向instance**，执行F的函数体。
（4）判断F的返回值类型：
**如果是值类型，就丢弃它，还是返回instance。**
**如果是引用类型，就返回这个引用类型的对象，替换掉instance。**

-----

（1）如果**没有写return**，相当于return undefined，JavaScript中的函数都是这样。undefined是值类型的，因此丢弃它，返回instance。
（2）如果**return this**相当于返回一个引用类型的对象，它自己就是instance，无所谓替换不替换了。
（3）对instance并**不需要设置它的constructor属性**，这个属性在instance的原型中。

```js
console.assert(!p.hasOwnProperty('constructor'));
console.assert(F.prototype.hasOwnProperty('constructor'));
```

而且，任意一个新函数在创建时，原型的constructor就已经设置好了。

```js
var G=function(){};
console.assert(G.prototype.hasOwnProperty('constructor'));
console.assert(G.prototype.constructor===G);
```

于是，这也要求我们在对prototype重新赋值的时候，**重新指定constructor属性**。

```js
F.prototype={
    constructor:F
};
```

# 三

### 1、红宝书上解释：

（1）创建一个新对象

（2）将构造函数的作用域赋给新对象

（3）执行构造函数中的代码

（4）返回新对象

 

### 2、MDN上的解释：

（1）一个继承自 Foo.prototype 的新对象被创建

（2）使用指定的参数调用构造函数 Foo，并将 this 绑定到新创建的对象。new Foo 等同于 new Foo()，也就是没有指定参数时，Foo 不带任何参数调用的情况

（3）如果构造函数返回了一个“对象”，那么这个对象会取代整个 new 出来的结果。如果构造函数没有返回对象，那么 new 出来的结果为步骤（1）创建的对象

 

### 3、总结：

使用 new 运算符时，其实就做了这三件事：

```js
// 创建了一个空对象 obj，并将这个空对象的__proto__成员指向 Foo 函数对象 prototype 成员对象
var obj  = {};
obj.__proto__ = Foo.prototype;

// 将 Foo 函数对象的 this 指针替换成 obj，然后再调用Foo函数
Foo.call(obj);// 判断实例类型是否为对象，是则返回实例，否则返回构造函数
if (typeof obj === 'object') {
    return obj;
} else {
    return Foo;
}
```

也就是说，函数内部发生了以下变化：

（1）创建一个新对象，并继承构造函数 Foo 的原型对象 Foo.prototype

（2）构造函数 Foo 被执行，执行时传入相应的参数，并将 this 指定为新的实例

（3）判断实例是否为对象，是则返回实例，否则返回构造函数

# 四

```js
function Parent(age, name) {
    this.age = age
    this.name = name
    this.sayAge = function() {
        console.log('this.age :', this.age)
    }
}
Parent.prototype.sayName = function() {
    console.log('this.name :', this.name)
}
```



```js
function New(Fn) {
    let obj = {}
    let arg = Array.prototype.slice.call(arguments, 1)
    obj.__proto__ = Fn.prototype
    obj.__proto__.constructor = Fn
    Fn.apply(obj, arg)
    return obj
}
```

**三、测试**

```js
let son = new Parent(18, 'lining')      
let newSon = New(Parent, 18, 'lining')

console.log('son :', son)       // son : Parent { age: 18, name: 'lining', sayAge: [Function] }
console.log('newSon :', newSon) // newSon : Parent { age: 18, name: 'lining', sayAge: [Function] }

console.log('son.constructor :', son.constructor)
// son.constructor : function Parent(age, name) {
//     this.age = age
//     this.name = name
//     this.sayAge = function() {
//         console.log('this.age :', this.age)
//     }
// }

console.log('newSon.constructor :', newSon.constructor)
// newSon.constructor : function Parent(age, name) {
//     this.age = age
//     this.name = name
//     this.sayAge = function() {
//         console.log('this.age :', this.age)
//     }
// }
```

 

**四、特殊情况**

上面的实现过程是一般我们所普遍知晓的构造过程

但是在 JavaScript 实现中，如果构造函数返回的是个对象/原始类型

那么最终得到的实例就不一样了

 

**各个返回类型情况：**

```js
function Parent(age, name) {
    this.age = age
    this.name = name
    this.sayAge = function() {
        console.log('this.age :', this.age)
    }
    // return 0
    // return null
    // return undefined
    // return false
    // return true
    // return ''
    // return 'parent'
    // return Symbol('parent')
    // return []
    // return {}
    // return {
    //     hands: 2,
    //     foot: 2
    // }
}
Parent.prototype.sayName = function() {
    console.log('this.name :', this.name)
}
```

**下面分别对应上面 11 种 return 情况**

```js
return 0:
let p1 = new Parent(18, 'jolin')
console.log('p1 :', p1) // p1 : Parent { age: 18, name: 'jolin', sayAge: [Function] }

return null:
let p2 = new Parent(18, 'jolin')
console.log('p2 :', p2) // p2 : Parent { age: 18, name: 'jolin', sayAge: [Function] }

return undefined:
let p3 = new Parent(18, 'jolin')
console.log('p3 :', p3) // p3 : Parent { age: 18, name: 'jolin', sayAge: [Function] }

return false:
let p4 = new Parent(18, 'jolin')
console.log('p4 :', p4) // p4 : Parent { age: 18, name: 'jolin', sayAge: [Function] }

return true:
let p5 = new Parent(18, 'jolin')
console.log('p5 :', p5) // p5 : Parent { age: 18, name: 'jolin', sayAge: [Function] }

return '':
let p6 = new Parent(18, 'jolin')
console.log('p6 :', p6) // p6 : Parent { age: 18, name: 'jolin', sayAge: [Function] }

return 'parent':
let p7 = new Parent(18, 'jolin')
console.log('p7 :', p7) // p7 : Parent { age: 18, name: 'jolin', sayAge: [Function] }

return Symbol('parent'):
let p8 = new Parent(18, 'jolin')
console.log('p8 :', p8) // p8 : Parent { age: 18, name: 'jolin', sayAge: [Function] }

return []:
let p9 = new Parent(18, 'jolin')
console.log('p9 :', p9) // p9 : []

return {}:
let p10 = new Parent(18, 'jolin')
console.log('p10 :', p10) // p10 : {}

return {
    hands: 2,
    foot: 2
}
let p11 = new Parent(18, 'jolin')
console.log('p11 :', p11) // p11 : { hands: 2, foot: 2 }

return function(){}
let p11 = new Parent(18, 'jolin')
console.log('p11 :', p11) // p11 : function(){}
```

**因此实现原理改一下：**

**1、创建一个空对象 obj**

**2、将该对象 obj 的原型链 `__proto__` 指向构造函数的原型 prototype，**

**并且在原型链 `__proto__` 上设置 构造函数 constructor 为要实例化的 Fn**

**3、传入参数，并让 构造函数 Fn 改变指向到 obj，并执行**

**4、如果 Fn 执行后返回的是对象类型（null除外），则返回该对象，否则返回 obj**

**修改模拟 new 运算符功能函数**

```js
function New(Fn) {
//     let obj = {}
//     obj.__proto__ = Fn.prototype
//     obj.__proto__.constructor = Fn
    var obj = Object.create(Fn.prototype);

    let arg = Array.prototype.slice.call(arguments, 1)
    let ret = Fn.apply(obj, arg)
    //return typeof ret === 'object' ? ret || obj : obj
    if((ret !== null && typeof ret === "object") ||(typeof ret === "function")) {
        return ret;
    }
    return obj;
}
```

