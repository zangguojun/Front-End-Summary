### 图片大小如何优化

- **减少像素点**
- **减少每个像素点能够显示的颜色**

### 图片加载如何优化

1. 用 CSS 去代替。
2. 用 CDN 加载，计算出适配屏幕的宽度，然后去请求相应裁剪好的图片。
3. 小图使用 base64 格式
4. 将多个图标文件整合到一张图片中（雪碧图）
5. 选择正确的图片格式：
   - 尽量使用 WebP 格式。因为 WebP 格式具有更好的图像数据压缩算法，能带来更小的图片体积，而且拥有肉眼识别无差异的图像质量。
   - 小图使用 PNG，其实对于大部分图标这类图片，完全可以使用 SVG 代替
   - 照片使用 JPEG

# 懒加载

## **一、什么是懒加载**

将图片src先赋值为一张默认图片，当用户滚动滚动条到可视区域图片的时候，再去加载后续真正的图片，如果用户只对第一张图片感兴趣，那剩余的图片请求就可以节省了

## **二、为什么要引入懒加载**

懒加载（LazyLoad）是前端优化的一种有效方式，极大的提升用户体验。图片一直是页面加载的流浪大户，现在一张图片几兆已经是很正常的事，远远大于代码的大小。倘若一次ajax请求10张图片的地址，一次性把10张图片都加载出来，肯定是不合理的。

## **三、懒加载实现的原理**

先将img标签中的src链接设置为空，将真正的图片链接放在自定义属性（data-src），当js监听到图片元素进入到可视窗口的时候，将自定义属性中的地址存储到src中，达到懒加载的效果。

## **四、懒加载中涉及的属性**

```js
document.documentElement.clientHeight;   
//表示浏览器可见区域高度：

document.body.clientHeight 
//是整个页面内容的高度，而非浏览器可见区域的高度

document.documentElement.scrollTop;   
//滚动条已滚动的高度：

chrome 中 document.body.scrollTop  
//滚动条滚过的高度

//那么要得到滚动条的高度：有一个技巧：

var scrollTop=document.body.scrollTop  || document.documentElement.scrollTop;

//这两个值总会有一个恒为0，所以不用担心会对真正的scrollTop造成影响。一点小技巧，但很实用。

//offsetTop、offsetLeft

obj.offsetTop 指 obj 距离上方或上层控件的位置，整型，单位像素。

obj.offsetLeft 指 obj 距离左方或上层控件的位置，整型，单位像素。

obj.offsetWidth 指 obj 控件自身的宽度，整型，单位像素。

obj.offsetHeight 指 obj 控件自身的高度，整型，单位像素。

offsetParent 不同于parentNode ,offsetParent 返回的是在结构层次中与这个元素最近的position为absolute\relative\static的元素或者body
```

## **五、懒加载的实现**

```html
<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <title>Lazyload</title>
    <style>
        img {
            display: block;
            margin-bottom: 50px;
            height: 200px;
        }
    </style>
</head>

<body>
    <img src="loading.jpg" data-src="test.jpg">
    <img src="loading.jpg" data-src="test.jpg">
    <img src="loading.jpg" data-src="test.jpg">
    <img src="loading.jpg" data-src="test.jpg">
    <img src="loading.jpg" data-src="test.jpg">
    <img src="loading.jpg" data-src="test.jpg">
    <img src="loading.jpg" data-src="test.jpg">
    <img src="loading.jpg" data-src="test.jpg">
    <img src="loading.jpg" data-src="test.jpg">
    <img src="loading.jpg" data-src="test.jpg">
    <img src="loading.jpg" data-src="test.jpg">
    <img src="loading.jpg" data-src="test.jpg">
    <script>
        function throttle(fn, delay, atleast) {
            //函数绑定在 scroll 事件上，当页面滚动时，避免函数被高频触发，
            var timeout = null,//进行去抖处理
                startTime = new Date();
            return function () {
                var curTime = new Date();
                clearTimeout(timeout);
                if (curTime - startTime >= atleast) {
                    fn();
                    startTime = curTime;
                } else {
                    timeout = setTimeout(fn, delay);
                }
            }
        }
        function lazyload() {
            var images = document.getElementsByTagName('img');
            var len = images.length;
            var n = 0;      
            //存储图片加载到的位置，避免每次都从第一张图片开始遍历        
            return function () {
                var seeHeight = document.documentElement.clientHeight;
                // console.log(seeHeight)
                var scrollTop = document.documentElement.scrollTop || document.body.scrollTop;
                // console.log(scrollTop)
                // console.log(seeHeight + scrollTop)

                for (var i = n; i < len; i++) {
                    console.log(images[i].offsetTop)
                    // console.log(images[i].offsetTop)
                    // console.log(seeHeight + scrollTop)
                    console.log('-----'+(seeHeight + scrollTop))
                    if (images[i].offsetTop < seeHeight + scrollTop) {
                        if (images[i].getAttribute('src') === 'loading.jpg') {
                            images[i].src = images[i].getAttribute('data-src');
                        }
                        n = n + 1;
                        // console.log(n)
                    }
                }
            }
        }
        var loadImages = lazyload();
        loadImages();          //初始化首页的页面图片
        window.addEventListener('scroll', throttle(loadImages, 500, 1000), false);
　　//函数节流（throttle）与函数去抖（debounce）处理,
//500ms 的延迟，和 1000ms 的间隔，当超过 1000ms 未触发该函数，则立即执行该函数，不然则延迟 500ms 执行该函数
    </script>
</body>

</html>
```

