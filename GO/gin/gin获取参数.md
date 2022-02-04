#  gin获取参数

### 1.获取QueryString 参数
```go
r := gin.Default()
r.GET("/hello", func(c *gin.Context) {
	//获取key 为username的value
    username := c.Query("username")
    //获取key 为address的value,如果value为空，则使用默认值China
	address := c.DefaultQuery("address", "China")

	c.JSON(http.StatusOK, gin.H{
		"username": username,
		"address":  address,
	})

})

r.Run()

```

### 2. 获取form参数
```go
r := gin.Default()
r.POST("/post", func(c *gin.Context) {
    //form 中key为username的value
	username := c.PostForm("username")
	password := c.PostForm("password")
    //key为空则有默认值
	address := c.DefaultPostForm("address", "China")
	c.JSON(http.StatusOK, gin.H{
		"username": username,
		"password": password,
		"address":  address,
	})
	})

r.Run()
```

### 3.获取URI路径参数
```go
r := gin.Default()
r.GET("/search/:username/:address", func(c *gin.Context) {
	username := c.Param("username")
	address := c.Param("address")

	c.JSON(http.StatusOK, gin.H{
		"username": username,
		"address":  address,
	})
})
r.Run()
```