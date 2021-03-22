## 判断数组的方法

### 1、从构造函数入手:

#### obj instanceof Array

**instanceof**：用于检测**构造函数**的 `prototype` 属性是否出现在某个实例对象的原型链上。



### 2、从原型入手:

#### Array.prototype.isPrototypeOf(obj);

isPrototypeOf:用于测试**一个对象**是否存在于另一个对象的**原型链**上



### 3、跨原型链调用：

#### Object.prototype.toString.call(obj)



### 4、ES5新增方法：

### Array.isArray(obj)







