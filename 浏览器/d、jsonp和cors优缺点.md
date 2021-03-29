## JSONP

## 优点：

+ 它不像**XMLHttpRequest**对象实现的Ajax请求那样受到同源策略的限制，JSONP可以**跨越同源策略**；

+ 它的**兼容性更好**，在更加古老的浏览器中都可以运行，不需要XMLHttpRequest或ActiveX的支持

+ 在请求完毕后可以通过调用callback的方式回传结果。将回调方法的权限给了调用方。这个就相当于将controller层和view层终于分 开了。
+ 我提供的jsonp服务只提供**纯服务的数据**，至于提供服务以后的页面渲染和后续**view操作**都由调用者来自己定义就好了。如果有两个页面需要渲染同一份数据，你们只需要有不同的渲染逻辑就可以了，逻辑都可以使用同一个jsonp服务。

## 缺点：

+ 它只支持GET请求而不支持POST等其它类型的HTTP请求

+ 无法判断jsonp请求成功或者失败，jsonp在调用失败的时候不会返回**错误的HTTP状态码**。

+ 缺点是安全性。万一假如提供jsonp的服务存在页面注入漏洞，即它返回的javascript的内容被人控制的。那么结果是什么？所有调用这个 jsonp的网站都会存在漏洞。

  ------------------

## CORS 中的预检请求

在 CORS 机制中，客户端将请求分为了两种：**简单请求**和**非简单请求**，当请求为非简单请求时，就会**触发浏览器发送预检请求**，这是浏览器的行为。

##### 1. 简单请求

简单请求不会触发CORS预检请求，“简属于
单请求”术语并不属于Fetch(其中定义了CORS)规范。
若满足所有下述条件，则该请求可视为“简单请求”：
\- 使用下列方法之一：
\- `GET`
\- `HEAD`
\- `POST`
\- `Content-Type`: (仅当POST方法的Content-Type值等于下列之一才算做简单需求)
\- `text/plain`
\- `multipart/form-data`
\- `application/x-www-form-urlencoded`

##### 2.需预检的请求

“需预检的请求”要求必须首先使用`OPTIONS`方法发起一个预检请求到服务区，以获知服务器是否允许该实际请求。“预检请求”的使用，可以避免跨域请求对服务器的用户数据产生未预期的影响。

