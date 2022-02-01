# OkHttp

### 1.Get 请求

1. 配置HTTP请求的基本信息

    ```kotlin
    val client = OkHttpClient.Builder()
        .connectTimeout(10, TimeUnit.SECONDS)//连接超时时间
        .readTimeout(10, TimeUnit.SECONDS) //读取超时
        .writeTimeout(10, TimeUnit.SECONDS) //写入超时
        .build();
    ```

2. Get同步请求

    ```kotlin
    //同步get
    fun get(url:String){
        Thread(Runnable {
            //构造请求体
            val request: Request = Request.Builder().url(url).build()
            //构造请求对象
            val call: Call = client.newCall(request)
            //发起get 同步请求execute
            val response: Response =call.execute()
            val body =response.body?.string()
            Log.e("OKHTTP","getResponse:${body}")
         }).start()
    }
    ```

    

3. Get 异步请求

    ```kotlin
        //异步get
    fun getAsync(url:String){
        //异步不需要手动开启子线程
        val request:Request = Request.Builder().url(url).build()
        //构造请求对象
        val call:Call = client.newCall(request)
        //发起get异步请求
        val response: Unit =call.enqueue(object:Callback{
            override fun onFailure(call: Call, e: IOException) {
                Log.e("getAsyncFiled","$e")
            }
    
            override fun onResponse(call: Call, response: Response) {
                val body =response.body?.string()
                Log.e("getAsyncSuccess","getResponse:${body}")
            }
       })
    }
    ```

    

### 2. Post请求

 1. 同步Post请求

    ```kotlin
    //post同步 表单
    fun postSync(url: String){
        val body=FormBody.Builder().add("Username","lisi").add("Password","123456").build();
    		val request = Request.Builder().url(url).post(body).build()
        val call = client.newCall(request)
    		Thread(Runnable {
            val response = call.execute()
            Log.e("post","post response:${response.body?.string()}")
        }).start()
    }
    ```

    

 2. 异步Post请求

    ```kotlin
    fun postAsync(url: String){
        val body=FormBody.Builder().add("Username","lisi").add("Password","123456").build();
    		val request = Request.Builder().url(url).post(body).build()
    		val call = client.newCall(request)
    		//异步请求没有返回值
        call.enqueue(object:Callback{
           override fun onFailure(call: Call, e: IOException) {
               Log.e("postAsyncfaied","post response:${e}")
            }
    				override fun onResponse(call: Call, response: Response) {
               Log.e("postSyncSuccess","post response:${response.body?.string()}")
            }
         })
    
    }
    
    ```

    