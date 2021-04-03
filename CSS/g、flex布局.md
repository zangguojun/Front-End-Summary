## 1 基本概念

> Flex属性分为两部分，一部分作用于容器称容器属性，另一部分作用于项目称为项目属性。下面我们就分部的来介绍它们。

## 2 Flex容器属性

### 2.1 flex容器定义

```js
.box {
    display: flex; /* 或者 inline-flex */
}
```

> 上述写法，定义了一个flex容器，根据设值的不同可以是块状容器或内联容器。这使得直接子结点拥有了一个flex上下文



### 2.2 `flex-direction`

`flex-direction`属性决定主轴的方向（即项目的排列方向）。

**基本语法：**

```css
.box {
    flex-direction: row | row-reverse | column | column-reverse;
}
```

- `row` 表示从左向右排列
- `row-reverse` 表示从右向左排列
- `column` 表示从上向下排列
- `column-reverse` 表示从下向上排列

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        .box {
            display: flex;
            /* flex-direction: row; */
            /* flex-direction: row-reverse; */
            /* flex-direction: column; */
            flex-direction: column-reverse;
        }
        .child {
            background-color: red;
            margin: 5px;
            color: yellow;
        }
    </style>
</head>
<body>
    <div class="box">
        <div class="child">11111111</div>
        <div class="child">22222222</div>
        <div class="child">33333333</div>
    </div>
</body>
</html>
```



![image-20210401224603663](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225059.png)

![image-20210401224617866](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225142.png)

![image-20210401224634979](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225139.png)

![image-20210401224655372](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225137.png)

### 2.3 `flex-wrap`

缺省情况下，Flex项目都排在一条线（又称"轴线"）上。我们可以通过`flex-wrap`属性的设置，让Flex项目换行排列。

**基本语法：**

```css
.box {
    flex-wrap: nowrap | wrap | wrap-reverse;
}
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            .box {
                display: flex;
                flex-direction: row;
                /* flex-wrap: nowrap; */
                /* flex-wrap: wrap; */
                flex-wrap: wrap-reverse;
            }
            .child {
                background-color: red;
                margin: 5px;
                color: yellow;
                width: 50%;
            }
        </style>
    </head>
    <body>
        <div class="box">
            <div class="child">11111111</div>
            <div class="child">22222222</div>
            <div class="child">33333333</div>
        </div>
    </body>
</html>
```



![image-20210401225002734](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225128.png)

![image-20210401225018474](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225124.png)

![image-20210401225033062](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225037.png)

### 2.4 `flex-flow`

> flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap

**基本语法：**

```
.box {
    flex-flow: <‘flex-direction’> || <‘flex-wrap’>
}
```

### 2.5 `justify-content`

`justify-content`属性定义了项目在主轴上的对齐方式及额外空间的分配方式。

**基本语法：**

```css
.box  {
    justify-content: flex-start | flex-end | center | space-between | space-around | space-evenly;
}
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            .box {
                display: flex;
                flex-direction: row;
                /* justify-content: flex-start; */
                /* justify-content: flex-end; */
                /* justify-content: center; */
                /* justify-content: space-between; */
                /* justify-content: space-around; */
                /* justify-content: space-evenly; */
            }
            .child {
                background-color: red;
                margin: 5px;
                color: yellow;
                width: 10%;
            }
        </style>
    </head>
    <body>
        <div class="box">
            <div class="child">11111111</div>
            <div class="child">22222222</div>
            <div class="child">33333333</div>
        </div>
    </body>
</html>
```



- `flex-start`(缺省)：从启点线开始顺序排列
  - ![image-20210401225513749](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225515.png)
- `flex-end`：相对终点线顺序排列
  - ![image-20210401225546375](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225547.png)
- `center`：居中排列
  - ![image-20210401225443841](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225444.png)
- `space-between`：项目均匀分布，第一项在启点线，最后一项在终点线
  - ![image-20210401225642670](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225643.png)
- `space-around`：项目均匀分布，每一个项目两侧有相同的留白空间，相邻项目之间的距离是两个项目之间留白的和
  - ![image-20210401225658040](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225658.png)
- `space-evenly`：项目均匀分布，所有项目之间及项目与边框之间距离相等
  - ![image-20210401225725443](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401225726.png)

### 2.6 `align-items`

`align-items`属性定义项目在交叉轴上的对齐方式。

**基本语法：**

```css
.box {
    align-items: stretch | flex-start | flex-end | center | baseline;
}
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            .box {
                height: 100px;
                background-color: blue;
                display: flex;
                flex-direction: row;
                /* align-items: stretch; */
                /* align-items: flex-start; */
                /* align-items: flex-end; */
                /* align-items: center; */
                align-items: baseline;
            }
            .child {
                background-color: red;
                margin: 5px;
                color: yellow;
                width: 10%;
            }
        </style>
    </head>
    <body>
        <div class="box">
            <div class="child">11111111</div>
            <div class="child">22222222</div>
            <div class="child">33333333</div>
        </div>
    </body>
