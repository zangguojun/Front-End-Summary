> cookie 验证准确的说是利用 cookie 来作为媒介，存储 session ID 进行验证，token 其实也可以借助 cookie 来存储，不过下文中提到的 cookie 验证主要是指 session ID 存储到 cookie 中进行的验证。token 验证主要是指 token 存储在 Authorization Header 中进行的验证。

## Cookie

cookie 验证是用于长时间用户验证，cookie 验证是**有状态的**，意味着验证记录或者会话需要一直在服务端和客户端保持。服务器需要保持对数据库活动**会话的追踪**，当在前端创建了一个 cookie，cookie 中包含了一个 **session 标识符**。传统 cookie 会话的验证流程：

- 用户登录，输入账号密码
- 服务器验证用户账号密码正确，创建一个 **session** 存储在**数据库（或者 redis）**
- 将 session ID 放进 cookie 中，被存储在用户浏览器中。
- 再次发起请求，服务器直接通过 session ID 对用户进行验证
- 一旦用户登出，则 **session** 在客户端和服务器端都被销毁



## Token

token 验证是**无状态的**，服务器不记录哪些用户登录了或者哪些 JWT 被发布了，而是每个请求都带上了服务器需要验证的 token，token 放在了 **Authorization header** 中，形式是 Bearer { JWT }，但是也可以在 **post body** 里发送，甚至作为 **query parameter**。
验证流程：

- 用户输入登录信息
- 服务器判断登录信息正确，返回一个 token
- **token 存储在客户端**，大多数通常在 **local storage**，但是也可以存储在 **session storage** 或者 **cookie** 中。
- 接着发起请求的时候将 token 放进 **Authorization header**，或者同样可以通过上面的方式。
- 服务器端**解码 JWT** 然后验证 token，如果 token 有效，则处理该请求。
- 一旦用户登出，token 在客户端被销毁，不需要经过服务器端。



## Token 验证的优势

- 无状态，可扩展和解耦
  使用 token 而不是 cookie 的最大优点应该就是无状态，后端不需要保持对 token 的记录，每个 token 都是独立的，包含了检查其有效性的所有数据，并通过申明传达了用户信息。
  服务器端的工作只需要在登录成功后，生成（或者 sign，签署） token，或者验证传入的 token 是否有效。有时候甚至不需要生成 token，第三方服务比如 Auth0 可以处理 token 的签发，服务器只需要验证 token 的有效性就可以。
- 跨域和 CORS
  cookie 能很好的处理单域和子域，但是遇到跨域的问题就会变得难以处理。而使用 token 的 CORS 可以很好的处理跨域的问题。由于每次发送请求到后端，都需要检查 JWT，只要它们被验证通过就可以处理请求。
- 在 JWT 中存储数据
  当使用 cookie 进行验证时，你是将 session id 存储到 cookie 里，JWT 允许你存储任何类型的元数据，只要是合法的 JSON。你可以在里面添加任何数据，可以只有用户 ID 和到期日，也可以添加其它的比如邮件地址，域名等等。
  比如：加入你有一个 API 是 /api/orders ，用于取回最新的订单，但是只有 admin 角色的用户可以获取到这些数据。在基于 cookie 的验证中，一旦请求被创建，就需要先去访问数据库去验证 session 是否正确（现在应该都是存储到 redis 里了，不会存数据库里了），另外还要去获取数据库里的用户权限去校验用户是否拥有 admin 的权限（这个应该是根据用户 role_id 查看权限是否是 admin），最后才是调用订单信息。而使用 JWT 的话，可以将用户角色放进 JWT 内，所以只要验证通过了，就可以直接调用订单信息。
- 移动平台
  现代的 API 不仅仅和浏览器交互，正确编写一个 API 可以同时支持浏览器，还有原生移动平台，比如 IOS 或者 Android。原生移动平台并不一定和 cookie 能良好的兼容，在使用中会存在一些限制和需要注意的地方。另一方面，token 更容易在 IOS 和 Android 上实现，Token 也更容易实现物联网应用程序和服务，没有 Cookie 存储的概念。



**共同的问题**
这里介绍一些常见的问题，或者在令牌 token 的情况下出现时经常出现。这里的关键重点将是安全性，但我们将介绍有关 token 大小，存储和加密的用例。

