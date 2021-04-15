## 略知一二
行内元素有：
a, span, label, strong, em, br, img, **input, select, textarea**

块级元素：
div, h1~h6, p, ul, li, ol, dl, **form, address, hr, menu, table, fieldset**

## 粗通皮毛
### （行内元素）内联元素(inline element)
+ a - 锚点

+ b - 粗体(不推荐)

+ big - 大字体

+ small - 小字体文本

+ br - 换行

+ cite - 引用

+ code - 计算机代码(在引用源码的时候需要)

+ em - 强调

+ i - 斜体

+ img - 图片

+ input - 输入框

+ label - 表格标签

+ q - 短引用

+ s - 中划线(不推荐)

+ select - 项目选择

+ span - 常用内联容器，定义文本内区块

+ strike - 中划线

+ strong - 粗体强调

+ sub - 下标

+ sup - 上标

+ textarea - 多行文本输入框

+ u - 下划线

### 块元素(block element)
+ address - 地址
+ blockquote - 块引用
+ center - 居中对齐
+ dir - 目录列表
+ div - 常用块级容易，也是css layout的主要标签
+ dl - 定义列表
+ fieldset - form控制组

+ form - 交互表单

+ h1 - 大标题

+ h2 - 副标题

+ h3-6 - 3-6级标题

+ hr - 水平分隔线
+ menu - 菜单列表

+ ol - 排序表单

+ p - 段落

+ pre - 格式化文本

+ table - 表格

+ ul - 非排序列表

#### 可变元素

可变元素为根据上下文语境决定该元素为块元素或者行内元素。

- button - 按钮
- del - 删除文本
- iframe - inline frame
- ins - 插入的文本
- map - 图片区块(map)
- object - object对象
- script - 客户端脚本

```html
<h1>行内元素</h1>
<br>
大字体big
<big>你</big>
<br>

小字体small
<small>你</small>
<br>

粗体b
<b>你</b>
<br>

粗体strong
<strong>你</strong>
<br>

短引用q
<q>你</q>
<br>

引用cite
<cite>你</cite>
<br>

强调em
<em>你</em>
<br>

斜体i
<i>你</i>
<br>

中划线s
<s>你</s>
<br>

中划线strike
<strike>你</strike>
<br>

下划线u
<u>你</u>
<br>

项目选择select
<select>
    <option name="lq" value="ball">篮球</option>
    <option name="zq" value="ball">足球</option>
    <option name="pq" value="ball">排球</option>
</select>
<br>

代码块code
<code >
for i in arr:
    print(i)
</code>
<br>
<hr>
<hr>
<h1>块级元素</h1>
<br>
块引用blockquote
<blockquote>你</blockquote>
<blockquote>你</blockquote>
<br>

地址address
<address>你</address>
<address>你</address>
<br>

居中对齐块
<center>你</center>
<center>你</center>
<br>

目录列表dir
<dir>
    <li>HTML</li>
    <li>XHTML</li>
    <li>CSS</li>
    <dir>
        <li>HTML</li>
        <li>XHTML</li>
        <li>CSS</li>
    </dir>
</dir>
<br>
<hr>
<hr>
<h1>可变元素</h1>
<br>
按钮button
<button>你</button>
<button>你</button>
<br>

删除del
<del>你</del>
<del>你</del>
<br>

<br>
<hr>
<hr>
<h1>行内元素的padding有效？</h1>
<h2>答：行内元素的上下左右padding都有效</h2>
<style>
    .test1 {
        padding: 50px;
    }
</style>
<span class="test1">你你你你你你你你你你你你你你你你你你你你你</span>

<h1>行内元素的border有效？</h1>
<h2>答：行内元素的上下左右padding都有效</h2>
<style>
    .test2 {
        border: 50px solid brown;
    }
</style>
<b class="test2">你你你你你你你你你你你你你你你你你你你你你</b>
<b class="test2">你你你你你你你你你你你你你你你你你你你你你</b>
```



### 驾轻就熟

#### 区别：

1. 块级元素**会独占一行**，其宽度**自动填满其父元素宽度**
   行内元素**不会独占一行**，相邻的行内元素会**排列在同一行**里，知道**一行排不下**，**才会换行**，**其宽度随元素的内容而变化**
2. 块级元素可以设置 width, height属性，【注意：**块级元素即使设置了宽度，仍然是独占一行的**】
   **行内元素设置width, height无效;**
3. margin/padding
   1. block是块级元素，能设置宽高，margin/padding都有效前后都有换行符
   2. inline设置宽高无效，margin在竖直方向无效，padding有效，前后无换行符
   3. inline-block可以设置宽高，margin/padding有效，前后无换行符

### 青出於蓝

- 行内元素与块级元素直观上的区别
  行内元素会在一条直线上排列，都是同一行的，水平方向排列
  块级元素各占据一行，垂直方向排列。块级元素从新行开始结束接着一个断行。
- **块级元素可以包含行内元素和块级元素**。**行内元素不能包含块级元素**。

### 融会贯通

- 行内元素属性
  1. 行内元素属性标签它和其它标签处在同一行内
  2. 行内元素属性标签无法设置宽度，高度 行高 距顶部距离 距底部距离
  3. 行内元素属性标签的宽度是**直接由内部的文字或者图片等内容撑开的**
  4. 行内元素属性标签**内部不能嵌套行属性标签**（a链接内不能嵌套其他链接）
- 块级元素属性
  1. 每一个块级元素属性标签都是从新的一行开始，而且之后的元素也都会从新的一行开始（因为每一个块属性标签都会直接占据一整行的内容，导致下面的内容也只能从新的一行开始）
  2. 块级元素属性标签都是可以设置宽度、高度，行高，距顶部距离，距底部距离
  3. 块级元素属性标签的宽度假如不做设置，会**直接默认为父元素宽度的100%**
  4. 块级元素属性标签是可以直接嵌套的
  5. p标签中**不能嵌套div标签**

### 出类拔萃

- CSS设置行内元素的

  - 水平居中

    ```css
    div{text-align:center} /*DIV内的行内元素均会水平居中*/ 
    ```

  - 垂直居中

    ```css
    div{height:30px; line-height:30px} /*DIV内的行内元素均会垂直居中*/ 
    ```

- CSS设置块级元素的

  - 水平居中

    - ```css
      div p{margin:0 auto; width:500px} 
      /*块级元素p一定要设置宽度， 才能相当于DIV父容器水平居中*/
      ```

  - 垂直居中

    - ```css
      div{width:500px} 
      /*DIV父容器设置宽度*/ 
      div p{margin:0 auto; height:30px; line-height:30px} 
      /*块级元素p也可以加个宽度， 以达到相对于DIV父容器的水平居中效果*/
      ```

### 返璞归真

在标准文档流里面，块级元素具有以下特点：

① 总是在新行上开始，占据一整行；
② 高度，行高以及外边距和内边距都可控制；
③ 宽带始终是与浏览器宽度一样，与内容无关；
④ 它可以容纳内联元素和其他块元素。



行内元素的特点：

① 和其他元素都在一行上；
② 高，行高及外边距和内边距部分可改变；
③ 宽度只与内容有关；
④ 行内元素只能容纳文本或者其他行内元素。

+ **不可以设置宽高，其宽度随着内容增加，高度随字体大小而改变**

+ **padding左右有效，margin上下左右都有效**

