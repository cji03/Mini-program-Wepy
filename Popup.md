# 31/05
css: transition + transform 实现动画效果。   
实现从底部popup出的动画效果 + 渐隐渐出的mask层。

```html
// 组件结构
<view class="popup" :class="{'popup-show' : flag}">
	<view class="popup-mask"></view>
	<view class="popup-container"></view>
</view>
```

```css
.popup {
  visibility: hidden;
	// 初始状态为不可见
}
.popup-mask {
  z-index: 0;
	// opacity动画效果和displa:none有冲突，所以使用z-index来隐藏遮罩层
	// 层级为0，藏到其它dom层级下
  opacity: 0;
  transition: .4s;
}
.popup-container {
  background: white;
  min-height: 67%;
  position: fixed;
  left: 0;
  right: 0;
  bottom: 0;
  transform: translate3d(0, 100%, 0);
	//	初始状态藏在可视区域外侧。
  transform-origin: center;
  transition: .4s;
  z-index: 11;
  opacity: 0;
}
.popup-show {
  visibility: visible;
}
.popup-show .popup-mask {
  opacity: 1;
  z-index: 10;
}
.popup-show .popup-container {
  opacity: 1;
  transform: translate3d(0, 0, 0);
}
```

> display:none 无法应用transition效果，所以使用z-index来隐藏mask层级。   
> visibility + opacity 来控制mask层淡入淡出。   
> transform + opacity 来控制popup的划入划出。
