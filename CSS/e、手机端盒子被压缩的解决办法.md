## `font-size`的取值

### `px`

一个盒子是100px * 100px  px指的是逻辑像素，也叫css像素
在适配时，使用px作为单位并不合适。



### `em`

`指的是一个汉字的宽度，1em = 1个font-size的大小`

> 如果 font-size:20px，那么1em = 20px，10em = 200px
>
> 

#### em不足

它是相对于父元素的font-size来说的。适配使用em也不合适，因为它一直是相对于父元素的font-size来说的



### rem

**r表示root   root是根的意思，指html标签**

> 1rem ==> html标签中的font-size的大小
> 1em  ==> 父标签中的font-size的大小



## 适配的实现

### 适配

**在大屏上显示大盒子，在小屏上显示小盒子。**

##### 什么是适配？

+ 如果是大屏，盒子或字体大上设置的大一点

+ 如果是小屏，盒子或字体大小设置的小一点 



### 使用媒体查询`+rem`，实现适配

1. 得到设计稿，一般情况下，设计稿的尺寸是750px。
2. 通常我们把浏览器的模拟器也调整成750px
3. 严格按照设计稿，以rem为单位，把设计稿还原出来。
4. 开始写样式，需要确定html标签的`font-size`，通常我会把根元素的`font-size`定成100px，叫rem的基准值。  
5. 最后一步，把写好的页面，迁移到其它屏上的，完成适配。
6. 在迁移时，也需要换算。 换算如下： 
   1.  在750px的屏上，html的font-size的大小为100px。
   2.  在375px的屏上，html的font-size的大小为50px。
   3.  在320px的屏上，html的font-size的大小为42.6666666px。
   4.  在414px的屏上，html的font-size的大小为55.2px。

```css
@media only screen and (width:750px) { html{ font-size: 100px; } }
@media only screen and (width:375px) { html{ font-size: 50px; } }
@media only screen and (width:320px) { html{ font-size: 42.6666px; } }
@media only screen and (width:414px) { html{ font-size: 55.2px; } }
```

> 如果我们写要适配的屏比较多，那么我们需要写大量的媒体查询的代码。



### 使用`JS+rem`来适配，实现适配

```js
// 表示获取窗口
let docEle = window.document.documentElement;
// 此函数是计算不同屏的html标签的font-size
function refresh(){
   	// width 表示手机屏的宽度
    let width = docEle.getBoundingClientRect().width;
    // 一屏的宽度 
    if(width>750){  width=750 }
    let fs = width / 7.5
    document.querySelector("html").style.fontSize = fs + "px"
}

refresh();  

// window叫事件源   addEventListenere用来绑定事件的
// resize叫浏览器窗口大小改变事件  ()=>{}叫事件监听器
window.addEventListener("resize",()=>{
    refresh() 
})
// pageshow 页面显示出来
window.addEventListener("pageshow",()=>{
    refresh()
})
```









