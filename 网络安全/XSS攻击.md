## XSS 攻击的介绍

### XSS 漏洞的发生和修复

**XSS 攻击是页面被注入了恶意的代码**

#### 一个案例

某天，公司需要一个搜索页面，根据 URL 参数决定关键词的内容。小明很快把页面写好并且上线。代码如下：

```php+HTML
<input type="text" value="<%= getParameter("keyword") %>">
<button>搜索</button>
<div>
  您搜索的关键词是：<%= getParameter("keyword") %>
</div>
```

然而，在上线后不久，小明就接到了安全组发来的一个神秘链接：

```html
http://xxx/search?keyword="><script>alert('XSS');</script>
```

小明带着一种不祥的预感点开了这个链接。果然，页面中弹出了写着"XSS"的对话框。

当浏览器请求 `http://xxx/search?keyword="><script>alert('XSS');</script>` 时，服务端会解析出请求参数 `keyword`，得到 `"><script>alert('XSS');</script>`，拼接到 HTML 中返回给浏览器。

形成了如下的 HTML：

```php+HTML
<input type="text" value=""><script>alert('XSS');</script>">
<button>搜索</button>
<div>
  您搜索的关键词是："><script>alert('XSS');</script>
</div>
```

浏览器无法分辨出 `<script>alert('XSS');</script>` 是恶意代码，因而将其执行。

这里不仅仅 div 的内容被注入了，而且 input 的 value 属性也被注入， alert 会弹出两次。

面对这种情况，我们应该如何进行防范呢？

其实，这只是**浏览器把用户的输入当成了脚本进行了执行**。那么只要告诉浏览器这段内容是文本就可以了。

聪明的小明很快找到解决方法，把这个漏洞修复：

```php+HTML
<input type="text" value="<%= escapeHTML(getParameter("keyword")) %>">
<button>搜索</button>
<div>
  您搜索的关键词是：<%= escapeHTML(getParameter("keyword")) %>
</div>
```

`escapeHTML()` 按照如下规则进行转义：

![image-20210408152901834](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210408152902.png)

经过了转义函数的处理后，最终浏览器接收到的响应为：

```php+HTML
<input type="text" value="&quot;&gt;&lt;script&gt;alert(&#x27;XSS&#x27;);&lt;&#x2F;script&gt;">
<button>搜索</button>
<div>
  您搜索的关键词是：&quot;&gt;&lt;script&gt;alert(&#x27;XSS&#x27;);&lt;&#x2F;script&gt;
</div>
```

恶意代码都被转义，不再被浏览器执行，而且搜索词能够完美的在页面显示出来。

通过这个事件，小明学习到了如下知识：

- 通常页面中包含的用户输入内容都在固定的容器或者属性内，以文本的形式展示。
- 攻击者利用这些页面的用户输入片段，拼接特殊格式的字符串，突破原有位置的限制，形成了代码片段。
- 攻击者通过在目标网站上注入脚本，使之在用户的浏览器上运行，从而引发潜在风险。
- **通过 HTML 转义，可以防止 XSS 攻击。**

#### 注意特殊的 HTML 属性、JavaScript API

自从上次事件之后，小明会小心的把插入到页面中的数据进行转义。而且他还发现了大部分模板都带有的转义配置，让所有插入到页面中的数据都默认进行转义。这样就不怕不小心漏掉未转义的变量啦，于是小明的工作又渐渐变得轻松起来。

但是，作为导演的我，不可能让小明这么简单、开心地改 Bug 。

不久，小明又收到安全组的神秘链接：`http://xxx/?redirect_to=javascript:alert('XSS')`。小明不敢大意，赶忙点开页面。然而，页面并没有自动弹出万恶的“XSS”。

小明打开对应页面的源码，发现有以下内容：

```php+HTML
<a href="<%= escapeHTML(getParameter("redirect_to")) %>">跳转...</a>
```

这段代码，当攻击 URL 为 `http://xxx/?redirect_to=javascript:alert('XSS')`，服务端响应就成了：

```php+HTML
<a href="javascript:alert(&#x27;XSS&#x27;)">跳转...</a>
```

虽然代码不会立即执行，但一旦用户点击 `a` 标签时，浏览器会就会弹出“XSS”。

在这里，用户的数据并没有在位置上突破我们的限制，仍然是正确的 href 属性。但其内容并不是我们所预期的类型。

