## JS脚本和CSS样式文件对页面渲染的影响

浏览器从服务器获取文档并从上到下进行解析，在脚本文件标签不包含defer和async属性的情况下，会按照如下规则执行文档内容：

- 解析html文档，遇到HTML标签时，构建DOM树
- 在构建DOM的过程中，如果遇到外联的样式声明或脚本声明，则暂停文档解析，创建新的网络连接，开始下载样式文件和脚本文件
- 样式文件下载完成后，构建CSS Rule DOM，脚本文件下载完成后，解释并立即执行。
- 构建DOM的同时，结合CSS规则树完成页面渲染。
- 如果DOM树先于CSS规则树构建完成，则在CSS规则树构建完成后，页面会发生一次重绘，将新构建的CSS规则应用于渲染树。

### （一）脚本文件对文档解析的影响

在这个过程中，脚本文件的**下载和执行**是与**文档解析同步**进行的，**脚本文件的下载和执行****都会阻塞文档的解析**，如果控制得不好，在用户体验上就会造成一定程度的影响。举个例子，编写一个简单的html页面如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script type="text/javascript" src="defer.js"></script>
</head>
<body>
    body render!
</body>
</html>
```

head中加载的脚本文件内容为：

```js
alert('i am the script');
```

在浏览器中运行时，会先弹出提示框，此时，body元素并没有进行渲染。

关闭弹出框以后，才完成对body的渲染：

![image-20210305202413487](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210305202421.png)

#### 脚本文件的defer和async属性

为了防止脚本加载使浏览器页面阻塞，妨碍用户体验。应该合理使用`<script>`标签的defer属性和async属性。这两个属性用于调整脚本的下载和执行顺序，使其不阻塞页面加载。

##### defer：开启新的线程下载脚本文件，并使脚本在文档解析完成后执行。

- defer只适用于外联脚本，如果script标签没有指定src属性，只是内联脚本，不要使用defer
- 如果有多个声明了defer的脚本，则会按顺序下载和执行
- defer脚本会在DOMContentLoaded和load事件之前执行

为了验证defer属性的相关规则，编写了如下测试代码：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script type="text/javascript" src="defer1.js" defer></script>
    <script type="text/javascript" src="defer2.js" defer></script>
    <script type="text/javascript" defer>
        console.log('使用了defer属性的内联js')
    </script>
</head>
<body>
    <script type="text/javascript">
       document.addEventListener("DOMContentLoaded", function() {
           console.log('DOMContentLoaded');
       }, false);
       window.addEventListener('load', function() {
           console.log('onload');
       }, false);
    </script>
</body>
</html>
```

`defer1.js`：

```js
console.log('defer1')
```

`defer2.js`:

```js
console.log('defer2')
```

控制台输出结果如下图，可以看出，对内联js脚本使用defer属性并不起作用。

![这里写图片描述](https://img-blog.csdn.net/20180316100948629?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNjU3MDI1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

##### async：异步下载脚本文件，下载完毕立即解释执行代码。 HTML5的新增属性。

+ 只适用于外联脚本，这一点和defer一致
+ 如果有多个声明async的脚本，其下载和执行都是异步的，不能确保彼此的先后顺序
+ async会在load事件之前执行，但并不能确保与DOMContentLoaded的执行先后顺序

为了验证async属性的特性，编写页面如下：

```HTML
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Document</title>
    <script type="text/javascript" src="async1.js" async></script>
    <script type="text/javascript" src="async2.js" async></script>
    <script type="text/javascript" async>
        console.log('使用了async属性的内联js')
    </script>
</head>
<body>
    <script type="text/javascript">
        document.addEventListener("DOMContentLoaded", function() {
           console.log('DOMContentLoaded');
       }, false);

       window.addEventListener('load', function() {
           console.log('onload');
       }, false);
    </script>
</body>
</html>
```

`async1.js`:

```js
console.log('async1.js')
```

`async2.js`:

```js
console.log('async2.js')
```

输出结果为：
![这里写图片描述](https://img-blog.csdn.net/20180316101735766?watermark/2/text/Ly9ibG9nLmNzZG4ubmV0L3FxXzMyNjU3MDI1/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

#### 无任何特殊属性

下面这张图清楚的阐述了`<script>`脚本在无任何特殊属性、具有defer属性、具有async属性三种情况下的下载执行与文档解析的关系。其中绿色带包文档解析线程；蓝色代表脚本下载线程；红色代表脚本执行线程。

![这里写图片描述](https://img-blog.csdn.net/20160503104416135)

综上所述，defer属性和async属性都能防止脚本下载阻塞页面渲染。如果脚本之间没有依赖关系，可以使用async属性，如果脚本之间有依赖关系，应使用defer属性。如果同时使用async和defer属性，后者不起作用，浏览器行为由async属性决定。如果不想使用这两个属性，也可将<script>标签放在在</body>之前，达到同样的效果。

### （二）CSS对页面渲染的影响

> 前端页面渲染时会根据DOM结构生成一个DOM树,然后加上CSS样式生成渲染树。

+ 如果CSS文件放在`<head>`标签中，则CSS Rule Tree会先于DOM树完成构建，之后浏览器就可以边构建DOM树边完成渲染；

+ 反之，CSS文件放在所有页面标签之后，比如`<body/>`之后，那么当DOM树构建完成了，渲染树才构建，浏览器不得不再重新渲染整个页面，这样造成了资源的浪费。而且页面还可能会出现闪跳的感觉，或者白屏或者布局混乱或者样式很丑，直到CSS加载完成，页面重绘才能恢复正常。
+ **但如果css文件较大，会让首页白屏时间更长**，所以并不是说把css都放顶部是一个完美的方法。权衡利弊，应该**把必须的css（js）放顶部，把不那么重要的css（js）放底部。**

