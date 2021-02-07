# 错误汇总

## permission denied

需要添加权限 此处 我的权限是需要网络访问权限
在AndroidManifest.xml中添加

```xml
<uses-permission android:name="android.permission.INTERNET" />
```

## 主线程不可运行网络连接相关代码

创建线程进行网络连接

## java.lang.RuntimeException: Can't toast on a thread that has not called Looper.prepare()

[参考解决方案](https://www.jianshu.com/p/86459c23bdf5)

解决方法

方法一：增加Looper.prepare();
```kt
new Thread() {

   public void run() {

    Log.i("log", "run");

    Looper.prepare();

    Toast.makeText(ActivityTestActivity.this, "toast", 1).show();

    Looper.loop();// 进入loop中的循环，查看消息队列

    };
 }.start();
```
方法二：post 给主线程去处理
```kt
mainHandler.post(new Runnable() {

   @Override
   public void run() {
      if (toast == null) {
         toast = Toast.makeText(context, "",     Toast.LENGTH_SHORT);
  }
  toast.setText(msg);
  toast.setDuration(Toast.LENGTH_SHORT);
  toast.show();
   }
});
```

## android.content.ActivityNotFoundException: Unable to find explicit activity class {com.example.myapplication2/com.example.myapplication2.RegisterActivity}; have you declared this activity in your AndroidManifest.xml?

创建新视图，准备跳转时，需要对视图进行声明， 在AndroidManifest.xml中进行声明 

```xml
<activity android:name=".Activityname">
</activity>
```

## 新建视图 发现跳转后的视图为空，可是视图内已添加控件 
考虑创建新视图类 与主页面类创建过程相似，新建类中override时会有两个参数，而主视图内的类仅有一个视图
因此，考虑与主视图创建方法相同
