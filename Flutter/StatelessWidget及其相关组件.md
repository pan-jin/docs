```dart
import 'package:flutter/material.dart';

void main() {
  runApp( Statelw());
}
//StatelessWidget 及其组件
class Statelw extends StatelessWidget{
  TextStyle textStyle = TextStyle(fontSize: 20);
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'StatelessWidget 及其组件',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: Scaffold(
        appBar: AppBar(
          title: Text('StatelessWidget 及其组件'),
        ),
        body: Container(
          alignment: Alignment.center,
          decoration:const BoxDecoration(color: Colors.white) ,
          child: Column(
            children: <Widget> [
              Text(
                'I am Text',style:textStyle,
              ),
              const Icon(
                Icons.android,
                size: 50,
                color: Colors.red,
              ),
              const CloseButton(),
              const BackButton(),
              const Chip(
                avatar: Icon(Icons.photo),
                  label:Text('StatelessWidget 及其组件'),
              ),
              const Divider(
                height: 10, //容器高度
                indent: 10,//左右间距
                color: Colors.orange,
              ),
              Card(
                margin: EdgeInsets.all(10),
                color: Colors.blue,
                elevation: 5,
                child: Container(
                  padding: EdgeInsets.all(10),
                  child: Text(
                      'I am Card ',
                      style:textStyle,
                  ),
                ),
              ),
              AlertDialog(
                title: Text('HEELO'),
                content: Text("糟老头子坏"),

              )
            ],
          ),
        ),
      ),
    );
  }
  
}
```

