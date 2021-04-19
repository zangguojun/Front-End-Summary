# JS宏任务和微任务

### **宏任务**

| #                       | 浏览器 | Node |
| ----------------------- | ------ | ---- |
| `I/O`                   | ✅      | ✅    |
| `setTimeout`            | ✅      | ✅    |
| `setInterval`           | ✅      | ✅    |
| `setImmediate`          | ❌      | ✅    |
| `requestAnimationFrame` | ✅      | ❌    |

### **微任务**

| #                            | 浏览器 | Node |
| ---------------------------- | ------ | ---- |
| `process.nextTick`           | ❌      | ✅    |
| `MutationObserver`           | ✅      | ❌    |
| `Promise.then catch finally` | ✅      | ✅    |

这个例子看懂基本js执行机制就理解了

### 例1

```js
setTimeout(() => {
    //执行后 回调一个宏事件
    console.log('内层宏事件3')
}, 0)
console.log('外层宏事件1');

new Promise((resolve) => {
    console.log('外层宏事件2');
    resolve()
}).then(() => {
    console.log('微事件1');
}).then(()=>{
    console.log('微事件2')
})
```

我们看看打印结果:

```
外层宏事件1
外层宏事件2
微事件1
微事件2
内层宏事件3
```

• 首先浏览器执行js，让第一个宏任务进入主线程, 遇到 **setTimeout** 分发到宏任务Event Queue中

• 遇到 **console.log()** 直接执行 输出 `外层宏事件1`

• 遇到 **Promise**， **new Promise** 直接执行 输出 `外层宏事件2`

• 执行**then** 被分发到微任务**Event Queue**中

•第一轮宏任务执行结束，开始执行微任务 打印 `微事件1` `微事件2`

•第一轮微任务执行完毕，执行第二轮宏事件，打印**setTimeout**里面内容 `内层宏事件3`

### 例2

```js
//主线程直接执行
console.log('1');
//丢到宏事件队列中
setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
//微事件1
process.nextTick(function() {
    console.log('6');
})
//主线程直接执行
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    //微事件2
    console.log('8')
})
//丢到宏事件队列中
setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})
```

我们看看打印结果:

```
1
7
6
8
2
4
3
5
9
11
10
12
```

•  首先浏览器执行js,让第一个宏任务进入主线程, 直接打印**console.log('1')**

• 遇到 **setTimeout** 分发到宏任务Event Queue中

• 遇到 **process.nextTick** 丢到微任务Event Queue中

• 遇到 **Promise**， **new Promise** 直接执行 输出 **console.log('7');**

• 执行**then** 被分发到微任务Event Queue中

•第一轮宏任务执行结束，开始执行微任务 打印 **6,8**

•第一轮微任务执行完毕，执行第二轮宏事件，执行**setTimeout**

•先执行主线程宏任务，在执行微任务，打印**2,4,3,5**

•在执行第二个**setTimeout**,同理打印 **9,11,10,12**

### 解释

**所有会进入的异步都是指的事件回调中的那部分代码**
也就是说`new Promise`在实例化的过程中所执行的代码都是同步进行的，而`then`中注册的回调才是异步执行的。在同步代码执行完成后才回去检查是否有异步任务完成，并执行对应的回调，而微任务又会在宏任务之前执行。

**部分表示同步执行的代码**

```js
+setTimeout(_ => {
-  console.log(4)
+})

+new Promise(resolve => {
+  resolve()
+  console.log(1)
+}).then(_ => {
-  console.log(3)
+})

+console.log(2)
```

### 例3

```js
setTimeout(_ => console.log(4))

new Promise(resolve => {
  resolve()
  console.log(1)
}).then(_ => {
  console.log(3)
  Promise.resolve().then(_ => {
    console.log('before timeout')
  }).then(_ => {
    Promise.resolve().then(_ => {
      console.log('also before timeout')
    })
  })
})

console.log(2)
```

我们看看打印结果:

```
1
2
3
before timeout 
also before timeout
4
```

## Event-Loop

> macrotask 和 microtask 表示异步任务的两种分类。
>
> `JavaScript`是一个单进程的语言，同一时间不能处理多个任务，所以何时执行宏任务，何时执行微任务？我们需要有这样的一个判断逻辑存在。

在挂起任务时，JS 引擎会将所有任务按照类别分到这两个队列中，首先在 macrotask 的队列（这个队列也被叫做 task queue）中取出第一个任务，执行完毕后取出 microtask 队列中的所有任务顺序执行（***检查还有没有微任务需要处理***）；之后再取 macrotask 任务（***结束本次宏任务、检查还有没有宏任务需要处理***），周而复始，直至两个队列的任务都取完。