原来不仅仅是特殊字符，连 `javascript:` 这样的字符串如果出现在特定的位置也会引发 XSS 攻击。

小明眉头一皱，想到了解决办法：

```php+HTML
// 禁止 URL 以 "javascript:" 开头
xss = getParameter("redirect_to").startsWith('javascript:');
if (!xss) {
  <a href="<%= escapeHTML(getParameter("redirect_to"))%>">
    跳转...
  </a>
} else {
  <a href="/404">
    跳转...
  </a>
}
```

只要 URL 的开头不是 `javascript:`，就安全了吧？

安全组随手又扔了一个连接：`http://xxx/?redirect_to=jAvascRipt:alert('XSS')`

小明欲哭无泪，在判断 URL 开头是否为 `javascript:` 之前，先把用户输入转成了小写，然后再进行比对。

不过，所谓“道高一尺，魔高一丈”。面对小明的防护策略，安全组就构造了这样一个连接：

```http
http://xxx/?redirect_to=%20javascript:alert('XSS')
```

`%20javascript:alert('XSS')` 经过 URL 解析后变成 `javascript:alert('XSS')`，这个字符串以空格开头。这样攻击者可以绕过后端的关键词规则，又成功的完成了注入。

最终，小明选择了白名单的方法，彻底解决了这个漏洞：

```php+HTML
// 根据项目情况进行过滤，禁止掉 "javascript:" 链接、非法 scheme 等
allowSchemes = ["http", "https"];
valid = isValid(getParameter("redirect_to"), allowSchemes);
if (valid) {
  <a href="<%= escapeHTML(getParameter("redirect_to"))%>">
    跳转...
  </a>
} else {
  <a href="/404">
    跳转...
  </a>
}
```

通过这个事件，小明学习到了如下知识：

- **做了 HTML 转义，并不等于高枕无忧。**
- 对于链接跳转，如 `<a href="xxx"` 或 `location.href="xxx"`，要检验其内容，禁止以 `javascript:` 开头的链接，**和其他非法的 scheme**。

#### 根据上下文采用不同的转义规则

某天，小明为了加快网页的加载速度，把一个数据通过 JSON 的方式内联到 HTML 中：

```php+HTML
<script>
var initData = <%= data.toJSON() %>
</script>
```

插入 JSON 的地方不能使用 `escapeHTML()`，因为转义 `"` 后，JSON 格式会被破坏。

但安全组又发现有漏洞，原来这样内联 JSON 也是不安全的：

- 当 JSON 中包含 `U+2028` 或 `U+2029` 这两个字符时，不能作为 JavaScript 的字面量使用，否则会抛出语法错误。
- 当 JSON 中包含字符串 `</script>` 时，**当前的 script 标签将会被闭合**，**后面的字符串内容浏览器会按照 HTML 进行解析**；通过增加下一个 `<script>` 标签等方法就可以完成注入。

于是我们又要实现一个 `escapeEmbedJSON()` 函数，对内联 JSON 进行转义。

转义规则如下：

![image-20210408153913390](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210408153914.png)

修复后的代码如下：

```php+HTML
<script>
var initData = <%= escapeEmbedJSON(data.toJSON()) %>
```

通过这个事件，小明学习到了如下知识：

- HTML 转义是非常复杂的，**在不同的情况下要采用不同的转义规则**。**如果采用了错误的转义规则，很有可能会埋下 XSS 隐患**。
- **应当尽量避免自己写转义库，而应当采用成熟的、业界通用的转义库**。

### 漏洞总结

小明的例子讲完了，下面我们来系统的看下 XSS 有哪些注入的方法：

##### html标签注入

主要入口为表单项、内联样式表

- 在 **HTML 中内嵌的文本**中，恶意内容以 **script 标签形成注入**。
- 在**内联的 JavaScript** 中，**拼接的数据突破了原本的限制**（字符串，变量，方法名等）。
- 在**标签属性**中，**恶意内容包含引号**，从而突破属性值的限制，**注入其他属性或者标签**。
- 在**标签的 href、src 等属性**中，包含 `javascript:` 等**可执行代码**。
- 在 `onload、onerror、onclick` 等事件中，**注入不受控制代码**。
- 在 style 属性和标签中，包含类似 `background-image:url("javascript:...");` 的代码（新版本浏览器已经可以防范）。
- 在 style 属性和标签中，包含类似 `expression(...)` 的 **CSS 表达式代码**（新版本浏览器已经可以防范）。

