# Linux 將 ISO檔燒入隨身碟(製作USB開機)

#### 首先先看Linux裝置，找到USB裝置名稱
## 
```sh
$ lsblk
```
大概會看到類似下面這樣 (下面的sdd就是USB)
```
NAME MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
sda 8:0 0 465.8G 0 disk 
├─sda1 8:1 0 487M 0 part /boot
├─sda2 8:2 0 4.7G 0 part /
├─sda3 8:3 0 18.6G 0 part /usr
├─sda4 8:4 0 1K 0 part 
├─sda5 8:5 0 9.3G 0 part /var
├─sda6 8:6 0 9.3G 0 part /usr/local
├─sda7 8:7 0 9.3G 0 part /tmp
├─sda8 8:8 0 18.6G 0 part /home
├─sda9 8:9 0 14.9G 0 part [SWAP]
├─sda10 8:10 0 18.6G 0 part /src1
├─sda11 8:11 0 18.6G 0 part /src2
├─sda12 8:12 0 139.7G 0 part /src3
├─sda13 8:13 0 139.7G 0 part /src4
└─sda14 8:14 0 63.9G 0 part /backup
sdd 8:48 1 7.5G 0 disk 
├─sdd1 8:49 1 6.5G 0 part /media/aven/2efcc0d3-9e31-4364-b224-f810999f06
├─sdd2 8:50 1 1K 0 part 
└─sdd5 8:53 1 975M 0 part 
sr0 11:0 1 1024M 0 rom 
```
#### 接下來要把USB格式化
## 
先將USB unmout
```sh
$ sudo umount /dev/sdd1
```
利用 mkfs 將USB格式化成fat格式，完成後會顯示: mkfs.fat 3.0.27 (2014-11-12)
```sh
$ sudo mkfs.vfat /dev/sdd1
```
mkfs 支援的格式如下圖 
###
![image](http://aven725.github.io/image/Linux_USB/image001.png)

#### 製作USB開機光碟
## 
製作前記得先將USB unmout
```sh
$ sudo umount /dev/sdd1
```
製作的指令為下
```sh
$ sudo dd if=/映像檔的路徑/映像檔檔名 of=/dev/隨身碟裝置名稱
```
`請注意 dd 寫入時需寫入整個裝置（/dev/sdd）而不是其中的一個分區（/dev/sdd1）`

**正確**的範例:
```sh
$ sudo dd if=~/debian-8.2.0-amd64-netinst.iso of=/dev/sdd
```
**錯誤**的範例:
```sh
$ sudo dd if=~/debian-8.2.0-amd64-netinst.iso of=/dev/sdd1
```
# Reference
* [使用 dd 指令製作 Live USB](https://chakra-zh.blogspot.tw/2012/04/dd-live-usb.html)

-------
* [回目錄](../README.md)

-------