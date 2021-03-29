# 虚拟dom的定义与作用

- ### 什么是虚拟dom

**虚拟dom就是一个普通的js对象**。是一个用来描述真实dom结构的js对象，因为他不是真实dom，所以才叫虚拟dom。

虚拟dom的结构，从下图中，我们来看一看虚拟dom结构到底是怎样的

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223170243607.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcwNzI4Nw==,size_16,color_FFFFFF,t_70)

如上图，这就是虚拟dom的结构，他是一个对象，下面有6个属性，**sel**表示**当前节点标签名**，**data**内是**节点的属性**，**elm**表示**当前虚拟节点对应的真实节点**（这里暂时没有），**text**表示当前**节点下的文本**，**children**表示当前节点下的**其他标签**



- ### 虚拟dom的作用

1、我们都知道，**传统dom数据发送变化的时候**，我们都需要不断的去**操作dom**，才能**更新dom的数据**，虽然后面出现了模板引擎这种东西，可以让我们一次性去更新多个dom。但模板引擎依旧没有一种可以**追踪状态的机制**，当引擎内某个数据发生变化时，他依然要操作dom**去重新渲染整个引擎**。
而虚拟dom可以很好的**跟踪当前dom状态**，因为他会根据当前数据生成一个**描述当前dom结构的虚拟dom**，**然后数据发送变化时，又会生成一个新的虚拟dom**，而**这两个虚拟dom恰恰保存了变化前后的状态**。然后**通过diff算法**，计算出两个前后两个虚拟dom之间的差异，得出一个更新的最优方法（哪些发生改变，就更新哪些）。可以很明显的**提升渲染效率以及用户体验**
**2**、因为**虚拟dom**是一个普通的javascript对象，故他不单单只能允许在浏览器端，渲染出来的虚拟dom可同时在node环境下或者weex的app环境下允许。有很好的跨端性





# 什么是diff算法

diff算法就是用于**比较新旧两个虚拟dom之间差异的一种算法**。

### vue中的虚拟dom

目前虚拟dom的类库有多种，常见的有**snabbdom**和**virtual-dom**， vue以前用的是virtual-dom，从2.x版本后都是使用的snabbdom。今天，我们就通过**snabbdom源码来解析vue的虚拟dom**，首先我们看下snabbdom源码结构。

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022317383662.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcwNzI4Nw==,size_16,color_FFFFFF,t_70)

要搞清楚vue虚拟dom，我们就需要搞清楚几个核心的方法

- **h函数**
- **patch函数**
- **updateChildren函数**
- **patchVnode函数**

这几个核心函数的源码，看着可能会比较累，我就不一一对源码做详细的介绍，我主要会介绍每个函数主要做了什么事情，然后后面再附上源码，会加点注释，看的懂得可以详细看看

## h函数

h函数，看着是不是很眼熟？ 他是在vue的什么阶段去调用的？

<img src="https://img-blog.csdnimg.cn/20210223174926575.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcwNzI4Nw==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom:150%;" />

眼熟吧，是不是在这地方看过啊。没错，**h函数就是在render函数内运行的**。我们在前面vue生命周期的文章中就提过，vue在`created–>beforeMount`之间的时候会**将模板编译成render函数**，其实就是**将模板编译成某种格式放在render函数内**，然后**当render函数运行**的时候，就会生成**虚拟dom**。那么编译成什么格式呢。就是编译成h函数所认可的格式。那么我们来看看h函数需要什么格式

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210223175803995.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcwNzI4Nw==,size_16,color_FFFFFF,t_70)

h函数是使用函数重载的方式定义的。

### 函数重载

函数重载就是定义多个重名函数，利用函数的参数个数以及参数类型来区分。当参数个数不同，参数类型不同时，函数内执行的代码也会相应不同。
下面，我们就来看下最典型得一种，也就是图中得第五种。

- 第一个参数sel 表示dom选择器，如： `div#app.wrap` ==》 `<div id="app" class="wrap"></div>`
- 第二个参数表示dom属性，是个对象如：`{ class: ‘ipt’, value: ‘今天天气很好’ }`
- 第三个参数表示子节点，子节点也可以是一个子虚拟节点，也可以是文本节点

```js
const vdom = h('div', { class: 'vdom'}, [
    h('p', { class: 'text'}, ['hello word']),
    h('input', { class: 'ipt', value: '今天星期二' })
]) // 模板就是会编译成这种格式
console.log(vdom)
```

