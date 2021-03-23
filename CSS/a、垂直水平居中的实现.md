# 垂直、水平居中的实现

## ① 垂直且水平居中的实现

### 一、position + margin

* 子绝父相

* 子盒子top: 50%;left: 50%让子盒子的左上角成为父盒子的中心点

* 子盒子margin-left:子盒子宽度的一半，margin-top:子盒子高度的一半，让子盒子的中心成为父盒子的中心

```js
.parent{
	position: relative;
}
.child {
    width: 100px;
    height: 100px;
    background-color: yellowgreen;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-left: 50px;
	margin-top: 50px;
}
```

### 二、position + transform

* 子绝父相

* 子盒子top: 50%;left: 50%让子盒子的左上角成为父盒子的中心点

* 子盒子transform: translate(-50%,-50%)，平移为子盒子宽度和高度一半，让子盒子的中心成为父盒子的中心

```js
.parent{
	position: relative;
}
.child {
    width: 100px;
    height: 100px;
    background-color: yellowgreen;
    position: absolute;
    top: 50%;
    left: 50%;
    transform:translate(-50%,-50%);
}
```

### 三、position + 0

* 子绝父相

* 子盒子top: 0;bottom: 0;left: 0;right: 0
* 子盒子margin: auto

```js
.parent{
	position: relative;
}
.child {
    width: 100px;
    height: 100px;
    background-color: yellowgreen;
    position: absolute;
    top: 50%;
    left: 50%;
    margin-left: 50px;
	margin-top: 50px;
}
```

### 四、display: flex

* 父盒子display: flex;
* 父盒子justify-content: center;
* 父盒子align-items: center(单行);align-content: center

```js
.parent {
    width: 500px;
    height: 500px;
    display: flex;
    justify-content: center;
    align-items: center;
}
.child {
    width: 100px;
    height: 100px;
    background-color: yellowgreen;
}
```

## ②水平居中的实现

- 利用块级元素撑满父元素的特点，如果宽度已定，左右margin auto就可以平分剩余空间
- 利用行内块居中： 把父级元素设置为text-align=center，之后子元素的display设置为inline-block
- 绝对定位：postion ：absolute，之后left 50% + (margin/ transform)
- flex: 父元素**display:flex    justify-content：center**

## ③垂直居中的实现

- 元素无高度

  利用内边距，让块级文字包裹在padding中，实现垂直居中。

- 父元素高度确定的单行文本：

  使用行高的特性：height=line-height即可。

- 父元素高度确定的多行文本：

  利用vertical-align（只能内联元素）如果是div，可以设置为table和table-cell。

  - ```css
    vertical-align:center;
    isplay：table-cell;
    ```

- 父元素高度未知：

  绝对定位，设置top 50% + (margin/ transform)

  - ```css
    parent{
        position:relative;
    }
    child{
        position:absolute;
        top:50%;
        transform:translateY(-50%);
    }
    ```

- flex方法

  - ```css
    disply:flex;
    align-items: center;
    ```





