## 先说一下Cookie

> 管理服务器与客户端之间状态的 Cookie，虽然没有被编入标准化 HTTP/1.1 的 RFC2616 中，但在 Web 网站方面得到了广泛的应用。
>
>  **Cookie 的工作机制是用户识别及状态管理。**Web 网站为了**管理用户的状态**会通过 Web 浏览器，把一些数据临时写入用户的计算机内。接着当用户访问该Web网站时，可通过通信方式取回之前发放的 Cookie。 --- 图解HTTP

### Cookie 有关的首部字段

| 首部字段名 | 说明                            | 首部类型     |
| ---------- | ------------------------------- | ------------ |
| Set-Cookie | 开始状态管理 所使用的Cookie信息 | 响应首部字段 |
| Cookie     | 服务器接收到的Cookie信息        | 请求首部字段 |

#### 1、Set-Cookie

> Set-Cookie: status=enable; expires=Tue, 05 Jul 2011 07:26:31 G

##### Set-Cookie 字段的属性

![image-20210318183101726](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210318183109.png)

###### expires 属性

一旦 Cookie 从服务器端发送至客户端，服务器端就不存在可以显式删除 Cookie 的方法。但可通过覆盖已过期的 Cookie，实现对客户端 Cookie 的实质性删除操作

###### path 属性

Cookie 的 path 属性可用于限制指定 Cookie 的发送范围的文件目录。不过另有办法可避开这项限制，看来对其作为安全机制的效果不能抱有期待。

###### domain 属性

通过 Cookie 的 domain 属性指定的域名可做到**与结尾匹配一致**。比 如，当指定 example.com 后，除 example.com 以外，`www.example.com` 或 `www2.example.com` 等都可以发送 Cookie。因此，除了针对具体指定的多个域名发送 Cookie 之 外，不指定 domain 属性显得更安全。

###### secure 属性

Cookie 的 secure 属性用于限制 Web 页面仅在 HTTPS 安全连接时，才 可以发送 Cookie。

> Set-Cookie: name=value; secure

以上例子仅当在 `https://www.example.com/`（HTTPS）安全连接的情况 下才会进行 **Cookie 的回收**。也就是说，即使域名相同， `http://www.example.com/`（HTTP）也不会发生 Cookie 回收行为。 当省略 secure 属性时，不论 HTTP 还是 HTTPS，都会对 Cookie 进行 回收。

###### HttpOnly 属性

Cookie 的 HttpOnly 属性是 Cookie 的扩展功能，它使 JavaScript 脚本 无法获得 Cookie。其主要目的为防止**跨站脚本攻击**（Cross-site scripting，XSS）对 Cookie 的信息窃取。

> Set-Cookie: name=value; HttpOnly

通过上述设置，通常从 Web 页面内还可以对 Cookie 进行读取操作。 但使用 JavaScript 的 document.cookie 就无法读取附加 HttpOnly 属性后 的 Cookie 的内容了。因此，也就无法在 XSS 中利用 JavaScript 劫持 Cookie 了。另外顺带一提，该扩展并非是为了 防止 XSS 而开发的。

#### 2、Cookie

> Cookie: status=enable

首部字段 Cookie 会告知服务器，当客户端想获得 HTTP 状态管理支持时，就会在请求中包含从服务器接收到的 Cookie。接收到多个 Cookie 时，同样可以以多个 Cookie 形式发送。

### 其他首部字段

HTTP 首部字段是可以自行扩展的。所以在 Web 服务器和浏览器的应 用上，会出现各种非标准的首部字段。如X-Frame-Options，X-XSS-Protection，DNT，P3P等

### Cooike的特性

Cookie 是个存储在浏览器目录的文本文件，当浏览器运行时，存储在 RAM 中。一旦你从该网站或网络服务器退出，Cookie 也可存储在计算机的硬驱上。当访客结束其浏览器对话时，即终止的所有 Cookie。

#### Cookie内存大小受限

|                                | IE 6.0 | IE 7.0 8.0 | Opera | Fire Fox | Safari | Chrome |
| ------------------------------ | ------ | ---------- | ----- | -------- | ------ | ------ |
| Cookie个数<br />（每个域名下） | 20个   | 50个       | 30个  | 50个     | 无限   | 53个   |
| Cookie大小（字节）             | 4095   | 4095       | 4096  | 4097     | 4097   | 4097   |