而h函数内最主要得就是执行了 vnode函数，vnode函数得主要作用就是将**h函数传进来得参数**转行为了**js对象**（即**虚拟dom**）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210224141154768.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcwNzI4Nw==,size_16,color_FFFFFF,t_70)

而vnode函数，我就不多说了，没几句代码，也很简单，反正就是执行了**生成js对象**（虚拟dom）的代码。直接上图

<img src="https://img-blog.csdnimg.cn/20210224141636504.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcwNzI4Nw==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom:150%;" />

下面我们总结下虚拟dom生成的过程。

- 首先，**代码初次运行**，会走生命周期，当生命周期走到created到beforeMount之间的时候，**会编译template模板成render函数**。然后**当render函数运行时，h函数被调用，而h函数内调用了vnode函数生成虚拟dom，**并返回生成结果。故虚拟dom首次生成。
- 之后，当数据发生变化时会重新编译生成一个新vdom。再后面就等待新旧两个vdom进行对比吧。我们后面就继续说对比的事情。



### diff 比较规则

**①、**diff 比较**两个虚拟dom**只会在**同层级之间进行比较**，**不会跨层级进行比较**。而用来判断**是否是同层级的标准**就是

1. **是否在同一层**
2. **是否有相同的父级**



<img src="https://img-blog.csdnimg.cn/20210224145121751.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcwNzI4Nw==,size_16,color_FFFFFF,t_70" alt="在这里插入图片描述" style="zoom:150%;" />

**②、**diff是采用**先序深度优先遍历得方式进行节点比较**的，即，当比较某个节点时，如果该节点存在子节点，那么会优先比较他的**子节点，直到所有子节点全部比较完成**，才会开始去比较改**节点的下一个同层级节点**。不好理解吗？没关系，我们画个图看一下，就很清晰了



### diff比较整体思路

首先开始比较两个vdom时，这两个vdom肯定是都有各自的根节点的，且根节点必定是一个元素，不可能存在多个。我们首先要比较的肯定是根节点，那我们都知道根节点只有一个，就可以直接比较了。而一个节点的比较，通常分为3个部分

> 声明，下面所说的sel选择器相同，指的是标签名，id，class都相同。
>
> ```html
> <div class=“abc” id=“app”>
> ```
>
> 这样一个dom，他的sel是"**div#app.abc**"

- 比较两个节点是否是相同节点，判断是否是相同节点的条件是，**key和sel（选择器）必须都相同**（那有的人可能会说了，那我标签没有key怎么办啊，没有key那就是undefined，undefined === undefined 始终为true，**所以没有key只需要保证sel相同就行**）。如果不相同，那么**执行替换操作**（即**新增新vnode上的元素，删除旧vnode上的元素** `例如，原来是div，新vnode变成了p，那么就是新增p元素，再删除div元素。相当于就是p替换了div`），这一步，**只有比较根节点时**，**是在patch函数中进行的**。**非根节点**都是在**updateChildren函数**中执行的，**因为根节点只会有一个，可以直接比较**，**而其他节点会存在多个，需要通过一些算法来判断**，具体详情后面会说

- 如果节点相同，那么进去第二部分，**即比较两个节点的属性是否相同**，**节点是否存在文本，文本是否相同**。**是否存在子节点，子节点是否相同**。这部分主要在**patchVnode**中执行
  那么，在第二部分，会做哪些事情呢。
  1、**如果存在文本时，更新文本**
  2、**如果存在属性时，更新属性**
  3、**如果存在子节点时，更新子节点**
  那么，如何更新呢，逻辑也很简单，遵循以下规则：
  1、**如果旧vnode上存在，而新vnode上不存在，那么执行删除操作**
  2、**如果旧vnode上不存在，而新vnode上存在，那么执行新增操作**
  3、**如果新旧vnode上都存在，那么执行替换操作（即，新增新的，删除旧的），文本，和属性的替换是在这部分完成。**而对于子节点，如果新vnode和旧vnode上都存在子节点时，那么会进入第三部分比较。比较子节点的差异。

- 第三部分，主要在**updateChildren**函数中执行，主要用于比较**某个节点下的子节点差异**。而在这里，就要用到**diff的一个算法**了。具体怎么算。我们后面详细说updateChildren时再说。



## patch 函数

