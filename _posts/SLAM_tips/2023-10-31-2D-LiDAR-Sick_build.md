---
title : "[ROS2] 2D LiDAR Sick TIM 571 package 설치 및 IP setting"
category :
    - SlamTip
tag :
    - 2D-LiDAR
    - tips
toc : true
toc_sticky: true
comments: true
---  

2D LiDAR Sick TIM 571 제품의 IP setting 및 ROS2 package를 설치해보자.  

최근 연구실에서 새로운 로봇과 관련해 세팅을 해야할 일이 있는데 정리를 해두면 추후 편할 것 같아 포스팅을 진행하려고 한다.  

Velodyne 3D LiDAR를 세팅할 때도 고생을 좀 했지만, Sick의 경우도 마찬가지로 IP setting을 진행해 줘야 한다.  

**여기서 중요한건 Sick LiDAR의 경우 IP setting을 Window에서만 진행이 가능하다!!**  

따라서 나는 듀얼부팅이 되는 노트북을 활용했다. 현재 내가 사용한 운영체제는 다음과 같다.  
- Window 11
- Ubuntu 20.04  

## (Window) Sick LiDAR 관련 소프트웨어 설치  

우선 Sick 2D LiDAR를 PC와 ethernet 연결을 해줬다는 가정하에 시작을 하겠다.  
(당연히 2D LiDAR에 전원도 연결해준 상태)   

