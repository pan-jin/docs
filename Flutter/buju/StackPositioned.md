# 层叠布局 Stack、Positioned

Stack 和 Positioned 用于实现组件的定位，`Stack`允许子组件堆叠，而`Positioned`用于根据`Stack`的四个角来确定子组件的位置。

### 1.Stack

```dart
Stack({
  this.alignment = AlignmentDirectional.topStart, //alignment 用于去对齐没有定位或者部分定位的子组件
  this.textDirection, //用于确定alignment对齐的参考系
  this.fit = StackFit.loose, //用于确定没有定位的子组件如何适应Stack的大小 StackFit.loose表示使用子组件的大小，StackFit.expand表示扩伸到Stack的大小。
  this.overflow = Overflow.clip, //overflow：此属性决定如何显示超出Stack显示空间的子组件；值为Overflow.clip时，超出部分会被剪裁（隐藏），值为Overflow.visible 时则不会。

  List<Widget> children = const <Widget>[],
})
```





### 2.Positioned

```dart
const Positioned({
  Key? key,
  this.left, 
  this.top,
  this.right,
  this.bottom,
  this.width,
  this.height,
  required Widget child,
})
```

`left`、`top` 、`right`、 `bottom`分别代表离`Stack`左、上、右、底四边的距离。`width`和`height`用于指定需要定位元素的宽度和高度。注意，`Positioned`的`width`、`height` 和其它地方的意义稍微有点区别，此处用于配合`left`、`top` 、`right`、 `bottom`来定位组件，举个例子，在水平方向时，你只能指定`left`、`right`、`width`三个属性中的两个，如指定`left`和`width`后，`right`会自动算出(`left`+`width`)，如果同时指定三个属性则会报错，垂直方向同理。

