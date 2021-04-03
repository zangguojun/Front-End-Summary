## CSS布局的五种定位方式

### 1、static（静态定位）：

默认值。没有定位，元素出现在正常的流中（忽略 top, bottom, left, right 或者 z-index 声明）。

### 2、relative（相对定位）：

> 定位为relative的元素**脱离正常的文档流**，不确定！！！
>
> **MDN上：**
>
> 该关键字下，元素先放置在**未添加定位时**的位置，再在不改变页面布局的前提下调整元素位置（因此会在此元素未添加定位时所在位置留下空白）。position:relative 对 table-*-group, table-row, table-column, table-cell, table-caption 元素无效。

但其在**文档流中的位置依然存在**，**只是视觉上相对原来的位置有移动**。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>相对定位</title>
    <style>
    .static1{ 
        height:80px; 
        background-color: red;
    } 
    .relative{ 
        height:80px; 
        position:relative; 
        top:40px; 
        left:40px; 
        background-color: black;
    } 
    .static2{ 
        height:80px; 
        background-color: blue;
    }
    </style>
</head>
    <body>
        <div class="static1"></div>
        <div class="relative"></div>
        <div class="static2"></div>
    </body>
</html>
```

![image-20210307154108348](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210307154109.png)

通过top,bottom,left,right的设置相对于其正常（原先本身）位置进行定位。可通过z-index进行层次分级 。

他是默认参照父级的原始点为原始点（**父级不是必须设定position属性**），无论父级存在不存在，无论有没有TRBL，均是以父级的左上角进行定位，**但是父级的Padding属性会对其影响**。

**无父级则以文本流的顺序在上一个元素的底部为原始点**

```css
.static1{ 
    height:80px; 
    background-color: red;
    /* margin-bottom: 100px; */
    /* padding-bottom: 100px; */
    /* border-bottom: 100px; */
    /* 会让relative下移 */
} 
.father {
    background-color: green;
    /* margin: 10px; */
    /* 会让relative右移 */
    /* padding: 10px; */
    /* 会让relative右移 */
    /* border: 10px solid yellow; */
    /* 会让relative右移 */
}
```

```html
<div class="static1"></div>
<div class="father">
    <div class="relative"></div>
</div>
<div class="static2"></div>
```

### 3、absolute（绝对定位）：

定位为absolute的层**脱离正常文档流**，但与relative的区别是其**在正常流中的位置不再存在**。生成绝对定位的元素，相对于 **static 以外定位的第一个父元素**进行定位。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>绝对定位</title>
    <style>
        .static1 {
            height: 80px;
            background-color: red;
        }

        .father {
            height: 100px;
            background-color: pink;
            position: relative;
            left: 20px;
        }

        .relative {
            height: 80px;
            width: 80px;
            position: absolute;
            top: 10px;
            left: 10px;
            background-color: black;
        }

        .static2 {
            height: 80px;
            background-color: blue;
        }
    </style>
</head>

<body>
    <div class="static1"></div>
    <div class="father">
        <div class="relative"></div>
    </div>
    <div class="static2"></div>
</body>

</html>
```



1，如果没有TRBL(top、right、bottom、left)，以父级的左上角，在没有父级的时候，参照浏览器左上角。

2，如果设定TRBL，并且父级没有设定position属性（position:static;不算设定了属性），那么当前的absolute则以浏览器左上角为原始点进行定位，位置将由TRBL决定。

3，如果设定TRBL，并且父级设定position属性(无论是absolute还是relative)，则以父级的左上角为原点进行定位，位置由 TRBL决定。**即使父级有Padding属性，对其也不起作用**。

```css
.static1 {
    height: 80px;
    background-color: red;
    /* margin-bottom: 100px; */
    /* padding-bottom: 100px; */
    /* border-bottom: 100px; */
    /* 会让relative下移 */
}

.father {
    height: 100px;
    background-color: pink;
    position: relative;
    left: 20px;
    /* margin-left: 10px; */
    /* 会让relative右移 */
    /* border: 100px solid yellow; */
    /* 会让relative右移 */
    /* padding-left: 10px; */
    /* 不会让relative右移 */
}
```

元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。可通过z-index进行层次分级。

### 4、fixed（固定定位）：