这个检查的过程是持续进行的，每完成一个任务都会进行一次，而这样的操作就被称为`Event Loop`。

首先要明确的一点是，宏任务必然是在微任务之后才执行的（因为微任务实际上是宏任务的其中一个步骤）`I/O`这一项感觉有点儿笼统，有太多的东西都可以称之为`I/O`，点击一次`button`，上传一个文件，与程序产生交互的这些都可以称之为`I/O`。

```html
<style>
  #outer {
    padding: 20px;
    background: #616161;
  }

  #inner {
    width: 100px;
    height: 100px;
    background: #757575;
  }
</style>

<div id="outer">
  <div id="inner"></div>
</div>
<script>
    const $inner = document.querySelector('#inner')
    const $outer = document.querySelector('#outer')

    function handler () {
      console.log('click') // 直接输出
      Promise.resolve().then(_ => console.log('promise')) // 注册微任务
      setTimeout(_ => console.log('timeout')) // 注册宏任务
      requestAnimationFrame(_ => console.log('animationFrame')) // 注册宏任务
      $outer.setAttribute('data-random', Math.random()) // DOM属性修改，触发微任务
    }
    new MutationObserver(_ => {
      console.log('observer')
    }).observe($outer, {
      attributes: true
    })
    $inner.addEventListener('click', handler)
    $outer.addEventListener('click', handler)
</script>
```

执行顺序：

```
click
promise 
observer
click
promise 
observer
animationFrame
animationFrame
timeout 
timeout 
```

因为一次`I/O`创建了一个宏任务，也就是说在这次任务中会去触发`handler`。
按照代码中的注释，在同步的代码已经执行完以后，这时就会去查看是否有微任务可以执行，然后发现了`Promise`和`MutationObserver`两个微任务，遂执行之。

因为`click`事件会冒泡，所以对应的这次`I/O`会触发两次`handler`函数(*一次在`inner`、一次在`outer`*)，所以会优先执行冒泡的事件(*早于其他的宏任务*)，也就是说会重复上述的逻辑。

在执行完同步代码与微任务以后，这时继续向后查找有木有宏任务。
需要注意的一点是，因为我们触发了`setAttribute`，实际上修改了`DOM`的属性，这会导致页面的重绘，而这个`set`的操作是同步执行的，也就是说`requestAnimationFrame`的回调会早于`setTimeout`所执行。

### 一些小惊喜

使用上述的示例代码，如果将手动点击`DOM`元素的触发方式变为`$inner.click()`，那么会得到不一样的结果。
在`Chrome`下的输出顺序大致是这样的：

执行顺序：

```
click
click
promise
observer
promise
observer
animationFrame
animationFrame
timeout 
timeout 
```

与我们手动触发`click`的执行顺序不一样的原因是这样的，因为并不是用户通过点击元素实现的触发事件，而是类似`dispatchEvent`这样的方式，我个人觉得并不能算是一个有效的`I/O`，在执行了一次`handler`回调注册了微任务、注册了宏任务以后，实际上外边的`$inner.click()`并没有执行完。
所以在微任务执行之前，还要继续冒泡执行下一次事件，也就是说触发了第二次的`handler`。
所以输出了第二次`click`，等到这两次`handler`都执行完毕后才会去检查有没有微任务、有没有宏任务。

两点需要注意的：

+ `.click()`的这种触发事件的方式个人认为是类似`dispatchEvent`，可以理解为同步执行的代码

```js
document.body.addEventListener('click', _ => console.log('click'))
document.body.click()
document.body.dispatchEvent(new Event('click'))
console.log('done')

// > click
// > click
// > done
```

+ `MutationObserver`的监听不会说同时触发多次，多次修改只会有一次回调被触发。

```js
new MutationObserver(_ => {
  console.log('observer')
  // 如果在这输出DOM的data-random属性，必然是最后一次的值，不解释了
}).observe(document.body, {
  attributes: true
})

document.body.setAttribute('data-random', Math.random())
document.body.setAttribute('data-random', Math.random())
document.body.setAttribute('data-random', Math.random())

// 只会输出一次 ovserver
//这就像去饭店点餐，服务员喊了三次，XX号的牛肉面，不代表她会给你三碗牛肉面。
```



