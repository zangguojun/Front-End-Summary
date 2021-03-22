### 行内元素和块级元素

CSS规范规定，每个元素都有**display**属性，确定该元素的类型，**每个元素都有默认的display值**，如div的display默认值为“block”，则为“块级”元素；span默认display属性值为“inline”，是“行内”元素。

（1）行内元素有：a b span **img** **input** **select** **strong**（强调的语气）
（2）块级元素有：div ul ol li dl dt dd h1 h2 h3 h4…p
（3）常见的空元素：`<br><hr><img><input><link><meta>`

### 行内元素和块级元素的区别与互换

- 区别

  - 行内元素会在一行水平方向排列，块级元素独占一行，**自动填满父级元素**
  - 块级元素可以包含**行内元素和块级元素**，**行内只能包含文本和其他行内元素**
  - 盒模型属性上，行内元素width height无效，pading和margin垂直方向上无效

- 互换

  display：inline/block

- inline-block元素

  既可以设置高宽，又有inline元素不换行的特性。



### inline-block、inline和block的区别？

+ block是块级元素，能设置宽高，margin/padding都有效前后都有换行符

+ inline设置宽高无效，margin在竖直方向无效，padding有效，前后无换行符

+ inline-block可以设置宽高，margin/padding有效，前后无换行符