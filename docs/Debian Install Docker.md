# Debian Install Docker 

## 安裝環境


	lsb_release -a

```
No LSB modules are available.
Distributor ID:	Debian
Description:	Debian GNU/Linux 8.6 (jessie)
Release:	8.6
Codename:	jessie
```

	uname -a
	
>Linux debian 3.16.0-4-amd64 #1 SMP Debian 3.16.36-1+deb8u1 (2016-09-03) x86_64 GNU/Linux
	
## 事前工作

先確定自己的 kernel 版本 > 3.1
	
	uname -r
	
> 3.16.0-4-amd64

將先前的版本移除

```
sudo apt-get purge "lxc-docker*"
sudo apt-get purge "docker.io*"
```

更新套件資訊，安裝apt支援https以及安裝ca認證
```
sudo apt-get update
sudo apt-get install apt-transport-https ca-certificates
```

add GPG key

	sudo apt-key adv --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys 58118E89F3A912897C070ADBF76221572C52609D
	
編輯 /etc/apt/sources.list.d/docker.list 檔案，如果沒有請建立

```
sudo nano /etc/apt/sources.list.d/docker.list
```
將下列加入上述檔案

- On Debian Wheezy

``` 
deb https://apt.dockerproject.org/repo debian-wheezy main
```

- On Debian Jessie

``` 
deb https://apt.dockerproject.org/repo debian-jessie main
```

- On Debian Stretch/Sid

``` 
deb https://apt.dockerproject.org/repo debian-stretch main
```

更新套件

	sudo apt-get update
	
確認是否找的到 docker

	apt-cache policy docker-engine

## 安裝Docker

安裝

	sudo apt-get install docker-engine
	
背景執行

	sudo service docker start

確認是否安裝成功

	sudo docker run hello-world
	
正確會看到以下訊息

```
aven@debian:~$ sudo docker run hello-world
Unable to find image 'hello-world:latest' locally
latest: Pulling from library/hello-world
c04b14da8d14: Pull complete 
Digest: sha256:0256e8a36e2070f7bf2d0b0763dbabdd67798512411de4cdcf9431a1feb60fd9
Status: Downloaded newer image for hello-world:latest

Hello from Docker!
This message shows that your installation appears to be working correctly.

To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.

To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash

Share images, automate workflows, and more with a free Docker Hub account:
 https://hub.docker.com

For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```

# Reference
* [Install Docker Engine on Linux Debian](https://docs.docker.com/engine/installation/linux/debian/)

-------
* [回目錄](../README.md)

-------
