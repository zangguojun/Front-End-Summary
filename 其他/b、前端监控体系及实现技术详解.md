# 前端监控体系及实现技术详解

## 1.前端监控体系概况

一个完整的前端监控体系包括了采集、日志存储、日志切分&计算、数据分析、告警等流程，对于一名前端开发工程师来说，也就意味着工作不再局限于前端业务的开发工作，需要有Nginx服务运维能力、实时/离线分析能力、Node应用开发运维能力等等。



## 2.采集部分

### 2.1稳定性

### 2.1.1脚本错误

脚本错误主要有两类：语法错误、运行时错误。监控的方式主要有：

```js
window.onerror
try-catch
window.addEventListener('error'，()=>{})
window.addEventListener('unhandledrejection'()=>{})
```

在使用过程中的体会：onerror 主要用来捕获预料之外的错误，**而 try-catch 则可以用在预知情况下监控特定错误，两种形式结合使用更加高效**。

使用window.onerror捕获JS运行时错误

使用window.addEventListener('unhandledrejection')捕获未处理的promise reject错误

使用window.addEventListener('error')捕获资源加载错误。但它也能捕获js运行时错误，为避免重复上报js运行时错误，此时只有event.srcElement inatanceof HTMLScriptElement或HTMLLinkElement或HTMLImageElement时才进行数据采集。

另外，当try-catch中代码发生语法错误或异步错误时，则无法正常捕捉。

示例 · try-catch (语法报错)

```js
try {
    function empty()   // <-  throw error 语法错误
} catch(e){
    console.log('语法错误信息 ↙');
    console.log(e);
}
```

```js
try {
    setTimeout(function() {
        test // <- throw error 异步错误
    },0)
} catch(e){
    console.log('异步错误信息 ↙');
    console.log(e);
}
```

无法捕捉错误(直接在console中抛出错误)

### 2.1.1.1解决跨域资源Script error

还有一点要特别注意，要注意跨域资源的脚本的报错



 











