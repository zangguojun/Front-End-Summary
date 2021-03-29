### 在div中使用css样式，来实现应用css样式

```html
<div style="font-size:14px; color:#FF0000;">
```



### 使用html中的style来引用

```html
<head> 
    <style> 
        .content { 
            background: red;
        } 
    </style>
</head>
```



### 使用@import来引用外部css

```html
<head> 
    <style>
        @import url(style.css);
    </style>
</head>
```



### 使用link引用css文件

```html
<link rel="stylesheet" href="wcss.css" type="text/css" />
```



### >>link和@import的区别

#### 1、从属关系区别

**@import**是 CSS 提供的语法规则，**只有导入样式表的作用**；

**link**是**HTML提供的标签**，不仅可以加载 CSS 文件，**还可以定义 RSS、rel 连接属性等**。



#### 2、加载顺序区别

加载页面时，**link标签引入的 CSS 被同时加载**；@import**引入的 CSS 将在页面加载完毕后被加载**。



#### 3、兼容性区别

@import是 **CSS2.1** 才有的语法，故只可在 IE5+ 才能识别；link标签作为 HTML 元素，不存在兼容性问题。



#### 4、DOM可控性区别

可以通过 JS 操作 DOM ，**插入link标签来改变样式**；

由于 DOM 方法是基于文档的，**无法使用@import的方式插入样式**。