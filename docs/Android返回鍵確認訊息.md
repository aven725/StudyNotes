# [Android] 返回鍵、離開確認訊息

這是大學時期在痞客邦的部落格所紀錄的，時間為2014/02/28不確定現在的語法還通用。
[原文連結](http://aven725.pixnet.net/blog/post/87074882)

常常看到許多APP應用程式，當你要離開按下返回鍵(<-)時

就會跳出視窗詢問是否要離開，所以就試著寫寫看

原本以為寫在onPuse()裡面就好，結果完全不是這麼回事

後來查了一下才發現完全寫錯地方啊~~~

是要重寫onKeyDown才對，先來看看效果圖。

![image](https://raw.githubusercontent.com/aven725/aven725.github.io/master/image/Android/image001.jpg)

因為不難，接下來就上程式碼了

```java
package com.example.back_key;
 
import android.app.Activity;
import android.app.AlertDialog;
import android.content.DialogInterface;
import android.os.Bundle;
import android.view.KeyEvent;
 
public class Back_KeyActivity extends Activity {
 
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.back__key);
    }
 
    @Override
    public boolean onKeyDown(int keyCode, KeyEvent event) {
        // TODO Auto-generated method stub
 
        if (keyCode == KeyEvent.KEYCODE_BACK) { // 攔截返回鍵
            new AlertDialog.Builder(Back_KeyActivity.this)
                    .setTitle("確認視窗")
                    .setMessage("確定要結束應用程式嗎?")
                    .setIcon(R.drawable.ic_launcher)
                    .setPositiveButton("確定",
                            new DialogInterface.OnClickListener() {
 
                                @Override
                                public void onClick(DialogInterface dialog,
                                        int which) {
                                    finish();
                                }
                            })
                    .setNegativeButton("取消",
                            new DialogInterface.OnClickListener() {
 
                                @Override
                                public void onClick(DialogInterface dialog,
                                        int which) {
                                    // TODO Auto-generated method stub
 
                                }
                            }).show();
        }
        return true;
    }
 
}
```

KEYCODE_BACK 其實也可以改成 HOME 這樣的話也可以攔截HOME鍵了，當然也可以改成其他按鍵摟。

範例下載(Mega空間)：
https://mega.co.nz/#!dQBDwaQB!yIWMwlKTWvPmsQgWTVhFpmuBQ_PxyGqsfOluyGpSYyM