![img](https://images2018.cnblogs.com/blog/1053223/201808/1053223-20180831162152579-2034514663.png)

+ 同步和异步任务分别进入不同的执行"场所"，同步的进入主线程，异步的进入Event Table并注册函数。
+ 当指定的事情完成时，Event Table会将这个函数移入Event Queue。
+ 主线程内的任务执行完毕为空，会去Event Queue读取对应的函数，进入主线程执行。
+ 上述过程会不断重复，也就是常说的Event Loop(事件循环)。

我们不禁要问了，那怎么知道主线程执行栈为空啊？js引擎存在monitoring process进程，会持续不断的检查主线程执行栈是否为空，一旦为空，就会去Event Queue那里检查是否有等待被调用的函数。

说了这么多文字，不如直接一段代码更直白：

```javascript
let data = [];
$.ajax({
    url:www.javascript.com,
    data:data,
    success:() => {
        console.log('发送成功!');
    }
})
console.log('代码执行结束');
```

上面是一段简易的`ajax`请求代码：

- ajax进入Event Table，注册回调函数`success`。
- 执行`console.log('代码执行结束')`。
- ajax事件完成，回调函数`success`进入Event Queue。
- 主线程从Event Queue读取回调函数`success`并执行。

相信通过上面的文字和代码，你已经对js的执行顺序有了初步了解。接下来我们来研究进阶话题：setTimeout。

## 又爱又恨的setTimeout

大名鼎鼎的`setTimeout`无需再多言，大家对他的第一印象就是异步可以延时执行，我们经常这么实现延时3秒执行：

```javascript
setTimeout(() => {
    console.log('延时3秒');
},3000)
```

渐渐的`setTimeout`用的地方多了，问题也出现了，有时候明明写的延时3秒，实际却5，6秒才执行函数，这又咋回事啊？

先看一个例子：

```javascript
setTimeout(() => {
    task();
},3000)
console.log('执行console');
```

根据前面我们的结论，`setTimeout`是异步的，应该先执行`console.log`这个同步任务，所以我们的结论是：

```javascript
//执行console
//task()
```

去验证一下，结果正确！ 然后我们修改一下前面的代码：

```javascript
setTimeout(() => {
    task()
},3000)

sleep(10000000)
```

乍一看其实差不多嘛，但我们把这段代码在chrome执行一下，却发现控制台执行`task()`需要的时间远远超过3秒，说好的延时三秒，为啥现在需要这么长时间啊？

这时候我们需要重新理解`setTimeout`的定义。我们先说上述代码是怎么执行的：

- `task()`进入Event Table并注册,计时开始。
- 执行`sleep`函数，很慢，非常慢，计时仍在继续。
- 3秒到了，计时事件`timeout`完成，`task()`进入Event Queue，但是`sleep`也太慢了吧，还没执行完，只好等着。
- `sleep`终于执行完了，`task()`终于从Event Queue进入了主线程执行。

上述的流程走完，我们知道`setTimeout`这个函数，是经过指定时间后，把要执行的任务(本例中为`task()`)加入到Event Queue中，又因为是**单线程任务要一个一个执行**，如果**前面的任务需要的时间太久**，那么只能等着，导致真正的延迟时间远远大于3秒。

我们还经常遇到`setTimeout(fn,0)`这样的代码，0秒后执行又是什么意思呢？是不是可以立即执行呢？

答案是不会的，`setTimeout(fn,0)`的含义是，指定某个任务在主线程最早可得的空闲时间执行，意思就是不用再等多少秒了，只要主线程执行栈内的同步任务全部执行完成，栈为空就马上执行。举例说明：

```javascript
//代码1
console.log('先执行这里');
setTimeout(() => {
    console.log('执行啦')
},0);
//代码2
console.log('先执行这里');
setTimeout(() => {
    console.log('执行啦')
},3000);  
```

代码1的输出结果是：

```javascript
//先执行这里
//执行啦
```

代码2的输出结果是：

```javascript
//先执行这里
// ... 3s later
// 执行啦
```

关于`setTimeout`要补充的是，即便主线程为空，0毫秒实际上也是达不到的。根据HTML的标准，最低是4毫秒。有兴趣的同学可以自行了解。

## 又恨又爱的setInterval

上面说完了`setTimeout`，当然不能错过它的孪生兄弟`setInterval`。他俩差不多，只不过后者是循环的执行。对于执行顺序来说，`setInterval`会每隔指定的时间将注册的函数置入Event Queue，如果前面的任务耗时太久，那么同样需要等待。

唯一需要注意的一点是，对于`setInterval(fn,ms)`来说，我们已经知道不是每过`ms`秒会执行一次`fn`，而是每过`ms`秒，会有`fn`进入Event Queue。一旦**`setInterval`的回调函数`fn`执行时间超过了延迟时间`ms`，那么就完全看不出来有时间间隔了**。这句话请读者仔细品味。

## Promise与process.nextTick(callback)

传统的定时器我们已经研究过了，接着我们探究`Promise`与`process.nextTick(callback)`的表现。

`Promise`的定义和功能本文不再赘述，不了解的读者可以学习一下阮一峰老师的[Promise](http://es6.ruanyifeng.com/#docs/promise)。而`process.nextTick(callback)`类似node.js版的"setTimeout"，在事件循环的下一次循环中调用 callback 回调函数。

我们进入正题，除了广义的同步任务和异步任务，我们对任务有更精细的定义：

- macro-task(宏任务)：包括整体代码script，setTimeout，setInterval
- micro-task(微任务)：Promise.then，process.nextTick

不同类型的任务会进入对应的Event Queue，比如`setTimeout`和`setInterval`会进入相同的Event Queue。

事件循环的顺序，决定js代码的执行顺序。进入整体代码(宏任务)后，开始第一次循环。接着执行所有的微任务。然后再次从宏任务开始，找到其中一个任务队列执行完毕，再执行所有的微任务。听起来有点绕，我们用文章最开始的一段代码说明：

### 例4

```javascript
setTimeout(function() {
    console.log('setTimeout');
})

new Promise(function(resolve) {
    console.log('promise');
}).then(function() {
    console.log('then');
})

console.log('console');

//promise console then
```

- 这段代码作为宏任务，进入主线程。
- 先遇到`setTimeout`，那么将其回调函数注册后分发到宏任务Event Queue。(注册过程与上同，下文不再描述)
- 接下来遇到了`Promise`，`new Promise`立即执行，`then`函数分发到微任务Event Queue。
- 遇到`console.log()`，立即执行。
- 好啦，整体代码script作为第一个宏任务执行结束，看看有哪些微任务？我们发现了`then`在微任务Event Queue里面，执行。
- ok，第一轮事件循环结束了，我们开始第二轮循环，当然要从宏任务Event Queue开始。我们发现了宏任务Event Queue中`setTimeout`对应的回调函数，立即执行。
- 结束。

### 例5

```js
console.log('1');

setTimeout(function() {
    console.log('2');
    process.nextTick(function() {
        console.log('3');
    })
    new Promise(function(resolve) {
        console.log('4');
        resolve();
    }).then(function() {
        console.log('5')
    })
})
process.nextTick(function() {
    console.log('6');
})
new Promise(function(resolve) {
    console.log('7');
    resolve();
}).then(function() {
    console.log('8')
})

setTimeout(function() {
    console.log('9');
    process.nextTick(function() {
        console.log('10');
    })
    new Promise(function(resolve) {
        console.log('11');
        resolve();
    }).then(function() {
        console.log('12')
    })
})
//1 7 6 8 2 4 3 5 9 11 10 12
```

第一轮事件循环流程分析如下：

- 整体script作为第一个宏任务进入主线程，遇到`console.log`，输出1。
- 遇到`setTimeout`，其回调函数被分发到宏任务Event Queue中。我们暂且记为`setTimeout1`。
- 遇到`process.nextTick()`，其回调函数被分发到微任务Event Queue中。我们记为`process1`。
- 遇到`Promise`，`new Promise`直接执行，输出7。`then`被分发到微任务Event Queue中。我们记为`then1`。
- 又遇到了`setTimeout`，其回调函数被分发到宏任务Event Queue中，我们记为`setTimeout2`。

| 宏任务Event Queue | 微任务Event Queue |
| ----------------- | ----------------- |
| setTimeout1       | process1          |
| setTimeout2       | then1             |

- 上表是第一轮事件循环宏任务结束时各Event Queue的情况，此时已经输出了1和7。
- 我们发现了`process1`和`then1`两个微任务。
- 执行`process1`,输出6。
- 执行`then1`，输出8。

好了，第一轮事件循环正式结束，这一轮的结果是输出1，7，6，8。那么第二轮时间循环从`setTimeout1`宏任务开始：

- 首先输出2。接下来遇到了`process.nextTick()`，同样将其分发到微任务Event Queue中，记为`process2`。`new Promise`立即执行输出4，`then`也分发到微任务Event Queue中，记为`then2`。

| 宏任务Event Queue | 微任务Event Queue |
| ----------------- | ----------------- |
| setTimeout2       | process2          |
|                   | then2             |

- 第二轮事件循环宏任务结束，我们发现有`process2`和`then2`两个微任务可以执行。
- 输出3。
- 输出5。
- 第二轮事件循环结束，第二轮输出2，4，3，5。
- 第三轮事件循环开始，此时只剩setTimeout2了，执行。
- 直接输出9。
- 将`process.nextTick()`分发到微任务Event Queue中。记为`process3`。
- 直接执行`new Promise`，输出11。
- 将`then`分发到微任务Event Queue中，记为`then3`。

| 宏任务Event Queue | 微任务Event Queue |
| ----------------- | ----------------- |
|                   | process3          |
|                   | then3             |

- 第三轮事件循环宏任务执行结束，执行两个微任务`process3`和`then3`。
- 输出10。
- 输出12。
- 第三轮事件循环结束，第三轮输出9，11，10，12。

整段代码，共进行了三次事件循环，完整的输出为1，7，6，8，2，4，3，5，9，11，10，12。 (请注意，node环境下的事件监听依赖libuv与前端环境不完全相同，输出顺序可能会有误差)

### 例6

```js
setTimeout(() => console.log('setTimeout1'), 0);  //1宏任务

setTimeout(() => {								//2宏任务
    console.log('setTimeout2');
    Promise.resolve().then(() => {
        console.log('promise3');
        Promise.resolve().then(() => {
            console.log('promise4');
        })
        console.log(5)
    })
    setTimeout(() => console.log('setTimeout4'), 0);  //4宏任务
}, 0);

setTimeout(() => console.log('setTimeout3'), 0);  //3宏任务
Promise.resolve().then(() => {//1微任务
    console.log('promise1');
})
//promise1 setTimeout1 setTimeout2 promise3 5 promise4  setTimeout3 setTimeout4
```

![img](https://img2018.cnblogs.com/blog/1308525/201901/1308525-20190113115517470-573104940.png)

![img](https://img2018.cnblogs.com/blog/1308525/201901/1308525-20190113115539778-486942377.png)

### 例7

```js
async function async1() {
    console.log( 'async1 start' )
    await async2()
    console.log( 'async1 end' )
}
async function async2() {
    console.log( 'async2' )
}
async1()
console.log( 'script start' )
```

执行结果

```
async1 start
async2
script start
async1 end
```

一旦遇到await 就立刻让出线程,阻塞后面的代码

等候之后,对于await来说分两种情况

- 不是promise 对象
- 是promise对象

**如果不是promise,await会阻塞后面的代码,先执行async外面的同步代码,同步代码执行完毕后,在回到async内部,把promise的东西,作为await表达式的结果**

**如果它等到的是一个 promise 对象，await 也会暂停async后面的代码，先执行async外面的同步代码，等着 Promise 对象 fulfilled，然后把 resolve 的参数作为 await 表达式的运算结果。**

**如果一个 Promise 被传递给一个 await 操作符，await 将等待 Promise 正常处理完成并返回其处理结果。**

![img](https://images2018.cnblogs.com/blog/1053223/201808/1053223-20180831162350437-143973108.png)

## 多提一嘴async/await函数

因为，`async/await`本质上还是基于`Promise`的一些封装，而`Promise`是属于微任务的一种。所以在使用`await`关键字与`Promise.then`效果类似：

```js
setTimeout(_ => console.log(4))

async function main() {
  console.log(1)
  await Promise.resolve()
  console.log(3)
}

main()

console.log(2)
// 1 2 3 4
```



**async函数在await之前的代码都是同步执行的，可以理解为await之前的代码属于`new Promise`时传入的代码，await之后的所有代码都是在`Promise.then`中的回调**



## 写在最后

#### (1)js的异步

我们从最开头就说javascript是一门单线程语言，不管是什么新框架新语法糖实现的所谓异步，其实都是用同步的方法去模拟的，牢牢把握住单线程这点非常重要。

#### (2)事件循环Event Loop

事件循环是js实现异步的一种方法，也是js的执行机制。

#### (3)javascript的执行和运行

执行和运行有很大的区别，javascript在不同的环境下，比如node，浏览器，Ringo等等，执行方式是不同的。而运行大多指javascript解析引擎，是统一的。

#### (4)setImmediate

微任务和宏任务还有很多种类，比如`setImmediate`等等，执行都是有共同点的，有兴趣的同学可以自行了解。

#### (5)最后的最后

- javascript是一门单线程语言
- Event Loop是javascript的执行机制



