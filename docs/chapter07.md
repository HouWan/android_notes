> 当前文件是`chapter07.md`


## 1.ExpandableListView
`ExpandableListView`是可折叠列表，类似QQ分组通讯录，用到再补全资料。

## 2.Toast
`Toast`只能在UI线程当中使用，在非UI线程使用会抛异常；使用`Toast`时最好定义一个全局的 `Toast` 对象，这样可以避免连续显示
`Toast` 时不能取消上一次 `Toast` 消息的情况。
(如果你有连续弹出 `Toast` 的情况，避免使用`Toast.makeText`)。
取消`Toast`的方法为`toast.cancel();`

①基本的使用方法：
```java
// 在中也可以将 getApplicationContext() 换成 this
Toast toast = Toast.makeText(getApplicationContext(), "你好啊", Toast.LENGTH_SHORT);
toast.show();

// 两个时长枚举：Toast.LENGTH_SHORT、Toast.LENGTH_LONG
```

②调整Toast位置代码，在新版SDK已被废弃。
```java
Toast toast = Toast.makeText(getApplicationContext(), "你好啊", Toast.LENGTH_SHORT);
toast.setGravity(Gravity.CENTER, 0, 0);  // 废弃了，无效果
toast.show();
```

③参考资料：
https://juejin.cn/post/6844904144424140808


④有个`Snackbar`控件，也可以当提示信息，用法如下：
```java
import com.google.android.material.snackbar.Snackbar;

Snackbar snackbar = Snackbar.make(view, "我是内容", 2000);
snackbar.show();
```

## 3.AlertDialog
`AlertDialog`弹框提示或操作控件，使用方法基本如下步骤：

- 1:创建`AlertDialog.Builder`对象;
- 2:调用`setIcon()`设置图标，`setTitle()`或`setCustomTitle()`设置标题;
- 3:设置对话框的内容:`setMessage()`或其他方式的内容;
- 4:调用`setPositive`/`Negative`/`NeutralButton()`设置:确定，取消，中立按钮;
- 5:调用`create()`方法创建这个对象，再调用`show()`方法将对话框显示出来;

> 这里使用了 **建造者模式(Builder Pattern)**，该模式旨在通过提供一种灵活的方式来构建复杂对象。该模式将对象的构建过程与其表示分离，使得同样的构建过程可以创建不同的表示。

> 具体来说 构建者模式(翻译名而已)使用一个独立的构建器(`Bulider`)类来封装对象的构建过程。构建器类提供一些列方法来设置对象的属性，并最终返回构建好的对象。


①：`AlertDialog`基本的使用：
```java
AlertDialog.Builder builder = new AlertDialog.Builder(this);  // this 是 Context
builder.setIcon(R.drawable.three_dog)
        .setTitle("我是标题")
        .setMessage("我是内容，并且我还可以换行\n我是换行的内容")
        .setNegativeButton("取消", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
            }
        })
        .setPositiveButton("确定", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
            }
        })
        .setNeutralButton("中立", new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
            }
        });
AlertDialog alertView = builder.create();
alertView.show();
```

②：上面弹出`AlertDialog`之后，点击背景默认是会关闭的，想要不关闭，可以：
```java
AlertDialog.Builder builder = new AlertDialog.Builder(this); 
builder.setCancelable(false);
```

③：单击列表关闭的样式：
```java
final String[] lesson = new String[]{"语文", "数学", "物理"};
AlertDialog.Builder builder = new AlertDialog.Builder(this);
builder.setIcon(R.drawable.three_dog)
        .setTitle("我是标题")
        .setItems(lesson, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
                // 这里的which是索引值
            }
        })
        .setNegativeButton("取消", null);
AlertDialog alertView = builder.create();
alertView.show();
```

④：单选样式，选中一个之后，点击[确定]按钮关闭弹框：
```java
final String[] menu = new String[]{"麻婆豆腐", "番茄炒鸡蛋", "咖喱鸡"};
AlertDialog.Builder builder = new AlertDialog.Builder(this);
builder.setIcon(R.drawable.three_dog)
        .setTitle("我是标题")
        .setSingleChoiceItems(menu, 1, new DialogInterface.OnClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which) {
            }
        })
        .setNegativeButton("取消", null)
        .setPositiveButton("确定", null);
AlertDialog alertView = builder.create();
alertView.show();
```

⑤：多选样式：
```java
String[] choices = new String[]{"苹果", "西瓜", "荔枝"};
boolean[] checked = new boolean[]{false, true, false};  // 默认是否选中
AlertDialog dialog = new AlertDialog.Builder(this)
        .setTitle("我是标题")
        .setMultiChoiceItems(choices, checked, new DialogInterface.OnMultiChoiceClickListener() {
            @Override
            public void onClick(DialogInterface dialog, int which, boolean isChecked) {
            }
        })
        .setPositiveButton("确定", null)
        .create();
dialog.show();
```

