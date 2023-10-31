---
title : "[ROS2] 2D LiDAR Sick TIM 571 package 설치 및 실행"
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

혹시, 우분투에서도 가능한 방법이 있다면 댓글로 알려주시면 감사드리겠습니다. {: .notice--info}