生成固定定位的元素，相对于浏览器窗口进行定位。元素的位置通过 "left", "top", "right" 以及 "bottom" 属性进行规定。可通过z-index进行层次分级。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>固定定位</title>
    <style>
        .static1 {
            height: 80px;
            background-color: red;
        }

        .father {
            height: 100px;
            width: 300px;
            background-color: pink;
            left: 100px;
            top: 100px;
        }

        .relative {
            height: 80px;
            width: 80px;
            position: fixed;
            /* left: 20px; */
            background-color: black;
        }

        .static2 {
            height: 80px;
            background-color: blue;
        }
    </style>
</head>

<body>
    <div class="static1"></div>
    <div class="father">
        <div class="relative"></div>
    </div>
    <div class="static2"></div>
</body>

</html>
```

1、如果没有TRBL(top、right、bottom、left)，默认参照父级的原始点为原始点（父级不是必须设定position属性）。

2、如果设定TRBL，相对于浏览器窗口进行定位。

### z-index属性

见本目录下`层叠上下文.md`

z-index，又称为对象的**层叠顺序**，它用一个整数来定义堆叠的层次，整数值越大，则被层叠在越上面，当然这是指同级元素间的堆叠，如果两个对象的此属性具有同样的值，那么将依据它们在HTML文档中流的顺序层叠，写在后面的将会覆盖前面的。需要注意的是，**父子关系是无法用z-index来设定上下关系 的，一定是子级在上父级在下**。

Note：使用static 定位或无position定位的元素z-index属性是无效的

### 总结

+ 四种定位方式（position）：
  + 静态定位（默认） static
  + 相对定位 relative
  + 绝对定位 absolute
  + 固定定位 fixed

> 只有静态定位（默认方式）不会脱离文档流，其他3种定位方式都会脱离文档流。

+ 关于定位上下文的总结：
  + static: 没有定位上下文，所以 top bottom left right 属性无效。
  + relative: 定位上下文是该元素原先的位置。
  + absolute: 定位上下文是该元素的“设置了 relative 定位的最小级祖先容器"，如果该元素的所有祖先容器都没有设置 relative 定位，则定位上下文是 body元素。
  + fixed：定位上下文是窗口（浏览器窗口或手持设备的屏幕）。

+ top bottom left right 属性都是相对于该元素的定位上下文而言的。
  + 由于 static 定位没有定位上下文，所以为 static 定位的元素设置 top bottom left right 属性将无效
  + 只有为 relative, absolute, fixed 定位的元素设置 top bottom left right 属性才有效
+ absolute 定位和 fixed定位的区别是： 
  + 定位上下文不同，导致 absolute 定位的元素会随着文档的滚动而一同滚动， fixed 定位的元素不会随着文档的滚动而一同滚动。

### 事件触发位置的结论

任何定位下，为元素设定的事件的**触发位置**，都是该元素在页面上的当前显示位置（**而不是该元素的原始位置—对于相对定位的元素来说**）

### 5、sticky（粘性定位）

效果是一个吸顶效果，可以说是**相对定位relative和固定定位fixed的结合**；它主要用在对scroll事件的监听上；

#### top/bottom属性

top,bottom的距离阈值取决于最近一个overflow是hidden scroll auto或 overlay 的祖先元素（并不是相对于viewport 视口）。

例如我们设置top:100px时，

在元素滚动到距离祖先元素顶部**小于设置的top:100px之前，元素为相对定位**。

当滚动到超过或等于top:100px时，元素将**固定**在与**祖先元素顶部距离 top:100px 的相对位置**，直到距离回滚到阈值以下。

根据下面的例子，我们可以看到position:sticky元素设置的top值是最近一个设置了overflow的祖先元素的距离，当它滚到距离低于其值时将固定在视口当中，此时该元素的效果就为固定定位。

```html
<!DOCTYPE html>
<html>

<head>
    <title></title>
    <style>
        .container {
            display: flex;
        }

        .right {
            border: 1px solid red;
            width: 100%;
            margin-top: 40px;
            overflow: scroll;
            height: 400px;
        }

        nav {
            position: sticky;
            top: 100px;
        }
    </style>
</head>

<body>
    <div class="container">
        <div class="right">
            <h1 style="height:200px;">顶部高200px 红线为中线</h1>
            <nav style="background: red;">这是导航 top:100px</nav>
            <p>滚一个</p>
            <p>滚一个</p>
            ..........
            <p>滚一个</p>
            <p>滚一个</p>
            <p>到底啦！</p>
        </div>
    </div>
</body>
</html>
```

![image-20210307162037601](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210307162038.png)

![image-20210307162051044](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210307162051.png)





