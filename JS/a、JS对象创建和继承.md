# 对象创建

### 1、Object构造函数创建

```js
var Person = new Object();
Person.name = 'Nike';
Person.age = 29;
```

### 2、使用对象字面量表示法

```js
var Person = {};//相当于var Person = new Object();
var Person = {
    name:'Nike';
    age:29;  
}
```

对象字面量是对象定义的一种简写形式，目的在于简化创建包含大量属性的对象的过程。也就是说，第一种和第二种方式创建对象的方法其实都是一样的，只是写法上的区别不同而已。

在介绍第三种的创建方法之前，我们应该要明白为什么还要用别的方法来创建对象，也就是第一种，第二种方法的缺点所在：**它们都是用了同一个接口创建很多对象，会产生大量的重复代码，就是如果你有100个对象，那你要输入100次很多相同的代码。**那我们有什么方法来避免过多的重复代码呢，就是把创建对象的过程封装在函数体内，通过函数的调用直接生成对象。

### 3、使用工厂模式创建对象

```js
function createPerson(name,age,job){
    var o = new Object();
    o.name = name;
    o.age = age;
    o.job = job;
    o.sayName = function(){
        alert(this.name); 
    };
 	return o; 
}
var person1 = createPerson('Nike',29,'teacher');
var person2 = createPerson('Arvin',20,'student');
```

在使用工厂模式创建对象的时候，我们都可以注意到，在createPerson函数中，返回的是一个对象。那么我们就**无法判断返回的对象究竟是一个什么样的类型**。于是就出现了第四种创建对象的模式。

### 4、使用构造函数创建对象

`new的工作原理`详见本目录下`new的工作原理.md`

```js
function Person(name,age,job){
    this.name = name;
    this.age = age;
    this.job = job;
    this.sayName = function(){
        alert(this.name);
    }; 
}
var person1 = new Person('Nike',29,'teacher');
var person2 = new Person('Arvin',20,'student');
```

**对比工厂模式，我们可以发现以下区别：**

1.没有显示地创建对象

2.直接将属性和方法赋给了this对象

3.没有return语句

4.终于可以识别的对象的类型。对于检测对象类型，我们应该使用instanceof操作符，我们来进行自主检测：

```js
alert(person1 instanceof Object);//ture
alert(person1 instanceof Person);//ture
alert(person2 instanceof Object);//ture
alert(person2 instanceof Object);//ture
```

同时我们也应该明白，**按照惯例，构造函数始终要应该以一个大写字母开头，而非构造函数则应该以一个小写字母开头。**

那么构造函数确实挺好用的，但是它也有它的**缺点**：

就是**每个方法都要在每个实例上重新创建一遍**，方法指的就是我们在对象里面定义的函数。如果方法的数量很多，就会占用很多不必要的内存。于是出现了第五种创建对象的方法:

### 5、原型创建对象模式

```js
function Person(){}
Person.prototype.name = 'Nike';
Person.prototype.age = 20;
Person.prototype.jbo = 'teacher';
Person.prototype.sayName = function(){
    alert(this.name);
};
var person1 = new Person();
person1.sayName();
```

使用**原型创建对象**的方式，可以让**所有对象实例**共享它所包含的属性和方法。

如果是使用**原型创建对象模式**，请看下面代码：

```js
function Person(){}
Person.prototype.name = 'Nike';
Person.prototype.age = 20;
Person.prototype.jbo = 'teacher';
Person.prototype.sayName = function(){
	alert(this.name);
};
var person1 = new Person();
var person2 = new Person();
person1.name ='Greg';
alert(person1.name); //'Greg' --来自实例
alert(person2.name); //'Nike' --来自原型
```

### 6、组合构造函数模式和原型模式

```js
function Person(name,age,job){
    this.name =name;
    this.age = age;
    this.job = job;
}
Person.prototype = {
    constructor:Person,
    sayName: function(){
        alert(this.name);
    };
}
var person1 = new Person('Nike',20,'teacher');
```





# 对象继承

```js
function People(name){
    //属性
    this.name  = name || 'aaa'
    //实例方法
    this.sleep=function(){
        console.log(this.name + '正在睡觉')
    }
}
//原型方法
People.prototype.eat = function(food){
    console.log(this.name + '正在吃：' + food);
}
```

### 1、原型链继承 

#### 父类的实例作为子类的原型

> 重点：利用原型让**一个引用类型继承另外一个引用类型的属性和方法**。构造函数，原型，实例之间的关系：每个**构造函数**都有一个**原型对象**，**原型对象**包含一个指向**构造函数的指针**，而**实例**都包含一个**指向原型对象的内部指针**。

