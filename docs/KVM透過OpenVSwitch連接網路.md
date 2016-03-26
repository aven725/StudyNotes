# KVM透過OpenVSwitch連接網路
首先要安裝 kvm 以及 openvswitch 這邊就不做說明了 

因為安全性問題，以下IP不會全顯示出來 

先來看一下原本的網卡設定 

    $ ifconfig

大概會長的像下面這張圖，只有一張網路卡的情況下  

![image](http://aven725.github.io/image/KVM/Selection_001.png) 

因為我們要透過ovs的bridge來實現KVM連接外部網路的方法，所以首先先創出ovs的bridge
```sh
$ sudo ovs-vsctl add-br brLAN
$ sudo ovs-vsctl add-br brWAN
```
首先我們創造出了兩個bridge，分別是brLAN、brWAN，那這邊我們要將brLAN用來跟內部虛擬機做溝通，brWAN則是連接外部網路用的  

那現在這個brWAN是沒有功能的，我們必須把eth0的IP給他，並且把eth0接到brWAN上面的port，這樣才可以透過brWAN連接外部網路 
```sh
$ sudo ovs-vsctl add-port brWAN eth0
$ sudo ifconfig eth0 0
$ sudo ifconfig brWAN 140.120.X.X netmask 255.255.255.192
$ sudo route add default gw 140.120.X.X brWAN
```
做到這邊我們先看一下現在的網路卡設定變成什麼樣子

    $ ifconfig

![image](http://aven725.github.io/image/KVM/Selection_002.png) 

在來設定brLAN，brLAN就是虛擬機的gateway，所以設定完IP要記得 

    $ sudo ifconfig brLAN 192.168.100.254 netmask 255.255.255.0
    
那因為我們要跟虛擬機連接，所以要創一個虛擬的網卡，給虛擬機跟brLAN連接，也就是做類似上面eth0跟brWAN的動作，只是變成要創一個虛擬網卡出來 
```sh
$ sudo tunctl -u aven -t tapLAN
$ sudo ifconfig tapLAN up
$ sudo ovs-vsctl add-port brLAN tapLAN
```
![image](http://aven725.github.io/image/KVM/Selection_004.png) 

![image](http://aven725.github.io/image/KVM/Selection_005.png) 

到這裡就可以開啟KVM摟，參數如下 
```sh
sudo kvm -m 1024M \
-net nic,vlan=0,netdev=tapLAN,macaddr=00:00:00:00:cc:10,model=virtio \
-netdev tap,id=tapLAN,ifname=tapLAN,script=no \
-hda ~/KVM/img/vm001.img
```

開機之後要設定interfaces，記得要用root權限 

    $ nano /etc/network/interfaces

將內容改成
```sh
auto eth0
iface eth0 inet static
  address 192.168.100.2
  netmask 255.255.255.0
  gateway 192.168.100.254
```
![image](http://aven725.github.io/image/KVM/Selection_006.png)

改完之後記得重啟網路服務，或是重新開機 
```sh
$ sudo service networking restart
或
$ init 6
```
重啟後我們看一下目前的route狀態

    $ route -n
![image](http://aven725.github.io/image/KVM/Selection_010.png) 

這樣應該就可以ping到主機摟

    $ ping 192.168.100.254
	
![image](http://aven725.github.io/image/KVM/Selection_007.png) 

但是應該無法連出去(ping google DNS) 

![image](http://aven725.github.io/image/KVM/Selection_008.png) 

首先改一下DNS
```sh
$ sudo nano /etc/resolv.conf
```
加入
    
    nameserver 8.8.8.8

不能出去的原因是因為我們的IP是private的IP，必須透過NAT的技術改成public才行連到外部網路，所以在主機的iptable需要加入新的規則 

    $ sudo iptables -t nat -A POSTROUTING -s 192.168.100.0/24 -o brWAN -j MASQUERADE
上面大概是說，新增一個NAT的規則，來源是192.168.100.0/24這個網段，我的出口為brWAN並且做偽裝的動作 

這時候我們回到KVM應該就可以ping的到外部網路摟 

    $ ping 8.8.8.8
    
![image](http://aven725.github.io/image/KVM/Selection_009.png) 

那如果我要再KVM裡面架設web服務，我應該怎麼做呢？

很簡單，一樣下一個NAT的規則就可以
```sh
$ sudo iptables -t nat -A PREROUTING -i brWAN -p tcp --dport 80 \
     -j DNAT --to-destination 192.168.100.2:80
```

上面大概是說，從brWAN近來的封包要往80port的幫我做解NAT的動作，送到192.168.100.2的80port，80port就是預設的Web PORT，當然也可以設定在別的port 

這樣的話，在別台電腦上輸入這台電腦的ip就會連到虛擬機所架設的網站摟 
![image](http://aven725.github.io/image/KVM/apache2.png) 

最後我們看一下HOST上面的設定

    $ route -n
    
![image](http://aven725.github.io/image/KVM/Selection_011.png) 

    $ sudo iptables -t nat -L -n

![image](http://aven725.github.io/image/KVM/Selection_012.png)  

    $ sudo iptables-save

![image](http://aven725.github.io/image/KVM/Selection_013.png) 

結束後我們必須把設定還原
```sh
$ sudo iptables -F -t nat
$ sudo ovs-vsctl del-br brWAN
$ sudo ovs-vsctl del-br brLAN
$ sudo ifconfig tapLAN down
$ sudo tunctl -d tapLAN
$ sudo ifconfig eth0 140.120.X.X/X
$ sudo route add default gw 140.120.X.X eth0
```

這樣就完成拉！

# Reference
* [Virtual Technelogy: Kernel-based Virtual Machine (KVM)](http://www.cs.nchu.edu.tw/~snmlab/CloudMgnt201409/Lab1.html)
* [鳥哥的 Linux 私房菜-NAT](http://linux.vbird.org/linux_server/0250simple_firewall.php#nat)
* [How to Create KVM Virtual Machine and Attach to OpenvSwitch Bridge](https://www.youtube.com/watch?v=Gud2GoI-W_w)

-------
* [回目錄](../README.md)

-------