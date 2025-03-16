---
title : "3D LiDAR PTP Setting Tutorial"
category :
    - SlamTip
tag :
    - 3D-LiDAR
    - PTP
    - tips
toc : true
toc_sticky: true
comments: true
---  

LiDAR PTP setting에 대해서 알아보자!

최근 Obsidian을 활용하면서 블로그에 포스팅을 하는 빈도수가 상당히 줄었다.. 하지만 꼭 기록하고 싶은 내용은 차근차근 포스팅을 진행해보겠다.  

## PTP란?

Precision Time Protocol의 줄임마로 네트워크 상의 장치들이 정밀한 시간 동기화를 위해 서로 메시지를 교환하는 표준 프로토콜을 의미한다.

- **메시지 교환:**
    - **Sync:** 마스터가 현재 시간을 전송
    - **Follow_Up:** 정확한 타임스탬프 추가 정보 전송 (Two-step clock 방식)
    - **Delay_Request/Delay_Response:** 네트워크 지연을 측정하여 시간 오프셋 계산
- **동작 원리:**  
    이 메시지들을 이용해 슬레이브는 마스터와의 시간 차이를 계산하고 클럭을 조정.

보통 마스터는 하나의 PC를 의미한다.


## 필요한 이유?

LiDAR SLAM 알고리즘들을 돌리기 위해서 보통 point당 timestamp를 활용하는데 이 때 제공되는 timestamp가 만약 PC와 동기화가 안되어 있다면, 센서 퓨전 및 SLAM 알고리즘을 수행하기 상당히 어렵기 때문이다.

따라서 PTP setting은 Calibration과 함께 필수적으로 진행되어야 한다! 어떻게 보면 PTP setting을 진행하는 것이 Temporal calibraiton의 첫번째 과정으로 봐도 되지 않을까 싶다.

<p align="center"><img src="https://github.com/user-attachments/assets/3414e8d9-a76a-48b8-a689-84df2dc4819c" width = "500" ></p>  

위 그림과 같이 PTP를 진행하면 slave (LiDAR 입장)에서 master time과의 Transmission delay와 Time offset을 알 수 있다.

## 센서 Time synchronization를 맞추는데 PTP 방법 밖에 없을까?

당연히 PTP 이외에도 다른 방법으로 time synchronization을 맞출 수 있다. 크게 두 가지 방법이 있는데 **물리적 전기 신호(Pulse Per Second)를 사용하거나 INS나 GNSS 수신기가 제공하는 정밀한 펄스로 시스템을 동기화를 진행**한다.

하지만 두 방법 모두 추가적인 하드웨어 (및 신호)가 필요하기 때문에 유연성과 확장성을 고려한다면 PTP가 최선의 선택이다.


---

## PTP 진행하기

PTP 세팅을 진행하기 전, 몇 가지를 알아 봐야 한다.

1. 가지고 있는 센서가 PTP 동기화 기능을 지원하는가?
2. 네트워크 카드에서 하드웨어 타임스탬핑 및 멀티캐스트를 지원하는가?
3. Master (주로 PC)와 센서는 ethernet으로 바로 연결하는가? 아니면 공유기를 거치는가?

	1,2번의 경우 필수적으로 알아봐야 하는 부분이고, 3번은 직접 ethernet연결을 하는 것이 아니라면 조금 더 고려해야할 사항이 늘어나기 때문이다. 공유기를 사용하는 경우는 뒤에서 좀 더 이야기 해보자.

PTP를 사용하기 위해 먼저 필요한 패키지들에 대해서 정리해보자.

```bash
sudo apt update
sudo apt install linuxptp chrony ethtool tshark
```

각각의 패키지는 아래의 역할 때문에 필요로 하다.