当请求满足下述任一条件时，即应首先发送预检请求：
\- 使用了下面任一 HTTP 方法：
\- `PUT`
\- `DELETE`
\- `CONNECT`
\- `OPTIONS`
\- `TRACE`
\- `PATCH`
\- 人为设置了[对 CORS 安全的首部字段集合](https://fetch.spec.whatwg.org/#cors-safelisted-request-header)之外的其他首部字段。该集合为：
\- `Accept`
\- `Accept-Language`
\- `Content-Language`
\- `Content-Type`
\- `DPR`
\- `Downlink`
\- `Save-Data`
\- `Viewport-Width`
\- `Width`
\- `Content-Type`的值不属于下列之一:
\- `application/x-www-form-urlencoded`
\- `multipart/form-data`
\- `text/plain`



预检请求会**向服务器确认跨域是否允许**，服务返回的**响应头里有对应字段**`Access-Control-Allow-Origin`来给浏览器判断：**如果允许**，**浏览器紧接着发送实际请求**；**不允许**，**报错并禁止客户端脚本读取响应**相关的任何东西。

> **HTTP 的 `OPTIONS 方法`** 用于**获取目的资源所支持的通信选项**。客户端可以对特定的 URL 使用 OPTIONS 方法，也可以对整站（通过将 URL 设置为“*****”）使用该方法。



#### 既然服务端做了请求限制，而且浏览器判断跨域只和 `Access-Control-Allow-Origin` 有关，预检请求是否有点多余？

浏览器为了安全的数据传输，提出了 CORS 机制，它更像一种授权机制，需要**浏览器**和**服务器**共同配合实现，对于没有实现此机制的客户端，比如 curl，是不受限制的：

<img src="https://pic1.zhimg.com/80/v2-ee2b893ed6c9187df5ccc41016a6f0bc_720w.jpg" alt="img" style="zoom:150%;" />

<img src="https://pic2.zhimg.com/80/v2-6b53dc211d3edaf0d5d280d032351aed_720w.jpg" alt="img" style="zoom:200%;" />

上图中，服务器实现很简单，仅仅返回预先定义好的数据，curl 的返回头中也没有和`Access-Control-*`相关字段，但是返回体中，我们能够看到返回数据，想必解析出来也并不困难。同样的请求在浏览器中就会报错了

![img](https://pic1.zhimg.com/80/v2-bfb956a627b2e8cfdc9caa8559d7e174_720w.jpg)

从这里可以看出，在 CORS 机制中，**默认服务器为禁止跨域**，服务器啥也不做就**能禁止浏览器跨域**了；但是，实际中能发请求的客户端很多，每个请求的目的很复杂，对于**那些真正要禁止跨域传输的服务自然有一套处理逻辑**，这些**逻辑很可能是复杂和高消耗的**。

如果类似浏览器这种，包含 CORS 机制的客户端发送的请求，每次都要经过一个复杂逻辑才能知道自己是否跨域，服务器的压力和用户体验是不理想的，那么预检请求就孕育而生：发送实际请求前，先发送预检请求询问服务器是否允许跨域，不允许就不发送实际请求，服务器只需要对预检请求进行跨域处理。

这样来看，在 CORS 机制中，发送预检请求是一种保护机制，**保护资源不被未授权的请求修改**。和**授权服务**很像，预检请求通过了，浏览器后续对同一服务的请求，不需要做跨域询问，服务端不想支持跨域访问，啥也不用做。



## 表单请求

> 原生 form 表单可以提交 POST 请求，而且为一个简单请求，很可能修改服务端数据，仅仅依靠 CORS 机制也不安全。

#### from的重要属性

**`action`**

处理表单提交的 URL。

```html
这个值可被 <button>、<input type="submit"> 或 <input type="image"> 元素上的 formaction 属性覆盖。
```

**`enctype`**

当 `method` 属性值为 `post` 时，`enctype` 就是将**表单的内容**提交给服务器的 [MIME 类型](http://en.wikipedia.org/wiki/Mime_type) 。可能的取值有：

- `application/x-www-form-urlencoded`：未指定属性时的默认值。

- `multipart/form-data`：当表单包含 `type=file` 的 [``](https://developer.mozilla.org/zh-CN/docs/Web/HTML/Element/Input) 元素时使用此值。

- `text/plain`：出现于 HTML5，用于调试。

- ```html
  这个值可被 <button>、<input type="submit"> 或 <input type="image"> 元素上的 formenctype 属性覆盖。
  ```

**`method`**

浏览器使用这种 [HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP) 方式来提交 表单. 可能的值有：

- `post`：指的是 HTTP [POST 方法](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.5)；表单数据会包含在表单体内然后发送给服务器.

- `get`：指的是 HTTP [GET 方法](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html#sec9.3)；表单数据会附加在 `action` 属性的 URL 中，并以 '?' 作为分隔符，[没有副作用](https://wiki.developer.mozilla.org/en-US/docs/Glossary/Idempotent) 时使用这个方法。

- `dialog`：如果表单在 `<dialog>`  元素中，提交时关闭对话框。

- ```html
  此值可以被 <button>、<input type="submit"> 或 <input type="image"> 元素中的 formmethod 属性覆盖。
  ```

**`target`**

表示在提交表单之后，在哪里显示响应信息。在 HTML 4 中, 这是一个 frame 的名字/关键字对。在 HTML5 里，这是一个*浏览上下文* 的名字/关键字（如标签页、窗口或 iframe）。下述关键字有特别含义：

- `_self`：默认值。在相同**浏览上下文**中加载。
- `_blank`：在**新的未命名的浏览上下文**中加载。
- `_parent`：在**当前上下文的父级浏览上下文**中加载，**如果没有父级，则与 _self 表现一致**。
- `_top`：在**最顶级的浏览上下文中**（即当前上下文的一个没有父级的祖先浏览上下文），**如果没有父级，则与 _self 表现一致。**



原生 form 表单请求，和 action，method，enctype 属性有关。form 表单提交后，会自动跳转页面到 action 所指向的 URL 来获取结果，最后变成同域，在没有 AJAX 技术的时候，**我们发 POST 一般会提交到当前 URL，后端响应 POST 请求，处理之后，又将当前页面返回浏览器重新渲染，这也是每次提交表单会刷新页面的原因**。

## **最后，HTTP 服务是无状态的哦！**

即便通过了预检请求的检查，浏览器的后续请求，服务端只会响应当前请求；所以，可能出现预检请求没问题，后续的实际请求出现跨域：

<img src="https://pic1.zhimg.com/80/v2-8e32a8176ba79f81ea02fc2cbb78a700_720w.jpg" alt="img" style="zoom:150%;" />

<img src="https://pic1.zhimg.com/80/v2-a3449ca90b1a90209307ab72429c8a54_720w.jpg" alt="img" style="zoom:150%;" />

所以，对于**无状态服务**，真想限制跨域资源传输，仅仅依赖 CORS 机制是做不到的， 除了浏览器这种客户端遵守了并实现了 CORS，世界上这么多客户端是不遵守的：**curl（可以回顾之前的示例图）、wget、server proxy**，还有**各种爬虫脚本**，**postman 这种 API 测试工具**，可能都没实现，都能正常发送请求和获取数据。

想一想，这个请求可能是 CRUD 中的任何一种，成功执行服务端逻辑并返回 2xx，这时，CORS 验证机制像是马后炮。假如服务器不想支持跨域访问，还是得在响应处理的时候，**去实现过滤机制防止资源被非法获取或修改**。



## 携带credentials的请求

对于跨域（发生CORS）的请求默认是不会带上凭证信息（credentials）的，如果要发送凭证信息（credentials）就需要设置对应的标识位。

请求： - 请求中要设置*withCredentials*为true。

响应： - Access-Control-Allow-Credentials： true - Access-Control-Allow-Origin的值不再是通配符*，应该是单一的origin。

HTTP规范规定`Access-Control-Allow-Origin`不能是通配符*并且只能是单一的`origin`。这是因为如果能设置多个的话，证明该服务器就能接受多个域名下面的cookie，这是很危险的。

## CORS中的请求头和响应头

## 响应头

## Access-Control-Allow-Origin

```
Access-Control-Allow-Origin: <origin> | *
```

origin参数的值制定了允许访问服务器资源的外域URI。对于不需要携带身份凭证的请求，服务器可以指定这个字段的值为通配符*，表示允许来自所有域的请求。

## Access-Control-Expose-Headers

该头信息服务器把**允许浏览器访问的头放入白名单**，例如：

`Access-Control-Expose-Headers: X-My-Custom-Header, X-Another-Custom-Header` 在跨域访问的时候，XHR对象的getResponseHeader()只能拿到一些最基本的响应头。

## Access-Control-Max-Age

指定了**预检请求（preflight）请求的结果能被缓存多久**（秒为单位）。

## Access-Control-Allow-Credentials

当浏览器的credentials设置为true时，**是否允许浏览器读取response的内容**

## Access-Control-Allow-Methods

作为预检请求的响应头，**指明了实际请求所允许的HTTP方法**。

## Access-Control-Allow-Headers

用于预检请求的响应。其指明了**实际请求中允许携带的首部字段**。 以逗号分割。



## 请求头

这些字段一般无需手动设置。

## Origin

预检请求或实际请求的源站。

不包含任何路径，只是服务器的名称。（不管是否为跨域，这个字段都被发送。）

## Access-Control-Request-Method

用于预检请求。其作用是，将实际请求所使用的 HTTP 方法告诉服务器。



------------------------



## JSONP和CORS对比

+ JSONP的主要优势在于**对浏览器的支持较好**；虽然目前主流浏览器支持CORS，但**IE10以下不支持CORS。**

+ JSONP只能**用于获取资源**（即只读，类似于**GET请求**）；CORS**支持所有类型的HTTP请求**，功能完善。（这点JSONP被玩虐，但大部分情况下GET已经能满足需求了） 
+ JSONP的错误处理机制并不完善，我们没办法进行**错误处理**；而CORS可以通过**onerror事件监听错误**，并且浏览器控制台**会看到报错信息，利于排查**。 
+ JSONP只会发一次请求；而**对于复杂请求，CORS会发两次请求**。 
+ 始终觉得安全性这个东西是相对的，没有绝对的安全，也做不到绝对的安全。
  + 毕竟JSONP并不是跨域规范，它存在很明显的安全问题：callback参数注入和资源访问授权设置。
  + CORS好歹也算是个**跨域规范**，**在资源访问授权方面进行了限制**（**Access-Control-Allow-Origin**），而且标准浏览器都做了安全限制，比如拒绝手动设置origin字段，相对来说是安全了一点。 
  + 但是回过头来看一下，就算是不安全的JSONP，我们依然可以在服务端端进行一些权限的限制，服务端和客户端也都依然可以做一些注入的安全处理，哪怕被攻克，它也只能读一些东西。就算是比较安全的CORS，同样可以在服务端设置出现漏洞或者不在浏览器的跨域限制环境下进行攻击，而且它不仅可以读，还可以写。







