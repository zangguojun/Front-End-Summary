setTimeout(loop,3000)和setInterval(loop,3000)这两个函数的作用都是隔3秒执行loop这个函数，

两者的差距的是：setTimeout()只执行一次那段代码，而setInterval()在执行完一次代码之后，经过了3秒，它还会自动重复执行loop函数（类似于递归）

1、setInterval(loop,3000)，在执行完一次代码后，它自己会自动执行

```js
setInterval(loop,3000);
function loop(){
    alert("需要执行的代码");
}
```

2、如果setTimeout(loop,3000);需要进行循环执行的话,需要使用递归的方法 

```js
function loop(){
    alert("执行的代码");
    setTimeout(loop,3000);
}
```

 其中两者的区别是，**setInterval(loop,3000)是经过3秒执行一次**，不会随着loop里面函数的变化而变化，但是**setTimeout(loop,3000)**

表示loop函数执行完后，**经过三秒后在执行loop**，如果loop函数执行需要1秒，**那么整个循环执行之间相隔4秒**。

