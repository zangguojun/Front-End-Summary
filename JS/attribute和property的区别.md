# attribute和property的区别

property 和 attribute非常容易混淆，两个单词的中文翻译也都非常相近（property：属性，attribute：特性），但实际上，二者是不同的东西，属于不同的范畴。

- property是DOM中的属性，是JavaScript里的对象；
- attribute是HTML标签上的特性，它的值只能够是字符串；

## attribute和property介绍

简单理解，Attribute就是dom节点自带的属性，例如html中常用的id、class、title、align等。

而Property是这个DOM元素作为对象，其附加的内容，例如childNodes、firstChild等。

有以下代码：

```html
<div id="div1" class="divClass" title="divTitle" title1="divTitle1"></div>

var in1=document.getElementById("div1");
console.log(in1);
```

对于id为div1的div，它的property内容如下：（部分）

![img](https://images2018.cnblogs.com/blog/1264915/201804/1264915-20180409144407228-1277683674.png)

可以发现有一个名为“attributes”的属性，类型是NamedNodeMap，同时有“id”和“className”、”title“等基本的属性，但没有“title1”这个自定义的属性。

```js
console.log(in1.id);               	//div1
console.log(in1.className);        	//divClass
console.log(in1.title);            	//divTitle
console.log(in1.title1);       		//undefined
```

可以发现，标签中的属性，“id”和“className”、”title“会在in1上创建，而“titles”不会被创建。这是由于，每一个DOM对象都会有它默认的基本属性，而在创建的时候，它只会创建这些基本属性，我们在TAG标签中自定义的属性是不会直接放到DOM中的。

那自定义的”title1“去哪里了呢？在attributes属性里可以看到如下：

![img](https://images2018.cnblogs.com/blog/1264915/201804/1264915-20180409150827148-16914777.png)

“title1”被放在了attributes这个对象里，这个对象按顺序记录了我们在TAG中定义的属性和属性的数量。

从这里就可以看出，**attributes是属于property的一个子集**，它保存了HTML标签上定义属性。如果再进一步探索attitudes中的每一个属性，会发现它们并不是简单的对象，它是一个Attr类型的对象，拥有NodeType、NodeName等属性。关于这一点，稍后再研究。注意，打印attribute属性不会直接得到对象的值，而是获取一个包含属性名和值的字符串，如：

```js
console.log(in1.attibutes.title1);        // divTitle1
```

由此可以得出：

> - HTML标签中定义的属性和值会保存该DOM对象的attributes属性里面；
> - 这些attribute属性在JavaScript中的类型是Attr，而不仅仅是保存属性名和值这么简单；

再如下：

```html
<input id="in_2">
```

在它的property中有如下部分：

![img](https://images2018.cnblogs.com/blog/1264915/201804/1264915-20180409150333193-1009214473.png)

![img](https://images2018.cnblogs.com/blog/1264915/201804/1264915-20180409150400726-535275682.png)

尽管我们没有在TAG中定义“value”，但由于它是DOM默认的基本属性，在DOM初始化的时候它照样会被创建。

## “脚踏两只船”

常用的Attribute，例如id、class、title等，已经被作为Property附加到DOM对象上，可以和Property一样取值和赋值。但是自定义的Attribute，就不会有这样的特殊优待，例如：

```html
<div id="div1" class="divClass" title="divTitle" title1="divTitle1">100</div>
```

这个div里面的“title1”就不会变成Property。

即，只要是DOM标签中出现的属性（html代码），都是Attribute。然后有些常用特性（id、class、title等），会被转化为Property。可以很形象的说，这些特性/属性，是“脚踏两只船”的。

![img](https://images0.cnblogs.com/i/138012/201403/282211042979059.png)

![img](https://images0.cnblogs.com/i/138012/201403/282211130472053.png)

最后注意：“class”变成Property之后叫做“className”，因为“class”是ECMA的关键字。

> - DOM有其默认的基本属性，而这些属性就是所谓的“property”，无论如何，它们都会在初始化的时候再DOM对象上创建。
> - 如果在TAG对这些属性进行赋值，那么这些值就会作为初始值赋给DOM的同名property。

## attribute和property的取值和赋值

### 1、attribute取值

《js高级程序设计》中提到，为了方便操作，建议大家用setAttribute()和getAttribute()来操作即可。

```html
<div id="div1" class="divClass" title="divTitle" align="left" title1="divTitle1"></div>
```

```js
var id = div1.getAttribute("id");              
var className1 = div1.getAttribute("class");
var title = div1.getAttribute("title");
var title1 = div1.getAttribute("title1");   //自定义特性
```

`getAttribute()`可以取得任何特性，不管是标准的还是自定义的。

但是**这个方法的浏览器兼容性有问题**，有些浏览器可能会获取属性Property的值，因此jQuery要做一个测试，看getAttribute()是否是绝对获取特性Attribute的值。

```js
div1.className = 'a';
var judge = div1.getAttribute("className") === 'a';
```

如果以上代码成立，说明getAttribute()方法出现了问题，将不再使用。

### 2、attribute赋值

```js
div1.setAttribute('class', 'a');
div1.setAttribute('title', 'b');
div1.setAttribute('title1', 'c');
div1.setAttribute('title2', 'd');
```

用setAttrbute()赋值，任何Attribute都可以，包括自定义的。而且，赋值的Attribute会立刻表现到DOM元素上。

![img](https://images0.cnblogs.com/i/138012/201403/282227103289497.png)

如果是标准特性，也会更新它们关联的属性的值：

![img](https://images0.cnblogs.com/i/138012/201403/282228476091011.png)

**最后注意，setAttribute()的两个参数，都必须是字符串。**即对特性Attribute只能赋值字符串，而对属性Property就可以赋任何类型的值了。

### 3、property取值

属性取值很简单。取任何属性的只，用“.”就可以：

```js
var id = div1.id;
var className = div1.className;
var childNodes = div1.childNodes;
var attrs = div1.attributes;
```

此处再次强调：

1. class特性在变成属性时，名字改成了“className”，因此div1.className和div1.getAttrbute('class')相同。
2. 上面代码中的div1.attributes是取的attributes这一属性，取出来保存到attrs变量中，attrs就成了一个NamedNodeList类型的对象，里面存储了若干个Attr类型。

###  **4. Property赋值**

赋值和基本的js对象属性赋值一样，用“.”即可：

```js
div1.className = 'a';
div1.align = 'center';
div1.AAAAA = true;
div1.BBBBB = [1, 2, 3];
```

**对属性Property可以赋任何类型的值，而对特性Attribute只能赋值字符串！**

另外，对于属性Property的赋值在IE中可能会引起**循环引用**，内存泄漏。为了防止这个问题，jQuery.data()做了特殊处理，**解耦了数据和DOM对象**。

## 更改property和attribute其中一个值，会出现什么结果

```js
in1.value='new value of prop';
console.log(in1.value);               // 'new value of prop'
console.log(in1.attributes.value);         // 'value="1"'
```

此时，页面中的输入栏的值变成了“new value of prop”，而propety中的value也变成了新的值，但attributes却仍然是“1”。从这里可以推断，property和attribute的同名属性的值并不是双向绑定的。

如果反过来，设置attitudes中的值，效果会怎样呢？

```js
in2.setAttribute('value','ni')
console.log(in2.value);          	//ni
console.log(in2.attributes.value); 	//value='ni'
```

由此，可得出结论：

> - property能够从attribute中得到同步；
> - attribute不会同步property上的值；
> - attribute和property之间的数据绑定是单向的，attribute->property；
> - 更改property和attribute上的任意值，都会将更新反映到HTML页面中；

### 总结

attribute是dom元素在文档中作为html标签拥有的属性； 

property就是dom元素在js中作为对象拥有的属性。 

所以

对于html的标准属性来说，attribute和property是同步的，是会自动更新的， 

但是对于自定义的属性来说，他们是不同步的，









