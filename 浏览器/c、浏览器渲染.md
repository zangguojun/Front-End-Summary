## 浏览器渲染（详细版）

> 浏览器渲染页面的下载是自上而下的，渲染顺序也是自上而下的，下载与渲染同时进行；

+ (1)、如果HTTP响应报文是301或302重定向，则浏览器会相应头中的location再次发送请求

+ (2)、浏览器处理HTTP响应报文中的主体内容，首先使用loader模块加载相应的资源

  + loader模块有两条资源加载路径：主资源加载路径和派生资源加载路径。
    + 主资源即google主页的index.html文件 
    + 派生资源即index.html文件中用到的资源
  + 主资源到达后，浏览器的Parser模块解析主资源的内容，生成派生资源对应的DOM结构，然后根据需求触发派生资源的加载流程。比如，在解析过程中，如果遇到img的起始标签，会创建相应的image元素HTMLImageElement，接着依据img标签的内容设置HTMLImageElement的属性。在设置src属性时，会触发图片资源加载，发起加载资源请求
  + 这里常见的优化点是对派生资源使用缓存

+ (3)、使用parse模块解析HTML、CSS、Javascript资源

  + 解析HTML变成DOM树
    + HTML解析分为可以分为解码、分词、解析、建树四个步骤
    + （1）解码：将网络上接收到的经过编码的字节流，解码成Unicode字符
    + （2）分词：按照一定的切词规则，将Unicode字符流切成一个个的词语
    + （3）解析：根据词语的语义，创建相应的节点(Node)
    + （4）建树：将节点关联到一起，创建DOM树
    + ![img](https://images2015.cnblogs.com/blog/1034346/201703/1034346-20170329153234951-1328599578.png)
  + 解析CSS变成渲染树
    + ![img](https://images2015.cnblogs.com/blog/1034346/201703/1034346-20170329153247436-670728382.png)
  + 遇到JS时解析JS，解析完成后立即执行
    + JavaScript一般由单独的脚本引擎解析执行，它的作用通常是动态地改变DOM树（比如为DOM节点添加事件响应处理函数），即根据时间（timer）或事件（event）映射一棵DOM树到另一棵DOM树
    + 简单来说，经过了Parser模块的处理，浏览器把页面文本转换成了一棵节点带CSS Style、会响应自定义事件的Styled DOM树

+ (4)、构建DOM树、Render树及RenderLayer树

  > 浏览器的解析过程就是将字节流形式的网页内容构建成DOM树、Render树及RenderLayer树的过程

  > 使用parse解析HTML的过程，已经完成了DOM树的构建，接下来构建Render树

  + 【创建Render树】

    + Render树用于表示文档的可视信息，记录了文档中每个可视元素的布局及渲染方式
    + RenderObject是Render树所有节点的基类，作用类似于DOM树的Node类。这个类存储了绘制页面可视元素所需要的样式及布局信息，RenderObject对象及其子类都知道如何绘制自己。事实上绘制Render树的过程就是RenderObject按照一定顺序绘制自身的过程
    + DOM树上的节点与Render树上的节点并不是一一对应的。只有DOM树的根节点及可视节点才会创建对应的RenderObject节点

  + 【创建Render Layer树】

    + Render Layer树以层为节点组织文档的可视信息，网页上的每一层对应一个Render Layer对象。

    + RenderLayer树可以看作Render树的稀疏表示，每个RenderLayer树的节点都对应着一棵Render树的子树，这棵子树上所有Render节点都在网页的同一层显示

    + RenderLayer树是基于RenderObject树构建的，满足一定条件的RenderObject才会建立对应的RenderLayer节点

    + 下面是RenderLayer节点的创建条件：

      + （1）网页的root节点
      + （2）有显式的CSS position属性（relative，absolute，fixed）
      + （3）元素设置了transform
      + （4）元素是透明的，即opacity不等于1
      + （5）节点有溢出（overflow）、alpha mask或者反射（reflection）效果。
      + （6）元素有CSS filter（滤镜）属性
      + （7）2D Canvas或者WebGL
      + （8）Video元素

+ (5)、布局和渲染

  + 布局就是安排和计算页面中每个元素大小位置等几何信息的过程。HTML采用流式布局模型，基本的原则是页面元素在顺序遍历过程中依次按从左至右、从上至下的排列方式确定各自的位置区域

+ 简单情况下，布局可以顺序遍历一次Render树完成，但也有需要迭代的情况。当祖先元素的大小位置依赖于后代元素或者互相依赖时，一次遍历就无法完成布局，如Table元素的宽高未明确指定而其下某一子元素Tr指定其高度为父Table高度的30%的情况

  + Paint模块负责将Render树映射成可视的图形，它会遍历Render树调用每个Render节点的绘制方法将其内容显示在一块画布或者位图上，并最终呈现在浏览器应用窗口中成为用户看到的实际页面

+ 主要绘制顺序如下：

  + （1）背景颜色
  + （2）背景图片
  + （3）边框
  + （4）子呈现树节点
  + （5）轮廓

+ 6、硬件加速

  + 开启硬件渲染，即合成加速，会为需要单独绘制的每一层创建一个GraphicsLayer

![http](https://pic.xiaohuochai.site/blog/browserRender4.png)



## 浏览器渲染(简单版)

#### （1）浏览器渲染页面

+ 浏览器渲染页面的下载是自上而下的，渲染顺序也是自上而下的，下载与渲染同时进行；
+ 解析HTML变成DOM树；
+ 解析CSS变成CSSOM渲染树；
+ 遇到JS时解析JS，解析完成后立即执行；

> （1，2同时进行，一边解析一边生成页面，会随着页面的内容不断调整）

【重绘和回流】

#### （2）关联资源的处理

+ 在渲染页面到某一部分时，其上面的所有部分（当前的HTML元素和CSS）都已下载完成；
+ 但是相关联的的元素不一定下载完成了（例如：图片，视屏等元素会并行下载）；
+ 同一个域名下并行下载数量有限制（有些网站图片等其他资源会放在另外的服务器上）；

#### （3）JS和AJAX

+ 下载解析JS时候是以阻塞的方式进行下载，下载JS时，浏览器在发送了请求后直到响应前都会阻塞其它资源的下载和渲染；
+ 可以用HTML5的异步下载属性（async和defer）
  + async：遇到JS后与其他资源一起下载，下载完成后执行；
  + defer：遇到JS后与其他资源一起下载，等所有资源下载完成后再去执行；
+ JS的阻塞性下载是因为JS有可能会改变DOM结构，导致页面重绘；
+ 遇到AJAX后执行，然后进行下面的步骤，等待AJAX拿到数据后再执行AJAX回调；

#### （4）CSS

+ CSS下载完成后，将和原先下载的所有样式表一起进行解析，解析完成后，将对此前所有元素（包含已经渲染过的元素）再渲染一遍；

+ JS、CSS如果有重定义，首先查看优先级，以优先级高的为主，若无优先级问题则浏览器以后定义的为主；



## 浏览器渲染(总结版)

### 1、解析文档构建DOM树

浏览器的解析内容可以分为三个部分：

+ HTML/XHTML/SVG：解析这三种文件后，会生成DOM树（DOM Tree）
+ CSS：解析样式表，生成CSS规则树（CSS Rule Tree）
+ JavaScript：解析脚本，通过DOM API和CSSOM API操作DOM Tree和CSS Rule Tree，与用户进行交互。

以上三类文件的执行顺序会根据其在文档中的位置及其标签属性的不同而有异同，具体在后文进行讨论。

### 2、构建渲染树

解析文档完成后，浏览器引擎会将 CSS Rule Tree 附着到DOM Tree 上，并根据DOM Tree 和 CSS Rule Tree构造 Rendering Tree（渲染树）。此处需要注意：

+ Render Tree和DOM Tree的区别在于，类似Head或display：node之类的东西不会放在渲染树中；
+ 将CSS Rule Tree匹配到DOM Tree需要解析CSS的选择器，为了提高该过程的性能，DOM树应该尽量小，CSS Selector应该尽量使用id和class，避免过度层叠。

### 3、布局与绘制渲染树

解析position, overflow, z-index等等属性，计算每一个渲染树节点的位置和大小，此过程被称为reflow。最后调用操作系统的Native GUI API完成绘制（repaint）。注意：

- 渲染树的节点，在Gecko中称为frame，而在webkit中称为renderer；
- reflow和repaint是两个不同的概念，其区别会在后文进行探讨。

