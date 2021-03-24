## 1、对象常量 

结合 writable:false 和 configurable:false 就可以创建一个真正的常量属性（不可修改、不可重定义或者不可删除）

```js
var myObject = {};
Object.defineProperty( myObject, "FAVORITE_NUMBER", {
    value: 42,
    writable: false,
    configurable: false
} );
```

## 2、禁止扩展

如 果 你 想 禁 止 一 个 对 象 添 加 新 属 性 并 且 保 留 已 有 属 性， 可 以 使 用 **Object.preventExtensions(..)**：

```js
var myObject = {
    a:2
};
Object.preventExtensions( myObject );
myObject.b = 3;
myObject.b; // undefined
```

在非严格模式下，创建属性 b 会**静默失败**。在严格模式下，将会**抛出 TypeError 错误**。



## 3、密封（禁止扩展+configurable)

`Object.seal(..)` 会创建一个“密封”的对象，这个方法实际上会在一个现有对象上调用 Object.preventExtensions(..) 并把**所有现有属性**标记为 `configurable:false`。 所以，密封之后不仅**不能添加新属性**，**也不能重新配置**或者**删除任何现有属性**（**虽然可以修改属性的值**）。

## 4. 冻结（密封+writable）

`Object.freeze(..)` 会创建一个冻结对象，这个方法实际上会在一个现有对象上调用 `Object.seal(..)` 并把所有“数据访问”属性标记为 `writable:false`，这样就无法修改它们的值。

这个方法是你可以应用在**对象上的级别最高**的不可变性，它会禁止对于对象本身及其任意直接属性的修改（不过就像我们之前说过的，这个对象引用的其他对象是不受影响的）。

你可以“**深度冻结**”一个对象，具体方法为，首先在这个对象上调用 `Object.freeze(..)`， 然后遍历它引用的所有对象并在这些对象上调用 `Object.freeze(..)`。但是一定要小心，因为这样做有可能会在无意中冻结其他（共享）对象。