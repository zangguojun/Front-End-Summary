## 浅比较

浅比较也称**引用相等**。

在javascript中 `===` 是做**浅比较**，只检查左右两边的对象是否**指向同一个引用**。

```js
const a = { x: 1 };
const b = { x: 1 };
console.log(a === b);
// false
```

> 此时 a 和 b指向的是不同的对象引用，所以打印值为 false

```js
const a = { x: 1 };
const b = a;
console.log(a === b);
// true
```

> 此时 a 和 b指向的是同一个对象的引用，所以打印值为 true

## 深比较

深比较会检查**两个对象的所有属性是否都相等**，需要用**递归的方式遍历两个对象的所有属性**，**不管对象是不是同一对象的引用**

## 比较两个对象是否相等

```js
var deepEqual = function(x, y) {
    // 指向同一内存
    if(x === y) {
        return true;
    }
    // 判断是否是对象
    if(typeof x === 'object' && x != null && typeof y === 'object' && y != null){
        // 判断对象属性的个数是否相等
        if(Object.keys(x).length !== Object.keys(y).length) {
            return false;
        }
        for(let prop in x) {
            if(y.hasOwnProperty(prop)) {
                if(!deepEqual(x[prop], y[prop])) {
                    return false;
                }
            } else {
                return false
            }
        }
        return true;
    } else {
        // 不是对象且不满足 === 则直接返回false
        return false;
    } 
}
```

