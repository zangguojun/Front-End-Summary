+ 网页可见区域高：`document.body.clientHeight`

+ 网页正文全文高：`document.body.scrollHeight`
+ 网页可见区域高（包括边线的高）：`document.body.offsetHeight`
+ 网页被卷去的高：`document.body.scrollTop`
+ 屏幕分辨率高：`window.screen.height`

每个HTML元素都具有`clientHeight offsetHeight scrollHeight offsetTop scrollTop`

这5个和元素高度、滚动、位置相关的属性，单凭单词很难搞清楚分别代表什么意思之间有什么区别。**通过阅读它们的文档总结出规律如下：**
`clientHeight`和`offsetHeight`属性和元素的滚动、位置没有关系，它代表元素的高度，其中：

`clientHeight`：包括padding但不包括border、水平滚动条、margin的元素的高度。对于inline的元素这个属性一直是0，单位px，只读元素。

![图片描述](https://img.mukewang.com/58f5b2fa00012ea604110247.jpg)

`offsetHeight`：包括padding、border、水平滚动条，但不包括margin的元素的高度。对于inline的元素这个属性一直是0，单位px，只读元素。

![图片描述](https://img.mukewang.com/58f5b32e0001a2fa04110247.jpg)

**接下来讨论出现有滚动条时的情况**：
当本元素的子元素比本元素高且overflow=scroll时，本元素会scroll，这时：
`scrollHeight`: 因为子元素比父元素高，父元素不想被子元素撑的一样高就显示出了滚动条，在滚动的过程中本元素有部分被隐藏了，scrollHeight代表包括当前不可见部分的元素的高度。而可见部分的高度其实就是clientHeight，也就是scrollHeight>=clientHeight恒成立。在有滚动条时讨论scrollHeight才有意义，在没有滚动条时scrollHeight==clientHeight恒成立。单位px，只读元素。

![图片描述](https://img.mukewang.com/58f5b3700001627d05110413.jpg)

`scrollTop`: 代表在有滚动条时，滚动条向下滚动的距离也就是元素顶部被遮住部分的高度。在没有滚动条时scrollTop==0恒成立。单位px，可读可设置。
`offsetTop`: 当前元素顶部距离最近"设置了position属性的"父元素顶部的距离,和有没有滚动条没有关系。单位px，只读元素。

![图片描述](https://img.mukewang.com/58f5b4050001e99204480302.jpg)

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        #scrolldiv {
            width: 150px;
            height: 150px;
            display: block;
            overflow-y: auto;
            padding: 5px;
            border: solid red 2px;
            margin: 30px;
            box-sizing: content-box;
        }
    </style>
</head>

<body>
    <div id="scrolldiv">offsetheight，它包含padding、border、横向滚动轴高度。
        offsetheight=padding+height+border+横向滚动轴高度。scrollheight，可滚动高度，就是将滚动框拉直，不再滚动的高度，这个很好理解。scrollHeight通常用在iframe自适应内容高度的情况。</div>
    <div id="text"></div>
    <script>
        let text = document.getElementById('text')
        text.innerHTML += 'clientHeight:' + document.body.clientHeight + 'px</br>'
        text.innerHTML += 'offsetHeight:' + document.body.offsetHeight + 'px</br>'
        text.innerHTML += 'clientWidth:' + document.body.clientWidth + 'px</br>'
        text.innerHTML += 'offsetWidth:' + document.body.offsetWidth + 'px</br>'
    </script>
</body>

</html>
```

## 验证clientHeight

根据公式，**clientHeight=height+padding-横向滚动轴高度**，143=150+（5+5）-横向滚动轴高度，得出横向滚动轴高度是17。为了看一下横向滚动轴高度是否是17，看一下第二个没有滚动轴的div的clientHeight正好是160,160-143=17，所以，我们可以肯定横向滚动轴高度是17，并且公式是正确的。

这是我们反推出来的横向滚动轴高度是17，根据浏览器的渲染模型也能正向推算出clientHeight，如图

![image-20210314202045227](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210314202053.png)

可见内容可视高度是133，padding是5，所以clientHeight=渲染后的高度+padding=133+（5+5）=143，也符合预期。

## 验证offsetHeight

根据公式，**offsetheight=height+padding+border+横向滚动轴高度**，得出133+（5+5）+（2+2）+17=164，跟显示出来的高度一样。

# 结论

通过上述实验，我们能得出，实际的可视区域比设置的值要小，因为滚动轴占去一部分。在计算时，clientHeight会剔除滚动轴，而offsetHeight会包含滚动轴。记住这两个公式

```text
clientHeight=height+padding-横向滚动轴高度
offsetheight=height+padding+border+横向滚动轴高度
```

# 注意

在做测试时，一定要将**css样式放在javascript之前或者window加载完毕后执行js**，否则解析结果跟预期不同。

