</html>
```

- `stretch`(缺省)：交叉轴方向拉伸显示
  - ![image-20210401232159199](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401232200.png)
- `flex-start`：项目按交叉轴起点线对齐
  - ![image-20210401232221026](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401232222.png)
- `flex-end`：项目按交叉轴终点线对齐
  - ![image-20210401232240083](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401232241.png)
- `center`：交叉轴方向项目中间对齐
  - ![image-20210401232300297](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401232301.png)
- `baseline`：交叉轴方向按第一行文字基线对齐
  - ![image-20210401232341294](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401232343.png)



### 2.7 `align-content`

`align-content`属性定义了在交叉轴方向的对齐方式及额外空间分配，类似于主轴上`justify-content`的作用。

**基本语法：**

```css
.box {
    align-content: stretch | flex-start | flex-end | center | space-between | space-around ;
}
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            .box {
                height: 100px;
                background-color: blue;
                display: flex;
                flex-flow: row wrap;
                /* align-content: stretch; */
                /* align-content: flex-start; */
                /* align-content: flex-end; */
                /* align-content: center; */
                /* align-content: space-between; */
                /* align-content: space-around; */
            }
            .child {
                background-color: red;
                margin: 5px;
                color: yellow;
                width: 40%;
            }
        </style>
    </head>
    <body>
        <div class="box">
            <div class="child">11111111</div>
            <div class="child">22222222</div>
            <div class="child">33333333</div>
        </div>
    </body>
</html>
```

- `stretch` (缺省)：拉伸显示
  - ![image-20210401232704987](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401232706.png)
- `flex-start`：从启点线开始顺序排列
  - ![image-20210401232736039](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401232736.png)
- `flex-end`：相对终点线顺序排列
  - ![image-20210401232755766](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401232756.png)
- `center`：居中排列
  - ![image-20210401232815323](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401232816.png)
- `space-between`：项目均匀分布，第一项在启点线，最后一项在终点线
  - ![image-20210401232836356](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401232837.png)
- `space-around`：项目均匀分布，每一个项目两侧有相同的留白空间，相邻项目之间的距离是两个项目之间留白的和
  - ![image-20210401232851498](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401232852.png)

## 3 Flex项目属性

### 3.1 `order`

缺省情况下，**Flex项目是按照在代码中出现的先后顺序排列的**。然而`order`属性可以**控制**项目在容器中的**先后顺序**。

**基本语法：**

```css
.item {
    order: <integer>; /* 缺省 0 */
}
```

按`order`值从小到大顺序排列，可以为负值，缺省为0。

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            .box {
                height: 100px;
                background-color: blue;
                display: flex;
                flex-direction: row;
            }
            .child {
                background-color: red;
                margin: 5px;
                color: yellow;
                width: 10%;
            }
            .d1 {
                order: 1;
            }
            .d2 {
                order: 0;
            }
            .d3 {
                order: 1;
            }
        </style>
    </head>
    <body>
        <div class="box">
            <div class="child d1">11111111</div>
            <div class="child d2">22222222</div>
            <div class="child d3">33333333</div>
        </div>
    </body>
</html>
```

### 3.2 `flex-grow`

`flex-grow`属性定义项目的放大比例，`flex-grow` 值是一个单位的**正整数**，表示**放大的比例**。**默认为0，即如果存在额外空间，也不放大，负值无效。**

如果所有项目的flex-grow属性都为1，则它们将等分剩余空间（如果有的话）。**如果一个项目的flex-grow属性为2，其他项目都为1，则前者占据的剩余空间将比其他项多一倍。**

**基本语法：**

```css
.item {
    flex-grow: <number>; /* 缺省 0 */
}
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            .box {
                height: 100px;
                background-color: blue;
                display: flex;
                flex-direction: row;
            }
            .child {
                background-color: red;
                margin: 5px;
                color: yellow;
                width: 10%;
            }
            .d1 {
                order: 1;
                flex-grow:1;
            }
            .d2 {
                order: 0;
                flex-grow:2;
            }
            .d3 {
                order: 1;
                flex-grow:1;
            }
        </style>
    </head>
    <body>
        <div class="box">
            <div class="child d1">11111111</div>
            <div class="child d2">22222222</div>
            <div class="child d3">33333333</div>
        </div>
    </body>
</html>
```

![image-20210401234146131](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401234147.png)

### 3.3 `flex-shrink`

`flex-shrink`属性定义了项目的缩小比例，默认为1，即如果空间不足，该项目将缩小。0表示不缩小，负值无效。

**基本语法：**

```css
.item {
    flex-shrink: <number>; /* 缺省 1 */
}
```

```html
<!DOCTYPE html>
<html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>Document</title>
        <style>
            .box {
                height: 100px;
                background-color: blue;
                display: flex;
                flex-direction: row;
            }
            .child {
                background-color: red;
                margin: 5px;
                color: yellow;
                width: 100%;
            }
            .d1 {
                flex-shrink:1;
            }
            .d2 {
                flex-shrink:2;
            }
            .d3 {
                flex-shrink:1;
            }
        </style>
    </head>
    <body>
        <div class="box">
            <div class="child d1">11111111</div>
            <div class="child d2">22222222</div>
            <div class="child d3">33333333</div>
        </div>
    </body>
</html>
```

![image-20210401234348685](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210401234349.png)



### 3.4 `flex-basis`

`flex-basis`属性定义项目在分配额外空间之前的缺省尺寸。属性值可以是长度（20%，10rem等）或者关键字`auto`。它的默认值为auto，即项目的本来大小。

**基本语法：**

```css
.item {
    flex-basis: <length> | auto; /* 缺省 auto */
}
```











