### 第一种：改变原数组

借用原生数组方法：splice(index,len,[item]) **剪接**

借用原生对象方法：delete array[index] + array.slice(0, index).concat(array.slice(index, array.length-1))

小结：

**delete**：只是将该元素变为undefined（数组大小并没有改变）。 

**splice**：可以对数组进行增、删、改操作。



### 第二种：返回新数组slice

巧用原生数组方法：

```js
array.slice(0, num-1).concat(array.slice(num))
```

