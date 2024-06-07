## 1.RadioGroup

`RadioGroup`单选按钮，继承于`LinearLayout`，里面需要使用`RadioButton`实现单选效果，`RadioButton`继承关系是：`RadioButton` -> `CompoundButton` -> `Button`。

| `RadioButton`属性 | 属性含义 |
| --- | --- |
| android:checked | 是否选中，`true`为选中，`false`为没有选中 |
| android:enabled | 是否可用，`true`为可用，`false`为不可用 |

代码：
```xml
<RadioGroup
    android:id="@+id/choose_sex"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:orientation="horizontal" >
    <RadioButton
        android:id="@+id/sex_man"
        android:layout_width="100dp"
        android:layout_height="48dp"
        android:checked="true"
        android:text="男"
        android:textSize="30sp" />
    <RadioButton
        android:id="@+id/sex_woman"
        android:layout_width="100dp"
        android:layout_height="48dp"
        android:checked="false"
        android:text="女"
        android:textSize="30sp" />
</RadioGroup>
```

> `RadioButton`必须指定`android:id`，不然不会有单选效果

`RadioGroup`的选中事件：
```java
import android.widget.RadioGroup;
import android.widget.Toast;

RadioGroup radioView = (RadioGroup)findViewById(R.id.choose_sex);
radioView.setOnCheckedChangeListener(new RadioGroup.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(RadioGroup group, int checkedId) {
        if (checkedId == R.id.sex_man) {
            Toast.makeText(getApplicationContext(), "选中：男", Toast.LENGTH_SHORT).show();
        } else if (checkedId == R.id.sex_woman) {
            Toast.makeText(getApplicationContext(), "选中：女", Toast.LENGTH_SHORT).show();
        }
    }
});
```

在其他方法获取当前选择的按钮是：
```java
private void test() {
    RadioGroup radioView = (RadioGroup)findViewById(R.id.choose_sex);
    for (int i = 0; i < radioView.getChildCount(); i++) {
        RadioButton btn = (RadioButton)radioView.getChildAt(i);
        if (btn.isChecked()) {
            Toast.makeText(getApplicationContext(), "选中："+ btn.getText(), Toast.LENGTH_SHORT).show();
        }
    }
}
```

## 2.CheckBox
`CheckBox`是复选框，继承关系是`CheckBox` -> `CompoundButton` -> `Button`
```xml
<CheckBox
    android:id="@+id/sg_xiangjiao"
    android:layout_width="wrap_content"
    android:layout_height="50dp"
    android:text="香蕉"
    android:textSize="30sp" />
<CheckBox
    android:id="@+id/sg_pingguo"
    android:layout_width="wrap_content"
    android:layout_height="50dp"
    android:paddingStart="40dp"  <!-- 注意这里增加了文字和icon的间距 -->
    android:text="苹果"
    android:textSize="30sp" />
<CheckBox
    android:id="@+id/sg_xigua"
    android:layout_width="wrap_content"
    android:layout_height="50dp"
    android:button="@mipmap/dm_white_persion"  <!-- 注意这里使用自定义的icon -->
    android:text="西瓜"
    android:textSize="30sp" />
```

`CheckBox`的事件是：
```java
CheckBox box = findViewById(R.id.sg_pingguo);
box.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
        // TODO...
    }
});

box.isChecked();  // 判断是否选中了
```

修改文字和选择框icon的距离：
```css
android:background="@null"  // 有的版本需要加固定这句，有的不需要
android:paddingStart="40dp"
```

自定义`CheckBox`的点击效果：

1.在`res/drawable/`文件夹新建一个`selector`，例如叫`cus_checkbox.xml`：
```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item
        android:state_enabled="true"
        android:state_checked="true"
        android:drawable="@mipmap/checked"/>  <!-- 这里是选择自定义效果的icon -->
    <item
        android:state_enabled="true"
        android:state_checked="false"
        android:drawable="@mipmap/uncheck" />
</selector>
```

下面有2种方案使用这个自定义的`selector`。

①是使用`CheckBox`的`android:button`属性：
```xml
android:button="@drawable/cus_checkbox"
```

②是在`res/values/themes.xml`中新定义一个属性：
```xml
<style name="MyCheckBox" parent="@android:style/Widget.CompoundButton.CheckBox">
    <item name="android:button">@drawable/cus_checkbox</item>
</style>
```

