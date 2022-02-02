# 认识Gin

### 1.安装

1.  下载 Gin

```shell
go get -u github.com/gin-gonic/gin //在项目目录的终端中运行此命令
```

2. 引入代码中

```go
import "github.com/gin-gonic/gin"
```

​    

### 2.使用

```go
func main() {
  //返回默认的路由引擎
	r := gin.Default()
  //Get请求地址并返回json 数据
	r.GET("/hello", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"message": "hello",
		})
	})
  //启动服务 默认为8080端口 如需更改在括号中填入端口号就行如r.Run(":9090")
	r.Run()

}
```

