# 物理层

## 1.思维导图

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200305225150210.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200307130858669.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

## 2.典型的数据通信模型

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200305215603511.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

## 3.三种通信方式

根据信息在传输线上的传送方向，分为以下三种通信方式：

- 单工通信：单向传输,需要一条信道
- 半双工通信：双向交替传输,需要两条信道
- 全双工通信：双向同时传输,需要两条信道

## 4.两种数据传输方式

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200305221906424.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

## 5.数据通信相关术语

### （1）数据、信号、信源、信宿、信道

![img](https://img-blog.csdnimg.cn/20200305220514167.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

### （2）码元

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200305180247399.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

### （3）速率、波特、带宽

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200305181950214.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)



## 6.编码与调制

### 1.背景知识

#### 基带信号和宽带信号

- **根据原始电信号的特征**，**基带信号**可分为**数字基带信号**和**模拟基带信号**（相应地，信源也分为数字信源和模拟信源）其由信源决定
- **宽带信号**的定义是传输速度达到**200kbps或以上**,不管模拟信号还是数字信号只要满足就可算作宽带信号。

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306215449820.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)



### 2.编码与调制

- 数据无论是数字的，还是模拟的，为了传输的目的都必须转变成信号。
- 把数据变换为模拟信号的过程称为**调制**，把数据变换为数字信号的过程称为**编码**

#### 四种编码与调制方式

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306215955705.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

### 3.四种编码与调制方式

### （1）数字数据`编码`为数字信号

#### 1️⃣非归零编码

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306222245407.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

#### 2️⃣归零编码

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306222700226.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

#### 3️⃣反向不归零编码

![img](https://img-blog.csdnimg.cn/20200306223023795.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

#### 4️⃣曼彻斯特编码

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306223205529.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

- 以太网的编码方式就是曼彻斯特编码

#### 5️⃣差曼彻斯特编码

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306223357966.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

#### 6️⃣4B/5B编码

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306223513565.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

### （2）数字数据`调制`为模拟信号

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200306230830184.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

### （3）模拟数据`编码`为数字信号

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200307124421641.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)

### （4）模拟数据`调制`为模拟信号

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200307124835493.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)



### 7.中继器、集线器

#### 1.中继器

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200307185255373.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)



#### 2.集线器

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200307191345779.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3dlaXhpbl80MzkxNDYwNA==,size_16,color_FFFFFF,t_70)









