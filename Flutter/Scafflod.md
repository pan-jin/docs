# Scafflod

### 1.AppBar

```dart
appBar: AppBar(
  title: Text('StatelessWidget 及其组件'), //导航栏标题
  leading:Icon(Icons.home), //导航栏左侧按钮 通常为返回按钮
  automaticallyImplyLeading: false,//如果leading为null，是否自动实现默认的leading按钮
  centerTitle: true, //导航栏标题是否居中
  elevation: 5, //导航栏阴影
  // bottom:  // 导航栏底部菜单，通常为Tab按钮组
  actions:<Widget> [
      IconButton(onPressed: (){}, icon: Icon(Icons.share)),
  ],//导航栏右侧菜单
  backgroundColor: Colors.red,//背景色

)
        
```

