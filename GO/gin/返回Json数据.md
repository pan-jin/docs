# 返回Json数据
### 1.使用map返回Json
```go
data := map[string]interface{}{
	"name": "张三",
	"age":  18,
}
//type H map[string]interface{} H本质也是一个Map
data2 := gin.H{
	"name": "李四",
	"age":  33,
}
r := gin.Default()
r.GET("/json", func(c *gin.Context) {
	c.JSON(http.StatusOK, data)
	c.JSON(http.StatusOK, data2)
	})
```

### 2.使用结构体返回Json
```go
type userinfo struct {
    //结构体的属性名称首字母必须大写才可访问
    // 如果希望得到的是小写的可以使用tag来定制
    Name string `json:"name"`
	Age  int    `json:"age"`
}
var zhang = userinfo{"张三", 22}
r.GET("/userinfo", func(c *gin.Context) {
	c.JSON(http.StatusOK, zhang)
})
```