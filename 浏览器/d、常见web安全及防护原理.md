## sql注入原理及防范 

​	就是通过把SQL命令插入到Web表单递交或输入域名或页面请求的查询字符串，最终达到欺骗服务器执行恶意的SQL命令。 

#### sql注入防范

1）永远不要信任用户的输入，要对用户的输入进行校验，可以通过正则表达式，或限制长度，对单引 号和双"-"进行转换等。 

2）永远不要使用动态拼装SQL，可以使用参数化的SQL或者直接使用存储过程进行数据查询存取。 

3）永远不要使用管理员权限的数据库连接，为每个应用使用单独的权限有限的数据库连接。 

4）不要把机密信息明文存放，请加密或者hash掉密码和敏感的信息。

## XSS原理及防范

​	Xss(cross-site scripting)攻击指的是攻击者往Web页面里插入恶意 html标签或者javascript代码。比 如：攻击者在论坛中放一个 看似安全的链接，骗取用户点击后，窃取cookie中的用户私密信息；或者攻击者在论坛中加一个恶意表单， 当用户提交表单的时候，却把信息传送到攻击者的服务器中，而不是用户原本以为的信任站点。

#### XSS防范方法

​	首先代码里对用户输入的地方和变量都需要仔细检查长度和对”<”,”>”,”;”,”’”等字符做过滤；其次任何内容 写到页面之前都必须加以encode，避免不小心把html tag 弄出来。这一个层面做好，至少可以堵住超过 一半的XSS 攻击。

​	首先，避免直接在cookie 中泄露用户隐私，例如email、密码等等。 其次，通过使cookie 和系统ip 绑定来降低cookie 泄露后的危险。这样攻击者得到的cookie 没有实际价值，不可能拿来重放。 如果网站不需要再浏览器端对cookie 进行操作，可以在Set-Cookie 末尾加上HttpOnly 来防止javascript 代码直接获取cookie 。 尽量采用POST 而非GET 提交表单

## XSS与CSRF有什么区别

XSS是获取信息，不需要提前知道其他用户页面的代码和数据包。CSRF是代替用户完成指定的动作，需要知道其他用户页面的代码和数据包。 要完成一次CSRF攻击，受害者必须依次完成两个步骤： 登录受信任网站A，并在本地生成Cookie。 在不登出A的情况下，访问危险网站B。 

CSRF的防御服务端的CSRF方式方法很多样，但总的思想都是一致的，就是

+ 在客户端页面增加伪随机数。 

+ 通过验证码的方法

