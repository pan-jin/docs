# gin 参数绑定

### 1.querystring参数绑定

```go
import (
	"fmt"
	"github.com/gin-gonic/gin"
	"net/http"
)

type UserInfo struct {
	Username string `form:"username" json:"username"`
	Password string `form:"password" json:"password"`
}

func main() {
	//	参数绑定

	r := gin.Default()

	r.GET("/user", func(c *gin.Context) {
		var u UserInfo
		err := c.ShouldBind(&u) //使用shouldBind进行自动绑定 出入结构体的地址
		if err != nil {
			c.JSON(http.StatusBadRequest, gin.H{
				"err": err.Error(),
			})
		} else {
			fmt.Println(u)
			c.JSON(http.StatusOK, gin.H{
				"message": "ok",
			})

		}

	})

	r.Run()

}

```



### 2.form 参数绑定

```go
	r.POST("/post", func(c *gin.Context) {
		var u UserInfo
		err := c.ShouldBind(&u)
		if err != nil {
			c.JSON(http.StatusBadRequest, gin.H{
				"err": err.Error(),
			})
		} else {
			fmt.Println(u)
			c.JSON(http.StatusOK, gin.H{
				"message": "ok",
			})

		}

	})
```



### 3.Json参数绑定

```go
	r.POST("/json", func(c *gin.Context) {
		var u UserInfo
		err := c.ShouldBind(&u)
		if err != nil {
			c.JSON(http.StatusBadRequest, gin.H{
				"err": err.Error(),
			})
		} else {
			fmt.Println(u)
			c.JSON(http.StatusOK, gin.H{
				"message": "ok",
			})

		}

	})
```

