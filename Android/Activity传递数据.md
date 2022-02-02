# Activity数据传递
### 1. 向下一个Activity 传递数据
```kotlin
but3.setOnClickListener{
    //通过使用Intent进行显示跳转
    val intent:Intent=Intent(this,
    FourthActivity::class.java)
    //将需要传递的数据放入intent key-value形式
    intent.putExtra("key","value")
    startActivity(intent)
}
```
```kotlin
//提取上一个Activity传输过来的数据
val extraData = intent.getStringExtra("key")
Log.d("FourthActivity","======$extraData")
```


### 2. 向上一个Activity 传递数据
```kotlin
//FirstActivity
backbutton.setOnClickListener {
    val intent =Intent(this,FirthActivity::class.java)
    //通过使用startActivityForResult() 方法跳转到需要返回数据的Activity
    startActivityForResult(intent,1)
}
```

```kotlin
//SecondActivity
back.setOnClickListener {
    val intent = Intent()
    intent.putExtra("return_data","hello i am back")
    //通过setResult将数据返回FirstActivity
    setResult(RESULT_OK,intent)
    finish()
}
```

```kotlin
//重写FirstActivity中的onActivityResult()方法读取返回的数据
override fun onActivityResult(requestCode: Int, resultCode: Int, data: Intent?) {
    super.onActivityResult(requestCode, resultCode, data)
    when(requestCode){
        1 ->if (resultCode== RESULT_OK){
        val returnDate = data?.getStringExtra("return_data")
        Log.e("Return","return data is $returnDate")
        }
    }
}

```