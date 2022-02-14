# BottomNavigationBar



```dart
//item(包含导航的列表) currentIndex(当前导航索引) type(导航类型) onTap(导航点击事件)


import 'package:flutter/material.dart';

class BNDemo extends StatefulWidget{
  @override
  State<StatefulWidget> createState()=>_BNDemo();

}

//导航列表
class _BNDemo extends State<BNDemo> {
  final List<BottomNavigationBarItem>  bottomNavItems = [
    const BottomNavigationBarItem(
      icon: Icon(Icons.home),
      backgroundColor: Colors.deepPurpleAccent,
      label: "首页",
    ),
    const BottomNavigationBarItem(
      icon: Icon(Icons.shopping_cart),
      backgroundColor: Colors.deepPurpleAccent,
      label: "购物车",
    ),
    const BottomNavigationBarItem(
      icon: Icon(Icons.message),
      backgroundColor: Colors.red,
      label: "消息",
    ),
    const BottomNavigationBarItem(
      icon: Icon(Icons.person),
      backgroundColor: Colors.deepPurpleAccent,
      label: "我的",
    ),

  ];

  final  pages = [
    Center(
      child: Text("Home",style: TextStyle(fontSize: 50),),
    ),
    Center(
      child: Text("Message",style: TextStyle(fontSize: 50),),
    ),
    Center(
      child: Text("Shopping",style: TextStyle(fontSize: 50),),
    ),
    Center(
      child: Text("Person",style: TextStyle(fontSize: 50),),
    ),

  ] ;

  late int currentIndex;

  @override
  void initState() {
    // TODO: implement initState
    super.initState();
    currentIndex = 0;
  }


  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('BottomNavigationBar'),
      ),
      bottomNavigationBar: BottomNavigationBar(
        items: bottomNavItems,
        currentIndex: currentIndex,
        type: BottomNavigationBarType.fixed,
        onTap: (index){
          _changePage(index);
        },
      ),
      body: pages[currentIndex],
    );
  }
  void _changePage(int index){
    if( index !=currentIndex){
      setState(() {
        currentIndex = index;
      });
    }
  }
}


```

