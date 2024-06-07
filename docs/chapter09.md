## 1.线程概念

**相关概念：**

- **程序：** 为了完成特定任务，用某种语言编写的一组指令集合(一组静态代码)
- **进程：** 运行中的程序，系统调度与资源分配的一个**独立单位**，操作系统会 为每个进程分配一段内存空间！程序的依次动态执行，经历代码的加载，执行， 执行完毕的完整过程！
- **线程：** 比进程更小的执行单元，每个进程可能有多条线程，线程需要放在一个 进程中才能执行，线程由程序负责管理，而进程则由系统进行调度！
- **多线程的理解**： 并行执行多个条指令，将**CPU时间片**按照调度算法分配给各个 线程，实际上是分时执行的，只是这个切换的时间很短，用户感觉到"同时"而已！


创建线程的第一种方式：继承`Thread`类
```java
public class MyThread extends Thread {
    @Override
    public void run() {
        super.run();

        try {
            doSomething();
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }

    private void doSomething() throws InterruptedException {
        while (true) {
            sleep(1000);
            Log.d("hou", "doSomething: " + (new Date().toString()));
        }

    }
}

// 调用的时候：
MyThread myThread = new MyThread();
myThread.start();  // 开启一个线程
```

创建线程的第二种方式：实现`Runnable`接口
```java
public class MyRunableThread implements Runnable {
    @Override
    public void run() {
        // TODO...
    }
}

// 用的时候，需要借助 Thread 类
MyRunableThread runableThread = new MyRunableThread();
Thread thread = new Thread(runableThread);
thread.start();
```

实现`Runnable`接口，也可以使用匿名类的方式简写：
```java
// 测试延迟执行
private void testDelay() {
    new Thread(new Runnable() {
        @Override
        public void run() {
            try {
                Thread.sleep(9000);
            } catch (InterruptedException e) {
                throw new RuntimeException(e);
            }
        }
    }).start();
}

// 或者直接 lambda 表达式：
private void testDelay() {
    new Thread(() -> {
        try {
            Thread.sleep(9000);
        } catch (InterruptedException e) {
            throw new RuntimeException(e);
        }
    }).start();
}
```

创建线程的第三种方式：实现`Callable<T>`接口
```java
import java.util.concurrent.Callable;

public class CallableThread implements Callable<String> {

    @Override
    public String call() throws Exception {
        Thread.sleep(3000);
        return "ok";  // 需要有返回结果
    }
}

// 用法
CallableThread callableThread = new CallableThread();
FutureTask<String> task = new FutureTask<String>(callableThread);  // FutureTask为了获取线程返回值
Thread thread = new Thread(task);
thread.start();

try {
    String resultVal = task.get();  // 获取线程执行完的返回值，注意这句代码会阻塞当前线程
    Log.d("hou", "test: " + resultVal);
} catch (ExecutionException | InterruptedException e) {
    e.printStackTrace();
}
```

`Service`与`Thread`线程的区别：

> 其实他们两者并没有太大的关系！ `Thread`是线程，程序执行的最小单元，分配CPU的基本单位！ 而`Service`则是Android提供一个允许长时间留驻后台的一个组件，最常见的 用法就是做轮询操作！或者想在后台做一些事情，比如后台下载更新！ 记得别把这两个概念混淆

## 2.Service生命周期

两种`Service`生命周期的情况：

![Service](./image/0039.jpg)

- `onCreate()`: 当`Service`第一次被创建后立即回调该方法，该方法在整个生命周期 中只会调用一 次!
- `onDestory()`: 当`Service`被关闭时会回调该方法，该方法只会回调一次!
- `onStartCommand(intent,flag,startId)`: 当客户端调用`startService(Intent)`方法时会回调，可多次调用`StartService`方法，但不会再创建新的`Service`对象，而是继续复用前面产生的`Service`对象，但会继续调用复用对象的 `onStartCommand()`方法!
- `onOnbind(intent)`: 该方法是继承`Service`都必须实现的方法，该方法会返回一个 `IBinder` 对象，app通过该对象与`Service`组件进行通信!

> 论启动了多少次`Service`,只需调用一次`StopService`即可停掉`Service`

## 3.Service使用

当继承`Service`后，需要再`AndroidManifest.xml`完成`Service`注册，才能去`StartService()`或`bindService()`
```xml
<!-- 配置Service组件,同时配置一个action -->  
<service android:name="com.demo.TestService">  
    <intent-filter>  
        <action android:name="com.demo.TestService"/>  
    </intent-filter>  
</service>
```

启动`Server`：
```java
// 1.使用Intent去启动一个Server
final Intent intent = new Intent(ServiceActivity.this, MyService1.class);
intent.putExtra("url", "https://xxx.com");  // 可以给server传参数
startService(intent);  // 启动Server
stopService(intent);  // 停止Server
```

使用`BindService`启动`Service`方式：
- Step 1:在自定义的`Service`中继承`Binder`,实现自己的`IBinder`对象
- Step 2:通过`onBind()`方法返回自己的`IBinder`对象
- Step 3:在绑定该`Service`的类中定义一个`ServiceConnection`对象,重写两个方法, `onServiceConnected()`和`onDisconnected()`，然后直接读取`IBinder`传递过来的参数即可!
- 注意：`Binder`类是`Service`与`Activity`进行通信的关键


参考： https://www.runoob.com/w3cnote/android-tutorial-service-1.html

## 4.IntentService

**📢Service有下面2个注意点：**
- 1.`Service`不是一个单独的进程,它和它的应用程序在同一个进程中
- 2.`Service`不是一个线程,这样就意味着我们应该避免在

如果非要在`Service`中进行耗时或异步操作，Android推荐使用`IntentService`。 `IntentService`是继承于`Service`并处理异步请求的一个类，在`IntentService`中有 一个工作线程来处理耗时操作,请求的`Intent`记录会加入队列.

**工作流程：**

> 客户端通过`startService(Intent)`来启动`IntentService`; 我们并不需要手动地区控制`IntentService`,当任务执行完后,`IntentService`会自动停止; 可以启动`IntentService`多次,每个耗时操作会以工作队列的方式在`IntentService`的 `onHandleIntent`回调方法中执行,并且每次只会执行一个工作线程,执行完一，再到二这样!

> IntentService已废弃，替代者为`WorkManager`

https://developer.android.com/topic/libraries/architecture/workmanager

