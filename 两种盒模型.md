# 盒模型的理解

前端的盒模型包括两种，分别是W3C盒模型和IE盒模型，IE盒子模型与W3C的盒子模型唯一区别就是元素的宽度。

W3C盒模型包括content、padding、border、margin。其中width = content。

IE盒模型包括content、padding、border、margin。其中width = content+padding+border。

后来W3C在CSS3中新增了box-sizing的样式，属性包含content-box和border-box；

content-box就是默认的样式（W3C盒模型），

border-box是width包含了content+padding+boder（IE盒模型）