`ContentProvider`的2种用途：
- 1.我们想在自己的应用中访问别的应用，或者说一些`ContentProvider`暴露给我们的一些数据，比如手机联系人，短信等！我们想对这些数据进行读取或者修改，这就需要用到`ContentProvider`了！
- 2.我们自己的应用，想把自己的一些数据暴露出来，给其他的应用进行读取或操作，我们也可以用到`ContentProvider`，另外我们可以选择要暴露的数据，就避免了我们隐私数据的的泄露！

## 1.读取联系人

①：在`AndroidManifest.xml`中注明读取通讯录权限
```xml
<uses-permission android:name="android.permission.READ_CONTACTS" />
```

②：判断权限
```java
int check = ContextCompat.checkSelfPermission(this, Manifest.permission.READ_CONTACTS);
if (check != PackageManager.PERMISSION_GRANTED) {
    // 没有权限，开始动态申请读取通讯录的权限
    ActivityCompat.requestPermissions(this, new String[]{Manifest.permission.READ_CONTACTS}, 0);
    return;
}

// 申请权限的回调方法 （在Activity中）
@Override
public void onRequestPermissionsResult(int requestCode, @NonNull String[] permissions, @NonNull int[] grantResults) {
    super.onRequestPermissionsResult(requestCode, permissions, grantResults);
    switch (requestCode) {
        case 0:  // 这个0其实是申请的时候，传入的requestCode
            if ((grantResults.length > 0) && (grantResults[0] == PackageManager.PERMISSION_GRANTED)) {
                Toast.makeText(this, "权限获取成功", Toast.LENGTH_SHORT).show();
            }
    }
}
```

③：读取联系人：
```java
void getContacts() {  // 已有权限的情况
    ContentResolver resolver = getContentResolver();
    Uri url = ContactsContract.CommonDataKinds.Phone.CONTENT_URI;

    // 查询联系人数据
    Cursor cursor = resolver.query(url, null, null, null, null);
    while (cursor.moveToNext()) {
        // 获取联系人姓名、手机号
        int inx = cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.DISPLAY_NAME);
        if (inx < 0) { continue; }
        String cName = cursor.getString(inx);  // 联系人的姓名

        inx = cursor.getColumnIndex(ContactsContract.CommonDataKinds.Phone.NUMBER);
        if (inx < 0) { continue; }
        String cNum = cursor.getString(inx);  // 联系人的手机号
    }
    cursor.close();
}
```

