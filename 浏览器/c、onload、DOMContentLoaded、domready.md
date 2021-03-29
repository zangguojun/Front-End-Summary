## 1、DOMContentLoaded

+ DOMContentLoaded事件是当初始HTML文档完全被加载和解析（即所有的DOM完全解析）时触发的，无需要等待样式表，图片，子框架完成加载。

## 2、onload

+ 而onload事件要等页面所有元素，包括图片以及脚本等全部加载完成才触发，因此它比DOMContentLoaded要更晚执行。


> **在页面的图片很多，网络不好的情况下，**从用户访问到onload触发可能需要很长的时间，此时如果在onload中加入许多初始化的动作, 必然会影响用户的体验。**这时候使用DOMContentLoaded事件代替onload事件是更合适的。**