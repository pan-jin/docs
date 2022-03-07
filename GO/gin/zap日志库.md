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

将日志写入问价而不是终端，我们通常使用zap.New(...)来手动配置

```go
package main

//使用zap 日志库

import (

	"go.uber.org/zap"
	"go.uber.org/zap/zapcore"
	"net/http"

    "github.com/natefinch/lumberjack"
)

var logger *zap.Logger


func main() {

	InitLogger()
	defer logger.Sync()
	simpleHttpGet("www.google.com")
	simpleHttpGet("https://baidu.com")


}




func InitLogger(){
	writerSync := getLogWriter()
	encoder := getEncode()

	// 编码 写入文件 以及日志记录等级
	core := zapcore.NewCore(encoder,writerSync,zap.DebugLevel)
	//AddCaller 表示在日志中显示调用方
	logger = zap.New(core,zap.AddCaller())
}


// .Encoder:编码器(如何写入日志)
func getEncode() zapcore.Encoder {
	encoderConfig := zap.NewProductionEncoderConfig()
	//设置写入日志的时间格式
	encoderConfig.EncodeTime = zapcore.ISO8601TimeEncoder

	return zapcore.NewJSONEncoder(encoderConfig)
}


//使用Lumberjack进行日志切割归档
//WriterSyncer 表示把日志写入的文件
func getLogWriter() zapcore.WriteSyncer {
	lumberJackLogger := &lumberjack.Logger{
		Filename:   "./test.log",
		MaxSize:    10, //在进行切割之前，日志文件的最大大小（以MB为单位）
		MaxBackups: 5, //保留的最大文件个数
		MaxAge:     30, //保留旧文件最大天数
		Compress:   false, //是否压缩/归档旧文件
	}
	return zapcore.AddSync(lumberJackLogger)
}


func simpleHttpGet(url string) {
	resp, err := http.Get(url)
	if err != nil {
		logger.Error(
			"Error fetching url..",
			zap.String("url", url),
			zap.Error(err))
	} else {
		logger.Info("Success..",
			zap.String("statusCode", resp.Status),
			zap.String("url", url))
		resp.Body.Close()
	}
}
```





### 4. 在gin中使用 Zap 日志库

```go
package main



import (
	"go.uber.org/zap"
	"go.uber.org/zap/zapcore"
	"net"
	"net/http"
	"net/http/httputil"
	"os"
	"runtime/debug"
	"strings"
	"time"

	"github.com/gin-gonic/gin"
	"github.com/natefinch/lumberjack"
)

var logger *zap.Logger


func main() {

	InitLogger()
	r := gin.New()
	r.Use(GinLogger(logger),GinRecovery(logger,true))


	r.GET("/", func(c *gin.Context) {
		c.JSON(http.StatusOK,gin.H{
			"hell":"hi",
		})
	})


	r.Run(":9090")


}


func InitLogger(){
	writerSync := getLogWriter()
	encoder := getEncode()

	// 编码 写入文件 以及日志记录等级
	core := zapcore.NewCore(encoder,writerSync,zap.DebugLevel)
	//AddCaller 表示在日志中显示调用方
	logger = zap.New(core,zap.AddCaller())
}


// .Encoder:编码器(如何写入日志)
func getEncode() zapcore.Encoder {
	encoderConfig := zap.NewProductionEncoderConfig()
	//设置写入日志的时间格式
	encoderConfig.EncodeTime = zapcore.ISO8601TimeEncoder

	return zapcore.NewJSONEncoder(encoderConfig)
}


//使用Lumberjack进行日志切割归档
//WriterSyncer 表示把日志写入的文件
func getLogWriter() zapcore.WriteSyncer {
	lumberJackLogger := &lumberjack.Logger{
		Filename:   "./zapAndGin.log",
		MaxSize:    10, //在进行切割之前，日志文件的最大大小（以MB为单位）
		MaxBackups: 5, //保留的最大文件个数
		MaxAge:     30, //保留旧文件最大天数
		Compress:   false, //是否压缩/归档旧文件
	}
	return zapcore.AddSync(lumberJackLogger)
}










//GinLogger 接收gin框架默认的日志
func GinLogger(logger *zap.Logger) gin.HandlerFunc {
	return func(c *gin.Context) {
		start := time.Now()
		path := c.Request.URL.Path
		query := c.Request.URL.RawQuery
		c.Next()

		cost := time.Since(start)
		logger.Info(path,
			zap.Int("status", c.Writer.Status()),
			zap.String("method", c.Request.Method),
			zap.String("path", path),
			zap.String("query", query),
			zap.String("ip", c.ClientIP()),
			zap.String("user-agent", c.Request.UserAgent()),
			zap.String("errors", c.Errors.ByType(gin.ErrorTypePrivate).String()),
			zap.Duration("cost", cost),
		)
	}
}

// GinRecovery recover掉项目可能出现的panic
func GinRecovery(logger *zap.Logger, stack bool) gin.HandlerFunc {
	return func(c *gin.Context) {
		defer func() {
			if err := recover(); err != nil {
				// Check for a broken connection, as it is not really a
				// condition that warrants a panic stack trace.
				var brokenPipe bool
				if ne, ok := err.(*net.OpError); ok {
					if se, ok := ne.Err.(*os.SyscallError); ok {
						if strings.Contains(strings.ToLower(se.Error()), "broken pipe") || strings.Contains(strings.ToLower(se.Error()), "connection reset by peer") {
							brokenPipe = true
						}
					}
				}

				httpRequest, _ := httputil.DumpRequest(c.Request, false)
				if brokenPipe {
					logger.Error(c.Request.URL.Path,
						zap.Any("error", err),
						zap.String("request", string(httpRequest)),
					)
					// If the connection is dead, we can't write a status to it.
					c.Error(err.(error)) // nolint: errcheck
					c.Abort()
					return
				}

				if stack {
					logger.Error("[Recovery from panic]",
						zap.Any("error", err),
						zap.String("request", string(httpRequest)),
						zap.String("stack", string(debug.Stack())),
					)
				} else {
					logger.Error("[Recovery from panic]",
						zap.Any("error", err),
						zap.String("request", string(httpRequest)),
					)
				}
				c.AbortWithStatus(http.StatusInternalServerError)
			}
		}()
		c.Next()
	}
}
```