上面我们说了，patch是比较的开始，**相当于是diff的入口**，diff就是从这一步开始的。那么既然是开始，说明patch函数比较的肯定就是两个新旧vdom的根节点了。所以，两个vdom直接的比较，**patch是只会触发一次的**。
作用：**比较两个虚拟dom根节点是否相同**。下面我们看下主要的核心代码

![在这里插入图片描述](https://img-blog.csdnimg.cn/202102251751396.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcwNzI4Nw==,size_16,color_FFFFFF,t_70)



## patchVnode

patchVnode 是用于比较**两个相同节点的子级**（**文本，或子节点**）的一个函数。故它的调用总是在**sameVnode**判断之后。只有判断当前比较的**两个vnode**相同时（这里我最后再解释一次，两个vnode相同仅仅代表key相同且sel选择器相同），才会被执行。
但，在比对之前，会先判断下oldVnode === vNode ，因为如果全等，代表子级肯定也完全相等，那么就没必要对比了，直接return;

<img src="https://img-blog.csdnimg.cn/20210225185420214.png" alt="在这里插入图片描述" style="zoom:150%;" />

作用：**对比新旧两个节点，更新dom的子级**（子级包含文本或者是子节点）
对比过程：

1、**如果新vnode有text属性**

- 旧vnode是否有子节点，如果有，代表原来是子节点，现在变成文本了，那么删除子节点，并且设置vnode对象的真实dom的text值（使用setTextContent函数）
- 其他情况不用管，直接设置vnode对象的真实dom的text值

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225183221828.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcwNzI4Nw==,size_16,color_FFFFFF,t_70)



2、如果新vnode**没有text属性**

- 如果新vnode和旧vnode都存在子节点时。是不是要深度对比两个vnode的子节点啊。这个时候会进入第三步，比较子节点（**执行updateChildren**）

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225184328997.png)

如果只有新vnode有子节点，老vnode没有，那么很简单，执行添加节点的操作

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225184530667.png)

如果只有旧vnode有子节点，新vnode没有子节点，很明显，要执行删除旧vnode子节点的操作

![在这里插入图片描述](https://img-blog.csdnimg.cn/2021022518485858.png)

如果两个vnode上**都没有子节点**。但旧节点有text，那么很简单，说明原来有文本，现在没有了，**清空vnode对应dom的text**

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210225183618294.png)

下面，我们看下整体代码

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210226100203463.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcwNzI4Nw==,size_16,color_FFFFFF,t_70)



## updateChildren

终于到这最复杂的一步了。首先，我先说一下**这一步的作用以及具体做了些什么**
作用：**用于比较新旧两个vnode的子节点**
那具体做了什么，怎么比较的。

> 声明：下文中所指的匹配上，指的就是判断是否是**sameVnode**，即上文中所说的，**key相同，sel选择器相同**

- 首先，会将新旧vnode的子节点（oldCh, Ch）提取出来，并分别加上两个指针oldStart, oldEnd, newStart, newEnd。分别指向odlCh的第一个节点，oldCh的最后一个节点，Ch的第一个节点，Ch的最后一个节点
- 比较时，会优先拿`oldStart<—>newStart`，`oldStart<—>newEnd`，`oldEnd<—>newStart`，`oldEnd<—>newEnd` 两两进行对比。如果匹配上，那么会将旧节点对应的真实dom移到新节点的位置上。并将匹配上了的指针往中间移动。**同时匹配上了的两个节点会继续指向patchVnode函数去进一步比对**（指针的移动相当于永远保持指针中间的节点还是尚未匹配状态，已经匹配到的移到指针外面去）
- 如果上面4种比较都没有匹配上，那么这个时候，有key和没key处理方式就不一样了。具体怎么处理，后面会细说。
- 当oldStart > oldEnd 或者 newStart > newEnd时，结束对比。此时
  1、如果是oldStart > oldEnd，代表oldCh都已匹配完成，而此时，如果newStart <= newEnd，那么代表 newStart 和 newEnd直接的节点为新增节点。那么真实dom会在当前newStart 和newEnd之间新增newStart 和 newEnd中间还未匹配的节点。
  2、如果是newStart > newEnd，代表Ch全都已经匹配完成，而此时，如果oldStart 和 newEnd之间还有节点，则说明，这些节点是原来存在的，但现在没有了，此时真实dom删除这些节点。
  此时，比较结束。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210226110052738.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MjcwNzI4Nw==,size_16,color_FFFFFF,t_70)















