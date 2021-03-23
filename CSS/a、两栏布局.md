### 两栏布局，左边固定，先加载内容区

- float。
  - 两个div。左边float:left，width:200 px，右边 margin-left=width。
- 绝对定位。
  - 两个div。左边absolute或者fixed 右边margin-left=width
- table布局。
  - 三个div，父元素display：table，子元素display table-cell width，右边自适应
- flex布局。
  - 三个div,父元素display flex; 子元素flex:1