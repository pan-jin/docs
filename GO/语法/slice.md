# slice

### 1. 切片的创建

```go
	//创建切片的方式
	sliceOne := make([]int, 5, 6) //表示创建了一个长度为5，容量为6的切片
	sliceTwo := make([]int, 5)    //表示创建一个长度为5的切片
	var sliceThree []int
	fmt.Println(sliceOne, sliceTwo, sliceThree)

```



### 2.向切片中添加元素

```go
	//切片中添加元素
	sliceThree = append(sliceThree, 3, 4, 5)
	fmt.Println(sliceThree)
```



### 3.遍历切片

```go
	//遍历切片
	// i 是下标 v 是value 
	for i, v := range sliceThree {
		fmt.Println(i, v)
	}
```

