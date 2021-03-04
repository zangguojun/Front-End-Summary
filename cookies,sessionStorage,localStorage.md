## 三者的区别

`cookie`是网站为了标示用户身份而储存在用户本地终端`（Client Side）`上的数据（通常经过加密），数据会在浏览器和服务器间来回传递。

`sessionStorage`和`localStorage`不会自动把数据发给服务器，仅在本地保存。

**存储大小**：

- `cookie`数据大小不能超过`4k`。
- `sessionStorage`和`localStorage` 虽然也有存储大小的限制，但比`cookie`大得多，可以达到 `5M` 或更大。

**有期时间**：

- `localStorage` 存储持久数据，浏览器关闭后数据不丢失，除非主动删除数据
- `sessionStorage` 数据在当前浏览器窗口关闭后自动删除
- `cookie`      设置的cookie过期时间之前一直有效，即使窗口或浏览器关闭



## 如何实现浏览器内多个标签页之间的通信? 

调用`localstorge`、`cookies`等本地存储方式



## 页面访问cookie的限制条件

跨域问题，设置了`HttpOnly`。

