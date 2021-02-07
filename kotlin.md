# Kotlin语法

## 开启线程

```kotlin
thread {
    // 线程代码
}
```

## 调试输出

```kotlin
System.out.println("接收到："+message)
```

## json格式

```kt
val msg = "{\"account\":\"xxx\",\"passwd\":\"xxx\"}"
```

## JSONArray to Arraylist

直接读取JSONArray里的内容再写入Arraylist

## 页面切换

```kt
val intent = Intent(this@MainActivity, ChattingActivity::class.java)
startActivity(intent)
```