- 浮动布局 float:left right，中间根据两边的width设置margin(要加两边的border)

- 绝对定位 父元素 relative，左右leftright各为0，absolute，中间元素设置margin

- BFC 左右float，中间overflow：hidden

- Flex 写法左中右，父元素display:flex，中间区域flex:1

- table布局，写法左中右，父元素display：table，三个元素table-cell

- 圣杯布局 写法中左右，中间width100%，左边margin-left -100%，右边margin-left= -width（-100px); 然后防遮住中间，左右相对定位relative。

- 双飞翼 中间再包裹一层，左，右。中间父元素 float left，width：100%。

  中间子元素 margin left right 左右的width。 左右float,左边margin-left -100%，右边margin-left -width