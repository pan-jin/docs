# Row 水平布局

```dart
Row({
  ...  
  TextDirection textDirection,    
  MainAxisSize mainAxisSize = MainAxisSize.max,    
  MainAxisAlignment mainAxisAlignment = MainAxisAlignment.start,
  VerticalDirection verticalDirection = VerticalDirection.down,  
  CrossAxisAlignment crossAxisAlignment = CrossAxisAlignment.center,
  List<Widget> children = const <Widget>[],
})
```



### 1.Row 和 Column常见属性

1. `crossAxisAlignment` 子组件在纵轴的对齐方式。`crossAxisAlignment`的参考系是`verticalDirection`，即`verticalDirection`值为`VerticalDirection.down`时`crossAxisAlignment.start`指顶部对齐，`verticalDirection`值为`VerticalDirection.up`时，`crossAxisAlignment.start`指底部对齐；而`crossAxisAlignment.end`和`crossAxisAlignment.start`正好相反；

    ```dart
    CrossAxisAlignment.start：子组件在纵轴中顶部对齐。
    CrossAxisAlignment.end：子组件在纵轴中底部对齐。
    CrossAxisAlignment.center：子组件在纵轴中居中对齐。
    CrossAxisAlignment.stretch：纵轴中拉伸填充满父布局。
    CrossAxisAlignment.baseline：在纵轴中会报错。
    ```

2. `mainAxisAlignment`  子组件在主轴的对齐方式

    ```dart
        MainAxisAlignment.start：靠左排列。
        MainAxisAlignment.end：靠右排列。
        MainAxisAlignment.center：居中排列。
        MainAxisAlignment.spaceAround：每个子组件左右间隔相等，也就是 margin 相等。
        MainAxisAlignment.spaceBetween：两端对齐，也就是第一个子组件靠左，最后一个子组件靠右，剩余组件在中间平均分散排列。
        MainAxisAlignment.spaceEvenly：每个子组件平均分散排列，也就是宽度相等。
    ```

3. `mainAxisSize` 主轴大小

    ```dart
    MainAxisSize.max：相当于 Android 的 match_parent。
    MainAxisSize.min：相当于 Android 的 wrap_content。
    ```

4. `textDirection`子组件排列顺序

    ```dart
    TextDirection.ltr：从左往右开始排列。
    TextDirection.rtl：从右往左开始排列。
    ```

5. `verticalDirection` 在纵轴的对齐方向

    ```dart
    VerticalDirection.down :从上到下
    VerticalDirection.up :从下到上
    ```



### 2.Row 案例

1. 子组件平均分散

```dart
body: Row(
        mainAxisAlignment: MainAxisAlignment.spaceEvenly, //每个子组件平均分散排列 也就是宽度相等
        mainAxisSize: MainAxisSize.max, //主轴大小 max相当于Android中的march_parent min相当于android中的wrap_content
        verticalDirection: VerticalDirection.down, 
        crossAxisAlignment: CrossAxisAlignment.start,
        children: [
          Container(
            color: Colors.blue,
            child: Text('蓝色'),
          ),
          Container(
            color: Colors.red,
            child: Text('红色'),
          ),
          Container(
            color: Colors.yellow,
            child: Text('黄色'),
          )
        ],
      ),
```



2. 子组件平分Row的宽度(使用Expanded包裹)

    ```dart
    Row(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly, //每个子组件平均分散排列 也就是宽度相等
            mainAxisSize: MainAxisSize.max, //主轴大小 max相当于Android中的march_parent min相当于android中的wrap_content
            verticalDirection: VerticalDirection.down,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Expanded(
                  child:Container(
                    color: Colors.blue,
                    child: Text('蓝色'),
                  ),
              ),
    
              Expanded(
                child:Container(
                  color: Colors.red,
                  child: Text('红色'),
              ),
              ),
    
              Expanded(
                  child: Container(
                    color: Colors.yellow,
                    child: Text('黄色'),
                  ),
              ),
    
            ],
          ),
    
        );
      }
    
    }
    ```

    ### 3.Column 布局案例

    ```dart
    body: Column(
            mainAxisAlignment: MainAxisAlignment.spaceEvenly, //每个子组件平均分散排列 也就是宽度相等
            mainAxisSize: MainAxisSize.max, //主轴大小 max相当于Android中的march_parent min相当于android中的wrap_content
            verticalDirection: VerticalDirection.down,
            crossAxisAlignment: CrossAxisAlignment.start,
            children: [
              Expanded(
                child:Container(
                  color: Colors.blue,
                  width: double.infinity,
                  child: Text('蓝色'),
    
                ),
              ),
    
              Expanded(
                child:Container(
                  color: Colors.red,
                  width: double.infinity,
                  child: Text('红色'),
                ),
              ),
    
              Expanded(
                child: Container(
                  color: Colors.yellow,
                  width: double.infinity,
                  child: Text('黄色'),
                ),
              ),
            ],
    
          ),
    ```

    