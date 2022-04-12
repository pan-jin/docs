# SwiftUI中使用原生网络请求

### 1.Get 请求

```swift
func getRequestDemo(){
   //如有请求参数加在URL后
   let url = URL(string: "https://httpbin.org/get")!
//        创建URLSession
   let task = URLSession.shared.dataTask(with: url) { data, response, error in
        if let error = error {
           DispatchQueue.main.async {
                // 主线程中输出错误信息
                self.info =  error.localizedDescription
            } 
                return
            }
            guard let data = data else {
                DispatchQueue.main.async {
                    self.info = "data error"
                }
                
                return
            }
            
            guard let response = response else {
                DispatchQueue.main.async {
                    self.info = "response error"
                }
                
                return
            }
            print(data)
            print(response)
        }
        
        task.resume()
        
    }}
```



### 2.Post请求

```swift
func postRequestDemo(){
        let url = URL(string: "https://httpbin.org/post")!
        var request = URLRequest(url: url)
        //设置请求超时时间
        request.timeoutInterval = 15
        //设置请求方法
        request.httpMethod =  "POST"
        //设置请求头
        request.addValue("application/json", forHTTPHeaderField: "Content-Type")
        
        let dict = ["key":"value"]
        //将字典转化为JSON
        let data = try! JSONSerialization.data(withJSONObject: dict, options: .prettyPrinted)
        //设置请求体
        request.httpBody = data
        
       let postTask = URLSession.shared.dataTask(with: request) { data, response, error in
           if let error = error {
               DispatchQueue.main.async {
                   self.postInfo = error.localizedDescription
               }
               return
           }
           
          guard let httpPostResponse = response as? HTTPURLResponse,
                httpPostResponse.statusCode == 200 else{
              DispatchQueue.main.async {
                  self.postInfo = "response error"
              }
              return
          }
           guard let data = data else {
               DispatchQueue.main.async {
                   self.postInfo = "data error"
               }
               return
           }
           print(data)
           print(httpPostResponse)
        }
        postTask.resume()
    }
}
```

