# Linux 編譯 Open vSwitch(2.5.0)
 
## Environment

> OS : Ubuntu 14.04 LTS Server 64 bit
> Kernel : 4.2.0-42-generic 

	uname -a

> Linux VM-Server 4.2.0-42-generic #49~14.04.1-Ubuntu SMP Wed Jun 29 20:22:11 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux

事前安裝

```
sudo apt-get update
sudo apt-get upgrade
sudo apt-get install build-essential fakeroot
sudo apt-get build-dep openvswitch
sudo apt-get install graphviz -y
```
下載及解壓縮

```
mkdir OVS ; cd OVS
wget http://openvswitch.org/releases/openvswitch-2.5.0.tar.gz
tar -zxvf openvswitch-2.5.0.tar.gz
cd openvswitch-2.5.0/
```

檢查一下是否有未安裝的套件

	dpkg-checkbuilddeps

如果有訊息，代表有未安裝的套件，再使用apt-get install安裝就好，如果沒有代表都安裝了。

編譯(需要一段時間)

	DEB_BUILD_OPTIONS='parallel=4 nocheck' fakeroot debian/rules binary

安裝

```
cd ..
sudo dpkg -i *.deb
```

如果無法安裝，缺少依賴套件的話，推薦gdebi這個工具

	sudo apt-get install gdebi
用法為

	sudo gdebi *.deb


lsmod | grep openvswitch 確認module有起來

```
openvswitch           196608  0
nf_defrag_ipv6         36864  1 openvswitch
nf_conntrack          106496  1 openvswitch
gre                    16384  1 openvswitch
libcrc32c              16384  2 xfs,openvswitch
```

重開機之後會變正常
```
openvswitch           196608  0
nf_defrag_ipv6         36864  1 openvswitch
nf_conntrack          106496  1 openvswitch
gre                    16384  1 openvswitch
libcrc32c              16384  1 openvswitch
```

	
完成，使用 ovs-vsctl show 可以看到一串識別碼

 sudo ovs-vsctl show
 
看到下面 代表成功，識別碼不一定會長一樣。

>bd2ccba3-2adc-40ab-8e3b-9579c38ca88a
>ovs_version: "2.5.0"

# Reference
* [Open vSwitch](http://openvswitch.org/)
* [Open vSwitch GitHub](https://github.com/openvswitch/ovs/blob/master/INSTALL.md)
* [Lab 1: Virtual Technelogy: Kernel-based Virtual Machine (KVM)](http://www.cs.nchu.edu.tw/~snmlab/CloudMgnt201409/Lab1.html)

-------
* [回目錄](../README.md)

-------