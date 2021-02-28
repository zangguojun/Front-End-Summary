# DOM

> DOM，全称Document Object Model文档对象模型
>
> * 文档表示的就是整个的HTML网页文档
> * 对象表示将网页中的每一个部分都转换为了一个对象
> * 使用模型来表示对象之间的关系，这样方便我们获取对象

## 节点

> 构成我们网页的最基本的组成部分，网页中的每一个部分都可以称为是一个节点
>
> > html标签、属性、文本、注释、整个文档等都是一个节点
> >
> > 标签称为元素节点 -- HTML文档中的HTML标签
> >
> > 属性称为属性节点 -- 元素的属性
> >
> > 文本称为文本节点 -- HTML标签中的文本内容
> >
> > 文档称为文档节点 -- 整个HTML文档
> >
> > 注释称为注释节点 -- 

![image-20210228125419290](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210228125427.png)

### 文档节点（document）

> 文档节点document，代表的是整个HTML文档，网页中的所有节点都是它的子节点。
>
> document对象作为window对象的属性存在 的，我们不用获取可以直接使用。
>
> > window.document

### 元素节点（Element）

> HTML中的各种标签都是元素节点，这也是我们最常用 的一个节点
>
> 浏览器会将页面中所有的标签都转换为一个元素节点，我们可以通过document的方法来获取元素节点。
>
> > document.getElementById(‘app’)

### 文本节点（Text）

> 文本节点表示的是HTML标签以外的文本内容，任意非HTML的文本都是文本节点。
>
> 它包括可以字面解释的纯文本内容。
>
> 文本节点一般是作为元素节点的子节点存在的。获取文本节点时，一般先要获取元素节点。在通过元素节点获取文本节点。
>
> >元素节点.firstChild;
> >
> >获取元素节点的第一个子节点，一般为文本节点

### 属性节点（Attr）

> 属性节点表示的是标签中的一个一个的属性，这里要注意的是属性节点并非是元素节点的子节点，而是元素节点的一部分。可以通过元素节点来获取指定的属性节点
>
> >元素节点.getAttributeNode("属性名");

## 事件

> 文档或浏览器窗口中发生的一些特定的交互瞬间
>
> JavaScript 与 HTML 之间的交互是通过事件实现的。
>
> > 点击某个元素
> >
> > 将鼠标移动至某个元素上方
> >
> > 按下键盘上某个键

### 获取元素节点

> 通过document对象调用下面**方法**

* getElementById()  一个
* getElementsByTagName()  一组
* getElementsByName()  一组

### 获取元素节点的子节点

> 通过具体的元素节点调用

> **方法**

* getElementsByTagName()
  * 返回当前节点的指定标签名后代节点

> **属性**

* childNodes
  * 表示当前节点的所有子节点
* firstChild
  * 表示当前节点的第一个子节点
* lastChild
  * 表示当前节点的最后一个子节点

### 获取父节点和兄弟节点

> 通过具体的节点调用

> 属性

+ parentNode
  + 表示当前节点的父节点
+ previousSibling
  + 表示当前节点的前一个兄弟节点
+ nextSibling
  + 表示当前节点的后一个兄弟节点

### 元素节点的属性

+ 获取（元素对象.属性名）
  + element.value
  + element.id
  + element.className
+ 设置（元素对象.属性名=新的值）
  + element.value = “hello”
  + element.id = “id01”
  + element.className = “newClass”

### 其他属性

+ nodeValue
  + 文本节点可以通过nodeValue属性获取和设置文本节点的内容
+ innerHTML
  + – 元素节点通过该属性获取和设置标签内部的html代码

### 使用CSS选择器进行查询

+ querySelector()
+ querySelectorAll()

> 这两个方法都是用document对象来调用，两个方法使用相同， 都是传递一个选择器字符串作为参数，方法会自动根据选择器字符串去网页中查找元素
>
> 不同的地方是querySelector()只会返回找到的第一个元素，而 querySelectorAll()会返回所有符合条件的元素。

### 元素节点的修改

> 主要指对元素节点的操作。

+ 创建节点
  + document.createElement(标签名)
+ 删除节点
  + 父节点.removeChild(子节点)
+ 替换节点
  + 父节点.replaceChild(新节点 , 旧节点)
+ 插入节点
  + 父节点.appendChild(子节点)
  + 父节点.insertBefore(新节点 , 旧节点)





