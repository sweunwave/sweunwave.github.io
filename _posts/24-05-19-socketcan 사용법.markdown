---
layout: post can
title:  "socketcan 사용법"
date:   2024-05-19 21:03:36 +0530
categories: 
---
자율주행 대회에서 CAN 개발을 맡게 되었다.

그동안은 특정 변수에 값을 보내기만 하면 제어가 되도록 통신이 다 만들어진 차량에서만 개발을 해왔는데, 처음해보려니 생소하다.

목표는 CAN Data를 ROS2에서 송수신할 수 있도록 하는 것.

개발 환경은  
* ROS2 Humble
* Ubuntu 22.04
* Kvaser Leaf Light2

개발 순서는

### **수신 Rx**
1. socketcan으로 ubuntu와 CAN 케이블 연결
2. ros2-socketcan driver로 can raw 데이터 수신
3. 제공받은 table에 맞게 can msg 정의
4. 3번에서 정의한 msg를 사용하여 rostopic 생성
   
### **송신 Tx**
1. 제공받은 table에 맞게 encoding 개발
2. 송신부 개발 (아직 ros2-socketcan driver를 완벽히 파악못해서 만들어야 하는 건지도 모르겠음)

여기서는 수신 2번까지의 결과를 기록한다.

### 개발 과정
![alt text](/images/can1.png)
![alt text](/images/can2.png)

연구실에 Ethernet과 연결된 D-sub 9-pin이 있길래 해체해서 2번과 7번과 전선을 납땜하여 CAN 용으로 만들었다.   
전선은 24 awg 규격을 사용하였음.

### **수신**
1. tool 설치
```
sudo apt-get install can-utils
sudo apt-get install net-tools
sudo apt-get install libmuparser-dev
```

2. 컴퓨터와 can 장치 연결
* Kvaser를 사용하는 경우 (물리적으로 연결하는 경우)
```
sudo modprobe can    
sudo modprobe kvaser_usb    
sudo ip link set can0 type can bitrate 500000    
sudo ifconfig can0 up
```

* vcan으로 개발하는 경우 (가상 can)  
실제로는 항상 can 케이블을 연결한 채로 개발 할 수 없기 때문에 vcan을 사용하여 테스트하며 개발하려함.
```
sudo modprobe vcan
sudo ip link add dev vcan0 type vcan
sudo ip link set vcan0 txqueuelen 1000
sudo ip link set up vcan0
```

```candump can0```  
혹은  
```candump vcan0```  

으로 데이터 확인할 수 있음.

![alt text](/images/can3.png)

두서없이 적어서 내용이 어지럽습니다.