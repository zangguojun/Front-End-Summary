- 新增选择器：E：nth-child(n) E:nth-last-child(n)

- Transition、Transform和Animation

  - ```css
    transform:
    div{
        transform:rotate(7deg);
        -ms-transform:rotate(7deg);/*IE9*/
        -moz-transform:rotate(7deg);/*FireFox*/
        -webkit-transform:rotate(7deg);/*Safari和Chrome*/
        -o-transform:rotate(7deg);/*Opwea*/
    }
    ```

  - ```css
    transition：
    div{
        transition-property:width;
        transition-duration:1s;
        transition-timing-function:linear;
        transition-delay:25;
        /*简写形式*/
        transition:width 1s linear 25;
    }
    
    
    animation:
    @keyframes myAnimation{
        0%{background:red;width:100px;}
        25%{background:orange;width:200px;}
        50%{background:yellow;width:100px;}
        75%{background:green;width:200px;}
        100%{background:blue:width:100px;}
    }
    #box{
        animation:myAnimation 5s;
        width:100px;
        height:100px;
        background:red;
    }
    ```

  - 创建动画序列，需要animation属性或其子属性，属性允许配置动画时间、时长和动画细节。动画的实际表现由@keyframes 规则实现

  - transtion也可以实现动画，但强调过渡，是元素的一个或多个属性变化时产生的过渡效果，同一个元素通过两个不同的途径获取样式，而第二个途径当某种改变发生时（如：hover)才能获取样式，这样就会产生过渡动画。

  - ##### transition、animation的区别

    - animation和transition大部分属性相同，都是随时间改变元素的属性值，区别是transition需要触发一个事件才能改变属性；animation不需要触发任何事件随时间改变属性。transition为2帧，从from……to ，animation可以一帧一帧的。

  - 边框：box-shadow（阴影）,border-radius（弧度）

  - 背景：background-clip(背景展示),background-size

  - 文字：text-shadow（阴影）,text-overflow（溢出）

  - 字体：@font-face

    - ```css
      @font-face{
          font-family:myFirstFont;
          src:url(sansation_light,woff);
      }
      ```