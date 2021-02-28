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
    margin-left: 50px;
	margin-top: 50px;
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