```js
function Woman(){}
Woman.prototype= new People();
Woman.prototype.name = 'lll';
let womanObj = new Woman();
```

优点：

+ 简单易于实现，父类的通过原型（prototype）新增的原型方法与属性子类都能访问

缺点：

+ 原型链继承多个实例的引用类型属性指向相同，一个实例修改了原型属性，另一个实例的原型属性也会被修改
+ 无法实现多继承
+ 创建子类实例时，不能向父类构造函数中传参数



### 2.借用构造函数继承

#### 子类构造函数内部使用call（以及this）调用父类构造函数，并通过this添加方法/属性

```js
function Woman(name){
    //继承了People
    People.call(this);
    //People.call(this，'wangxiaoxia'); 
    this.name = name || 'renbo'
}
let womanObj = new Woman();
```

优点：

+ 解决了子类构造函数向父类构造函数中传递参数

+ 可以实现多继承（call或者apply多个父类）

缺点：

+ 方法都在构造函数中定义，无法复用

+ 不能继承原型属性/方法，只能继承父类的实例属性和方法



### 3.实例继承

#### 子类构造函数内部new父类对象，并在生成的父类实例对象上添加方法/属性

```js
function Wonman(name){
    let instance = new People();
    instance.name = name || 'wangxiaoxia';
    return instance;
}
let wonmanObj = new Wonman();
```

优点：

+ 不限制调用方式

+ 简单，易实现

缺点：

+ 不能多次继承
+ 不能继承原型属性/方法，只能继承父类的实例属性和方法



### 4.组合式继承

#### 调用父类构造函数，继承父类的属性，通过将父类实例作为子类原型，实现函数复用

```js
function Woman(name,age){
    People.call(this,name,age)
}
Woman.prototype = new People();
Woman.prototype.constructor = Woman;
let wonmanObj = new Woman(ren,27);
wonmanObj.eat();
```

缺点：

+ 由于调用了两次父类，所以产生了两份实例

优点：

+ 函数可以复用

+ 不存在引用属性问题

+ 可以继承属性和方法，并且可以继承原型的属性和方法

### 5.寄生组合继承

通过寄生的方式来修复组合式继承的不足，完美的实现继承

```js
//父类
function People(name,age){
    this.name = name || 'wangxiao'
    this.age = age || 27
}
//父类方法
People.prototype.eat = function(){
    return this.name + this.age + 'eat sleep'
}


//子类
function Woman(name,age){
    //继承父类属性
    People.call(this,name,age)
}
//继承父类方法
(function(){
    // 创建空类
    let Super = function(){};
    Super.prototype = People.prototype;
    //父类的实例作为子类的原型
    Woman.prototype = new Super();
})();
//修复构造函数指向问题
Woman.prototype.constructor = Woman;
let womanObj = new Woman();
```



### 6.es6继承

```js
//class 相当于es5中构造函数
//class中定义方法时，前后不能加function，全部定义在class的protopyte属性中
//class中定义的所有方法是不可枚举的
//class中只能定义方法，不能定义对象，变量等
//class和方法内默认都是严格模式
//es5中constructor为隐式属性
class People{
    constructor(name='wang',age='27'){
        this.name = name;
        this.age = age;
    }
    eat(){
        console.log(`${this.name} ${this.age} eat food`)
    }
}
//继承父类
class Woman extends People{ 
   constructor(name = 'ren',age = '27'){ 
     //继承父类属性
     super(name, age); 
   } 
    eat(){ 
     //继承父类方法
      super.eat() 
    } 
} 
let wonmanObj=new Woman('xiaoxiami'); 
wonmanObj.eat();
```



#### ES5继承和ES6继承的区别

ES5的继承是通过prototype或构造函数机制来实现。

ES5的继承实质上是**先创建子类的实例对象**，然后**再将父类的方法添加到this上**(Parent.apply(this))。

ES6的继承机制实质上是**先创建父类的实例对象this** `(所以必须先调用父类的super()方法)`，然后**再用子类的构造函数修改this**。

具体为ES6通过class关键字定义类，里面有构造方法，类之间通过extends关键字实现继承。**子类必须在constructor方法中调用super方法**，否则新建实例报错。**因为子类没有自己的this对象，而是继承了父类的this对象，然后对其调用。**如果不调用super方法，子类得不到this对象。

> 注意：**super关键字指代父类的实例**，即父类的this对象。在子类构造函数中，调用super后，才可使用this关键字，否则报错。













































