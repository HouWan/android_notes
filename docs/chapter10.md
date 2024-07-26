> 当前文件是`chapter10.md`

`BroadcastReceiver`是App之间的广播通信机制，Android系统自己在很多时候都会发送广播，比如电量低或者充足，插入耳机等。`BroadcastReceiver`有两种广播类型：

- 标准广播
    - 完全异步执行的广播，发出广播后，所有广播接收器几乎会在同一时刻收到这条广播通知

- 有序广播
    - 同步执行的一种广播，发出广播后，同一时间只有一个广播接收者能收到，当这个广播接收者的逻辑执行完毕后，才会传递到下一个接收者，当然，前面的接收者还可以截断广播的继续传递，那么后续的接收者就无法收到广播信息了。


**一：动态注册**
监听网络状态变化实例：

1.1 自定义`BroadcastReceiver`，在`onReceive()`方法中完成广播要处理的逻辑：
```java
public class MyBRReceiver extends BroadcastReceiver{
    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context,"网络状态发生改变~", Toast.LENGTH_SHORT).show();
    }
}
```

1.2 在Activity中动态注册广播：
```java
@Override
MyBRReceiver myReceiver;

protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    // 监听网络变化的代码：
    myReceiver = new MyBRReceiver();
    IntentFilter itFilter = new IntentFilter("android.net.conn.CONNECTIVITY_CHANGE");
    registerReceiver(myReceiver, itFilter);
}

// 销毁时，取消广播
@Override
protected void onDestroy() {
    super.onDestroy();
    unregisterReceiver(myReceiver);
}
```