⑥：使用`setView`方法，参数可以是`view`对象或者`布局文件的资源Id`。但是注意这个方法要求`API level21`以上。
```java
AlertDialog dialog = new AlertDialog.Builder(this)
        .setView(R.layout.list_cell)  // 这里自定义view
        .create();
dialog.show();
```

上面👆🏻这么完全自定义的话，关闭弹框可以这么写：
```java
// R.layout.custom_alert_view 自定义的弹框布局
LayoutInflater inflater = getLayoutInflater();
View dialogView = inflater.inflate(R.layout.custom_alert_view, null);
// 布局里面有个按钮
Button okBtn = dialogView.findViewById(R.id.cus_alert_ok_btn);

AlertDialog dialog = new AlertDialog.Builder(this, R.drawable.view_radius)
        .setView(dialogView)
        .create();
dialog.show();

okBtn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        dialog.dismiss();  // 关闭弹框
    }
});
```


⑦：修改`AlertDialog`的背景色、圆角等：

1.创建一个自定义的样式文件（例如：`custom_alert_dialog_style.xml`），并在其中定义`AlertDialog`的背景样式。可以使用颜色、图片或者自定义的`Drawable`作为背景：
```xml
<?xml version="1.0" encoding="utf-8"?>
<resources>
    <style name="CustomAlertDialogStyle" parent="Theme.AppCompat.Light.Dialog.Alert">
        <item name="android:background">@drawable/custom_dialog_background</item>
    <item name="android:windowBackground">@color/color_tou</item>
    </style>
</resources>
```

2.创建一个自定义的`Drawable`文件（例如：`custom_dialog_background.xml`），并在其中定义`AlertDialog`的背景样式。可以使用颜色、形状、渐变等来自定义背景。
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android">
    <solid android:color="#FFFFFF" /> <!-- 设置背景颜色 -->
    <corners android:radius="20dp" /> <!-- 设置圆角 -->
</shape>
```

3.并将样式应用到对话框上：
```java
// 这里第2个参数传入了样式
AlertDialog.Builder builder = new AlertDialog.Builder(context, R.style.CustomAlertDialogStyle);
...
```

> 这里的样式设置之后，其实有点怪，不过完全自定义View时候，可以通过这个方式改变`AlertDialog`默认的背景色。

## 4.PopupWindow
`PopupWindow`是弹出窗口控件，它可以在屏幕上面或下方弹出一个类似浮层的视图。常用于实现一些弹出式菜单、提示信息、弹出式交互界面等。

通过`PopupWindow`，可以在当前Activity的顶层展示一个新的视图，并且可以设置其位置、大小、动画等。还可以为`PopupWindow`设置背景、边框、点击外部区域消失等属性。

`AlertDialog`和`PopupWindow`两者的最主要区别也有以下两点：

- 1 、位置是否固定。 `AlertDialog`在位置显示上是固定的，而`PopupWindow`则相对比较随意，能够在主屏幕上的任意位置显示。
- 2、是否会阻塞UI线程。 `AlertDialog`在显示的时候不会阻塞UI线程，而`PopupWindow`在显示的时候会阻塞UI线程。

代码：
1.自定义个一个弹框xml
2.在代码中初始化`PopupWindow`:
```java
// popup_tips_view 为自定义的弹框布局
View contentView = LayoutInflater.from(this).inflate(R.layout.popup_tips_view, null, false);
// 自定义布局里面的按钮
Button btn = contentView.findViewById(R.id.pop_tips_view_btn);

// 1.构造一个 PopupWindow，参数依次为 加载view、宽、高
PopupWindow popupWindow = new PopupWindow(contentView, ViewGroup.LayoutParams.WRAP_CONTENT, ViewGroup.LayoutParams.WRAP_CONTENT, true);

popupWindow.setAnimationStyle(R.anim.anim_popup_tips);  // 没有生效

btn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View v) {
        popupWindow.dismiss();
    }
});

// 设置透明背景、点击外部可以关闭
popupWindow.setBackgroundDrawable(new ColorDrawable(Color.TRANSPARENT));
popupWindow.setOutsideTouchable(true);

// 这是显示在当前Activity中央的示例
View anchorView = getWindow().getDecorView().getRootView();
popupWindow.showAtLocation(anchorView, Gravity.CENTER, 0, 0);

// 这里显示在锚点view下面的示例：
View anchorView = findViewById(R.id.main_test_button);
popupWindow.showAsDropDown(anchorView, 0, 0);
```

> 获取当前Activity的view
> https://blog.51cto.com/u_16175523/9099313



颜色
https://www.toolhelper.cn/Color/RGBToHex