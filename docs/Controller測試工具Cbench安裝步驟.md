# Controller測試工具Cbench安裝步驟
* 下載依賴套件

        $ sudo apt-get install autoconf automake libtool libsnmp-dev libpcap-dev

* 安裝OpenFlow

        $ git clone git://gitosis.stanford.edu/openflow.git
        $ cd openflow
        $ git checkout -b mybranch origin/release/1.0.0
        $ cd..
        
* 安裝oflops

        $ git clone git://gitosis.stanford.edu/oflops.git
        $ cd oflops
        $ git submodule init
        $ git submodule update
        $ cd ..
        
* 安裝libconfig

        $ wget http://www.hyperrealm.com/libconfig/libconfig-1.4.9.tar.gz
        $ tar -xvzf libconfig-1.4.9.tar.gz
        $ cd libconfig-1.4.9
        $ ./configure
        $ sudo make
        $ sudo make install
        $ cd ../oflops/netfpga-packet-generator-c-library/
        $ ./autogen.sh
        $ ./configure
        $ make
        $ cd ..
        $ sh ./boot.sh
        $ ./configure --with-openflow-src-dir=
        
    dir後面接的是openflow的絕對路徑，例如/home/user/openflow
    
    ( ./configure --with-openflow-src-dir=/home/user/openflow )
        
        $ make
        $ sudo make install

    結束安裝。

# Reference:
* [Cbench性能測試工具驗證SDN網絡](http://toutiao.com/i6223263426622259713/)
* [SDN控制器性能測試工具Cbench安裝與使用說明](http://www.sdnlab.com/2334.html)