然后在 CheckBox 的布局中这么使用： 
```xml
<CheckBox
    style="@style/MyCheckBox"
    ...
/>
```

## 3.ToggleButton和Switch

`ToggleButton`两个状态的按钮，继承关系是`ToggleButton` -> `CompoundButton` -> `Button`。常用属性如下：

| 属性 | 属性含义 |
| --- | --- |
| android:textOff | 按钮关闭时显示的文字 |
| android:textOn | 按钮打开时的文字，也可以自己写个`selector`，然后设置`background`属性即可 |

`Switch`开关，继承关系是`Switch` -> `CompoundButton` -> `Button`。常用属性如下：

| 属性 | 属性含义 |
| --- | --- |
| android:showText | 设置on/off的时候是否显示文字,boolean |
| android:splitTrack | 是否设置一个间隙，让滑块与底部图片分隔,boolean |
| android:switchMinWidth | 设置开关的最小宽度 |
| android:switchPadding | 设置滑块内文字的间隔 |
| android:track | 底部的图片（也是使用`selector`） |
| android:thumb | 滑块的图片 |

`Switch`有时会使用`androidx.appcompat.widget.SwitchCompat`代替使用。其事件是：

```java
Switch sw = findViewById(R.id.hou_switch);
sw.setOnCheckedChangeListener(new CompoundButton.OnCheckedChangeListener() {
    @Override
    public void onCheckedChanged(CompoundButton buttonView, boolean isChecked) {
        // TODO...
    }
});
```

## 4.ProgressBar

`ProgressBar`进度条，父类是`View`，常用属性如下：

```html
android:max:进度条的最大值
android:progress:进度条已完成进度值 
android:progressDrawable:设置轨道对应的Drawable对象 
android:indeterminate:如果设置成true，则进度条不精确显示进度 
android:indeterminateDrawable:设置不显示进度的进度条的Drawable对象 
android:indeterminateDuration:设置不精确显示进度的持续时间 
android:secondaryProgress:二级进度条，类似于视频播放的一条是当前播放进度，一条是 缓冲进度，前者通过progress属性进行设置!
```

常用的方法：
```java
getMax()  // 返回进度条的最大值
getProgress()  // 返回当前进度
setProgress(int)  // 设置当前进度
```

代码示例：
```xml
<!-- 默认是圆形效果 -->
<ProgressBar
    android:layout_width="wrap_content"
    android:layout_height="wrap_content" />

<!-- 设置成水平进度样式 -->
<ProgressBar
    android:id="@+id/hou_probar"
    style="@style/Widget.AppCompat.ProgressBar.Horizontal"
    android:max="100"
    android:progress="0"
    android:layout_width="200dp"
    android:layout_height="wrap_content" />
```

代码每秒更新一次进度：
```java
protected void onStart() {
    super.onStart();

    CountDownTimer timer = new CountDownTimer(60000, 1000) {
        @Override
        public void onTick(long millisUntilFinished) {
            // proBar 是 ProgressBar
            proBar.setProgress((int) (millisUntilFinished / 1000));
        }

        @Override
        public void onFinish() {
            Log.d("hou", "onFinish");
        }
    };
    timer.start();
}
```

**自定义进度条**的例子：
```java
package xxxxx;

import android.view.View;
import android.content.Context;
import android.graphics.Canvas;
import android.graphics.Color;
import android.graphics.Paint;
import android.graphics.RectF;
import android.graphics.drawable.GradientDrawable;
import android.util.AttributeSet;

public class HouProgressBar extends View {
    private Paint paint;
    private int progress;
    private int maxProgress = 100;

    public HouProgressBar(Context context) {
        super(context);
    }
    public HouProgressBar(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }
    public HouProgressBar(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init();
    }

    private void init() {
        paint = new Paint();
        paint.setColor(Color.RED);
        paint.setStyle(Paint.Style.FILL);

        // 设置圆角、背景色等
        GradientDrawable gradientDrawable = new GradientDrawable();
        gradientDrawable.setColor(Color.GREEN);
        setBackground(gradientDrawable);
    }

    @Override
    protected void onSizeChanged(int w, int h, int oldw, int oldh) {
        super.onSizeChanged(w, h, oldw, oldh);
        if (getBackground() != null && getBackground() instanceof GradientDrawable) {
            GradientDrawable gradientDrawable = (GradientDrawable) getBackground();
            float radius = (float) (getHeight() * 0.5);
            gradientDrawable.setCornerRadius(radius);
        }
    }

    public void setProgress(int progress) {
        this.progress = progress;
        invalidate(); // Redraw the view
    }

    public void setMaxProgress(int maxProgress) {
        this.maxProgress = maxProgress;
    }

    @Override
    protected void onDraw(Canvas canvas) {
        super.onDraw(canvas);
        // Calculate the width of the progress bar
        int progressWidth = (int) ((getWidth() / (float) maxProgress) * progress);

        // 绘制矩形
//        canvas.drawRect(0, 0, progressWidth, getHeight(), paint);

        float radius = (float) (getHeight() * 0.5);  // 计算圆角的半径
        // 绘制圆角矩形（只是演示，不要在draw方法new对象）
        RectF f = new RectF(0, 0, progressWidth, getHeight());
        canvas.drawRoundRect(f, radius, radius, paint);
    }
}
```

