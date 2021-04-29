# WebPack 

WebPack 是一个模块打包工具，你可以使用WebPack管理你的模块依赖，并编绎输出模块们所需的静态文件。它能够很好地管理、打包Web开发中所用到的HTML、JavaScript、CSS以及各种静态文件（图片、字体等），让开发过程更加高效。对于不同类型的资源，webpack有对应的模块加载器。webpack 模块打包器会分析模块间的依赖关系，最后生成了优化且合并后的静态资源。

### 1.2 webpack 五个核心概念

#### 1.2.1 Entry 

入口(Entry)：指示 webpack 以哪个文件为入口起点开始打包，分析构建内部依赖图。

+ string
+ []
+ {}
+ {key:[]}

#### 1.2.2 Output

输出(Output)：指示 webpack 打包后的资源 bundles 输出到哪里去，以及如何命名。

+ filename:’js/build.js’
+ filename:’js/[name].js’

#### 1.2.3 Loader

Loader：让 webpack 能够去处理那些非 JS 的文件，比如样式文件、图片文件(webpack 自身只理解JS)

module:{rule:[]}

#### 1.2.4 Plugins

插件(Plugins)：可以用于执行范围更广的任务。插件的范围包括，从打包优化和压缩，一直到重新定义环境中的变量等。

#### 1.2.5 Mode

模式(Mode)：指示 webpack 使用相应模式的配置。

| 选项        | 特点                       | 描述                                                         |
| ----------- | -------------------------- | ------------------------------------------------------------ |
| development | 能让代码本地调试运行的环境 | 会将 DefinePlugin 中 process.env.NODE_ENV 的值设置为 development。启用 NamedChunksPlugin 和 NamedModulesPlugin。 |
| production  | 能让代码优化上线运行的环境 | 会将 DefinePlugin 中 process.env.NODE_ENV 的值设置为 production。启用 FlagDependencyUsagePlugin, FlagIncludedChunksPlugin, ModuleConcatenationPlugin, NoEmitOnErrorsPlugin, OccurrenceOrderPlugin, SideEffectsFlagPlugin 和 TerserPlugin。 |

###  使用base64编码图片的适用场景 && 优缺点

　　适用场景：

　　　　1.有些作为背景的图片，但又能制作成css sprite

　　　　2.转换成base64编码后体积不是太大的时候，适合小型图片，比如logo等

　　**优点：**

　　　　1.减少http请求

　　　　2.放在css中使用的，可以利用缓存，从而减少http请求

　　**缺点：**

　　　　1.部分浏览器不支持(IE)

　　　　2.base64后的图片比较大，会增加插入文件的体积

　　　　3.图片完成后还需要base64编码，目前估计手工完成的多，因此，增加了一定的工作量，虽然不多

**webpack**的两大特色： 

1）code splitting（可以自动完成） 

2）loader 可以处理各种类型的静态文件，并且支持串联操作

 **webpack**具有requireJs和browserify的功能，但仍有很多自己的新特性：

1） 对 CommonJS 、 AMD、ES6的语法做了兼容 

2）对js、css、图片等资源文件都支持打包 

3） 串联式模块加载器以及插件机制，让其具有更好的灵活性和扩展性，例如提供对CoffeeScript、ES6 的支持 

4） 有独立的配置文件webpack.config.js 

5） 可以将代码切割成不同的chunk，实现按需加载，降低了初始化时间 

6）支持 SourceUrls 和SourceMaps，易于调试 

7） 具有强大的Plugin接口，大多是内部插件，使用起来比较灵活 

8）webpack 使用异步 IO 并具有多级缓存。这使得 webpack 很快且在增量编译上更加快

