### 1.安装Alamofire

1. 安装Alamofire前需要电脑先安装[CocoaPods](https://cocoapods.org/) 

2. 在终端打开项目的根目录运行`pod init`命令

3. 运行上面命令后会出现一个Podfile文件，打开此文件

4. 在文件中添加Alamofire

    ```shell
      # Uncomment the next line to define a global platform for your project
    	platform :ios, '15.0' 
      target 'AlamofireDemo' do
      # Comment the next line if you don't want to use dynamic frameworks
      use_frameworks!
      # Pods for AlamofireDemo
      # 添加Alamofire版本信息
      pod 'Alamofire', '~> 5.5' 
     	end
    ```

5. 运行`pod install`命令安装Alamofire

6. 使用Xcode切换到后缀名为`.xcworkspace`目录

### 2.Alamofire发送Get请求

```swift
func GetAlamofire(){
  	//请求参数
    let parameters:Parameters = ["key":"value"]
    let url = URL(string: "https://www.baidu.com")!
    AF.request(url, method: .get, parameters: parameters).response { response in
        print(response.data ?? "default data")
        print(response.response ?? "default response")
    }
}
```



### 3.Alamofire发送post 请求

```swift
func PostAlamofire(){
    let parameter: [String:[String:String]] = [
        "张三":["二年级":"上海小学"],
        "李四":["初一":"松江外国语中学"]
 		]
    let url = URL(string: "https://httpbin.org/post")!
    AF.request(url, method: .post, parameters: parameter).response { response in
    print(response.response ?? "no response")
    }
}
```

