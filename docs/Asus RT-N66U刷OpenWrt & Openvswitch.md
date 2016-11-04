# Asus RT-N66U刷OpenWrt & Openvswitch

步驟

1. 安裝git以下載OpenWrt source code
```
sudo apt-get update
sudo apt-get install git-core build-essential libssl-dev libncurses5-d
```
2. 下載OpenWrt
```
git clone git://git.openwrt.org/openwrt.git
```
3. 更新 Feeds (備註:feeds在OpenWrt中，代表packages的集合)
```
cd openwrt
./scripts/feeds update -a
./scripts/feeds install -a
```	
4. Configure
到 https://wiki.openwrt.org/toh/start 找自己的硬體配備，並設定config檔
```
make menuconfig
```

Target System

![image](http://aven725.github.io/image/OpenWrt/1.png)

Brcm47xx

![image](http://aven725.github.io/image/OpenWrt/2.png)

Subtarget

![image](http://aven725.github.io/image/OpenWrt/3.png)

Generic

![image](http://aven725.github.io/image/OpenWrt/4.png)

![image](http://aven725.github.io/image/OpenWrt/5.png)

![image](http://aven725.github.io/image/OpenWrt/6.png)

![image](http://aven725.github.io/image/OpenWrt/7.png)

![image](http://aven725.github.io/image/OpenWrt/8.png)


每一個選項大致有三種，y, m, n 分別表示:
按 y 設定成 <*> 標籤 這個套件將包含在映像檔裡
	按 m 設定成 <M> 標籤 
	這個套件會被編譯 (to be installed with opkg after Flashing OpenWrt) but the package will not be included in the firmware-image
	按 n 設定成 < > 標籤 
這個套件不會被編譯

![image](http://aven725.github.io/image/OpenWrt/9.png)

save
	
make kernel_menuconfig

![image](http://aven725.github.io/image/OpenWrt/10.png)

![image](http://aven725.github.io/image/OpenWrt/11.png)

Save

5. 編譯 ( 這邊要滿久的 )
```
make -j
```

編譯完會在bin/brcm47xx(設備的Target) 底下，選擇自己的型號
	
安裝，直接用web ui 上傳

6. 安裝

到 http://192.168.1.1/Advanced_FirmwareUpgrade_Content.asp
192.168.1.1是ASUS固定的WEB IP
![image](http://aven725.github.io/image/OpenWrt/12.png)

選擇編好的.trx (這裡是 openwrt-brcm47xx-mips74k-asus-rt-n66u-squashfs.trx) 然後上傳
![image](http://aven725.github.io/image/OpenWrt/13.png)

## 會說版本錯誤 !!  只能進救援模式 強制刷 ( [RT-N66U_rescue_mode](RT-N66U_rescue_mode.md) ) 

其實不編譯 直接下載別人編好的 TRX，然後直接刷就OK
( https://wiki.openwrt.org/toh/start )

```
刷完後 登入 ssh root@192.168.1.1 ，如果不能SSH 請先用telnet 試試看

安裝 LUCI & ovs
root@OpenWrt:/# opkg update
root@OpenWrt:/# opkg install luci                   //luci是web UI所需的套件
root@OpenWrt:/# opkg install openvswitch
root@OpenWrt:/# ps w | grep ovs
root@OpenWrt:/# opkg info openvswitch
![](https://i.imgur.com/TEbVjBI.png)
```

# Reference
* [以Asus RT-N66U刷OpenWrt & Openvswitch](https://www.evernote.com/shard/s373/sh/9673f489-afe4-4855-868a-aafe9992fab7/652540f0034954541433e32ec12a416e)
* [OpenWrt Wiki](https://wiki.openwrt.org/zh-tw/doc/howto/build)
* [Building and Configuring Open vSwitch on OpenWrt for Cloud Networking](http://blog.zymr.com/building-and-configuring-open-vswitch-on-openwrt-for-cloud-networking)

-------
* [回目錄](../README.md)

-------