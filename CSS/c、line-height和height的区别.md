## line-height与font-size

![image-20210408211401480](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210408211402.png)

+ **行高**与**行距**从字面的意思是非常容易理解的
+ **行高**是指文本行**基线间**的**垂直距离**
+  基线（base line）并不是汉字文字的下端沿，而是**英文字母**“x”的**下端沿**
+ **上行的底线**和**下一行顶线**之间的距离就是**行距**，**行距的一半是半行距**
+ **同一行**顶线和底线之间的距离是**font-size**的大小

## 图像判断

```html
<style>
    .div {
        text-align: center;
        font-size: 60px;
        line-height: 60px;
    }
</style>

<div class="div">abc</div>
<div class="div">abc</div>
```

+ 当font-size等于line-height时，行距 = line-height - font-size = 0；

![image-20210408212230646](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210408212231.png)

![image-20210408212246937](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210408212248.png)

+ 当font-size**大于**line-height时，则会出现行距为负值，则两行重叠

![image-20210408212411766](C:%5CUsers%5Cbuchiyu%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20210408212411766.png)

![image-20210408212357954](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210408212358.png)

### CSS boxes的四种类型

containing boxes

inline boxes

line boxes

content area

### line-height取值

line-height : normal;（相当于1.2）

继承 | line-height : inherit;

百分比 | line-height : 120%;

长度 | line-height : 25px;

纯数字 |line-height : 1.2 



**第一个问题：**

如果不设置div的高度时，是div的**font-size**决定了div的高度还是**line-height**的值

![image-20210408221407067](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210408221408.png)

在**没有设置div的height属性时**，div的高度根据**line-height**的大小而变化，且**文字垂直居中**

```html
<style>
    .test1 {
        font-size: 20px;
        text-align: center;
        line-height: 0;/*不同点*/
        border: 1px solid black;
        background: red;
    }
    .test2 {
        font-size: 1px;
        text-align: center;
        line-height: 20px;/*不同点*/
        border: 1px solid black;
        background: red;
    }
</style>
<div class="test1">测试1</div>
<div class="test2">测试2</div>
```



**第二问题：**

div的**height**与**line-height**的大小关系不同时，会有什么显示结果呢？
（1）height = line-height时

![image-20210408221933809](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210408221934.png)

```css
line-height: 20px;
height: 20px;
```

（2）height < line-height时

![image-20210408222037642](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210408222038.png)

```css
line-height: 40px;
height: 20px;
```



3）height > line-height时

![image-20210408222134412](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210408222135.png)

```css
line-height: 20px;
height: 40px;
```

## line-height 和 height的区别

1、定义不同：

**line-height**是**行高**的意思

**height**则是定义**元素自身**的高度

2、表示意义不同： 

**height**用来表示**容器的高度**

**line-height**用来表示**这一容器内的每行文字的高度**

3、使用范围不同：

**line-height**只针对**行内元素**

**height**针对**其他所有元素**

4、针对对象不同：

**line-height**一般针对字体来设置，如果一行文字在DIV里面，且行高等于高度的话，则文字会垂直居中

**height**一般用来**设置文字外围的DIV容器**。