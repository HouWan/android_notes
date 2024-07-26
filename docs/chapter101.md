> 当前文件是`chapter101.md`


## 1.Android Shape

在Android开发中，我们可以使用`shape`定义各种各样的形状，也可以定义一些图片资源。相对于传统
图片来说，使用`shape`可以减少资源占用，减少安装包大小，还能够很好地适配不同尺寸的手机。

`shape`属性基本语法示例:

```css
<?xml version="1.0" encoding="utf-8"?>
<shape
    xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape=["rectangle" | "oval" | "line" | "ring"] > // 定义形状 
    <corners //圆角属性
        android:radius="integer"
        android:topLeftRadius="integer"
        android:topRightRadius="integer"
        android:bottomLeftRadius="integer"
        android:bottomRightRadius="integer" />
    <gradient //渐变属性
        android:angle="integer" 
        android:centerX="integer" 
        android:centerY="integer" 
        android:centerColor="integer" 
        android:endColor="color" 
        android:gradientRadius="integer" 
        android:startColor="color" 
        android:type=["linear" | "radial" | "sweep"] 
        android:useLevel=["true" | "false"] />
    <padding //边距属性 
        android:left="integer" 
        android:top="integer" 
        android:right="integer" 
        android:bottom="integer" />
    <size //大小属性 
        android:width="integer" 
        android:height="integer" />
    <solid //填充属性 
        android:color="color" />
    <stroke //描边属性 
        android:width="integer" 
        android:color="color" 
        android:dashWidth="integer" 
        android:dashGap="integer" />
</shape>
```

**corners(圆角)** 是用来定义圆角
```css
<shape xmlns:android="http://schemas.android.com/apk/res/android" > 
    <corners //定义圆角
        android:radius="10dp" //全部的圆角半径; 
        android:topLeftRadius="5dp" //左上角的圆角半径; 
        android:topRightRadius="5dp" //右上角的圆角半径; 
        android:bottomLeftRadius="5dp" //左下角的圆角半径; 
        android:bottomRightRadius="5dp" /> //右下角的圆角半径。
</shape>
```

**solid(填充色)** 是用以指定内部填充色
```css
<shape xmlns:android="http://schemas.android.com/apk/res/android" > 
    <solid android:color="#ffff00"/> //内部填充色
</shape>
```

**gradient(渐变)** 用以定义渐变色，可以定义两色渐变和三色渐变，及渐变样式;
```css
<shape xmlns:android="http://schemas.android.com/apk/res/android" >
    <gradient
        android:type=["linear" | "radial" | "sweep"] //共有3中渐变类型，线性渐变 (默认)/放射渐变/扫描式渐变;
        android:angle="90" //渐变角度，必须为45的倍数，0为从左到右，90为从上到下; 
        android:centerX="0.5" //渐变中心X的相当位置，范围为0~1; 
        android:centerY="0.5" //渐变中心Y的相当位置，范围为0~1; 
        android:startColor="#24e9f2" //渐变开始点的颜色; 
        android:centerColor="#2564ef" //渐变中间点的颜色，在开始与结束点之间; 
        android:endColor="#25f1ef" //渐变结束点的颜色; 
        android:gradientRadius="5dp" //渐变的半径，只有当渐变类型为radial时才能使用;
        android:useLevel="false" /> //使用LevelListDrawable时就要设置为true。设为 false时才有渐变效果。
</shape>
```

**stroke(描边)** 是描边属性，可以定义描边的宽度，颜色，虚实线等;
```css
<shape xmlns:android="http://schemas.android.com/apk/res/android" >
    <stroke
        android:width="1dp" //描边的宽度 
        android:color="#ff0000" //描边的颜色
        // 以下两个属性设置虚线
        android:dashWidth="1dp" //虚线的宽度，值为0时是实线 
        android:dashGap="1dp" />//虚线的间隔
</shape>
```

**padding(内边距)** 是用来定义内部边距
```css
<shape xmlns:android="http://schemas.android.com/apk/res/android" >
    <padding
        android:left="10dp" //左内边距; 
        android:top="10dp" //上内边距; 
        android:right="10dp" //右内边距; 
        android:bottom="10dp" /> //下内边距。
</shape>
```

**size(大小)** 标签是用来定义图形的大小的
```css
<shape xmlns:android="http://schemas.android.com/apk/res/android" >
    <size
        android:width="50dp" //宽度
        android:height="50dp" />// 高度 
</shape>
```

`Shape`可以定义当前`Shape`的形状的，比如矩形，椭圆形，线形和环形;这些都是通过 `shape` 标签属性
来定义的， `shape` 标签有下面几个属性:`rectangle`，`oval`，`line`，`ring`:

```css
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    // shape的形状，默认为矩形，可以设置为矩形(rectangle)、椭圆形(oval)、线性形状(line)、环形(ring)
    android:shape=["rectangle" | "oval" | "line" | "ring"] 
        //下面的属性只有在android:shape="ring"时可用: 
        android:innerRadius="10dp" // 内环的半径; 
        android:innerRadiusRatio="2" // 浮点型，以环的宽度比率来表示内环的半径; 
        android:thickness="3dp" // 环的厚度;
        android:thicknessRatio="2" // 浮点型，以环的宽度比率来表示环的厚度;
        android:useLevel="false"> // boolean值，如果当做是LevelListDrawable使用时值为 true，否则为false。
</shape>
```

**rectangle(矩形)**
```xml
<?xml version="1.0" encoding="utf-8"?>
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="rectangle">
    <solid android:color="@color/colorPrimary"/>
</shape>
```

***oval(椭圆)***
```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="oval">
    <solid android:color="@color/colorPrimary"/>
    <size android:height="100dp"
        android:width="100dp"/>
</shape>
```

**line(线)**
```xml
<shape xmlns:android="http://schemas.android.com/apk/res/android"
    android:shape="line">
    <stroke android:width="1dp" 
        android:color="@color/colorAccent" 
        android:dashGap="3dp"//虚线间距 
        android:dashWidth="4dp"/>//虚线宽度
    <size android:height="3dp"/>
</shape>
```

**ring(圆环)**
```css
<shape xmlns:android="http://schemas.android.com/apk/res/android" 
    android:shape="ring"
    android:useLevel="false"
    android:innerRadius="20dp" // 内环的半径 
    android:thickness="10dp"> // 圆环宽度 
    <!--useLevel需要设置为false-->
    <solid android:color="@color/colorAccent"/>
</shape>
```

