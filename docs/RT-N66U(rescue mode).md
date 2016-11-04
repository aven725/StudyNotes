# RT-N66U(rescue mode)

到 http://www.asus.com/tw/Networking/RTN66U/HelpDesk_Download/ 下載ASUS 救援模式 以及 ASUS RT-N66U 韌體

設定接Switch的網卡IP為 192.168.1.X 遮罩255.255.255.0

進入Rescue mode

1. 拔開路由器的電源線(DC IN).

![image](http://aven725.github.io/image/OpenWrt/RT66U_RescueMode/1.jpg)

2. 長壓 [RESTORE] 鍵約5秒.

![image](http://aven725.github.io/image/OpenWrt/RT66U_RescueMode/2.jpg)

3. 請勿放開 [RESTORE].接上路由器的電源線.

![image](http://aven725.github.io/image/OpenWrt/RT66U_RescueMode/3.jpg)

4. 如電源燈號呈現慢閃爍，代表路由器處於Rescue模式.

![image](http://aven725.github.io/image/OpenWrt/RT66U_RescueMode/4.jpg)

5. 開啟 Firmware Restoration

![image](http://aven725.github.io/image/OpenWrt/RT66U_RescueMode/5.png)

6. 選擇下載的韌體版本 按上載( 失敗 QQ 換TFTP)

![image](http://aven725.github.io/image/OpenWrt/RT66U_RescueMode/6.png)

## TFTP

安裝 tftp
```
sudo apt-get install tftpd-hpa tftp
$ tftp
tftp> connect
(to) 192.168.1.1
tftp> binary
tftp> put /hoem/aven/RT-AC66U_3.0.0.4_266.trx
Sent 22765568 bytes in 76.5 seconds
tftp> quit
```
![image](http://aven725.github.io/image/OpenWrt/RT66U_RescueMode/7.png)

等待約5分鐘，之後重開Switch 
trx可以直接刷openwrt

救援模式後來有找到另一種，可以開WEBUI直接上傳的 之後整理資料再放上來。

# Reference
* [ASUS常見問題](https://www.asus.com/tw/support/faq/1000814)
* [Recovering ASUS router firmware without Windows](https://chrishardie.com/2013/02/asus-router-firmware-windows-mac-linux/)

-------
* [回目錄](../README.md)

-------
