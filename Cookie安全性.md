当设置为true时，表示创建的 Cookie 会被以安全的形式向服务器传输，也就是只能在 HTTPS 连接中被浏览器传递到服务器端进行会话验证，如果是 HTTP 连接则不会传递该信息，所以不会被窃取到Cookie 的具体内容。

一、属性说明：
1 secure属性
当设置为true时，表示创建的 Cookie 会被以安全的形式向服务器传输，也就是只能在 HTTPS 连接中被浏览器传递到服务器端进行会话验证，如果是 HTTP 连接则不会传递该信息，所以不会被窃取到Cookie 的具体内容。
2 HttpOnly属性
如果在Cookie中设置了"HttpOnly"属性，那么通过程序(JS脚本、Applet等)将无法读取到Cookie信息，这样能有效的防止XSS攻击。

对于以上两个属性，
首先，secure属性是防止信息在传递的过程中被监听捕获后信息泄漏，HttpOnly属性的目的是防止程序获取cookie后进行攻击。
其次，GlassFish2.x支持的是servlet2.5，而servlet2.5不支持Session Cookie的"HttpOnly"属性。不过使用Filter做一定的处理可以简单的实现HttpOnly属性。GlashFish3.0(支持servlet3.0)默认开启Session Cookie的HttpOnly属性。
也就是说两个属性，并不能解决cookie在本机出现的信息泄漏的问题(FireFox的插件FireBug能直接看到cookie的相关信息)。也就是说两个属性，并不能解决cookie在本机出现的信息泄漏的问题(FireFox的插件FireBug能直接看到cookie的相关信息)。

Session cookies (或者包含JSSESSIONID的cookie)是指用来管理web应用的session会话的cookies.这些cookie中保存特定使用者的session ID标识，而且相同的session ID以及session生命周期内相关的数据也在服务器端保存。在web应用中最常用的session管理方式是通过每次请求的时候将cookies传送到服务器端来进行session识别。

应对方法很简单。你必须在session cookie添加secure标识（如果有可能的话最好保证请求中的所有cookies都是通过Https方式传输）

如下是示例：未添加secure标识的session cookie-可能会被泄露

`Cookie: jsessionid=AS348AF929FK219CKA9FK3B79870H;`

添加secure标识：

`Cookie: jsessionid=AS348AF929FK219CKA9FK3B79870H; secure;`















