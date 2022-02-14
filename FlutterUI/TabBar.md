# TabBar

### 1.TabBar源码

```dart
const TabBar({
    Key key,
    @required this.tabs,//必须实现的，设置需要展示的tabs，最少需要两个
    this.controller,
    this.isScrollable = false,//是否需要滚动，true为需要
    this.indicatorColor,//选中下划线的颜色
    this.indicatorWeight = 2.0,//选中下划线的高度，值越大高度越高，默认为2
    this.indicatorPadding = EdgeInsets.zero,
    this.indicator,//用于设定选中状态下的展示样式
    this.indicatorSize,//选中下划线的长度，label时跟文字内容长度一样，tab时跟一个Tab的长度一样
    this.labelColor,//设置选中时的字体颜色，tabs里面的字体样式优先级最高
    this.labelStyle,//设置选中时的字体样式，tabs里面的字体样式优先级最高
    this.labelPadding,
    this.unselectedLabelColor,//设置未选中时的字体颜色，tabs里面的字体样式优先级最高
    this.unselectedLabelStyle,//设置未选中时的字体样式，tabs里面的字体样式优先级最高
    this.dragStartBehavior = DragStartBehavior.start,
    this.onTap,//点击事件
  })

```



### 2. TabBar 基本用法

菜单数组=> 页面数组=>appBar中bottom属性=>body中tabView

```dart
import 'package:flutter/material.dart';

class TabBarDemo extends StatelessWidget{
  //菜单数组
  final List<Widget> _tabs = [
    Tab(text: "首页",icon: Icon(Icons.home)),
    Tab(text: "添加",icon: Icon(Icons.add)),
    Tab(text: "搜索",icon: Icon(Icons.search)),

  ];
  //页面数组
  final List<Widget> _tabViews =[

    Icon(Icons.home,size: 120,color: Colors.red,),
    Icon(Icons.add,size: 120,color: Colors.green,),
    Icon(Icons.search,size: 120,color: Colors.blue,),
    //这里也可使用之前写的widget
    // buttonDemo(),
    // ProgressDemo(),
    // iconDemo()
  ];



  @override
  Widget build(BuildContext context) {
    return DefaultTabController(

        length: _tabs.length, //几个菜单
        child: Scaffold(
          appBar: AppBar(
            title: Text('TabBar'),
            bottom: TabBar(
              tabs: _tabs,
              labelColor: Colors.red,
              unselectedLabelColor: Colors.black, //未选中的tab颜色
              indicatorSize: TabBarIndicatorSize.label, //短线
              indicatorColor: Colors.red,

            ),
          ),
          body: TabBarView(
            children: _tabViews,
          ),
        ),
    );
      
  }
  
}
```

