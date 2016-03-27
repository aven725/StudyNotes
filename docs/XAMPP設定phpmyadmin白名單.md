# XAMPP設定phpmyadmin白名單(Indexes)

因為懷疑對外公開的mysql被攻擊，XAMPP預設使用phpmyadmin，所以乾脆把phpmyadmin設定白名單

首先找到`httpd-xampp.conf`
預設路徑大概會在`C:\xampp\apache\conf\extra\httpd-xampp.conf` 

打開檔案後大約在第90行找到
```
<Directory "C:/xampp/phpMyAdmin">
    AllowOverride AuthConfig
    Require all granted
</Directory>
```
Require all granted 註解改成 Require local 或是 Require ip X.X.X.X 

如果是一個team要用的話，IP也可以用網段的方式設定 Ex:Require ip X.X.X.

注意不是Require ip X.X.X.* 不用加星號

如下圖，因為我是裝在D曹，所以圖上是寫D:/xampp/phpMyAdmin

![image](http://aven725.github.io/image/WEB/XAMPP/image004.png)

設定完後記得重啟Apache

-------
* [回目錄](../README.md)

-------