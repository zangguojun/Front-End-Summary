# margin塌陷和margin合并问题及解决方案

只有垂直方向存在margin塌陷和合并，水平方向是正常的，即水平方向上子元素是正常相对父元素的，**只是垂直方向上需要注意塌陷和合并问题**



## 零、BFC(Block formatting context)块级格式化上下文

### 0.1 原理

> 它是一个独立的渲染区域，只有Block-level box参与， 它规定了内部的Block-level Box如何布局，并且与这个区域外部毫不相干。

在解释什么是BFC之前，我们需要先知道**Box**、**Formatting Context**的概念。

#### Box

Box 是 CSS 布局的对象和基本单位， 直观点来说，就是一个页面是由很多个 Box 组成的。元素的类型和 display 属性，决定了这个 Box 的类型。 不同类型的 Box， 会参与不同的 Formatting Context（一个决定如何渲染文档的容器），因此Box内的元素会以不同的方式渲染。让我们看看有哪些盒子：

+ block-level 
  + box:display 属性为 block, list-item, table 的元素，会生成 block-level box。
  + 并且参与 block fomatting context；
+ inline-level 
  + box:display 属性为 inline, inline-block, inline-table 的元素，会生成 inline-level box。
  + 并且参与 inline formatting context；
+ run-in box: css3 中才有， 这儿先不讲了。

#### Formatting Context

Formatting context 是 W3C CSS2.1 规范中的一个概念。它是页面中的一块渲染区域，并且有一套渲染规则

它决定了其子元素将如何定位，以及和其他元素的关系和相互作用。最常见的 Formatting context 有 Block fomatting context (简称BFC)和 Inline formatting context (简称IFC)。

> BFC是一个独立的布局环境，其中的元素布局是不受外界的影响，并且在一个BFC中，块盒与行盒（行盒由一行中所有的内联元素所组成）都会垂直的沿着其父元素的边框排列。

##### BFC的布局规则

+ 内部的Box会在垂直方向，一个接一个地放置。

+ Box垂直方向的距离由margin决定。属于同一个BFC的两个相邻Box的margin会发生重叠。

+ 每个盒子（块盒与行盒）的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

+ BFC的区域不会与float box重叠。

+ BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

+ 计算BFC的高度时，浮动元素也参与计算。

##### 如何创建BFC(body 元素本身就是一个BFC容器)

- 1、float的值不是none。
- 2、position的值不是static或者relative。
- 3、display的值是inline-block、table-cell、flex、table-caption或者inline-flex
- 4、overflow的值不是visible，比如hidden、scroll、auto

### 0.2 BFC的作用

#### 1.利用BFC避免margin合并

#### 2.自适应两栏布局

> 每个盒子的margin box的左边，与包含块border box的左边相接触(对于从左往右的格式化，否则相反)。即使存在浮动也是如此。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
</head>
<style>
    *{
        margin: 0;
        padding: 0;
    }
    body {
        width: 100%;
        position: relative;
    }
 
    .left {
        width: 100px;
        height: 150px;
        background: rgb(139, 214, 78);
        text-align: center;
        line-height: 150px;
        font-size: 20px;

        float: left;
    }
 
    .right {
        height: 300px;
        background: rgb(170, 54, 236);
        text-align: center;
        line-height: 300px;
        font-size: 40px;
    }
</style>
<body>
    <div class="left">LEFT</div>
    <div class="right">RIGHT</div>
</body>
</html>
```

![image-20210307124014488](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210307124015.png)

又因为 BFC的区域不会与float box重叠，所以我们让right单独成为一个BFC

```css
    .right {
        height: 300px;
        background: rgb(170, 54, 236);
        text-align: center;
        line-height: 300px;
        font-size: 40px;
        /*添加下面这句*/
        overflow: hidden;
    }
```



right会自动的适应宽度，这时候就形成了一个两栏自适应的布局。

![image-20210307123955941](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210307124004.png)

#### 3.解决高度塌陷

> 在文档流中，若父元素未设置高度，那么父元素的高度默认是被子元素撑开的，即子元素多高，父元素就有多高。但是当子元素设置浮动之后，子元素就会完全脱离文档流，父元素还在文档流中，此时父元素的高度就没有子元素撑起（子元素无法撑起父元素的高度），从而导致父元素的高度塌陷。简单来说，就是包含含有浮动的元素的上一级的高度变为0了，下面的元素会上去，这样会导致页面布局混乱。

当我们**不给父节点设置高度**，**子节点设置浮动**的时候，会发生**高度塌陷**，这个时候我们就要清除浮动。

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>清除浮动</title>
</head>
<style>
    .par {
        border: 5px solid rgb(91, 243, 30);
        width: 300px;
    }
    
    .child {
        border: 5px solid rgb(233, 250, 84);
        width:100px;
        height: 100px;
        float: left;
    }
</style>
<body>
    <div class="par">
        <div class="child"></div>
        <div class="child"></div>
    </div>
</body>
</html>
```



![image-20210307124302004](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210307124303.png)

```css
.par {
    border: 5px solid rgb(91, 243, 30);
    width: 300px;
    /*添加下面这句*/
    overflow: hidden;
}
```

![image-20210307124457731](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210307124458.png)

### 0.3 总结

> BFC就是页面上的一个隔离的独立容器，容器里面的子元素不会影响到外面的元素。反之也如此。

因为BFC内部的元素和外部的元素绝对不会互相影响，