![Web安全之CSRF攻击](https://pic1.zhimg.com/v2-31e6d640aa10bade4e9c3666c39c5f7a_1440w.jpg?source=172ae18b)

![img](https://pic4.zhimg.com/80/v2-0bdbc7f56a9dac0b413f0e835acdbdfb_720w.jpg)**充分利用好cookie的SameSite属性**

SameSite选项通常由Strict、Lax和None三个值

- Strict最为严格，如果cookie设置了Strict，那么浏览器会完全禁止第三方Cookie。
- Lax相对宽松一点，在跨站点的情况下，从第三方站点的链接打开和从第三方站点提交Get的表单都会携带cookie.但是如果在第三方站点中使用Post方法或者通过img、iframe等标签加载的URL,都不会携带Cookie。
- None, 任何情况下都会发送Cookie。

**验证请求的来源站点**

在服务器端验证请求的来源站点。因为CSRF攻击大多数都是来自第三方站点。

通过http请求头中的Referer和Origin属性

- referer属性

记录了该http请求的来源地址，但有些场景不适合将来源URL暴露给服务器，所以可以设置不用上传，并且referer属性是可以修改的，所以在服务器端校验referer属性并没有那么可靠

- origin属性

通过XMLHttpRequest、Fetch发起的跨站请求或者Post方法发送请求时，都会带上origin,所以服务器可以优先判断Origin属性，再根据实际情况判断是否使用referer判断。

**CSRF Token**

除了上面两个方法之外，还可以使用CSRF Token来验证。

- 在浏览器向服务器发起请求时，服务器生成一个CSRF Token（字符串）发送给浏览器，然后将该字符串放入页面中
- 浏览器请求时（如表单提交）需要带上这个CSRF Token。服务器收到请求后，验证CSRF是否合法，如果不合法拒绝即可。





# **1. xss**

XSS，即 Cross Site Script，中译是跨站脚本攻击；其原本缩写是 CSS，但为了和层叠样式表(Cascading Style Sheet)有所区分，因而在安全领域叫做 XSS。

XSS 攻击是指攻击者在网站上注入恶意的客户端代码，通过恶意脚本对客户端网页进行篡改，从而在用户浏览网页时，对用户浏览器进行控制或者获取用户隐私数据的一种攻击方式。

攻击者对客户端网页注入的恶意脚本一般包括 JavaScript，有时也会包含 HTML 和 Flash。有很多种方式进行 XSS 攻击，但它们的共同点为：将一些隐私数据像 cookie、session 发送给攻击者，将受害者重定向到一个由攻击者控制的网站，在受害者的机器上进行一些恶意操作。

XSS攻击可以分为3类：反射型（非持久型）、存储型（持久型）、基于DOM。

**1.1反射性**

反射型 XSS 只是简单地把用户输入的数据 “反射” 给浏览器，这种攻击方式往往需要攻击者诱使用户点击一个恶意链接，或者提交一个表单，或者进入一个恶意网站时，注入脚本进入被攻击者的网站。

看一个示例。我先准备一个如下的静态页：

![img](https://img-blog.csdn.net/20180817095431429?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjMxMzIy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

恶意链接的地址指向了 `localhost:8001/?q=111&p=222`。然后，我再启一个简单的 Node 服务处理恶意链接的请求：

```javascript
const http = require('http');
function handleReequest(req, res) {
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.writeHead(200, {'Content-Type': 'text/html; charset=UTF-8'});
    res.write('<script>alert("反射型 XSS 攻击")</script>');
    res.end();
}
 
const server = new http.Server();
server.listen(8001, '127.0.0.1');
server.on('request', handleReequest);
```

当用户点击恶意链接时，页面跳转到攻击者预先准备的页面，会发现在攻击者的页面执行了 js 脚本：

<img src="https://img-blog.csdn.net/20180817095525589?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjMxMzIy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" alt="img" style="zoom:150%;" />

这样就产生了反射型 XSS 攻击。攻击者可以注入任意的恶意脚本进行攻击，可能注入恶作剧脚本，或者注入能获取用户隐私数据(如`cookie`)的脚本，这取决于攻击者的目的。



**1.2存储型**

存储型 XSS 会把用户输入的数据 “存储” 在服务器端，当浏览器请求数据时，脚本从服务器上传回并执行。这种 XSS 攻击具有很强的稳定性。

比较常见的一个场景是攻击者在社区或论坛上写下一篇包含恶意 JavaScript 代码的文章或评论，文章或评论发表后，所有访问该文章或评论的用户，都会在他们的浏览器中执行这段恶意的 JavaScript 代码。

举一个示例。

先准备一个输入页面：

```html
<input type="text" id="input">
<button id="btn">Submit</button>  
<script>
    const input = document.getElementById('input');
    const btn = document.getElementById('btn');

    let val;
    input.addEventListener('change', (e) => {
        val = e.target.value;
    },false);
    btn.addEventListener('click', (e) => {
        fetch('http://localhost:8001/save', {
            method: 'POST',
            body: val
        });
    }, false);
</script>
```

启动一个 Node 服务监听 save 请求。为了简化，用一个变量来保存用户的输入：

```javascript
const http = require('http');
let userInput = '';

function handleReequest(req, res) {
    const method = req.method;
    res.setHeader('Access-Control-Allow-Origin', '*');
    res.setHeader('Access-Control-Allow-Headers', 'Content-Type')
    if (method === 'POST' && req.url === '/save'){
        let body = '';
        req.on('data',chunk => {
            body += chunk;
        });
        req.on('end', () => {
            if (body) {
                userInput = body;
            }
            res.end();
        });
    } else {
        res.writeHead(200, {'Content-Type': 'text/html; charset=UTF-8'});
        res.write(userInput);  //将输入的脚本内容返回给前端页面
        res.end();
    }
}

const server = new http.Server();
server.listen(8001, '127.0.0.1');
server.on('request', handleReequest);
```

当用户点击提交按钮将输入信息提交到服务端时，服务端通过 userInput 变量保存了输入内容。当用户通过 `http://localhost:8001/${id}` 访问时，服务端会返回与 id 对应的内容(本示例简化了处理)。如果用户输入了恶意脚本内容，则其他用户访问该内容时，恶意脚本就会在浏览器端执行：

<img src="https://img-blog.csdn.net/20180817095634576?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjMxMzIy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70" alt="img" style="zoom:150%;" />



**1.3 基于DOM**

基于 DOM 的 XSS 攻击是指通过恶意脚本修改页面的 DOM 结构，是纯粹发生在客户端的攻击。

看如下代码：

```html
<h2>XSS: </h2>
<input type="text" id="input">
<button id="btn">Submit</button>
<div id="div"></div>
<script>
    const input = document.getElementById('input');
    const btn = document.getElementById('btn');
    const div = document.getElementById('div');
    let val;
    input.addEventListener('change', (e) => {
        val = e.target.value;
    }, false);
    btn.addEventListener('click', () => {
        div.innerHTML = `<a href=${val}>testLink</a>`
    }, false);
</script>
```

点击 Submit 按钮后，会在当前页面插入一个链接，其地址为用户的输入内容。如果用户在输入时构造了如下内容：

```
'' onclick=alert(/xss/)
```

用户提交之后，页面代码就变成了：

```html
<a href onlick="alert(/xss/)">testLink</a>
```

此时，用户点击生成的链接，就会执行对应的脚本：

![img](https://img-blog.csdn.net/20180817095905282?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjMxMzIy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

### **2. XSS攻击的防范**

现在主流的浏览器内置了防范 XSS 的措施，例如 CSP。但对于开发者来说，也应该寻找可靠的解决方案来防止 XSS 攻击。



2.1 **HttpOnly 防止劫取 Cookie**

HttpOnly 最早由微软提出，至今已经成为一个标准。浏览器将禁止页面的Javascript 访问带有 HttpOnly 属性的Cookie。

上文有说到，攻击者可以通过注入恶意脚本获取用户的 Cookie 信息。通常 Cookie 中都包含了用户的登录凭证信息，攻击者在获取到 Cookie 之后，则可以发起 Cookie 劫持攻击。所以，严格来说，HttpOnly 并非阻止 XSS 攻击，而是能阻止 XSS 攻击后的 Cookie 劫持攻击。



**2.2 输入检查**

不要相信用户的任何输入。 对于用户的任何输入要进行检查、过滤和转义。建立可信任的字符和 HTML 标签白名单，对于不在白名单之列的字符或者标签进行过滤或编码。

在 XSS 防御中，输入检查一般是检查用户输入的数据中是否包含 <，> 等特殊字符，如果存在，则对特殊字符进行过滤或编码，这种方式也称为 XSS Filter。

而在一些前端框架中，都会有一份 decodingMap， 用于对用户输入所包含的特殊字符或标签进行编码或过滤，如 <，>，script，防止 XSS 攻击：

```js
// vuejs 中的 decodingMap
// 在 vuejs 中，如果输入带 script 标签的内容，会直接过滤掉
const decodingMap = {
    '&lt;': '<',
    '&gt;': '>',
    '&quot;': '"',
    '&amp;': '&',
    '
    ': '\n'
}
```



**2.3输出检查**

用户的输入会存在问题，服务端的输出也会存在问题。一般来说，除富文本的输出外，在变量输出到 HTML 页面时，可以使用编码或转义的方式来防御 XSS 攻击。例如利用 sanitize-html 对输出内容进行有规则的过滤之后再输出到页面中。



### 3.CSRF

CSRF，即 Cross Site Request Forgery，中译是跨站请求伪造，是一种劫持受信任用户向服务器发送非预期请求的攻击方式。

通常情况下，CSRF 攻击是攻击者借助受害者的 Cookie 骗取服务器的信任，可以在受害者毫不知情的情况下以受害者名义伪造请求发送给受攻击服务器，从而在并未授权的情况下执行在权限保护之下的操作。

在举例子之前，先说说浏览器的 Cookie 策略

**3.1 浏览器的 Cookie 策略**

Cookie 是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。Cookie 主要用于以下三个方面：

- 会话状态管理（如用户登录状态、购物车、游戏分数或其它需要记录的信息）
- 个性化设置（如用户自定义设置、主题等）

而浏览器所持有的 Cookie 分为两种：

- Session Cookie(会话期 Cookie)：会话期 Cookie 是最简单的Cookie，它不需要指定过期时间（Expires）或者有效期（Max-Age），它仅在会话期内有效，浏览器关闭之后它会被自动删除。
- Permanent Cookie(持久性 Cookie)：与会话期 Cookie 不同的是，持久性 Cookie 可以指定一个特定的过期时间（Expires）或有效期（Max-Age）。

```js
res.setHeader('Set-Cookie', ['mycookie=222', 'test=3333; expires=Sat, 21 Jul 2018 00:00:00 GMT;']);
```

上述代码创建了两个 Cookie:mycookie 和 test，前者属于会话期 Cookie，后者则属于持久性 Cookie。当我们去查看 Cookie 相关的属性时，不同的浏览器对会话期 Cookie 的 Expires 属性值会不一样：

![img](https://img-blog.csdn.net/20180817100018735?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjMxMzIy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

此外，每个 Cookie 都会有与之关联的域，这个域的范围一般通过 domain 属性指定。如果 Cookie 的域和页面的域相同，那么我们称这个 Cookie 为第一方 Cookie（first-party cookie），如果 Cookie 的域和页面的域不同，则称之为第三方 Cookie（third-party cookie）。一个页面包含图片或存放在其他域上的资源（如图片）时，第一方的 Cookie 也只会发送给设置它们的服务器。



**3.2 通过 Cookie 进行 CSRF 攻击**

假设有一个 bbs 站点：`http://www.c.com`，当登录后的用户发起如下 GET 请求时，会删除 ID 指定的帖子：

`http://www.c.com:8002/content/delete/:id`

如发起 `http://www.c.com:8002/content/delete/87343` 请求时，会删除 id 为 87343 的帖子。当用户登录之后，会设置如下 cookie：

```js
res.setHeader('Set-Cookie', ['user=22333; expires=Sat, 21 Jul 2018 00:00:00 GMT;']);
```

![img](https://img-blog.csdn.net/20180817100134753?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjMxMzIy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

user 对应的值是用户 ID。然后构造一个页面 A：

CSRF 攻击者准备的网站：

```html
<p>CSRF 攻击者准备的网站：</p>
<img src="http://www.c.com:8002/content/delete/87343">
```

页面 A 使用了一个 img 标签，其地址指向了删除用户帖子的链接：

![img](https://img-blog.csdn.net/20180817100336155?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L20wXzM3NjMxMzIy/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

可以看到，当登录用户访问攻击者的网站时，会向 `www.c.com` 发起一个删除用户帖子的请求。此时若用户在切换到 `www.c.com` 的帖子页面刷新，会发现ID 为 87343 的帖子已经被删除。

由于 Cookie 中包含了用户的认证信息，当用户访问攻击者准备的攻击环境时，攻击者就可以对服务器发起 CSRF 攻击。在这个攻击过程中，攻击者借助受害者的 Cookie 骗取服务器的信任，但并不能拿到 Cookie，也看不到 Cookie 的内容。而对于服务器返回的结果，由于浏览器同源策略的限制，攻击者也无法进行解析。因此，攻击者无法从返回的结果中得到任何东西，**他所能做的就是给服务器发送请求**，**以执行请求中所描述的命令**，**在服务器端直接改变数据的值**，**而非窃取服务器中的数据**。

但若 CSRF 攻击的目标并不需要使用 Cookie，则也不必顾虑浏览器的 Cookie 策略了。

### **4. CSRF 攻击的防范**

当前，对 CSRF 攻击的防范措施主要有如下几种方式。

**4.1 验证码**

验证码被认为是对抗 CSRF 攻击最简洁而有效的防御方法。

从上述示例中可以看出，CSRF 攻击往往是在用户不知情的情况下构造了网络请求。而验证码会**强制用户必须与应用进行交互**，才能完成最终请求。因为通常情况下，验证码能够很好地遏制 CSRF 攻击。

但验证码并不是万能的，因为出于用户考虑，不能给网站所有的操作都加上验证码。因此，验证码只能作为防御 CSRF 的一种辅助手段，而不能作为最主要的解决方案。

**4.2 Referer Check**

根据 HTTP 协议，在 HTTP 头中有一个字段叫 Referer，它记录了该 HTTP 请求的来源地址。通过 Referer Check，可以检查请求是否来自合法的”源”。

比如，如果用户要删除自己的帖子，那么先要登录 `www.c.com`，然后找到对应的页面，发起删除帖子的请求。此时，Referer 的值是 `http://www.c.com`；当请求是从 `www.a.com` 发起时，Referer 的值是 `http://www.a.com` 了。因此，要防御 CSRF 攻击，只需要对于每一个删帖请求验证其 Referer 值，如果是以 `www.c.com` 开头的域名，则说明该请求是来自网站自己的请求，是合法的。如果 Referer 是其他网站的话，则有可能是 CSRF 攻击，可以拒绝该请求。

针对上文的例子，可以在服务端增加如下代码：

```js
if (req.headers.referer !== 'http://www.c.com:8002/') {
    res.write('csrf 攻击');
    return;
}
```

Referer Check 不仅能防范 CSRF 攻击，另一个应用场景是 “防止图片盗链”。

**4.3 添加 token 验证(token==令牌)**

CSRF 攻击之所以能够成功，是因为攻击者可以完全伪造用户的请求，该请求中所有的用户验证信息都是存在于 Cookie 中，因此攻击者可以在不知道这些验证信息的情况下直接利用用户自己的 Cookie 来通过安全验证。要抵御 CSRF，**关键在于在请求中放入攻击者所不能伪造的信息**，并且该信息**不存在于 Cookie 之中**。可以在 HTTP 请求中**以参数的形式加入一个随机产生的 token**，并在服务器端建立一个拦截器来验证这个 token，如果请求中没有 token 或者 token 内容不正确，则认为可能是 CSRF 攻击而拒绝该请求。



#### 1.防御XSS攻击

- HttpOnly 防止劫取 Cookie
- 用户的输入检查
- 服务端的输出检查

#### 2.防御CSRF攻击

-  验证码
- Referer Check
- Token 验证





