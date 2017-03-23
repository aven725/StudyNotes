# [Android] ScrollView ���M�� ListView

�o�O�j�Ǯɴ��b�l�Ȩ���������Ҭ������A�ɶ���2014/03/13���T�w�{�b���y�k�ٳq�ΡC
[���s��](http://aven725.pixnet.net/blog/post/94425611)

�b�s�@APP���ɭԡA�`�`�i��ݭn�Ψ�ScrollView������

���O�p�G ListView ��b�̭�����

�N�|�L�k���`��������� �{�b�N�ӭק� �����i�H������ܧa !

���ݭӹw����

���ק蠟�e(�ݭn�b�̭��u�ʡA��ƥ���������)�A�p�U��

![image](http://aven725.github.io/image/Android/image004.png)

�ק蠟��(��ƥ�����ܡA�L�k�u��) 

![image](http://aven725.github.io/image/Android/image005.png)

�ѩ�W��Google�F�@�U�A���\�h���P����k�A�Ʀ��٦��ϥ�LinearLayout �Ө��N ListView ��

���O��ڲ{�b���{�רӻ����I�Y�O�A�ҥH�N��ܤF��²�檺��k�Ӥ��Щ� !

�����OLayout������( scrollview__listview.xml (���ק蠟�e) )
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
java������( Scrollview_ListviewActivity.java )�G
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
        // �M�䤸��ID
        ls = (ListView) findViewById(R.id.ls);
        // �إ�ArrayAdapter
        ArrayAdapter ad = new ArrayAdapter(this,
                android.R.layout.simple_expandable_list_item_1, testdata);
        // �]�w��ƨӷ�
        ls.setAdapter(ad);
    }
 
}
```
����o����檺�����ӬO�ݭn�ưʪ� 

![image](http://aven725.github.io/image/Android/image006.png)

�{�b�s�W�@��Class �~�� ListView ( MyListView.java ) 

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
        //�ھڼҦ��p��C��Child�����שM�e��
        int expandSpec = MeasureSpec.makeMeasureSpec(Integer.MAX_VALUE >> 2,
                MeasureSpec.AT_MOST);
        super.onMeasure(widthMeasureSpec, expandSpec);
    }
}
```
�̫�N scrollview__listview.xml �ק� 

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

�o�˴N�j�\�i����~~ 

![image](http://aven725.github.io/image/Android/image007.png) 

�̫���W�d��(Mega�Ŷ�) �G
https://mega.co.nz/#!BFQ11YAC!pGHMyCDxO-SFwHj60tTNMZZeprENjtTzTSwF05HEQ-g

# Reference
* [MeasureSpec���ϥ�](http://blog.csdn.net/jinlong_lou/article/details/8444796)

-------
* [�^�ؿ�](../README.md)

-------