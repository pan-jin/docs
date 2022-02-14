# 流式布局Wrap

```dart
Wrap({
  ...
  this.direction = Axis.horizontal, //布局方向默认水平
  this.alignment = WrapAlignment.start,
  this.spacing = 0.0, //主轴方向上间距
  this.runAlignment = WrapAlignment.start, 
  this.runSpacing = 0.0,  //纵轴方向上间距
  this.crossAxisAlignment = WrapCrossAlignment.start, //子组件在纵轴对齐方式
  this.textDirection, //子组件排列顺序
  this.verticalDirection = VerticalDirection.down,//纵轴上对齐方向
  List<Widget> children = const <Widget>[],
})
```



### 案列

```dart
Wrap(
        spacing: 8.0, //主轴方向上间距
        runSpacing: 4.0, //纵轴方向上间距
        alignment: WrapAlignment.center,//沿主轴方向居中
        children: [
          Container(
            color: Colors.red,
            width: 100,
            height: 100,
            child: Text('红色'),
          ),
          Container(
            color: Colors.yellow,
            width: 100,
            height: 100,
            child: Text('黄色'),
          ),
          Container(
            color: Colors.green,
            width: 100,
            height: 100,
            child: Text('绿色'),
          ),
          Container(
            color: Colors.orange,
            width: 100,
            height: 100,
            child: Text('绿色'),
          ),
          Container(
            color: Colors.grey,
            width: 100,
            height: 100,
            child: Text('灰色'),
          ),
        ],
      ),
```

