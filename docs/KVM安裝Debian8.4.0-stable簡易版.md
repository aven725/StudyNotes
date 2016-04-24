# KVM安裝Debian8.4.0-stable簡易版

安裝的部分先跳過，請參考Reference

ISO檔下載

    wget http://cdimage.debian.org/debian-cd/8.4.0/amd64/iso-cd/debian-8.4.0-amd64-netinst.iso

或到官方網站下載 https://www.debian.org/CD/netinst/

建立虛擬硬碟，大小為 5GB

    qemu-img create vm001.img 5G
    
開啟KVM

    kvm -m 1024M -cdrom debian-8.4.0-amd64-netinst.iso -hda vm001.img

安裝過程如下圖：

![image](http://aven725.github.io/image/InstallationDebianKVM/debian001.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian002.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian003.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian004.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian005.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian006.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian007.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian008.jpg)

Domain name 沒有所以空白就好
![image](http://aven725.github.io/image/InstallationDebianKVM/debian009.jpg)

root密碼要記得
![image](http://aven725.github.io/image/InstallationDebianKVM/debian010.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian011.jpg)

使用者帳號跟密碼
![image](http://aven725.github.io/image/InstallationDebianKVM/debian012.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian013.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian014.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian015.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian016.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian017.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian018.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian019.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian020.jpg)

mirror選台灣就好
![image](http://aven725.github.io/image/InstallationDebianKVM/debian021.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian022.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian023.jpg)

這看個人喜好，是否願意提供軟體資訊，不同意就No不影響安裝
![image](http://aven725.github.io/image/InstallationDebianKVM/debian024.jpg)

精簡版，所以不用安裝桌面，安裝SSH 跟 standard system就可以
![image](http://aven725.github.io/image/InstallationDebianKVM/debian025.jpg)

GRUB boot這一定要Yes，否則會不能開機
![image](http://aven725.github.io/image/InstallationDebianKVM/debian026.jpg)

安裝在sda(開機磁碟)
![image](http://aven725.github.io/image/InstallationDebianKVM/debian027.jpg)

按下Continue完成安裝，會自動重開機
![image](http://aven725.github.io/image/InstallationDebianKVM/debian028.jpg)

安裝完後登入試試看 

![image](http://aven725.github.io/image/InstallationDebianKVM/debian029.jpg)

![image](http://aven725.github.io/image/InstallationDebianKVM/debian030.jpg)

查看版本 

![image](http://aven725.github.io/image/InstallationDebianKVM/debian031.jpg)


# Reference
* [Lab 1: Virtual Technelogy: Kernel-based Virtual Machine (KVM)](http://www.cs.nchu.edu.tw/~snmlab/CloudMgnt201409/Lab1.html)

-------
* [回目錄](../README.md)

-------