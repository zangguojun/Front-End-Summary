## 降低动画的分辨率

![image-20210314104201236](C:%5CUsers%5Cbuchiyu%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20210314104201236.png)



+ 动画分层（zlevel）
  + 把动的和不动的分层 
  + 动画有了层次，他的线条的光效和轨迹都变的更明显，而且流畅度也得到了很大的提升，即便是很多动画也没有问题

![image-20210314104345320](C:%5CUsers%5Cbuchiyu%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20210314104345320.png)



## chart不应该放到vue的data 的option里面

+ 把echarts的实例放到了data里面，这样vue在defineproperty的时候会改写实例，会影响echarts和vue的效率
+ 把data里面的chart去掉，在直接引用chart

![image-20210314104850843](C:%5CUsers%5Cbuchiyu%5CAppData%5CRoaming%5CTypora%5Ctypora-user-images%5Cimage-20210314104850843.png)



## 不要重复的set option

