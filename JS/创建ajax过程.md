(1)创建XMLHttpRequest对象,也就是创建一个异步调用对象. 

(2)创建一个新的HTTP请求,并指定该HTTP请求的方法、URL及验证信息. 

(3)设置响应HTTP请求状态变化的函数. 

(4)发送HTTP请求. 

(5)获取异步调用返回的数据. (6)使用JavaScript和DOM实现局部刷新.

```html
<script type="text/javascript">
    window.onload = function(){
        //第一步：创建xhr对象
        //xhr是一个对象；里面可以放很多东西，数据；
        var xhr = null;
        if(window.XMLHttpRequest){//标准浏览器
            xhr = new XMLHttpRequest();//创建一个对象
        }else{//早期的IE浏览器
            xhr = new ActiveXObject('Microsoft.XMLHTTP');//参数是规定的；
        }
        console.log("状态q"+xhr.readyState);//0
        //第二步：准备发送请求-配置发送请求的一些行为
        //open即打开链接，第一个参数是以什么方式；第二个是往哪儿发送请求，第三个可以不写，默认true,表示异步，false表示同步；；
        xhr.open('get','03form.php',true);
        console.log("状态w"+xhr.readyState);//1

        //第三步：执行发送的动作
        //send也可以写在前面，推荐写在后面；写null是兼容问题；
        xhr.send(null);
        console.log("状态e"+xhr.readyState);//1

        //第四步：指定一些回调函数，也属于事件函数；不触发不执行，触发条件是xhr.readyState;z这个值有0-4，共5个状态，是由浏览器控制的；
        xhr.onreadystatechange = function(){
            if(xhr.readyState == 4){//4指服务器返回的数据可以使用；
                if(xhr.status == 200){ //判断已经成功的获取了数据；200表示hTTP请求成功；404表示找不到页面；503表示服务器端有语法错误；
                    var data = xhr.responseText;//json，文本，主角；
                    // var data1 = xhr.responseXML;
                }
            }
            // console.log("状态t"+xhr.readyState);//2表示已经发送完成；

            // console.log(1234);
        }

        // console.log(456);
        console.log("状态r"+xhr.readyState);//1


    }
    </script>
```

ajax 状态值(readystate)和状态码(status) 前者体现的是服务器对请求的反馈，后者表明客户端与客户的交互状态过程

状态值（0~4）

0：(未初始化) 还没有调用open()方法。
1：(启动) 已经调用open()方法，但还没有调用send()方法。
2：(发送) 已经调用send()方法，但还没有接收到响应。
3：(接收) 已经接收到部分响应数据。
4：(完成) 已经接收到全部的响应数据，且可以在客户端使用了。

状态码（1xx 信息,2xx 成功,3xx 重定向,4xx 客户端错误,5xx 服务端错误)

100 Continue 正在处理，可以继续发送请求

200 ok 请求成功

301 永久性重定向 

302 临时性重定向

400 Bad Request 请求中包含语法错误

403 Forbidden 禁止访问

404 Not Found 找不到文件

500 服务器正在执行请求发生错误

503 服务器超载或停机维护，无法处理请求

