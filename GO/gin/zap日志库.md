# 使用zap 日志库

### 1. 安装

```go
go get -u go.uber.org/zap
```

### 2. 配置Zap Logger

#### 1. logger

- 通过调用`zap.NewProduction()`/`zap.NewDevelopment()`或者`zap.Example()`创建一个Logger。
- 上面的每一个函数都将创建一个logger。唯一的区别在于它将记录的信息不同。例如production logger默认记录调用函数信息、日期和时间等。
- 通过Logger调用Info/Error等。
- 默认情况下日志都会打印到应用程序的console界面。

```go

import (
	"fmt"
	"go.uber.org/zap"
	"net/http"
)

var logger *zap.Logger


func main() {
	err := initLogger()
	if err != nil {
		fmt.Printf("init logger failed %v",err)
	}

	defer logger.Sync()
	simpleHttpGet("https://panjin.me")
	simpleHttpGet("https://baidu.com")

}

//初始化
func initLogger()(err error){
	logger, err = zap.NewProduction()
	return  err
}

func simpleHttpGet(url string){
	resp, err := http.Get(url)
	if err != nil {
		logger.Error(
			"Error fatching url",
			zap.String("url",url),
			zap.Error(err))
	}else {
    //使用info /error debug等级来记录日志
		logger.Info("success",
			zap.String("statusCode",resp.Status),
			zap.String("url",url))
		resp.Body.Close()
	}
}
```





### 3.将日志写入文件

