### 一 、不同浏览器margin和padding不同，这个一般都会遇到

解决方法：`*{margin：0；padding：0}` 统一格式



### 二、ie6小于19px，会当成19px处理，也就元素宽高小于19px的bug

解决方法：overflow:hidden;

```html
<div class="box"></div>
<style>
    .box {
        height:1px;
        background: red;
        overflow: hidden;
        /*此处如果没有这个，会默认19px*/
    }
</style>
```

### 三、在IE6下不支持1px的dotted边框样式

解决方法：可以采取**切背景平铺**的方法

### 四、在IE6和IE7下父级有边框的时候，子元素的margin会失效

解决方法：触发`haslayout`，父级元素设置`zoom：1`；

```html
<div class="box">
    <div class="div"></div>
</div>

<style>
    .box {
        background: red;
        border: 1px solid black;
        zoom:1;/*此处不设置，margin无效*/
    }
    .div {
        width:200px;
        height:200px;
        background: blue;
        margin:100px;
    }
</style>
```

### 五、浮动后的元素在ie6下出现横向的双边距bug

解决方法给元素添加display：inline；

```html
<div class="box"></div>；

<style>
    .box {
        width:100px;
        height: 100px;
        background: red;
        float:left;
        margin:100px;
        display: inline;
        /*此处不添加会出现双边距bug*/
    }
</style>
```











