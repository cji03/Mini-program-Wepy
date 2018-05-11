### Toggle - Animation

Accordion手风琴效果，在点击之后实现动态展开/收起效果。

#### 相关知识:
> API: wx.creatAnimation(), 可以创建动画实例（实际效果是动态添加css animation样式来实现)。
> :class="{ 'show' : status }" 动态添加样式。

#### 相关需求:

1. 开关；可以设置初始状态为打开或者关闭。

#### 相关问题:

1. CSS Animation 在没有初始高度时无法做出动画效果，高度会瞬间坍塌。例: 初始状态为打开的Accordion在点击关闭时，无法做出动画效果。
2. Wepy组件只有onLoad()生命周期组件，但是在onLoad时还无法获得相关高度，setTimeout(()=>{}, 50)后可以获得数据。

#### 解决方案: 

> 在动画执行时，确保有初始高度。
API: 读取相关节点信息。
```js
wx.createSelectorQuery().select('#the-id').boundingClientRect(function(rect){
  rect.id      // 节点的ID
  rect.dataset // 节点的dataset
  rect.left    // 节点的左边界坐标
  rect.right   // 节点的右边界坐标
  rect.top     // 节点的上边界坐标
  rect.bottom  // 节点的下边界坐标
  rect.width   // 节点的宽度
  rect.height  // 节点的高度
}).exec()
```

1. 页面加载完成之后读取Accordion高度，动态添加css高度。

2. 在点击Toggle时，读取高度。这种情况下需要先动态添加高度，再添加动画效果。

```js
const status = false
// 在读取到高度之后，status = true，将动画class加入DOM

style="height: {{ animationToggle ? 0 : contentHeight + 'px'}}"
// 动态控制view的高度。
```

获取元素高度API绑定到点击事件，点击Toggle按钮后读取高度，动态添加动画初始高度。因为API异步原因和wepy数据渲染次序，使用this.$apply(),确保数据绑定。
(获取高度后setTimeout()异步一下执行添加动画)