`cookie`数据大小不能超过`4k`。

#### Cookie具有生命周期

Cookie可以保持登录信息到用户下次与服务器的会话。而还有一些Cookie在用户退出会话的时候就被删除了，这样可以有效保护个人隐私。

Cookie在生成时就会被指定一个Expire值，这就是Cookie的生存周期，在这个周期内Cookie有效，超出周期Cookie就会被清除。有些页面将Cookie的生存周期设置为“0”或负值，这样在关闭浏览器时，就马上清除Cookie，不会记录用户信息，更加安全。

#### Cookie满足同源策略

```js
function setCookie(key, value, iDay) {
	var oDate = new Date();
	oDate.setDate(oDate.getDate() + iDay);
	document.cookie = key + '=' + value + ';expires=' + oDate;

}
function removeCookie(key) {
	setCookie(key, '', -1);//这里只需要把Cookie保质期退回一天便可以删除
}
function getCookie(key) {
	var cookieArr = document.cookie.split('; ');
	for(var i = 0; i < cookieArr.length; i++) {
		var arr = cookieArr[i].split('=');
		if(arr[0] === key) {
			return arr[1];
		}
	}
	return false;
}
```

## 在说一下Web Storage、SessionStorage和LocalStorage

`sessionStorage`和`localStorage`不会自动把数据发给服务器，仅在本地保存。

### 1、Web Storage

​    Web Storage 是HTML 5引入的一个重要的功能，在前端开发的过程中会经常用到，它可以在客户端本地存储数据，类似cookie，但其功能却比cookie强大的多。cookie的大小只有4Kb左右（浏览器不同，大小也不同），而web Storage的大小有5MB。其API提供的方法有以下几种：

+ setItem (key, value) 
  + 保存数据，以键值对的方式储存信息。
+ getItem (key)
  + 获取数据，将键值传入，即可获取到对应的value值。
+ removeItem (key) 
  + 删除单个数据，根据键值移除对应的信息。
+ clear () 
  + 删除所有的数据
+ key (index) 
  +  获取某个索引的key

###   2、localStorage

​    localStorage的生命周期是永久性的。假若使用localStorage存储数据，即使关闭浏览器，也不会让数据消失，除非主动的去删除数据，使用的方法如上所示。localStorage有length属性，可以查看其有多少条记录的数据。使用方法如下：

```js
var storage = null;  
if(window.localStorage){             
  //判断浏览器是否支持localStorage  
  storage = window.localStorage;       
  storage.setItem("name", "Rick");   
  //调用setItem方法，存储数据  
  alert(storage.getItem("name"));     
  //调用getItem方法，弹框显示 name 为 Rick  
  storage.removeItem("name");     
  //调用removeItem方法，移除数据  
  alert(storage.getItem("name"));   
  //调用getItem方法，弹框显示 name 为 null  
}
```

### 3、sessionStorage

​    sessionStorage 的生命周期是在浏览器关闭前。也就是说，在整个浏览器未关闭前，其数据一直都是存在的。sessionStorage也有length属性，其基本的判断和使用方法和localStorage的使用是一致的。需要注意的有以下几点：

+ 页面刷新不会消除数据
+ 只有在当前页面打开的链接，才可以访sessionStorage的数据
  + 可以实现多个标签页之间的通信
+ 使用window.open打开页面和改变localtion.href方式都可以获取到sessionStorage内部的数据

**只有当全部关闭index.html和从其内部打开的所有页面或者直接关闭浏览器，才可以消除sessionStorage中的数据**

`sessionStorage`和`localStorage` 不会自动把数据发给服务器，仅在本地保存，虽然也有存储大小的限制，但比`cookie`大得多，可以达到 `5M` 或更大。

他们有相同的方法

+ setItem (key, value) 
  + 保存数据，以键值对的方式储存信息。
+ key[value]
  + 同setItem (key, value) 
+ getItem (key)
  + 获取数据，将键值传入，即可获取到对应的value值。
+ removeItem (key) 
  + 删除单个数据，根据键值移除对应的信息。
+ clear () 
  + 删除所有的数据