- ***linuxptp*** : PTP를 구현하는 데몬과 관련 도구(예: ptp4l, phc2sys)를 제공
- ***ethtool*** : 네트워크 인터페이스의 기능 및 설정(특히 하드웨어/소프트웨어 타임스탬핑 지원 여부)을 확인하고 조정할 수 있는 유틸리티
- ***tshark*** : 네트워크 트래픽을 캡처하고 분석할 수 있는 커맨드라인 기반 패킷 분석 도구입니다(Wireshark의 CLI 버전)
- ***chrony*** : NTP 프로토콜 기반 시간 동기화 도구로, 시스템 시계(CLOCK_REALTIME)를 외부 시간 소스(예: NTP 서버 또는 PTP 하드웨어 클럭)와 동기화

chrony의 경우 PTP 설정 자체에는 크게 관여하지 않고 NTP 클라이언트/서버로서 시스템 시계를 외부 NTP 서버와 동기화하는 도구이다. 따라서 없어도 무방하다.


ethtool을 통해 Ethernet Hardware timestamp를 제공하는지 한번 알아보자.
예시에서는 `eno1` 포트를 사용했다.

실제로 사용할 때는 자신의 환경에 맞게 `ifconfig`에서 원하는 포트를 알아보자.

```bash
sudo ethtool -T eno1

Time stamping parameters for eno1:
Capabilities:
        hardware-transmit     (SOF_TIMESTAMPING_TX_HARDWARE)
        software-transmit     (SOF_TIMESTAMPING_TX_SOFTWARE)
        hardware-receive      (SOF_TIMESTAMPING_RX_HARDWARE)
        software-receive      (SOF_TIMESTAMPING_RX_SOFTWARE)
        software-system-clock (SOF_TIMESTAMPING_SOFTWARE)
        hardware-raw-clock    (SOF_TIMESTAMPING_RAW_HARDWARE)
PTP Hardware Clock: 0
Hardware Transmit Timestamp Modes:
        off                   (HWTSTAMP_TX_OFF)
        on                    (HWTSTAMP_TX_ON)
Hardware Receive Filter Modes:
        none                  (HWTSTAMP_FILTER_NONE)
        all                   (HWTSTAMP_FILTER_ALL)
```


### ptp4l, phc2sys 서비스 파일 만들기

- ***ptp4l*** : Linux 환경에서 IEEE 1588(PTP) 프로토콜을 구현하는 데몬
- ***phc2sys*** : phc2sys는 PTP 하드웨어 클럭(PHC)과 시스템 클럭(CLOCK_REALTIME) 간의 동기화를 담당하는 도구
- ***phc2shm*** : phc2shm은 PTP 하드웨어 클럭의 상태와 시간을 공유 메모리(shared memory)에 기록하여, 다른 애플리케이션이나 모니터링 도구가 이를 쉽게 읽어올 수 있도록 하는 역할

여담이지만 phc는 PTP Hardware Clock의 약자이다. 이번에는 따로 공유 메모리 기록은 필요없으니 ptp4l과 phc2sys만 설치를 해보자.

서비스 파일을 만드는 방법은 두 가지가 있다.
1. 베이스 파일 만들기 (\*.service)
2. 드롭인 파일 만들기 (override.conf)

 **베이스 유닛 파일**
- 목적:
    - 서비스에 대한 기본 설정을 정의하는 파일
    - 패키지 설치 시 또는 사용자가 처음부터 서비스를 새로 만들 때 작성
- 사용 기준:
    - 새 서비스를 만들거나, 기존에 제공되지 않은 커스텀 서비스를 정의할 때.
    - 기본 동작, 실행 명령어, 의존성 등 전체 서비스 설정을 처음부터 설정할 경우.

**드롭인(override) 파일**
- 목적:
    - 기존의 베이스 유닛 파일을 직접 수정하지 않고, 일부 설정만 변경하거나 추가할 때 사용
    - 패키지 업데이트 시 변경사항이 덮어씌워지는 것을 방지할 수 있다.
- 사용 기준:
    - 이미 설치된 서비스의 기본 유닛 파일을 수정하지 않고 추가 옵션이나 수정이 필요할 때.
    - 운영 환경에서 패키지 기본 설정을 유지하면서 사용자 지정 설정만 재정의하고 싶을 때.

이번 포스팅에서는 드롭인 파일을 활용하여 서비스 파일을 만드는 방법에 대해서 설명하겠다.

