# AppBar

### 1.AppBar介绍

`AppBar`是一个Material风格的导航栏，通过它可以设置导航栏标题、导航栏菜单、导航栏底部的Tab标题等.

```dart
AppBar({
  Key? key,
  this.leading, //导航栏最左侧Widget，常见为抽屉菜单按钮或返回按钮。
  this.automaticallyImplyLeading = true, //如果leading为null，是否自动实现默认的leading按钮
  this.title,// 页面标题
  this.actions, // 导航栏右侧菜单
  this.bottom, // 导航栏底部菜单，通常为Tab按钮组
  this.elevation = 4.0, // 导航栏阴影
  this.centerTitle, //标题是否居中 
  this.backgroundColor,
  ...   //其它属性见源码注释
})
```



### 2. AppBar 基本用法

```dart
 AppBar(
        title: Text('APPBar的基本使用方法'),
        //centerTitle 标题是否居中
        centerTitle: false,
        //leading 导航栏左侧widget 常用为抽屉菜单或返回按钮
        leading: IconButton(
           icon: Icon(Icons.add),
           onPressed: () {
             print("leadding");
           },
        ),
        //actions 导航栏右侧菜单
        actions: [
          IconButton(onPressed: (){print("hi actions one");}, icon: Icon(Icons.add)),
          IconButton(onPressed: (){print("hi actions two");}, icon: Icon(Icons.person))
        ],
        //elevation 导航栏阴影
        elevation: 4.0,
      ),
```

