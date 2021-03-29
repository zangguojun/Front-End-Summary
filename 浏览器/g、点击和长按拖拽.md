**onmousedown 和 onmouseup**事件

他们分别有触发时的时间，

另外还有鼠标的x y,如果只是点击，x y 在mouseup 和down里面应该是一样的，

但是拖拽的话时间间隔不仅比较久而且x,y是有数值差异的，因为位移了

先记录mouseDown时的坐标，move和up的时候判断XY坐标是否变化，变化了则是拖拽，没变就是点击