- JWT 大小
  token 最大的缺点就是它的大小，最小的它都比 cookie 要大，如果 token 中包含很多声明，那问题就会变得比较严重，毕竟向服务器发送的每个请求都要有这个 token。（意思应该是太大了会导致请求缓慢）
- 哪里存储 token
  通常 JWT 被存储在浏览器的 local storage 中并且能够很好的运用，但是这样存储也会有问题，不像 cookie，local storage 被沙盒化到特定域，其区域不能被任何其他域访问，包括子域。
  你可以存储 token 在 cookie 中，但是 cookie 最大的大小也只有 4kb，所以如果你有许多声明的时候可能会存储不够，session storage 就更不用说了，会话断开就被清除掉了。
  （个人记录：由于JWT前两个字符串采用base64进行编码，所以内容越多，编码字符串长度越长）
- XSS 和 XSRF 防护
  保护用户信息安全和服务器数据始终是首要任务。最常见的网络攻击就是 XSS 和 CSRF。
  如果用户输入或提交的信息没有得到过滤的话，如果一些攻击代码可以在你访问的域名下执行，那你的 JWT token 会被泄露。与 CSRF 相比，XSS 会更容易处理（因为 XSS 很容易理解）。许多框架包括 Angular 等等，都会自动过滤掉 input 输入的某些内容且防止任意代码执行。如果框架本身不自带这种过滤机制，可以采用一些插件比如 caja。这种过滤 input 输入框是大部分框架和语言解决 XSS 问题的一个方式。
  如果你通过 local storage 使用 JWT，那么可以避免 CSRF，但是另一方面，如果你用 cookie 来存储 JWT，那就需要防护 CSRF，CSRF 并不像 XSS 攻击那么好理解。解释 CSRF 攻击可能会非常耗时。为了避免过度简化，防止 CSRF 攻击，你的服务器会和客户端建立会话后，会生成一个唯一的 token（这不是 JWT）。然后随时将数据提交到你的服务器，隐藏的 input 将会包含这个 token，服务器将会检查这个 token 以确保 token 匹配。不够我们是建议将 JWT 存储在 local storage 中，你也不太需要担心 CSRF 攻击的问题。
  其中一个最好的保护用户和服务器的方法就是有一个给 token 一个短期过期时间，这样即使 token 被其他人获取，但是也会很快不能再用。此外，你可以维护一个受攻击的 token 黑名单，不允许这些黑名单的 token 访问系统。最后，还可以统一更改 token 算法，让所有 token 都失效，并让用户重新登录系统，不过这种方法一般不推荐，只有在被攻击比较严重的情况下使用。
- token Are Signed,Not Encrypted
  一个 JWT 是由三部分构成：header,payload,signature。
  header: 算法和 token 类型，如
  `{ "alg": "HS256", "typ": "JWT"}`
  payload: 数据，如
  `{ "sub": "12", "name": "Ado Kukic", "admin": true}`
  验证签名

```JS
HMACSHA256(
    base64UrlEncode(header)+"." +
    base64UrlEncode(payload),
    secret
)
```

JWT 的格式为 `header.payload.signature`
，如果我们需要通过 HMACSHA256 算法签发一个 JWT ，如果 secret 是 'shhhh' （secret是`盐`）以及 payload 是
`{ "sub": "1234567890", "name": "Ado Kukic", "admin": true}`
则 JWT 为
`eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkFkbyBLdWtpYyIsImFkbWluIjp0cnVlLCJpYXQiOjE0NjQyOTc4ODV9.Y47kJvnHzU9qeJIN48_bVna6O0EDFiMiQ9LpNVDFymM`

这里有一个**非常重要的提醒**，token 是通过 HMACSHA256 算法生成，header 和 payload 是 `Base64`URL 编码，这**不是加密**。如果你去访问 [jwt.io](https://link.jianshu.com/?t=https://jwt.io/) 网站，粘贴 token 以及选择 HMACSHA256 算法，可以解析出 token 读取它详细的内容。因此里面不能携带一些敏感数据，比如密码，绝对不能存储在 payload 中。
如果你必须存储敏感信息在 payload 中，或者使 JWT 被隐藏，你可以使用 JWE（JSON Web Encryption）。JWE 允许你去加密 JWT 的内容，让其除了服务器以外的任何人都不可读。JOSE 为 JWE 提供了一个很好的框架，并为许多流行框架（包括 NodeJS 和 Java）提供了 SDK。

