먼저, Sick 2D LiDAR의 [공식 홈페이지](https://www.sick.com/de/en/search?text=SOPAS%20Engineering%20Tool)에 들어가서 SOPAS engineering Tool 소프트웨어를 설치해준다.  
이 소프트웨어가 아쉽게도 .exe 파일로 되어있기 때문에 필수적으로 Window에서 실행을 해야한다.  

(혹시, 우분투에서도 가능한 방법이 있다면 댓글로 알려주시면 감사드리겠습니다. 😆)
{: .notice--info}

SOPAS engineering Tool을 실행할 경우, 아래의 그림처럼 2D LiDAR가 잡히는 것을 확인할 수 있다.  
여기서 Add 버튼을 눌러준다.    

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/75ea5255-8eed-4bcf-86fb-6306d41e8e98" width = "700" ></p>  

Add를 누르게 될 경우, 아래 그림처럼 화면이 바뀌게 된다.  
현재는 IP setting을 올바르게 맞췄기 때문에 online 표시로 뜨지만, 그렇지 않을 경우 offline으로 표시가 뜬다.  

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/312d7e8f-3b41-4401-9f0e-7affe9170400" width = "700" ></p>  

위 그림에서 빨간색 부분을 클릭하게 되면 IP setting과 관련된 또 다른 창이 하나 나오게 된다.  

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/bb50b59d-63cd-44ef-92c8-9681a57cec7b" width = "500" ></p>  

위 그림에서 Automatic 부분을 누르게 되면 PC IP setting에 맞춰 2D LiDAR IP setting을 알아서 진행해준다.  

💥 **하지만 여기서 주의할 점이 한 가지가 있다.**  💥  

PC의 IP setting을 따라서 2D LiDAR setting을 맞추기 때문에, PC IP setting을 먼저 원하는 IP 주소로 맞춰야 한다.  

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/8eee7cc8-2208-4f04-96ba-e8c41cc00151" width = "500" ></p>  

위 setting 창에서는 IP 주소를 바꿀 수 없다.  

윈도우에서 네트워크 및 인터넷에서 "이더넷" 설정과 관련된 부분을 들어가자.  

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/91564441-0a48-420d-9e11-6d19d8f36052" width = "500" ></p>  

수동으로 IP 주소를 바꿔서 원하는 IP 주소로 변경을 진행하자!! 

현재, 필자의 경우는 `192.168.1.XXX` 로 되어있는 IP 주소가 필요했다. 왜냐하면 Ubuntu 20.04에서 `192.168.1.XXX` 로 IP를 할당했기 때문이다.  
**만약 원하는 IP 주소가 다를 경우, 이 PC IP setting부터 원하는 IP 주소로 setting을 진행해주면 된다.**  

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/6caf34a8-a779-4ec0-86a0-1e21544d2a60" width = "300" ></p>  

원하는 IP 주소로 setting을 진행하고, 2D LiDAR의 IP까지 automatic으로 잡아줬다면, 아래의 그림에서 빨간색 부분을 더블클릭 해보자.  
이때는 아래의 그림처럼 초록색으로 online이라는 표시가 떠야 정상이다.  

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/e7b0be6b-8bae-46fe-839d-39325eed1efc" width = "700" ></p>  

왼쪽 상단의 `Monitor`라는 폴더에서 `Scan view pro`를 눌렀을 때, 아래의 그림처럼 2D LiDAR의 data가 잘 출력된다면 성공이다! 👍

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/6376e187-cf2a-4abc-9b30-15de7d7d8361" width = "700" ></p>  

## (Ubuntu) Sick LiDAR ROS2 package 설치  

이제 윈도우를 빠져 나온 후, Ubuntu에서 ROS2 package를 실행하자.  

이전에 이야기했지만, 필자의 경우, Ubuntu에서 IP setting을 아래와 같이 `192.168.1.XXX`로 진행했다.  

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/f813f547-99b8-4717-9d61-2897c2d1efb2" width = "500" ></p>  

현재 ROS2 foxy 버전에서 테스트를 진행했다.  
ROS2와 관련된 설치는 이미 되어 있다고 가정하고 포스팅을 작성한다.  

[sick_scan2](https://github.com/SICKAG/sick_scan2) 패키지를 활용했다.  
링크를 들어가보니 현재는 관리가 되지 않는 repository고 조만간 제거가 될 repository라고 한다.  
그럴 경우, [sick_scan_xd](https://github.com/SICKAG/sick_scan_xd)를 통해서 패키지를 받아야 한다.  

패키지 빌드를 하는데 체감상 큰 Trubleshooting은 없었던 것으로 기억된다.  

이제 이전에 setting을 진행한 IP 주소를 `config` 폴더에 있는 `.yaml` 파일에 알맞게 설정하자.  
필자의 경우, Sick tim 571 제품을 사용하기 때문에, `sick_tim_5xx.yaml` 파일을 수정했다.  

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/162346bb-846b-4339-bb63-87dc59d02b14" width = "600" ></p>  

이전에 윈도우에서 설정했던 IP와 동일하게 주소를 넣어준다. 여기서 Port 번호도 주의해야 하는데, 보통 `2111` 아니면 `2112`로 물려있는 경우가 많다.  

세팅을 완료하고 두근두근한 마음으로 `colocn build`를 진행해준다.  
`source install/setup/bash`까지 완료하고 아래와 같은 launch 파일을 실행해준다.  

```
ros2 launch sick_scan2 sick_tim_5xx.launch.py 
```

만약 정상적으로 연결이 되었다면, 아래의 그림처럼 출력이 된다.  

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/2d19bbdf-4494-4d9d-98ff-95cc85ee5d5b" width = "700" ></p>  

이제 `rviz2`를 이용해서 scan data를 시각화해보면..! (참고로 fixed frame은 `laser`이다.)  

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/57c2d4f3-deb5-46d6-b50f-8e413692c080" width = "700" ></p>  

참으로 짜릿한 순간이 아니한가...  
이 data를 못받아서 삽질을 한 날들이 주마등처럼 스쳐 지나갔다.  

추후 미래의 나에게 또 여러 사람들의 시간을 절약하는 포스팅이 되었으면 한다.  

## Troubleshooting

IP setting과 관련해서 Ubuntu에서 몇 가지 시도해본 명령어들을 기록해본다.  

1. LAN선으로 LiDAR를 연결했는데 갑자기 WIFI로 사용하던 인터넷이 잡히지 않을 때  

이 때는 `route`의 우선순위가 바뀌었을 가능성이 상당히 높다.  
터미널을 열고 `route`라는 명령어를 입력해보자.  

<p align="center"><img src="https://github.com/Taeyoung96/Taeyoung96.github.io/assets/41863759/d2bdce48-3ceb-47d2-809b-20fb933d9d27" width = "600" ></p>  

Metric이 낮으면 낮을수록 우선순위가 높은 것이다. 따라서 Wifi 모듈의 우선순위를 높도록 설정을 진행해주면 해결된다.  

```
sudo apt install ifmetric
```

```
sudo ifmetric "Iface name" "우선순위"
```

필자의 위 그림을 예시로 들면 WIFI 모듈 (wsl0)의 Metric이 600이고, LAN port (enp108s0)의 Metric이 100이므로 현재 LAN port의 우선순위가 높은 상태이다.  

따라서,

```
sudo ifmetric wlo1 10
```

이렇게 WIFI의 모듈의 우선순위가 높도록 변경해준다.  

2. ping 확인 및 TCP/IP test  

- Ping test  
```
ping 192.168.1.XXX
```

- TCP/IP test  
```
nc -z -v -w5 192.168.1.200 2112
```

간단히 정리하면 ping 명령어의 경우 네트워크 호스트의 연결 가능성을 테스트하고,
TCP/IP의 경우, 특정 포트의 연결성을 테스트한다.  

GPT의 답변을 요약하면 아래와 같다.  


1. **nc (Netcat) 명령어**:
   - `nc -z -v -w5 192.168.1.200 2112`는 Netcat을 사용하여 특정 호스트와 포트에 연결을 시도합니다.
   - 옵션 설명:
     - `-z`: 연결만 시도하고 데이터를 전송하거나 수신하지 않습니다. (스캔 모드)
     - `-v`: 상세한 출력을 제공합니다. (verbose 모드)
     - `-w5`: 5초 동안 연결 시도 후 타임아웃됩니다.
   - 이 명령어는 192.168.1.200 주소의 2112 포트가 열려 있는지 확인하는 데 사용됩니다.

2. **ping 명령어**:
   - `ping 192.168.1.200`는 ICMP (Internet Control Message Protocol)를 사용하여 특정 호스트에 패킷을 전송하고 응답을 기다립니다.
   - 이 명령어는 네트워크 연결의 상태, 지연 시간, 패킷 손실 등을 확인하는 데 사용됩니다.
   - 대상 호스트가 응답하면 "ping"이 성공하며, 응답 시간도 출력됩니다.

**차이점**:
- `nc` 명령어는 특정 포트가 열려 있는지 확인하는 데 사용되며, `ping` 명령어는 호스트의 가용성 및 네트워크 연결 상태를 확인하는 데 사용됩니다.
- `nc`는 TCP/UDP 연결을 기반으로 하며, `ping`은 ICMP를 기반으로 합니다.
- `nc`는 특정 포트에 대한 연결 가능성을 테스트하는 반면, `ping`은 네트워크 상의 호스트 간의 연결 가능성을 테스트합니다.

두 도구 모두 네트워크 진단 및 테스트에 유용하지만, 사용되는 프로토콜과 목적이 다릅니다.

위 명령어들로 디버깅 할 때 도움을 받으면서 2D LiDAR setting을 완료했다.  

## Reference  

- [[Linux] 라우팅 테이블 설정(route, Metric 우선순위)](https://changun516.tistory.com/141)  
- [IP address configuration](https://github.com/SICKAG/sick_scan/blob/master/doc/ipconfig/ipconfig.md)  
- [[Sensor] Sick TiM571 윈도우 ip 및 뷰어 셋팅](https://changun516.tistory.com/225)  