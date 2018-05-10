### wepy - \<repeat>

- version: 1.7.1

[![CdOb38.md.png](https://s1.ax1x.com/2018/05/09/CdOb38.md.png)](https://imgchr.com/i/CdOb38)

#### 官方解释：

> WePY 1.x 版本中，组件使用的是静态编译组件，即组件是在编译阶段编译进页面的，每个组件都是唯一的一个实例，目前只提供简单的 repeat 支持。不支持在 repeat 的组件中去使用 props, computed, watch 等等特性。

```js
<!-- 错误使用 --->
// list.wpy
<view>{{test.name}}</view>

// index.wpy
<repeat for="{{mylist}}">
   <List :test.sync="item"></List>
</repeat>

<!-- 推荐用法 --->
// list.wpy
<repeat for="{{mylist}}">
    <view>{{item.name}}</view>
</repeat>

// index.wpy
<List :mylist.sync="mylist"></List>
```


> 另外，在 1.7.2-alpha4 的实验版本中提供了对原生组件的支持。*

#### 遇到的问题：

1. repeat中多层嵌套时，里层子组件拿不到item的数据。
2. repeat单层嵌套时，子组件中不好使用onLoad, computed等计算属性，只能读取展示传进来的props（作为组件只会加载一次，onLoad，computed等只会操作数组第一个元素）。
3. repeat单层嵌套时，子组件中调用的子组件无法继续传值，因为props无法使用对象传值。

#### 暂时解决方案：

将文件结构拍扁，子组件不再调用孙子组件，孙子组件代码提升到子组件中操作，直接展示获取到的静态数据。

#### 相关issue

- https://github.com/Tencent/wepy/issues/1218
- https://github.com/Tencent/wepy/issues/1231
- https://github.com/Tencent/wepy/issues/1188

