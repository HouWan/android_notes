> 当前文件是`chapter14.md`

## 1.RecyclerView

### 1.1 基本使用流程
①：定义`RecyclerView`，并设置布局管理器、适配器
```java
RecyclerView recyclerView = findViewById(R.id.recyclerView);
// 如果RecyclerView的大小是固定的，为了提升RecyclerView的性能会更好，可以设置setHasFixedSize(true)
recyclerView.setHasFixedSize(true);
// 设置布局管理器，告诉RecyclerView应该如何排列Item（是横向还是纵向等）
recyclerView.setLayoutManager(new LinearLayoutManager(this));

// 创建Adapter (data是列表数据源)
RecyclerAdapter adapter = new RecyclerAdapter(this, data);
// 设置Adapter
recyclerView.setAdapter(adapter);
```

②：自定义`RecyclerAdapter`适配器，要继承`RecyclerView.Adapter`，泛型是下面的`ViewHolder`。

③：自定义`ViewHolder`，要继承`RecyclerView.ViewHolder`。

```java
public class RecyclerAdapter extends RecyclerView.Adapter<RecyclerAdapter.MyViewHolder> {
    /**
     * 定义 MyViewHolder 内部类，继承 RecyclerView.ViewHolder
     * MyViewHolder的作用是缓存cell的view，避免每次都重新inflate
     */
    class MyViewHolder extends RecyclerView.ViewHolder {
        TextView tvContent;  // 显示内容的TextView
        public MyViewHolder(@NonNull View itemView) {
            super(itemView);
            tvContent = itemView.findViewById(android.R.id.text1);
        }

        // 绑定数据到TextView上，一般会在 MyViewHolder 定义一个 bind 方法
        public void bind(String content) {
            tvContent.setText(content);
        }
    }

    private List<String> mData;  // 数据源
    private Context mContext;  // 上下文
    private LayoutInflater mInflater;  // 布局 inflater

    public RecyclerAdapter(Context context, List<String> data) {  // 适配器构造方法
        mData = data;
        mContext = context;
        mInflater = LayoutInflater.from(context);
    }

    // 【必须实现的方法一】根据要显示的内容创建一个ViewHolder
    @NonNull
    @Override
    public MyViewHolder onCreateViewHolder(@NonNull ViewGroup parent, int viewType) {
        // 一定要将 parent 传入 inflate 方法
        View cell = mInflater.inflate(android.R.layout.simple_list_item_1, parent, false);
        return new MyViewHolder(cell);
    }

    // 【必须实现的方法二】将对应的数据binding到ViewHolder上
    @Override
    public void onBindViewHolder(@NonNull MyViewHolder holder, int position) {
        holder.bind(mData.get(position));
    }

    // 【必须实现的方法三】返回数据源的数量
    @Override
    public int getItemCount() {
        return mData.size();
    }
}
```

### 1.2 IED中预览效果
如下所示，代码中增加`tools:listitem`和`tools:itemCount`两个属性，可以在IDE中实时看到列表的效果，而且只会在IDE中生效：
```xml
<!-- tools:listitem=@"layout/xxx" 在IED中预览cell效果 -->
<!-- tools:itemCount=2 在IED中预览cell的个数 -->
<androidx.recyclerview.widget.RecyclerView
    tools:listitem="@layout/item_discovery_content"
    tools:itemCount="2"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```


## 2.XXOO

### 1.1 Clock和计时器

`TextClock`和`AnalogClock`都动态显示当前的时间。`TextClock`继承于`TextView`，以格式化文本的方式动态显示时间；`AnalogClock`继承于`View`，以模拟表盘的方式显示当前时间。
```xml

```

| `DatePicker`属性 | 属性含义 |
| --- | --- |
| android:firstDayOfWeek | 第一列为星期几，默认是星期日，比如2为星期一 |
| android:maxDate | 限制最大日期。"mm/dd/yyyy"格式 |
| android:minDate | 限制最小日期。"mm/dd/yyyy"格式 |


![mvc](./image/0028.png)


颜色
https://www.toolhelper.cn/Color/RGBToHex