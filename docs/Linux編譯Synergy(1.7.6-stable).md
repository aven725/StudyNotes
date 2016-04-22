# linux編譯Synergy(1.7.6-stable)
 
```
git clone https://github.com/symless/synergy.git
cd synergy
git checkout -b v1.7.6-stable
sudo apt-get install build-essential cmake libavahi-compat-libdnssd-dev libcurl4-openssl-dev libssl-dev python qt4-dev-tools xorg-dev
./hm.sh conf -g1 [-d] # Use -d to build a debug version.
./hm.sh build [-d]
```

Building installers

	./hm.sh package deb

or 

```
cd bin
sudo dpkg -i synergy-v1.7.6-stable-stable-257ac01-Linux-x86_64.deb
```
放到桌面

	cp /usr/share/applications/synergy.desktop ~/Desktop/

# Reference
* [Synergy Github](https://github.com/symless/synergy/wiki/Compiling#Debian_8)

-------
* [回目錄](../README.md)

-------