## 5.SeekBar

`SeekBar`滑动条，继承关系是`SeekBar`->`AbsSeekBar`->`ProgressBar`->`View`。基本的使用方法是：
```xml
<SeekBar
    android:layout_width="match_parent"
    android:layout_height="30dp"
    android:max="100"  <!-- 进度最大值 -->
    android:progress="30" <!-- 进度当前值 --> />
```

`SeekBar`有3个事件监听方法：
```java
SeekBar seekBar = findViewById(R.id.myseek);
seekBar.setOnSeekBarChangeListener(new SeekBar.OnSeekBarChangeListener() {
    @Override
    public void onProgressChanged(SeekBar seekBar, int progress, boolean fromUser) {
        // 进度发生改变时回调
    }
    @Override
    public void onStartTrackingTouch(SeekBar seekBar) {
        // 按住 SeekBar 时触发
    }
    @Override
    public void onStopTrackingTouch(SeekBar seekBar) {
        // 放开 SeekBar 时触发
    }
});
```

SeekBar定制：2部分，1是滑块触摸的状态thumb，2是滑块背景

①.滑块状态Drawable: `res/drawable/seekbar_thumb.xml`：
```xml
<?xml version="1.0" encoding="utf-8"?>
<selector xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:drawable="@mipmap/seekbar_thumb_normal" android:state_pressed="false" />
    <item android:drawable="@mipmap/seekbar_thumb_pressed" android:state_pressed="true" />
</selector>
```

②.滑块背景的Drawabel：`res/drawable/seekbar_bg.xml`
```xml
<?xml version="1.0" encoding="utf-8"?>
<layer-list xmlns:android="http://schemas.android.com/apk/res/android">
    <item android:id="@android:id/background">
        <shape>
            <solid android:color="#AF7272" />
            <corners android:radius="2dp" />
        </shape>
    </item>
    <item android:id="@android:id/progress">
        <clip>
            <shape>
                <solid android:color="#42A5F5" />
                <corners android:radius="2dp" />
            </shape>
        </clip>
    </item>
</layer-list>
```

③.在布局中使用即可：
```xml
<SeekBar
    android:thumb="@drawable/seekbar_thumb"
    android:progressDrawable="@drawable/seekbar_bg"/>
```

## 6.ScrollView
`ScrollView`继承关系是：`ScrollView`->`FrameLayout`->`ViewGroup`；所以`ScrollView`的本质是一个`FrameLayout`。两个关键的滑动API是：
- `fullScroll(int)`将列表滚到顶部或底部：`ScrollView.FOCUS_UP`表示滚到顶部；`ScrollView.FOCUS_DOWN`表示滚到底部。
- `scrollTo(int)`将列表滚到指定位置，参数为x或y，分别表示横纵坐标值。
- 在`xml`中，设置`android:scrollbars:="none"`可以隐藏滑块指示器。


> 🌹注意：`ScrollView`控件中只能包含一个`View`或一个`ViewGroup`。

> `ScrollView`和`ListView`之类的嵌套使用时会有滑动冲突。不到不得已不要使用。

> `ScrollView`只支持竖直滑动，水平滑动使用`HorizontalScrollView`。

其他属性参考文章：https://blog.csdn.net/qq_27489007/article/details/126938423




















颜色
https://www.toolhelper.cn/Color/RGBToHex