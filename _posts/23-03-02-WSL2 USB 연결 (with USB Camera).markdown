---
layout: post
title:  "WSL2 USB 연결 (with USB Camera)"
date:   2023-03-02 21:03:36 +0530
categories: WSL
---
### Linux Kernel Update

Microsoft 에서 WSL2 의 USB Device 연결 필수 구성 요소를 보면 Linux Kernel Version 5.10.60.1 이상을 요구하고 있다.

![](https://velog.velcdn.com/images/swooeun/post/6e644a4e-369a-4e51-b09e-60ddf016fcf8/image.png)

WSL 에서 Linux 커널 버전 확인

* ```uname -a```

버전이 5.10.60.1 이상이라도 menuconfig 세팅을 커스텀해야하므로 다시 다운로드 해야 한다.

종속성 설치

```
sudo apt update && sudo apt upgrade -y && sudo apt install -y build-essential flex bison libgtk2.0-dev libelf-dev libncurses-dev autoconf libudev-dev libtool zip unzip v4l-utils libssl-dev python3-pip cmake git iputils-ping net-tools dwarves
```

Sources dir 생성 & 커널 다운로드
```
mkdir -p /mnt/c/Sources
cd /usr/src
VERSION=5.15.57.1
sudo git clone -b linux-msft-wsl-${VERSION} https://github.com/microsoft/WSL2-Linux-Kernel.git ${VERSION}-microsoft-standard && cd ${VERSION}-microsoft-standard
sudo cp /proc/config.gz config.gz
sudo gunzip config.gz
sudo mv config .config
```

menuconfig 실행 (```/usr/src/5.15.57.1-microsoft-standard``` 위치에서)

```
sudo make menuconfig
```

아래와 같이 설정 후 저장 (space 2번 누르면 * == built-in)

Device Drivers --->

	<*> Multimedia support

		[*] Filter media drivers

		[*] Autoselect ancillary drivers

		Media device types --->

			[*] Cameras and video grabbers

		Video4Linux options --->

			[*] V4L2 sub-device userspace API

		Media drviers

			[*]  Media USB Adapters --->

				<*> USB Video Class (UVC)

				[*] UVC input events device support

				<*> GSPCA based webcams ----
                
![](https://velog.velcdn.com/images/swooeun/post/ae55d687-0557-4f7c-bb73-69c704a46fb0/image.png)

```
sudo make -j$(nproc)

sudo make modules_install -j$(nproc)
sudo make install -j$(nproc)

sudo cp -rf vmlinux /mnt/c/Sources/
exit

# windows powershell 에서
wsl --shutdown
```

```C:\Users\your_username``` 으로 이동해서 .wslconfig 파일을 만들어준다
![](https://velog.velcdn.com/images/swooeun/post/0d8f67fd-35fc-4103-8592-86b80f686c69/image.png)

```
[wsl2]
kernel=C:\\Sources\\vmlinux
```
### usbipd 설치
윈도우 PC에 설치
[dorssel/usbipd-win](https://github.com/dorssel/usbipd-win/releases)

![](https://velog.velcdn.com/images/swooeun/post/f3d7bc39-30e3-4732-aa61-f7f3f08af798/image.png)

WSL 에 usbipd 설치
```
sudo apt install linux-tools-virtual hwdata
sudo update-alternatives --install /usr/local/bin/usbip usbip `ls /usr/lib/linux-tools/*/usbip | tail -n1` 20
```
마운트
**powershell 에서 ‘관리자 모드’로 실행 해야 함!!**

```
usbipd wsl list
usbipd wsl attach --busid <busid> (ex: 3-1)

//연결 해제시
usbipd wsl detach --busid <busid> (ex: 3-1)
```

WSL 에서 확인
```
ls -l /dev/ttyUSB*
or
ls -l /dev/video*
```

![](https://velog.velcdn.com/images/swooeun/post/ffa052d5-1fc7-4854-9ac7-723625534a31/image.png)

