# Drawer

### 1.Drawer介绍

`Scaffold`的`drawer`和`endDrawer`属性可以分别接受一个Widget来作为页面的左、右抽屉菜单。如果开发者提供了抽屉菜单，那么当用户手指从屏幕左（或右）侧向里滑动时便可打开抽屉菜单



### 2.使用案例

```dart
class DrawerDemo extends StatelessWidget{
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text('Drawer'),
      ),
      body: Container(
        child: Text('Drawer'),
      ),
    drawer: DrawerList(),
    );
  }
  
}

class DrawerList extends StatelessWidget{
  @override
  Widget build(BuildContext context) {
    return Drawer(
      child: ListView(
        padding: EdgeInsets.all(0),//外边距为0 从而使图片能覆盖整个状态栏
        children:  [
          UserAccountsDrawerHeader(
              accountName: Text('hi',style: TextStyle(color: Colors.red),),
              accountEmail: Text('hello@hello.com',style: TextStyle(color: Colors.red),),
              decoration: BoxDecoration(
              image:DecorationImage(
                  image:AssetImage("images/image.jpeg"),
                  fit: BoxFit.cover, //图片填充
              )
            ),
          currentAccountPicture:CircleAvatar(
            backgroundImage: AssetImage("images/image.jpeg"),

          ) , //当前账号的图片(头像)
          ),
          ListTile(
            leading: Icon(Icons.settings),
            title: Text('设置'),
            trailing: Icon(Icons.arrow_forward_ios),
          ),
          Divider(thickness: 1,),
          ListTile(
            leading: Icon(Icons.account_balance),
            title: Text('余额'),
            trailing: Icon(Icons.arrow_forward_ios),
          ),
          Divider(thickness: 1,),
          ListTile(
            leading: Icon(Icons.person),
            title: Text('我的'),
            trailing: Icon(Icons.arrow_forward_ios),
          ),
          Divider(thickness: 1,),
          ListTile(
            leading: Icon(Icons.backpack),
            title: Text('回退'),
            onTap:() => Navigator.pop(context),
            trailing: Icon(Icons.arrow_forward_ios),
          ),
           const AboutListTile(
            child: Text('关于'),
            applicationName: '张三',
            applicationVersion: "1.0.0",
          )
        ],
      ),
    );
  }
  
}
```



