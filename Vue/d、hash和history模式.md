vue-router（前端路由）有两种模式，hash模式和history模式，这里来谈谈两者的区别。

### **hash模式**

这里的 hash 就是指 url 尾巴后的 # 号以及后面的字符。这里的 # 和 css 里的 # 是一个意思。hash 也称作锚点，本身是用来做页面定位的，她可以使对应 id 的元素显示在可视区域内。

由于 **hash 值变化不会导致浏览器向服务器发出请求，而且 hash 改变会触发 hashchange 事件**（hashchange只能改变 # 后面的url片段**）*；更关键的一点是，**因为hash发生变化的url都会被浏览器记录下来，从而你会发现浏览器的前进后退都可以用了**，所以人们在 html5 的 history 出现前，基本都是使用 hash 来实现前端路由的。

hash模式背后的原理是`onhashchange`事件,可以在window对象上监听这个事件:

hash模式又叫前端路由，这成为了单页应用标配；

### history模式

 hash 能兼容到IE8， history 只能兼容到 IE10；

首先，hash 本来是拿来做页面定位的，如果拿来做路由的话，原来的锚点功能就不能用了。其次，hash 的传参是基于 url 的，如果要传递复杂的数据，会有体积的限制，而 history 模式不仅可以在url里放参数，还可以将数据存放在一个特定的对象中。

history api可以分为两大部分：**切换和修改**

+ 切换历史状态包括back、forward、go

​     这三个方法，对应浏览器的前进，后退，跳转操作；（跳转操作：在前进后退上长按鼠标，**会出来所有当前窗口的历史记录**，从而可以跳转）

```js
history.go(-2);//后退两次
history.go(2);//前进两次
history.back(); //后退
hsitory.forward(); //前进
```



+ 修改历史状态包括了 `pushState`, `replaceState`两个方法

这两个方法接收三个参数:`stateObj`, `title`, `url`

```js
window.history.pushState(state, title, url) 
// state：需要保存的数据，这个数据在触发popstate事件时，可以在event.state里获取
// title：标题，基本没用，一般传 null
// url：设定新的历史记录的 url。新的 url 与当前 url 的 origin 必须是一樣的，否则会抛出错误。url可以是绝对路径，也可以是相对路径。
//如 当前url是 https://www.baidu.com/a/,执行history.pushState(null, null, './qq/')，则变成 https://www.baidu.com/a/qq/，
//执行history.pushState(null, null, '/qq/')，则变成 https://www.baidu.com/qq/
 
window.history.replaceState(state, title, url)
// 与 pushState 基本相同，但她是修改当前历史记录，而 pushState 是创建新的历史记录
 
window.addEventListener("popstate", function() {
    // 监听浏览器前进后退事件，pushState 与 replaceState 方法不会触发              
});
```

通过pushstate把页面的状态保存在state对象中，当页面的url再变回这个url时，可以通过event.state取到这个state对象，从而可以对页面状态进行还原，这里的页面状态就是页面字体颜色，其实滚动条的位置，阅读进度，组件的开关的这些页面状态都可以存储到state的里面。

 history 模式改变 url 的方式会导致浏览器向服务器发送请求，这不是我们想看到的，我们需要在服务器端做处理：如果匹配不到任何静态资源，则应该始终返回同一个 html 页面。

通过history api，我们丢掉了丑陋的#但是，它也有个问题：不怕前进，不怕后退，**就怕刷新，f5**（如果后端没有准备的话，会分分钟刷出一个404来）,因为刷新是实实在在地去请求服务器的,不玩虚的。