因此， 当BFC外部存在浮动时，它不应该影响BFC内部Box的布局，BFC会通过变窄，而不与浮动有重叠（**自适应两栏布局**）。

同样的，当BFC内部有浮动时，为了不影响外部元素的布局，BFC计算高度时会包括浮动的高度。（**清除浮动**）

两个BFC不会相互影响，就更不会让两者的margin合并（**避免marin合并**）



## 一、margin塌陷

### 1.1 原理

正常情况下,父级元素应该相对浏览器进行定位,子级相对父级定位.

但由于margin的塌陷,父级相对浏览器定位.而子级没有相对父级定位,子级相对浏览器,就像坍塌了一样

父子嵌套元素在垂直方向的margin,父子元素是结合在一起的,他们两个的margin会取其中最大的值.

> 就是垂直方向的margin不但会合并; 而且当父元素没有设置内边距或边框,以及触发BFC时,如果子元素的值大于父元素时，它会带着父元素一起偏移,此时子元素是相对除了它父级之外的离它最近的元素偏移的

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>margin塌陷</title>
    <style>
        body{
            background-color:#000;
        }
        *{
            margin: 0;
            padding: 0;
        }
        .wrapper{
            width:200px;
            height:200px;
            background-color:red;
            margin:100px;
        }
        .box{
            width:50px;
            height:50px;
            background-color:#eee;
            opacity:0.8;
            margin: 75px;
            margin-top: 200px;
        }
    </style>
</head>
<body>
    <div class="wrapper">
        <div class="box"></div>
    </div>
</body>
</html>
```

### 1.2 解决方法

+ 给父级设置边框和内边距

  + ```css
    .wrapper{
        width:200px;
        height:200px;
        background-color:red;
        margin:100px;
        border-top:1px solid green;
    }
    ```

  + ```css
    .wrapper {
        width: 200px;
        height: 200px;
        background-color: red;
        margin: 100px;
        padding: 1px;
    }
    ```

+ 触发BFC

## 二、margin合并

### 2.1 原理

所谓的外边距合并就是，当两个垂直外边距相遇时，它们将形成一个外边距。合并的外边距的高度等于两个发生合并的外边距的高度中的较大者。

> 同一个BFC的两个相邻的Box会发生margin重叠

**发生合并的情况有以下几种**：

- 两个元素是兄弟关系：

  - ```html
    <!DOCTYPE html>
    <html lang="en">
    ```
<head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>margin合并</title>
        <style>
            * {
                margin: 0;
                padding: 0;
            }
            .box1 {
                height: 30px;
                margin-bottom: 50px;
                background-color: red;
            }
            .box2 {
                height: 30px;
                margin-top: 50px;
                background-color: green;
            }
        </style>
    </head>
    <body>
        <div class="box1"></div>
        <div class="box2"></div>
    </body>
    </html>
    ```

  - ![image-20210307111414893](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210307111416.png)

+ 两个元素是父子关系（没有内边距或边框把外边距分割开）：
  
+ ```HTML
    <!DOCTYPE html>
  <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <title>margin合并</title>
        <style>
            * {
                margin: 0;
                padding: 0;
            }
            .box1 {
                height: 30px;
                margin: 50px;
                background-color: red;
            }
            .box2 {
                height: 30px;
                margin: 30px;
                background-color: green;
            }
        </style>
    </head>
    <body>
        <div class="box1">
            <div class="box2"></div>
        </div>
    </body>
    </html>
  ```
  
  + 
    
  + ![这里写图片描述](https://img-blog.csdn.net/20180503145333478?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NoaV8xMjA0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
  
+ 一个空元素，没有边框和填充【有待商榷，没有达到效果】

  + ```html
    <!DOCTYPE html>
    <html lang="en">
    ```
<head>
        <meta charset="UTF-8">
      <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
      <title>margin合并</title>
        <style>
          * {
                margin: 0;
                padding: 0;
            }
            .box1 {
                height: 100px;
                background-color: red;
                margin-bottom: 50px;
            }
            .box2 {
                margin-top: 50px;
                margin-bottom: 50px;
            }
        </style>
    </head>
    <body>
        <div class="box1"></div>
        <div class="box2"></div>
    </body>
    </html>
    ```

  + ![这里写图片描述](https://img-blog.csdn.net/20180503150007411?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NoaV8xMjA0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)
  
  + 如果这个外边距遇到另一个元素的外边距，还会发生合并：
  
  + ![这里写图片描述](https://img-blog.csdn.net/20180503150256387?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3NoaV8xMjA0/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### 2.2 解决办法

+ 两个元素是兄弟关系：
  + 可以直接改变其中一个的外边距的值，使之达到想要的效果。(推荐使用)

  + BFC解决办法：

    + 将兄弟元素分别作为子元素放在块级元素内，然后将其父级元素的渲染规则该为BFC。(不推荐使用,会破坏HTML文档结构)

    + ```html
      <div class="box1"></div>
      <div class="wrapper">
          <div class="box2"></div>
      </div>
      ```

    + ```css
      .wrapper{
          overflow:hidden;
      }
      ```

    + 或者给两个都加一层父级再加BFC

+ 两个元素是父子关系：

  + 使用margin时，会出现另一个bug，**margin塌陷**
    + 可以通过给父元素添加边框或内边距(不建议使用,会破坏布局)
    + 还可以使用BFC解决: 将父元素的渲染模式改为BFC渲染模式