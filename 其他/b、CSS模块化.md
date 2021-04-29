## CSS 模块化

- 你是否为 class 命名而感到苦恼？
- 你是否有怕跟别人使用同样 class 名而感到担忧？
- 你是否因层级结构不清晰而感到烦躁？
- 你是否因代码难以复用而感到不爽？
- 你是否因为 common.css 的庞大而感到恐惧？

你如果遇到如上问题，那么就很有必要使用 css 模块化。

## CSS 模块化的实现方式

### CSS Modules

CSS Modules 指的是我们像 import js 一样去引入我们的 css 代码，代码中的**每一个类名都是引入对象的一个属性**，通过这种方式，即可在使用时明确指定所引用的 css 样式。

并且 CSS Modules 在打包的时候会自动将类名转换成 hash 值，完全杜绝 css 类名冲突的问题。

使用方式如下：

```css
.className {
    color: green;
}
/* 编写全局样式 */
:global(.className) {
    color: red;
}

/* 样式复用 */
.otherClassName {
    composes: className;
    color: yellow;
}

.otherClassName {
    composes: className from "./style.css";
}
```

2、在 js 模块中导入 css 文件。

```js
import styles from "./style.css";

element.innerHTML = '<div class="' + styles.className + '">';
```

**3、配置 css-loader 打包。**

**CSS Modules 不能直接使用，而是需要进行打包**，一般通过配置 **css-loader** 中的 **modules** 属性即可完成 css modules 的配置。

```js
rules: [
    {
        test: /\.css$/,
        use:{
            loader: 'css-loader',
            options: {
                modules: {
                    // 自定义 hash 名称
                    localIdentName: '[path][name]__[local]--[hash:base64:5]',
                }
            }
    }
]
```

4、最终打包出来的 css 类名就是由一长串 hash 值生成。

```js
._2DHwuiHWMnKTOYG45T0x34 {
    color: red;
}

._10B-buq6_BEOTOl9urIjf8 {
    background-color: blue;
}
```

### CSS In JS

CSS in JS，意思就是使用 js 语言写 css，完全不需要些单独的 css 文件，所有的 css 代码全部放在组件内部，以实现 css 的模块化。

CSS in JS 其实是一种编写思想，目前已经有超过 40 多种方案的实现，最出名的是 styled-components。

```js
import React from "react";
import styled from "styled-components";

// 创建一个带样式的 h1 标签
const Title = styled.h1`
  font-size: 1.5em;
  text-align: center;
  color: palevioletred;
`;

// 创建一个带样式的 section 标签
const Wrapper = styled.section`
  padding: 4em;
  background: papayawhip;
`;

// 通过属性动态定义样式
const Button = styled.button`
  background: ${props => (props.primary ? "palevioletred" : "white")};
  color: ${props => (props.primary ? "white" : "palevioletred")};

  font-size: 1em;
  margin: 1em;
  padding: 0.25em 1em;
  border: 2px solid palevioletred;
  border-radius: 3px;
`;

// 样式复用
const TomatoButton = styled(Button)`
  color: tomato;
  border-color: tomato;
`;

<Wrapper>
  <Title>Hello World, this is my first styled component!</Title>
  <Button primary>Primary</Button>
</Wrapper>;

```

可以看到，我们直接在 js 中编写 css，案例中在定义源生 html 时就创建好了样式，在使用的时候就可以渲染出带样式的组件了。

除此之外，还有其他比较出名的库：

- emotion
- radium
- glamorous

![image-20210406142035624](https://cdn.jsdelivr.net/gh/zangguojun/PicGo/20210406142043.png)













