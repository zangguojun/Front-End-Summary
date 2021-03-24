### 基本类型和引用类型

在开始讲解JavaScript的深拷贝和浅拷贝之前，先要认识JavaScript的两种基本数据类型。

一种是**基本类型**（值类型），另外一种是引用类型。其中基本类型包括`undefined`、`null`、`number`、`string`和`boolean`，这几种类型在内存中都有固定的大小和空间。引用类型包括`object`，这种值的大小不固定，可以动态添加属性和方法，而基本类型则不可以。

**基本类型的值**保存在**内存中的栈**中，而**引用数据类型的值**保存在**内存中的堆**中，在栈内存中保存着**指向堆内存的指针**。

对于**基本数据类型的值**的复制，是对**栈内存中的值直接进行复制**，所以**复制的就是值本身**，相当于**复制了一个副本**。会在栈中开辟一块**全新的内存**。所以修改一个变量的值不会影响另外一个变量的值。所以对于**基本数据类型而言**，没有浅拷贝和深拷贝之分，或者说直接就是深拷贝。

### 引用类型浅拷贝

对于引用类型进行复制，复制的并不是**堆内存中的数据**，而是**指向堆中的栈内存的指针**。

原先在标记为**a**的**栈内存中**有一个**Foo**变量，指向**对应的堆内存中的数据**，现在通过**复制这个指针**，在**栈内存中**新开辟一个内存空间，我们标记为**c**，其指向的和a是同样的**堆内存数据**。所以**修改a和c都会导致堆内存中的数据发生改变**。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20201030105506131.png#pic_center)

```js
let Foo = {
    a: 3,
    b: 4
}

let newFoo = Foo

newFoo.a = 5
```

<img src="https://img-blog.csdnimg.cn/20201030114215478.png#pic_center" alt="在这里插入图片描述" style="zoom:150%;" />

由于Foo和NewFoo都**指向同样的对象**，所以改变NewFoo中的数据，Foo中的数据同样也会发生改变。那如何去改变这一点呢？让Foo和NewFoo中的数据不同，改变一个不会影响另外一个？我们先谈一下浅拷贝的解决办法。

浅拷贝可以使用`Object.assign()`来实现。我们还是举上面的例子：

```js
let Foo = {
    a: 3,
    b: 4
}

// let newFoo = Foo
Object.assign(newFoo, Foo)

newFoo.a = 5
```

<img src="https://img-blog.csdnimg.cn/20201030145649698.png#pic_center" alt="在这里插入图片描述" style="zoom:150%;" />

可以看出，Foo的结果并没有发生改变，达到了拷贝的目的。但这为什么又称之为浅拷贝呢？我们来看下面一个例子。

```js
let Foo = {
    a: {
      c:1
    },
    b: 4
}

// let newFoo = Foo
Object.assign(newFoo, Foo)

newFoo.a.c=2
```

<img src="https://img-blog.csdnimg.cn/20201030150040739.png#pic_center" alt="在这里插入图片描述" style="zoom:150%;" />

可以看出，此时c的结果发生了改变，已经变成2了。这就是浅拷贝的意思。**只拷贝了最浅层的对象，当对象里面嵌套的对象发生改变时，其内部值也会发生改变，没有达到完全隔离的效果**，**只实现了浅层的拷贝**。下面我们一起来看一下如何实现深层拷贝。

### 引用类型深拷贝

实现深层拷贝方式有两种，一种是通过**JSON**的方式，另外一种是通过**类型判断和递归**的方式。我们分别来实现一下。

#### JSON方式

JSON有两个方法`JSON.parse()`和`JSON.stringify()`，前者可以将JSON字符串转换成JavaScript对象，后者可以将JavaScript对象转换成JSON字符串。

所以解决思路就是先将**引用类型数据变成JSON**，**再将其从JSON变回来**，这样经过这样一层转换，就会要求计算机从内存中重新开辟一块新的内存空间。Foo和newFoo之间就不会发生任何联系了。我们通过代码来去具体看一下。

```js
let Foo = {
    a: {
      c:1
    },
    b: 4
}

let str = JSON.stringify(Foo)
let newFoo = JSON.parse(str)

newFoo.a.c = 5
```

<img src="https://img-blog.csdnimg.cn/20201030152230770.png#pic_center" alt="在这里插入图片描述" style="zoom:150%;" />

#### 通过递归的方式实现

之前JSON方式实现深拷贝就是靠着返回**一个独立的对象来实现深拷贝**，受此启发，如果要是我们自己来实现深拷贝的话，我们可以将原来的数据完全拷贝之后，**然后返回一个独立的新对象**，这样的话，也可以实现深拷贝。

如果要实现完全遍历的话，就要考虑Object的不同具体数据类型了，**虽然`{}`和`[]`都是Object类型**，但显然生成的对象是不一样的，当然，可能Object还可能有别的数据类型的样子，这里我们就以这两个为例，来去自己动手实现深拷贝。

那么首先需要写一个数据类型判断的函数：

```js
let checkType = data => {
    return Object.prototype.toString.call(data).slice(8, -1)
}
```

这里我们使用Object的toString方法来去判断data的数据类型，后面的slice是返回值结果进行一定的截取，保留我们想要的结果。

```js
let deepClone = target => {
    let targetType = checkType(target)
    let result
    // 初始化操作
    if (targetType === 'Object') {
        result = {}
    } else if (targetType === 'Array') {
        result = []
    } else {
        // 都不是的话证明是基本数据类型，基本数据
        // 类型只会有一个值，所以直接返回这个值就可以了
        return target
    }
    // target不是基本类型，进入遍历
    for (let i in target) {
        let value = target[i]
        let valueType = checkType(value)
        if (valueType === 'Object' || valueType === 'Array') {
            result[i] = deepClone(value) // 递归
        } else {
            // 是基本类型直接赋值\
            
            result[i] = value
        }
    }
    return result
}
```



















