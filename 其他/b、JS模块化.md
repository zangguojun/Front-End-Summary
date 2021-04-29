# 对前端模块化的认识 

AMD 是 RequireJS 在推广过程中对模块定义的规范化产出。 

CMD 是 SeaJS 在推广过程中对模块定义的规范化产出。 

AMD 是提前执行，CMD 是延迟执行。 

AMD推荐的风格通过返回一个对象做为模块对象，

CommonJS的风格通过对module.exports或exports 的属性赋值来达到暴露模块对象的目的。 

CMD模块方式

```js
define(function(require, exports,module) {
// 模块代码
});
```

**AMD-异步模块定义**

**AMD是RequireJS**在推广过程中**对模块定义的规范化**产出，它是一个概念，RequireJS是对这个概念的实现，就好比JavaScript语言是对ECMAScript规范的实现。AMD是一个组织，RequireJS是在这个组织下自定义的一套脚本语言

![img](https://img-blog.csdn.net/20180718200247892?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmd4aXVqaWFuZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

**RequireJS**：**是一个AMD框架**，可以异步加载JS文件，按照模块加载方法，通过define()函数定义，第一个参数是一个数组，里面定义一些需要依赖的包，第二个参数是一个回调函数，通过形参来引用模块里面的方法，最后通过return来输出。

是一个**依赖前置、异步定义**的AMD框架（在参数里面引入js文件），在定义的同时如果需要用到别的模块，在最前面定义好即在参数数组里面进行引入，在回调里面加载



**2）CMD---**是**SeaJS**在推广过程中对模块定义的规范化产出，是一个同步模块定义，是SeaJS的一个标准，SeaJS是CMD概念的一个实现，SeaJS是淘宝团队提供的一个模块开发的js框架.

![img](https://img-blog.csdn.net/20180718203155130?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmd4aXVqaWFuZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

通过define()定义，没有依赖前置，通过require加载jQuery插件，CMD是依赖就近，在什么地方使用到插件就在什么地方require该插件，即用即返，这是一个同步的概念

## AMD与CMD区别

关于这两个的区别网上可以搜出一堆文章，简单总结一下

最明显的区别就是在模块定义时对依赖的处理不同

**1、AMD推崇依赖前置，在定义模块的时候就要声明其依赖的模块**
**2、CMD推崇就近依赖，只有在用到某个模块的时候再去require**

AMD和CMD最大的区别是对依赖模块的执行时机处理不同，注意不是加载的时机或者方式不同

同样都是异步加载模块，AMD在加载模块完成后就会执行该模块，所有模块都加载执行完后会进入require的回调函数，执行主逻辑，这样的效果就是依赖模块的执行顺序和书写顺序不一定一致，看网络速度，哪个先下载下来，哪个先执行，但是主逻辑一定在所有依赖加载完成后才执行

CMD加载完某个依赖模块后并不执行，只是下载而已，在所有依赖模块加载完成后进入主逻辑，遇到require语句的时候才执行对应的模块，这样模块的执行顺序和书写顺序是完全一致的

这也是很多人说AMD用户体验好，因为没有延迟，依赖模块提前执行了，CMD性能好，因为只有用户需要的时候才执行的原因



**3）CommonJS规范--**-是通过**module.exports定**义的，在前端浏览器里面并不支持module.exports,通过node.js后端使用的。Nodejs端是使用CommonJS规范的，前端浏览器一般使用AMD、CMD、ES6等定义模块化开发的

![img](https://img-blog.csdn.net/20180718203952178?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmd4aXVqaWFuZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)

输出方式有2种：默认输出---module export 和带有名字的输出---exports.area

**4）ES6特性，模块化**---**export/import对模块进行导出导入的**

![img](https://img-blog.csdn.net/20180718204535949?watermark/2/text/aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3Rhbmd4aXVqaWFuZw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70)









# require和import的区别

### 1. 出现的时间、地点不同

|                 | 年份 | 出处                  |
| --------------- | ---- | --------------------- |
| require/exports | 2009 | CommonJS              |
| import/export   | 2015 | ECMAScript2015（ES6） |



### 2. 不同端(客户端/服务器)的使用限制

|         | require/exports | import/export                                                |
| ------- | --------------- | ------------------------------------------------------------ |
| Node.js | 所有版本        | Node 9.0+（启动需加上 flag --experimental-modules） Node 13.2+（直接启动） |
| Chrome  | 不支持          | 61+                                                          |
| Firefox | 不支持          | 60+                                                          |
| Safari  | 不支持          | 10.1+                                                        |
| Edge    | 不支持          | 16+                                                          |

CommonJS 模块化方案 require/exports 是**为服务器端开发设计的**。服务器模块系统**同步读取模块文件内容**，编译执行后得到模块接口。（Node.js 是 CommonJS 规范的实现）。

在浏览器端，因为其**异步加载脚本文件**的特性，CommonJS 规范无法正常加载。所以出现了 RequireJS、SeaJS 等（兼容 CommonJS ）为浏览器设计的模块化方案。

两种方案各有各的限制，需要注意以下几点：

- **原生浏览器不支持 require/exports**，可使用支持 CommonJS 模块规范的 Browsersify、webpack 等打包工具，它们会将 require/exports 转换成能在浏览器使用的代码。
- **import/export 在浏览器中无法直接使用**，我们需要在引入模块的 <script> 元素上添加type=”module”属性。
- 即使 Node.js 13.2+ 已经支持 import/export，Node.js官方不建议在正式环境使用，目前可以使用 [babel](https://link.zhihu.com/?target=https%3A//babeljs.io/) 将 ES6 的模块系统编译成 CommonJS 规范（注意：语法一样，但具体实现还 是require/exports）。



### 3. require/exports 是运行时动态加载，import/export 是静态编译

CommonJS 加载的是一个对象（即 module.exports 属性），该对象**只有在脚本运行完才会生成**。

而 ES6 模块不是对象，它的**对外接口只是一种静态定义**，在**代码静态解析阶段就会生成**。



### 4. require/exports 输出的是一个值的拷贝，import/export 模块输出的是值的引用

require/exports 输出的是值的拷贝。也就是说，一旦输出一个值，模块内部的变化就影响不到这个值。

import/export 模块输出的是值的引用。JS 引擎对脚本静态分析的时候，遇到模块加载命令import，就会生成一个只读引用。等到脚本真正执行时，再根据这个只读引用，到被加载的那个模块里面去取值。

若文件引用的模块值改变，require 引入的模块值不会改变，而 import 引入的模块值会改变



### 5. 用法不一致

### (1). require/exports 的用法

```text
const fs = require('fs')
exports.fs = fs
module.exports = fs
```

exports 是对 module.exports 的引用，相当于

```text
exports = module.exports = {};
```

在不改变 exports 指向的情况下，使用 exports 和 module.exports 没有区别；如果将 exports 指向了其他对象，exports 改变不会改变模块输出值。示例如下：

```text
//utils.js
let a = 100;

exports.a = 200;
console.log(module.exports) //{a : 200}
exports = {a:300}; //exports 指向其他内存区

//test.js

var a = require('./utils');
console.log(a) // 打印为 {a : 200}
```

### (2). import/export 的写法

```text
import fs from 'fs'
import {readFile} from 'fs' //从 fs 导入 readFile 模块
import {default as fs} from 'fs' //从 fs 中导入使用 export default 导出的模块
import * as fileSystem from 'fs' //从 fs 导入所有模块，引用对象名为 fileSystem
import {readFile as read} from 'fs' //从 fs 导入 readFile 模块，引用对象名为 read

export default fs
export const fs
export function readFile
export {readFile, read}
export * from 'fs'
```

> 建议1：建议明确列出我们要引用的内容。，使用 * 虽然很方便，但是不利于现代的构建工具检测未被使用的函数，影响代码优化。

同时需要注意

1. 引入 export default 导出的模块不用加 {},引入非 export default 导出的模块需要加 {}。

```text
import fileSystem, {readFile} from 'fs'
```

\2. 一个文件只能导出一个 default 模块。

\3. 在验证代码的时候遇到如下报错

```text
access to script from origin 'null' has been blocked by CORS policy
```

前面提到过，浏览器引入模块的 `<script>` 元素要添加 `type="module` 属性，但 **module 不支持 FTP 文件协议(file://)，只支持 HTTP 协议**，所以本地需要使用 http-server 等[本地网络服务器](https://link.zhihu.com/?target=https%3A//juejin.im/post/6844903559851425805)打开网页文件。

### (3). import/export 不能对引入模块重新赋值/定义

当我尝试给 import 的模块重新赋值时

```text
import {e1} from './webUtils.js';
  e1=234;
```

浏览器显示

> Uncaught TypeError: Assignment to constant variable.

当我重新定义引用的模块

```text
import {e1} from './webUtils.js';
 var e1=1;
```

浏览器显示

> (index):17 Uncaught SyntaxError: Identifier 'e1' has already been declared

### (4). ES6 模块可以在 import 引用语句前使用模块，CommonJS 则需要先引用后使用

ES6 模块

```js
//webUtils.js
export var e='export';
console.log(e) //export
  import {e} from './webUtils.js';
  console.log(e) //export
```

CommonJS

```js
//utils.js
exports.e = 'export';
console.log(a) 
a = require('./utils');
console.log(a)
```

程序报错

> ReferenceError: a is not defined

### （5）import/export 只能在模块顶层使用，不能在函数、判断语句等代码块之中引用；require/exports 可以。

```js
   import fs from  './webUtils.js';
   function a(){
    import {e1} from './webUtils.js';
    console.log(e1)
   }
   a();
   console.log(fs())
```

程序报错

> Uncaught SyntaxError: Unexpected token '{'

前面提到过 import/export 在代码静态解析阶段就会生成，不会去分析代码块里面的 import/export，所以程序报语法错误，而不是运行时错误。

### 6. 是否采用严格模式

> 严格模式是采用具有限制性JavaScript变体的一种方式

import/export 导出的模块默认调用严格模式。

```text
var fun=()=>{
    mistypedVaraible = 17; //报错，mistypedVaraible is not defined
};
export default fun;
```

require/exports 默认不使用严格模式，可以自定义是否使用严格模式。 例如

```text
exports.fun = ()=>{
 mistypedVaraible = 17; //没有调用严格模式，不会报错
};
```

### 7. 其他模块化方法

### [动态导入](https://link.zhihu.com/?target=https%3A//zh.javascript.info/modules-dynamic-imports)

import(modulePath) 表达式加载模块并返回一个 promise，该 promise resolve 为一个包含其所有导出的模块对象。

我们可以在代码中的任意位置动态地使用它。例如：

```text
import('/modules/my-module.js') //动态导入
  .then((module) => {
    // Do something with the module.
});
```

> 建议: 请不要滥用动态导入 import()（只有在必要情况下采用）。静态框架能更好的初始化依赖，而且更有利于静态分析工具和 tree shaking 发挥作用







## 共同点：

都是为了实现js代码的模块化，只是两种不同的方式而已。

## 问：

那为什么会出现两种方案呢，又有什么不同呢？

## 清楚几种模块化的方案：

- 野生规范：`require/exports`
  （1）requirejs遵循AMD；
  （2）seajs遵循CMD；
  （3）node的module遵循CommonJs。
- 名门正派：`import/export`
  （1）TC39制定的新的ECMAScript版本，即在ES6（ES2015）中包含进来。
  虽然写法上有所不同，但都是为了能够间接实现模块化，保持较为一致的代码风格。

## 官方标准定义的模块化方案

即：import、export。
可是，标准毕竟是标准，各大浏览器和node终端要实现标准还是有一段距离的。目前都2018年了，各大主浏览器都还没实现，还得依赖转换工具（例如babel）将代码转为ES5之后浏览器才能解析。
所以这也就解释了为什么我们的工程化代码中nodeJS遵循的commonjs规范和ES6的模块化方案并存的现象。



3

## 两者的区别

- import是ES6标准中的模块化解决方案，require是node中遵循CommonJS规范的模块化解决方案。
- 后者支持动态引入，也就是`require(${path}/xx.js)`，前者目前不支持，但是已有提案。
- 前者是关键词，后者不是。
- 前者是编译时加载，必须放在模块顶部，在性能上比后者好一些；后者是运行时加载，理论来说放在哪里都可以。
- 前者采用的是实时绑定方式，即导入和导出的值都指向同一个内存地址，所以导入的值会随着导出值变化；而后者在导出时是指拷贝，就算导出的值变化了，导入的值也不会变化，如果想要更新导入的值，就要重新导入。
- 前者会编译成`require/exports`来执行。

### 一、 import的用法

- import导入模块
  `import { a } from 'test.js';`
  `import * as test from 'test.js';`
  `import { a as key, b as value } from 'test.js';`
- import导出模块
  `export {}`
  `export default {}`
  `export default XX` - （导出一个类）

（1）命名导出（每个模块包含任意数量）



```dart
// test.js

var a = 'aaa';
var b = 'bbb';
var c = 'ccc';
export {
    a,
    b,
    c
};
```



```jsx
// demo.js

import test from './test';
console.log('===test===:', test); // ===test===: undefined


import { a } from './test.js';
console.log('===test===:', a); // ===test===: aaa


import * as test from './test';
console.log('===test===:', test); // ===test===: {a: "aaa", b: "bbb", c: "ccc", __esModule: true}
// 导出的test为一个module{}，所有内容，截图链接：https://upload-images.jianshu.io/upload_images/3098902-114f164d0eb3ed4f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240


import { a as key, b as value } from './test';
console.log('===test===:', key + '-----' + value); // ===test===: aaa-----bbb
```

（2）默认导出（每个模块包含一个）



```dart
// test.js

var a = 'aaa';
var b = 'bbb';
var c = 'ccc';
var d = 'ddd';
export default {
    a,
    b,
    c,
    dd: d // 需要自定义属性名的，只能使用`export default`来导出，export {}不可以，会报错。
};
```



```jsx
// demo.js

import test from './test';
// 实际上这个default就是一个语法糖，只不过default是es6的一个关键字。
// 等同于
import { default as test } from './test';
console.log('===test===:', test); // ===test===: {a: "aaa", b: "bbb", c: "ccc", dd: "ddd"}


import { a } from './test.js';
console.log('===test===:', a); // ===test===: undefined


import * as test from './test';
console.log('===test===:', test); // ===test===: {default: {…}, __esModule: true}
// 导出的test为一个module{}，所有内容，截图链接：https://upload-images.jianshu.io/upload_images/3098902-5054c2ed429bb7ab.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240


import { a as key, b as value } from './test';
console.log('===test===:', key + '-----' + value); // ===test===: undefined-----undefined
```

（3）模块重定向
*在当前模块中，导出指定导入模块的默认导出（等于是创建了一个“重定向”）。*



```jsx
// test.js

let test1 = {
    a: 1
}
const test2 = (x, y) => {
    return x + y
}
export {
    test1
}
export default test2;
```



```jsx
// 重点：体现了重定向  module.js
// 这两种是重定向的标准写法

// 将export {} 原封不动导出去
export * from './test'; // 适用于 导出方式为export {}

// 导出出去的没有default属性，相当于导出的直接是default的值。
export { default } from './test'; // 适用于 导出方式为 export default xx;
```



```jsx
// 页面引用 demo.js

import test2, { test1 } from './module';

console.log('test1', test1); // {a: 1}
console.log('test2', test2) // ƒ test2(x, y) { return x + y; }
```

### 二、require

- require导入模块
  `const test = require('./test.js');`
- require导出模块
  `module.exports = {}`
  `exports.xx = xx`

（1）require导入模块就没那么复杂了，导出时是什么样，导入时就还是什么样：



```java
// test.js

var a = 'aaa';
var b = 'bbb';
var c = 'ccc';
module.exports = {
    a,
    b,
    c
};
```



```jsx
// demo.js

const test = require('./test');
console.log('===test===:', test); // ===test===: {a: "aaa", b: "bbb", c: "ccc"}

// 当然，也可以这样导入解析，不需要babel也可以正确运行
const { c } = require('./test');
console.log('===test===:', c); // ===test===: ccc
```

（2）直接导出 - 不常用的方式



```java
// test.js

var a = 'aaa';
var b = 'bbb';
var c = 'ccc';
exports.c = c;
```



```jsx
// demo.js

const test = require('./test');
console.log('===test===:', test); // ===test===: {c: "ccc"}

// 同上面，也可以以这样导入
const { c } = require('./test');
console.log('===test===:', c); // ===test===: ccc
```

ok，将两者终于进行比较并做了笔记，后面就容易查阅和理解了，所列出来的情况都有进行代码演示，完全真实。。后面有补充的话会进行更新喔。。。

