# Ubuntu 14.04 編譯 Open vSwitch(2.5.0)
 
## Environment

> OS : Ubuntu 14.04 LTS Server 64 bit
> Kernel : 4.2.0-42-generic 

	uname -a

> Linux VM-Server 4.2.0-42-generic #49~14.04.1-Ubuntu SMP Wed Jun 29 20:22:11 UTC 2016 x86_64 x86_64 x86_64 GNU/Linux

事前安裝

	sudo aptitude install dh-autoreconf libssl-dev openssl

下載及編譯

```
mkdir OVS ; cd OVS
wget http://openvswitch.org/releases/openvswitch-2.5.0.tar.gz
tar -zxvf openvswitch-2.5.0.tar.gz
cd openvswitch-2.5.0/
./boot.sh
./configure --with-linux=/lib/modules/`uname -r`/build
make -j
sudo make install
sudo make modules_install
sudo modprobe gre
sudo modprobe openvswitch
sudo modprobe libcrc32c
```

lsmod |grep openvswitch 確認module有起來

```
openvswitch           196608  0
nf_defrag_ipv6         36864  1 openvswitch
nf_conntrack          106496  1 openvswitch
libcrc32c              16384  1 openvswitch
gre                    16384  1 openvswitch
```

設定OVS DB

```
sudo mkdir -p /usr/local/etc/openvswitch
sudo ovsdb-tool create /usr/local/etc/openvswitch/conf.db vswitchd/vswitch.ovsschema
```

開啟 ovsdb-server (SSL)

```
sudo ovsdb-server --remote=punix:/usr/local/var/run/openvswitch/db.sock \
                  --remote=db:Open_vSwitch,Open_vSwitch,manager_options \
                  --private-key=db:Open_vSwitch,SSL,private_key \
                  --certificate=db:Open_vSwitch,SSL,certificate \
                  --bootstrap-ca-cert=db:Open_vSwitch,SSL,ca_cert \
                  --pidfile --detach
```

初始化 ovs db

	sudo ovs-vsctl --no-wait init
	
開啟 ovs socket

	sudo ovs-vswitchd --pidfile --detach
	
完成，使用 ovs-vsctl show 可以看到一串識別碼

	sudo ovs-vsctl show



# Reference
* [Open vSwitch](http://openvswitch.org/)
* [Open vSwitch GitHub](https://github.com/openvswitch/ovs/blob/master/INSTALL.md)
* [編譯 OpenvSwitch v2.3.1 on Ubuntu 14.04.1 LTS](http://roan.logdown.com/posts/220671-compile-openvswitch-v230-on-ubutnu-14041-lts)
* [Lab 1: Virtual Technelogy: Kernel-based Virtual Machine (KVM)](http://www.cs.nchu.edu.tw/~snmlab/CloudMgnt201409/Lab1.html)

-------
* [回目錄](../README.md)

-------