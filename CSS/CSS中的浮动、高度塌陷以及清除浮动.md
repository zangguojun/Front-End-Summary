# CSS中的浮动、高度塌陷以及清除浮动

​	浮动（float）的框可以左右移动，直至它的**外边缘（指的是margin边）碰到** **包含框**（父盒子）或**另一个浮动框**（兄弟盒子）的**外边缘（其实指的是padding边）**。

​	浮动框不属于文档中的普通流，当一个元素浮动之后，**不会影响到块级元素的布局而只会影响内联元素**（通常是文本）的排列，文档中的普通流就表现得如同浮动框不存在一样。当浮动框高度超出包含框的时候，就会出现包含框不会自动升高来闭合浮动元素（“**高度塌陷**”现象）。

```html
<!DOCTYPE html>
<html>
<head>
   <style type="text/css">
   * {
           margin:0;
           padding:0;
   }
       .container {
           width:300px;
           height:156px;
           border:1px solid blue;
           margin:100px;
       }
       .block1 {
           width:50px;
           height:50px;
           border:1px solid red;
       }
       .block2 {
           width:50px;
           height:50px;
           border:1px solid red;
       }
       .block3 {
           width:50px;
           height:50px;
           border:1px solid red;
       }
   </style>
</head>
<body>
    <div class="container">
        <div class="block1"><span>块1</span></div>
        <div class="block2"><span>块2</span></div>
        <div class="block3"><span>块3</span></div>
    </div>
</body>
</html
```

