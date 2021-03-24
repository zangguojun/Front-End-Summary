## mustache 的基本使用

### mustache 库简介

- `mustache` 是 “胡子” 的意思，因为它的嵌入标记 `{{ }}` 非常像胡子
- `mustache` 是最早的模板引擎库，比 `Vue` 诞生的早多了，它的底层实现机理在当时是非常有创造性的、轰动性的，为后续模板引擎的发展提供了崭新的思路
- 使用详见同级目录下`04、mustacheXXX`

### mustache 库的机理

![在这里插入图片描述](https://img-blog.csdnimg.cn/20210205165124937.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dhbmdodWFuMTAyMA==,size_16,color_FFFFFF,t_70#pic_center)

### 什么是 `tokens`？

- `tokens` 就是**JS的嵌套数组**，说白了，就是**模板字符串的JS表示**

- **它是“抽象语法树”、“虚拟节点”等等的开山鼻祖**

- 模板字符串

  - ```html
    <h1>我买了一个{{thing}}，好{{mood}}啊</h1>
    ```

- tokens

  - ```js
    [
      ["text", "<h1>我买了一个"],
      ["name", "thing"],
      ["text", "好"],
      ["name", "mood"],
      ["text", "啊</h1>"]
    ]
    ```

- 