ptp4l과 관련된 override.conf를 만들어주기 위해 먼저 아래와 같은 경로를 만들어준다.

```bash
sudo mkdir -p /etc/systemd/system/ptp4l.service.d
```

그리고 위 경로에 override.conf 파일을 만들어준다.
```
sudo nano /etc/systemd/system/ptp4l.service.d/override.conf
```


아래의 내용을 입력
```
[Service]
ExecStart=
ExecStart=/usr/sbin/ptp4l -f /etc/linuxptp/ptp4l.conf -i eno1
```

nano에서는 `Ctrl+O`를 눌러 저장한 후, `Ctrl+X`를 눌러 종료
서비스 파일을 reload 및 재시작을 진행해준다.

```bash
sudo systemctl daemon-reload
sudo systemctl restart ptp4l
sudo systemctl status ptp4l
```


마찬가지로 phc2sys와 관련된 override.conf를 만들어주기 위해 아래와 같은 경로를 만든다.

```bash
sudo mkdir -p /etc/systemd/system/phc2sys.service.d
```

그리고 위 경로에 override.conf 파일을 만들어준다.
```bash
sudo nano /etc/systemd/system/phc2sys.service.d/override.conf
```

```
[Service]
ExecStart=
ExecStart=/usr/sbin/phc2sys -w -O 0 -s CLOCK_REALTIME -c enp5s0
```

서비스 파일을 reload 및 재시작을 진행해준다.
```bash
sudo systemctl daemon-reload
sudo systemctl restart phc2sys
sudo systemctl status phc2sys
```


### PTP 동작 확인
서비스 파일의 status가 제대로 동작하고 있으면 작동을 하는지 한번 살펴보자.

1. `tshark`를 통한 확인
네트워크에서 PTP가 제대로 일어나고 있는지 확인할 수 있는 방법이다.
```
sudo tshark -i eno1 -Y ptp
```

2. `ros2 topic delay`를 통한 확인 
만약 바로 LiDAR topic이 publish 되는 상황이라면 간단히 topic으로 확인해볼 수 있다.
delay의 경우 Subscribe한 메시지의 헤더에 포함된 타임스탬프(보통 발행 시각)와 메시지를 수신한 시점(현재 시스템 시간) 사이의 차이를 계산하여, 네트워크 전송 지연(레이턴시)을 측정하기 때문이다.

0.05s 이내로 topic delay가 들어온다면 PTP setting이 적용이 된 것으로 볼 수 있다.

```
ros2 topic delay [LiDAR topic] -w 5
```


3. `pmc` 명령어를 통한 Log 확인
만약 PTP 세팅 설정을 해줬는데도 PTP가 적용이 되지 않는다면 아래의 명령어로 Log를 보자.
```
pmc -u -b 0 "GET TIME_STATUS_NP"
```

위 Log를 통해 서비스 파일에 `--utc_offset=37`과 같은 파라미터가 필요한지 아닌지 판단할 수 있다.

### 만약 공유기를 사용하는 환경이라면?

앞서 PTP를 사용하는 조건에서 공유기를 사용할 때는 **공유기 세팅도 살펴볼 필요가 있다.**
포트포워딩 사용 여부이나 멀티캐스트 지원 여부 그리고 NAT 및 방화벽 기능을 공유기에서 수행할 경우 PTP가 제대로 동작하지 않을 수 있다.

위 경우는 변수가 너무 많으므로, 그때그때 GPT를 활용하자.. :\)

---

이렇게 LiDAR PTP setting과 관련된 포스팅을 마치겠다. 추후 GPS 및 PPS 신호도 이용해보면 한번 정리해봐야겠다!


## Reference
- https://livox-wiki-en.readthedocs.io/en/latest/tutorials/new_product/common/time_sync.html#time-synchronization-instructions
- https://static.ouster.dev/sensor-docs/image_route1/image_route2/appendix/ptp-quickstart.html#ptp-quickstart-guide
- https://discourse.ros.org/t/experience-with-ptp-precision-time-protocol-for-mobile-robots/24707/6