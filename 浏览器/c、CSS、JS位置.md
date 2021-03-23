## 为什么要强调CSS要放在header里，js放在尾部？

构建Render树需要DOM和CSSOM，所以**HTML和CSS都会阻塞渲染**。所以需要让CSS**尽早加载**（如：放在头部），以**缩短首次渲染的时间**。除此之外，由于CSS不会阻塞文档的解析，但是**会阻塞文档渲染**。把CSS放在头部可以**先生成CSSOM树**，**后续渲染DOM**的时候，可以**一次性构建Render树**，只需要渲染一次；如果把CSS放在后面，**会先解析一次DOM**，**加载CSS之后**，**会重新渲染之前的DOM**，**需要两次渲染**。

阻塞浏览器的解析，也就是说发现一个外链脚本时，**需等待脚本下载完成并执行后才会继续解析HTML**。

普通的脚本会阻塞浏览器解析，**加上defer或async属性，脚本就变成异步，可等到解析完毕再执行**

- async异步执行，异步下载完毕后就会执行，不确保执行顺序，一定在onload前，但不确定在DOMContentLoaded事件的前后
- defer延迟执行，相当于放在body最后（理论上在DOMContentLoaded事件前）
- 执行js代码过长会卡住渲染，对于需要很多时间计算的代码可以考虑用Web Worker,它可以让我们另开一个线程执行脚本，不影响渲染

### Onload&DOMContentLoaded&domready

- DOMContentLoaded 事件触发时，仅当**DOM加载完成**，不包括样式表，图片...
- onload 事件触发时，页面上**所有的DOM，样式表，脚本，图片**都已加载完成
- domready事件在DOM加载后、资源加载之前被触发，在本地浏览器的DOMContentLoaded事件的形式被调用。



















