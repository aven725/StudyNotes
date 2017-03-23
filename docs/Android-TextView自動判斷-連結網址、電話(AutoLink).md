# [Android] TextView 自動判斷 連結網址、電話 (AutoLink)

這是大學時期在痞客邦的部落格所紀錄的，時間為2014/03/02不確定現在的語法還通用。
[原文連結](http://aven725.pixnet.net/blog/post/88175741)

TextView是我們經常使用的元件之一，

今天要來說如何讓你的 TextView 去自動判斷 內容是電話、Email、網址......等。

判斷之後當然就可以點擊  並且作相對的事件 Ex：點擊號碼就跳到撥打電話那邊。

實現的方法有兩種，並分別作介紹。

成果圖：

![image](http://aven725.github.io/image/Android/image002.png)

## 方法一

Layout 部分( text_view__auto_link.xml )：
```xml
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context=".TextView_AutoLinkActivity" >
 
    <TextView
        android:id="@+id/txt_phone"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="(03)4583333" />
 
    <TextView
        android:id="@+id/txt_web"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignLeft="@+id/txt_phone"
        android:layout_below="@+id/txt_phone"
        android:text="http://tw.yahoo.com" />
 
    <TextView
        android:id="@+id/txt_email"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignLeft="@+id/txt_web"
        android:layout_below="@+id/txt_web"
        android:text="abc@yahoo.com.tw" />
 
    <TextView
        android:id="@+id/txt_all"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignLeft="@+id/txt_email"
        android:layout_below="@+id/txt_email"
        android:layout_marginTop="30dp"
        android:text="0912345678\nabc@yahoo.com.tw\nhttp://tw.yahoo.com" />
 
</RelativeLayout>
```
 java的部分( TextView_AutoLinkActivity )：
 ```java
 package com.example.textview_autolink;
 
import android.app.Activity;
import android.os.Bundle;
import android.text.method.LinkMovementMethod;
import android.text.util.Linkify;
import android.widget.TextView;
 
public class TextView_AutoLinkActivity extends Activity {
    private TextView txt_web, txt_phone, txt_email, txt_all;
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.text_view__auto_link);
        // 尋找元件ID
        txt_web = (TextView) findViewById(R.id.txt_web);
        txt_phone = (TextView) findViewById(R.id.txt_phone);
        txt_email = (TextView) findViewById(R.id.txt_email);
        txt_all = (TextView) findViewById(R.id.txt_all);
        // 設定連結
        txt_web.setAutoLinkMask(Linkify.WEB_URLS);
        txt_phone.setAutoLinkMask(Linkify.PHONE_NUMBERS);
        txt_email.setAutoLinkMask(Linkify.EMAIL_ADDRESSES);
        txt_all.setAutoLinkMask(Linkify.ALL);
        // 如果要生效,那麼加入以下代碼
        txt_web.setMovementMethod(LinkMovementMethod.getInstance());
        txt_phone.setMovementMethod(LinkMovementMethod.getInstance());
        txt_email.setMovementMethod(LinkMovementMethod.getInstance());
        txt_all.setMovementMethod(LinkMovementMethod.getInstance());
    }
}
 ```
 
這樣就完成了，接下來說方法二，比方法一更簡單許多！
 
## 方法二
 
方法二非常簡單，只需要在建立TextView的時候多加上一行指令就好了，完全不會使用到Java那邊
```xml
android:autoLink="all"
//或是選擇你需要的
android:autoLink="web|email|phone"
```
例如
```xml
<TextView
        android:id="@+id/txt_all"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_alignLeft="@+id/txt_phone"
        android:autoLink="all"
        android:layout_below="@+id/txt_phone"
        android:text="http://tw.yahoo.com" />
```
因為比較簡單，就不列出Layout所有的程式碼了。 

接下來，如果你使用的是模擬器，可能在點擊E-mail的連結時，發生應用程式停止的情況，如下圖。 

![image](https://raw.githubusercontent.com/aven725/aven725.github.io/master/image/Android/image003.jpg) 

會發生這個原因是因為使用者沒有安裝發送Email相關的軟體，

解決方法如下，當找不到Email的程式的時候，我們需要攔截這個startActivity 

```java
// 如果沒有郵件系統處理方法
@Override
public void startActivity(Intent intent) {
    /**
     * Java - IndexOf 和 lastIndexOf 用法 lastIndexOf(char ch)
     * 可以找尋某字串中，最後一次出現ch字元的位置。位置的起始由0開始 如果字串中找不到該字元，則會回傳-1
     */
    if (intent.toString().indexOf("mailto") != -1) {
        // Any way to judge that this is to sead an email
        PackageManager pm = getPackageManager();
        // The first Method
        List activities = pm.queryIntentActivities(intent, 0);
        if (activities == null || activities.size() == 0) {
            // Do anything you like, or just return
            return;
        }
    }
    super.startActivity(intent);
}
```
 記得，不管是使用方法一、方法二，都要加入以上程式碼，來避免用戶當機的機會唷！
 
 最後附上方法一的原始檔(Mega)：https://mega.co.nz/#!RM41GZaZ!LSWqChlOiUjsMuoCsDKPwTKoY6ErvWIz7KMrqA6P8vI 
 
# Reference
* [autoLink=email當機問題](http://www.cnblogs.com/angeldevil/archive/2013/12/22/3485871.html)

-------
* [回目錄](../README.md)

-------