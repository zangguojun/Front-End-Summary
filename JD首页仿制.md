### 一、shortcut部分

+ 登陆和注册部分在一起放在同一个li标签中

  + ```html
    <div class="fl">
        <ul>
            <li>品优购欢迎你！</li>
            <li>
                <a href="#">请登陆</a>
                <a class="style-red" href="#">免费注册</a>
            </li>
        </ul>
    </div>
    ```

+ 版心可以这样定义

  + ```htm
    .w {
    	width: 1200px;
    	margin: 0 auto;
    }
    ```

+ div.header>div.w与div(.header+w)的区别

  + 前者的header可以实现比版心大的操作，而后面的header不行

  + 常用做如下

  + ```html
    .nav {
        border-bottom: 2px solid #b1191a;
        height: 45px;
    }
    <!-- 实现了在距顶端45px的部分画了一条 width:100% 宽度为2px的线 并非只有版心的1200px -->
    ```
  
+ ![image-20210209120341665](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210209120351.png)

  + 中间的竖线的实现方式多种，其中可以这样实现，html代码如下

    + ```html
      <ul>
          <li>我的订单</li>
          <li class="spacer"></li>
          <li>
              我的品优购
              <i class="iconfont icon-jiantou9"></i>
          </li>
          <li class="spacer"></li>
          <li>品优购会员</li>
          <li class="spacer"></li>
          <li>企业采购</li>
          <li class="spacer"></li>
          <li>
              关注品优购
              <i class="iconfont icon-jiantou9"></i>
          </li>
          <li class="spacer"></li>
          <li>
              客户服务
              <i class="iconfont icon-jiantou9"></i>
          </li>
          <li class="spacer"></li>
          <li>
              网站导航
              <i class="iconfont icon-jiantou9"></i>
          </li>
      </ul>
      ```

  + 此时字体图标可以会导致整段字体下移动，可以根据line height与height的大小关系进行进一步调节

    + 相等，能实现垂直方向对齐
    + 小于，文字向上移动
    + 大于，文字向下移动

+ logo的实现看起来简单，其实较为复杂，代码如下

  + ```html
    <div class="logo">
        <h1>
            <a href="index.html" title="品优购">品优购</a>
        </h1>
    </div>
    <!-- .logo>h1>a ，其中a标签的href需要放首页，title以及a标签文本需要放网站名称 -->
    ```

  + ```css
    .header {
        position: relative;
        height: 107px;
    }
    .header .logo {
        position: absolute;
        top: 25px;
        left: 0;
    }
    .logo h1 a {
        display: block;
        height: 60px;
        width: 175px;
        background: url(../img/logo.png) no-repeat;
        font-size: 0;
    }
    /*
    logo使用绝对定位实现，但子绝父相，需要.herader相对定位，.logo绝对定位
    其中logo内的a标签需要撑满整个盒子，从而达到鼠标移动到logo旁时，也能触发跳转
    */
    ```

+ <img src="https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210209122626.png" alt="image-20210209122625270" style="zoom:75%;" />此时的实现方式如下，简单理解就是，dt盒子和dd盒子都左浮动，而且dt盒子占满整个dropdown盒子，导致dd盒子被挤到下方。但是此时注意一个问题，每个li标签内的文字不能过多，否则会导致右箭头的字体图标被挤到下一行。

  + ```html
    <div class="dropdown fl">
        <div class="dt">全部商品分类</div>
        <div class="dd">
            <ul>
                <li class="menu_item">
                    <a href="#">图书</a>、
                    <a href="#">音像</a>、
                    <a href="#">数字商品</a>
                    <i class="iconfont icon-arrow-right-bold"></i>
                </li>
    			.......
                <li class="menu_item">
                    <a href="#">彩票、旅行</a>
                    <i class="iconfont icon-arrow-right-bold"></i>
                </li>
            </ul>
        </div>
    </div>
    ```

  + ```css
    .nav .dropdown {
        height: 45px;
        width: 209px;
        color: #ffffff;
        background-color: #b1191a;
    }
    .dropdown .dt {
        float: left;
        width: 209px;
        height: 45px;
        line-height: 45px;
        font-weight: 500;
        font-size: 16px;
        text-align: center;
    }
    .dropdown .dd {
        float: left;
        width: 209px;
    }
    ```

+ 

