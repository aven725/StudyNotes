# [Android] ScrollView 內套用 ListView

這是大學時期在痞客邦的部落格所紀錄的，時間為2014/03/13不確定現在的語法還通用。
[原文連結](http://aven725.pixnet.net/blog/post/94425611)

在製作APP的時候，常常可能需要用到ScrollView的元件

但是如果 ListView 放在裡面的話

就會無法正常的全部顯示 現在就來修改 讓它可以全部顯示吧 !

先看個預覽圖

未修改之前(需要在裡面滾動，資料未能全部顯示)，如下圖

![image](http://aven725.github.io/image/Android/image004.png)

修改之後(資料全部顯示，無法滾動) 

![image](http://aven725.github.io/image/Android/image005.png)

由於上網Google了一下，有許多不同的方法，甚至還有使用LinearLayout 來取代 ListView 的

但是對我現在的程度來說有點吃力，所以就選擇了最簡單的方法來介紹拉 !

首先是Layout的部分( scrollview__listview.xml (未修改之前) )
```xml
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/ScrollView1"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context=".Scrollview_ListviewActivity" >
 
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical" >
 
        <ListView
            android:id="@+id/ls"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" >
        </ListView>
 
    </LinearLayout>
 
</ScrollView>
```
java的部分( Scrollview_ListviewActivity.java )：
```java
package com.example.scrollview_listview;
 
import android.app.Activity;
import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.ListView;
 
public class Scrollview_ListviewActivity extends Activity {
    private ListView ls;
    private String[] testdata = new String[] { "test1", "test2", "test3",
            "test4", "test5", "test6" };
    static int totalHeight = 0;
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.scrollview__listview);
        // 尋找元件ID
        ls = (ListView) findViewById(R.id.ls);
        // 建立ArrayAdapter
        ArrayAdapter ad = new ArrayAdapter(this,
                android.R.layout.simple_expandable_list_item_1, testdata);
        // 設定資料來源
        ls.setAdapter(ad);
    }
 
}
```
做到這邊執行的話應該是需要滑動的 

![image](http://aven725.github.io/image/Android/image006.png)

現在新增一個Class 繼承 ListView ( MyListView.java ) 

```java
package com.example.scrollview_listview;
 
import android.content.Context;
import android.util.AttributeSet;
import android.widget.ListView;
 
public class MyListView extends ListView {
     
    public MyListView(Context context) {
        super(context);
        // TODO Auto-generated constructor stub
    }
 
    public MyListView(Context context, AttributeSet attrs) {
        super(context, attrs);
        // TODO Auto-generated constructor stub
    }
 
    public MyListView(Context context, AttributeSet attrs, int defStyle) {
        super(context, attrs, defStyle);
        // TODO Auto-generated constructor stub
    }
 
    @Override
    protected void onMeasure(int widthMeasureSpec, int heightMeasureSpec) {
        //根據模式計算每個Child的高度和寬度
        int expandSpec = MeasureSpec.makeMeasureSpec(Integer.MAX_VALUE >> 2,
                MeasureSpec.AT_MOST);
        super.onMeasure(widthMeasureSpec, expandSpec);
    }
}
```
最後將 scrollview__listview.xml 修改 

```xml
<ScrollView xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/ScrollView1"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context=".Scrollview_ListviewActivity" >
 
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:orientation="vertical" >
 
        <com.example.scrollview_listview.MyListView
            android:id="@+id/ls"
            android:layout_width="match_parent"
            android:layout_height="wrap_content" >
        </com.example.scrollview_listview.MyListView>
 
    </LinearLayout>
 
</ScrollView>
```

這樣就大功告成拉~~ 

![image](http://aven725.github.io/image/Android/image007.png) 

最後附上範例(Mega空間) ：
https://mega.co.nz/#!BFQ11YAC!pGHMyCDxO-SFwHj60tTNMZZeprENjtTzTSwF05HEQ-g

# Reference
* [MeasureSpec的使用](http://blog.csdn.net/jinlong_lou/article/details/8444796)

-------
* [回目錄](../README.md)

-------