![img](https://images2015.cnblogs.com/blog/1127212/201705/1127212-20170504213507617-1568282954.jpg)

以上代码是三块div均未加float时在浏览器显示效果如下图。块元素各自独占一行。

----

----



```css
.container {
    width:300px;
    height:156px;
    border:1px solid blue;
    margin:100px;
    /*添加下面这句*/
    padding-right:5px;
}
.block1 {
    width:50px;
    height:50px;
    border:1px solid red;
    /*添加下面这句*/
    float: right;
}
```

![img](https://images2015.cnblogs.com/blog/1127212/201705/1127212-20170504214747820-1562023736.jpg)

 以上是块1向右浮动，开始脱离文档普通流向右移动，知道块1的右边缘碰到包含它的块（父块）的右边缘。由于我们给包含块设置了padding，所以块1与父块之间有该内边距。**可知，所谓边缘相碰，应该是包含了内边距在内的。**

因为浮动块不在文档的普通流中，所以文档的普通流中的块框就表现得像浮动块不存在一样排列（浮动不影响块级元素排列）。所以块2、块3就按照没有块1存在那样排列。

----

----

```css
.container {
    width:300px;
    height:156px;
    border:1px solid blue;
    margin:100px;
    /*添加下面这句*/
    padding-right:5px;
}
.block1 {
    width:50px;
    height:50px;
    border:1px solid red;
    /*添加下面这句*/
    float: right;
}
.block3 {
    /*宽度变为原来的二倍*/
    width:100px;
    height:50px;
    border:1px solid red;
    /*添加下面这句 文本向右对齐*/
    text-align: right;
}
```

![img](https://images2015.cnblogs.com/blog/1127212/201705/1127212-20170504221130664-1774200335.jpg)

 以下是块1向左移动。在代码中我将块3的width变宽，块3文字靠右。可以发现，当块1向左浮动时，它脱离文档流并且向左移动，知道它的左边缘碰到父块的左边缘。因为它不再处于文档流中，所以它不占据空间，而因为又不影响块框的排列，所以实际上块1覆盖住了块2，使块2从视图中消失。

但是据图可知，块2的内容却显示在块1未浮动时块2所处的位置了。所以：**浮动元素不影响块级元素布局，但是可以影响内联元素**（主要是文本）布局。所以可以使用浮动来达到文字环绕图片的效果。

----

----

如果把三个框都向左浮动，那么块1向左浮动直到碰到包含框，另外两个框向左浮动直到碰到前一个浮动框。

![img](https://images2015.cnblogs.com/blog/1127212/201705/1127212-20170504221522273-62959529.jpg)

如果包含框太窄，无法容纳水平排列的三个浮动框，那么其他浮动框向下移动，直到有足够的空间。

如果浮动元素的高度不同，那么当它们向下移动时可能被其他元素"卡住"。

![img](https://images2015.cnblogs.com/blog/1127212/201705/1127212-20170504222147132-1990007741.jpg)

### 浮动导致的塌陷的解决办法：

如果父元素只包含浮动元素，且父元素未设置高度和宽度的时候，那么它的高度就会踏缩为零。这是因为浮动元素脱离了文档流，包围它们的父块中没有内容了，所以“”塌陷“”了。

#### 错误、使用带clear的父盒子

**不能清除浮动**

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>浮动</title>
    <style>
        .testAll {
            border: 2px solid #F00;
            margin: 100px;
        }
        .testAll2 {
            border: 2px solid #F00;
            clear: both;
        }
        .test {
            width: 150px;
            height: 100px;
            border: 1px solid #0F0;
            float: left
        }
    </style>
</head>

<body>
    <div class="testAll">
        <div class="test"></div>
        <div class="test"></div>
        <div class="test"></div>
    </div>
    <div class="testAll2">
        <div class="test"></div>
        <div class="test"></div>
        <div class="test"></div>
    </div>
</body>

</html>
```

![image-20210307150626971](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210307150627.png)

#### 方法一、父盒子内添加clear空元素

 在父块中、浮动元素后（也可以在两个需要清除浮动的盒子之间）使用一个空元素，如`<div class="clear"></div>`，并在css中赋予 `.clear{clear:both}`属性即可清除浮动。

也可使用`<br class="clear"/>`或`<hr class="clear"/>`来进行清除。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>浮动</title>
    <style>
        .testAll {
            border: 2px solid #F00;
        }
        .test {
            width: 150px;
            height: 100px;
            border: 1px solid #0F0;
            float: left
        }
        .testBottom {
            width: 200px;
            height: 50px;
            background: #00F;
        }
    </style>
</head>

<body>
    <div class="testAll">
        <div class="test"></div>
        <div class="test"></div>
        <div class="test"></div>
        <div style="clear:both;"></div>
    </div>
    <div class="testBottom"></div>
</body>

</html>
```

给空元素设置clear后，因为它的左右两边不能有任何浮动元素，所以空元素下移到浮动元素下方。而空元素又包含在父块中，相当于把父块撑开了，视觉上起到了父块包含浮动元素的效果。

+ 优点：简单，代码少，浏览器兼容性好。

+ 缺点：需要添加大量无语义的html元素，代码不够优雅，后期不容易维护。不推荐使用。

#### 方法二：父盒子overflow

 给浮动元素的容器（也就是父盒子）添加`overflow:hidden;`或`overflow:auto；`可以使浮动元素回到容器层内，清除浮动。

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>浮动</title>
    <style>
        .testAll {
            border: 2px solid #F00;
            overflow: hidden;
            /* overflow:auto; */
        }
        .test {
            width: 150px;
            height: 100px;
            border: 1px solid #0F0;
            float: left
        }
        .testBottom {
            width: 200px;
            height: 50px;
            background: #00F;
        }
    </style>
</head>

<body>
    <div class="testAll">
        <div class="test"></div>
        <div class="test"></div>
        <div class="test"></div>
    </div>
    <div class="testBottom"></div>
</body>

</html>
```

缺点：要是子元素要margin负值定位或是负的绝对定位，会被裁掉，所以此方法是有不小的局限性的。

```css
.test {
    width: 150px;
    height: 100px;
    border: 1px solid #0F0;
    float: left;
	/*如果此时进行margin负值的定位，
    子盒子将会被裁掉50px的高度，如下图*/
    margin-top: -50px;
	/*负的绝对定位也是如此*/
}
```

![image-20210307135332915](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210307135333.png)

#### 方法三：父盒子浮动

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>浮动</title>
    <style>
        .testAll {
            border: 2px solid #F00;
            float: left;
        }
        .test {
            width: 150px;
            height: 100px;
            border: 1px solid #0F0;
            float: left
        }
        .testBottom {
            width: 200px;
            height: 50px;
            background: #00F;
        }
    </style>
</head>

<body>
    <div class="testAll">
        <div class="test"></div>
        <div class="test"></div>
        <div class="test"></div>
    </div>
    <div class="testBottom"></div>
</body>

</html>
```

![image-20210307135844616](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210307135845.png)

 给浮动元素的容器（父元素）也添加上浮动属性即可清除浮动，

缺点：虽然可以撑开父元素，但是会导致父元素的**宽度丢失**（能设置宽度），而且会导致下边的元素上移,使得跟父元素相邻的元素的布局受到影响，（如上图所示，把累活交给了testBottom盒子），影响整体布局，不推荐使用。

#### 方法四：给父元素一个固定高度，此方法适用于子元素高度已知并且固定的情况。

#### 方法五：父元素设置display: inline-block;

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>浮动</title>
    <style>
        .testAll {
            border: 2px solid #F00;
            display: inline-block;
        }
        .test {
            width: 150px;
            height: 100px;
            border: 1px solid #0F0;
            float: left
        }
        .testBottom {
            width: 200px;
            height: 50px;
            background: #00F;
        }
    </style>
</head>

<body>
    <div class="testAll">
        <div class="test"></div>
        <div class="test"></div>
        <div class="test"></div>
    </div>
    <div class="testBottom"></div>
</body>

</html>
```

缺点：会导致父元素的宽度丢失

#### 方法六：使用CSS的:after伪元素

:after伪元素（注意这不是伪类，而是伪元素，代表一个元素之后最近的元素）。

给浮动元素的容器（父盒子）添加一个clearfix的class，然后给这个class添加一个:after伪元素实现元素末尾添加一个看不见的块元素清理浮动。

```css
.clearfix:after{
    content:".";
    /*生成内容作为最后一个元素，至于content里面是什么没有影响*/
    display:block;
    /*使生成的元素以块级元素显示，占满剩余空间*/
    clear: both;
    /*闭合浮动*/

    /* 
    下面两个属性取消后，也可以清除浮动。
    可能是为了保证after不在界面显示，所以使用。 
    */
    height:0;
    /*避免生成的内容破坏原有空间的高度*/
    visibility:hidden;
    /*使生成内容不可见，并允许可能生成内容盖住的内容进行点击和交互*/
}
.clearfix {
    /*触发IE6/7的haslayout
    只有IE6-IE7执行，其他浏览器不执行*/
    *zoom:1
}
```

通过CSS伪元素在容器内部元素最后添加了一个看不见的点“.”，并且赋予clear属性来清除浮动。 

利用伪类来清除浮动，其效果跟创建一个空的div并设置其为clear：both；是一样的，只不过这里用伪类代替了空的div元素，不会影响任何其他样式，通用性强，覆盖面广

#### 方法七：使用before和after双伪元素清除浮动

```css
/* 提示：当我们不想要外边距折叠（重叠取最大值）时，这个版本的清除浮动非常有效 */
.clearfix:after,.clearfix:before{
    content: " ";
    /* content中包含的空格也能生效，旧版本Opera浏览器中有个隐藏的bug，需要添加一个空格字符才能解决 */
	/* 外边距无法通过单元格元素折叠 */
    display: table;/* 防止伪元素外边距折叠 */
}
/* 只有after伪元素需要清除浮动 */
.clearfix:after{
    clear: both;
}
.clearfix{
    *zoom: 1;
}
```

#### zoom在清除浮动中的利用

它的原本功能是设置或检测对象的缩放比例（只在ie下起作用）

比如  

```
<div style="background:#f0f3f9; padding:20px; zoom:2;">
    <img data-src="https://home.cnblogs.com/u/nanshanlaoyao/" border="0" />
</div>
```

在ie中它会使图片放大两倍显示。

在我们日常见到这个玩意的时候他一般用于浮动的清除，老的说法是可以触发ie的haslayout 来清除浮动

在 非ie 和 ie7及其以上版本的浏览器中，可以使用 overflow ：hidden 等方法来进行清除浮动。

可是在ie6 及其以下的浏览器中并不能正确的理解 overflow 这个属性，这是早期ie的一个bug

所以我们就可以用以下方式来清除浮动：

```
<div style="zoom:1; background:#f0f3f9; padding:20px;">
    <img style="float:left;" data-src="http://XXXXX.jpg" />
</div>
```

在ie6 及其以下的浏览器中，div 便有了高度， 此时再给它加上 其他浏览器和ie8+的清除浮动的方法 就可以完美结局 