总之，如果开发者没有将**用户输入的文本**进行合适的**过滤**，就**贸然插入到 HTML** 中，这很容易造成注入漏洞。**攻击者可以利用漏洞，构造出恶意的代码指令，进而利用恶意代码危害数据安全。**

## XSS 攻击的分类

Cross-Site Scripting（**跨站脚本攻击**）简称 XSS，是一种**代码注入攻击**。攻击者通过在目标网站上**注入恶意脚本**，使之在用户的浏览器上运行。利**用这些恶意脚本**，**攻击者可获取用户的敏感信息如 Cookie、SessionID** 等，进而危害数据安全。

恶意代码未经过滤，**与网站正常的代码混在一起**；浏览器无法分辨哪些脚本是可信的，导致恶意脚本被执行。

而由于直接在用户的终端执行，恶意代码能够直接获取用户的信息，或者利用这些信息冒充用户向网站发起攻击者定义的请求。

在部分情况下，由于输入的限制，注入的恶意脚本比较短。但可以通过引入外部的脚本，并由浏览器执行，来完成比较复杂的攻击策略。

这里有一个问题：用户是通过哪种方法“注入”恶意脚本的呢？

不仅仅是业务上的“用户的 UGC 内容”可以进行注入，包括 URL 上的参数等都可以是攻击的来源。在处理输入时，以下内容都不可信：

- 来自用户的 UGC 信息
- 来自第三方的链接
- URL 参数
- POST 参数
- Referer （可能来自不可信的来源）
- Cookie （可能来自其他子域注入）

### XSS 分类

根据攻击的来源，XSS 攻击可分为存储型、反射型和 DOM 型三种。

#### 存储型 XSS

存储型 XSS 的攻击步骤：

1. 攻击者将恶意代码**提交到目标网站的数据库**中。
2. 用户打开目标网站时，**网站服务端将恶意代码从数据库取出**，**拼接在 HTML 中返回给浏览器**。
3. 用户浏览器接收到响应后解析执行，**混在其中的恶意代码也被执行**。
4. **恶意代码窃取用户数据并发送到攻击者的网站**，**或者冒充用户的行为**，**调用目标网站接口执行攻击者指定的操作**。

这种攻击常见于带有用户保存数据的网站功能，如论坛发帖、商品评论、用户私信等。



#### 反射型 XSS

反射型 XSS 的攻击步骤：

1. **攻击者构造出特殊的 URL，其中包含恶意代码。**
2. **用户打开带有恶意代码的 URL 时**，**网站服务端将恶意代码从 URL 中取出，拼接在 HTML 中返回给浏览器**。
3. **用户浏览器接收到响应后解析执行，混在其中的恶意代码也被执行。**
4. **恶意代码窃取用户数据并发送到攻击者的网站**，或者冒充用户的行为，**调用目标网站接口执行攻击者指定的操作**。

反射型 XSS 跟存储型 XSS 的区别是：**存储型 XSS 的恶意代码存在数据库里，反射型 XSS 的恶意代码存在 URL 里。**

**反射型 XSS** 漏洞常见于通过 **URL 传递参数**的功能，如**网站搜索、跳转**等。

**由于需要用户主动打开恶意的 URL 才能生效，攻击者往往会结合多种手段诱导用户点击。**

POST 的内容也可以触发反射型 XSS，只不过其触发条件比较苛刻（**需要构造表单提交页面，并引导用户点击**），所以非常少见。



#### DOM 型 XSS

DOM 型 XSS 的攻击步骤：

1. **攻击者构造出特殊的 URL，其中包含恶意代码。**
2. **用户打开带有恶意代码的 URL。**
3. 用户浏览器接收到响应后解析执行，**前端 JavaScript 取出 URL 中的恶意代码并执行**。
4. 恶意代码窃取用户数据并发送到攻击者的网站，或者冒充用户的行为，调用目标网站接口执行攻击者指定的操作。

DOM 型 XSS 跟前两种 XSS 的区别：DOM 型 XSS 攻击中，**取出和执行恶意代码由浏览器端完成**，属于前端 JavaScript 自身的安全漏洞，而其他两种 XSS 都属于服务端的安全漏洞。









