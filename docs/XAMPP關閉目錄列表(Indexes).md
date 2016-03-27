# XAMPP關閉目錄列表(Indexes)
剛裝完XAMPP時，因為Apache預設在找不到link(連結)時會列出該連結下所有的目錄，有時列出所有的檔案及目錄是不安全的，像是下圖，檔案都被看光光了 

![image](http://aven725.github.io/image/WEB/XAMPP/image001.png)

所以為了安全，我們必須關閉這項功能

首先，找到 `C:\xampp\apache\conf\httpd.conf` 這個檔案，打開後找到

    Options Indexes FollowSymLinks Includes ExecCGI

大概在260行左右，將上面的Indexes刪除，變成

    Options FollowSymLinks Includes ExecCGI
    
如下圖紅框

![image](http://aven725.github.io/image/WEB/XAMPP/image002.png)

改完後存檔，重啟Apache，打開網站目錄，會發現不能瀏覽了

![image](http://aven725.github.io/image/WEB/XAMPP/image003.png)

這樣檔案就不會被看光光摟

# Reference
* [鳥哥的 Linux 私房菜-Apache 的基本設定](http://linux.vbird.org/linux_server/0360apache.php#www_basic_basic)

-------
* [回目錄](../README.md)

-------