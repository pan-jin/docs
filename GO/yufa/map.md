# Map

### 1.创建map

```go
	//map的创建方式
	//1.通过使用map字面量创建
	var mapOne = map[int]string{
		0: "张三",
		1: "李四",
		2: "王五",
	}
	//2.使用make 关键字创建
	mapTwo := make(map[int]string)
	mapTwo[1] = "one"
	mapTwo[2] = "two"
	//3.使用map 创建空map
	mapThree := map[int]string{}

	fmt.Println(mapOne, mapTwo, mapThree)
```



### 2.遍历map

```go
	//map的遍历
	fmt.Println("遍历map")
	for i, v := range mapOne {
		fmt.Printf("i=%d,v=%v", i, v)
	}
```



### 3.检查map中是否存在某个元素

```go
	//检查map中是否存在某个元素
	fmt.Println("检查map中是否存在某个元素")
	i, ok := mapOne[2]
	if !ok {
		fmt.Println(i, ok)
	}
```



### 4.删除map中的某个元素

```go
	//删除map中的某个元素
	fmt.Println("删除map中的某个元素")
	delete(mapOne, 1)
	fmt.Println(mapOne)
```



