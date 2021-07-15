---
title : "ORB-SLAM3 빌드 (Ubuntu 18.04)"
category :
    - SlamTip
tag :
    - ORB-SLAM3
    - tips
toc : true
toc_sticky: true
comments: true
---  
Ubuntu 18.04에서 ORB-SLAM3를 빌드해보자!  

좋은 기회로 ORB SLAM3를 스터디할 기회가 생겼다.  

스터디를 진행하기 전 먼저 내 Local computer에서 돌려보려 하는데, 역시 한번에 빌드가 되지 않아 정리를 하려고 한다.  

ORB SLAM3의 기본적인 구조는 ORB SLAM2와 비슷하다.  

요구하는 라이브러리도 거의 비슷하여 예전에 작성한 [ORB-SLAM2 with WebCam](https://taeyoung96.github.io/slamtip/ORBwithWeb/)를 참고하여 빌드를 진행했다.  

## Development enviornment  

현재 ORB SLAM3의 빌드를 진행한 개발 환경은 다음과 같다.  

- OS : Ubuntu 18.04  
- Gcc : 7.5.0  
- OpenCV : 3.4.5  
- Eigen : 3.3.4  
- OpenSSL : 1.1.1  
- Python : 2.7.17  
