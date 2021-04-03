

- 绝对定位 父元素 relative，左右leftright各为0，absolute，中间元素设置margin

- BFC 左右float，中间overflow：hidden

- Flex 写法左中右，父元素display:flex，中间区域flex:1

- table布局，写法左中右，父元素display：table，三个元素table-cell

- 圣杯布局 写法中左右，中间width100%，左边margin-left -100%，右边margin-left= -width（-100px); 然后防遮住中间，左右相对定位relative。

- 双飞翼 中间再包裹一层，左，右。中间父元素 float left，width：100%。

  中间子元素 margin left right 左右的width。 左右float,左边margin-left -100%，右边margin-left -width

## 圣杯布局

+ (1)中间部分需要根据浏览器宽度的变化而变化，所以要用100%，这里设左中右向左浮动，因为中间100%，左层和右层根本没有位置上去

+ (2)把左层margin负100后，发现left上去了，因为负到出窗口没位置了，只能往上挪
+ (3)按第二步这个方法，可以得出它只要挪动窗口宽度那么宽就能到最左边了，利用负边距，把左右栏定位
+ (4)但由于左右栏遮挡住了中间部分，于是采用相对定位方法，各自相对于自己把自己挪出去，得到最终结果

```html
<div class="left">左栏</div>
<div class="right">右栏</div>
<div class="middle">中间栏</div>
```

## 两边两栏宽度固定，中间栏宽度自适应

### 左右浮动，中间设置margin

> 实现方法：需要左栏向左浮动，右栏向右浮动，中间设左右margin来撑开距离

```css
body{
    margin: 0;
    padding: 0;
}
.left{
    width:200px;
    height: 300px;
    background-color: #DC698A;
    float:left;
}
.middle{
    /*width:100%;*/
    /*中间栏不要设宽度，包括100%*/
    height: 300px;
    background-color: #8CB08B;
    margin:0 200px;
}
.right{
    width: 200px;
    height: 300px;
    background-color: #3EACDD;
    float: right;
}
```

```
<!-- 左栏左浮右栏右浮，中间不设宽度用左右margin值撑开距离，且布局中中间栏放最后 -->
<!-- 中间栏实际宽度是当前屏的100% -->
```

### 三栏左浮动，左栏margin-left:-100% 中栏width:100% 右栏margin-left:-右栏宽度

> 实现方法：左栏、右栏、中间栏向左浮动，左栏的margin-left设为-100%，中间栏的width设为100%，右栏的margin-left设为-右栏宽度





```
<!-- 左栏中间栏右栏左浮，左栏margin-left：-100%，中间栏宽100%,，右栏margin-left:-右栏宽度 
且布局上必须中间栏放第一个-->
```