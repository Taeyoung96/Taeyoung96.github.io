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

스터디를 진행하기 전 먼저 내 Local computer에서 돌려보려 하는데,  
역시 한번에 빌드가 되지 않아 정리를 하려고 한다.  

ORB SLAM3의 기본적인 구조는 ORB SLAM2와 비슷하다.  

요구하는 라이브러리도 거의 비슷하여 예전에 작성한 [ORB-SLAM2 with WebCam](https://taeyoung96.github.io/slamtip/ORBwithWeb/)를 참고하여  
빌드를 진행했다.  

## Development enviornment  

현재 ORB SLAM3의 Build를 진행한 개발 환경은 다음과 같다.  

- OS : Ubuntu 18.04  
- Gcc : 7.5.0  
- OpenCV : 3.4.5  
- Eigen : 3.3.4  
- OpenSSL : 1.1.1  
- Python : 2.7.17  

Github에 이슈를 보니 Eigen 버전에 따라 오류가 많이 생기는 것 같았다.  
참고해서 빌드를 진행하자!  

## How to build ORB SLAM3  

먼저 터미널에서 git clone을 이용해 소스코드를 다운받는다.  

`git clone https://github.com/UZ-SLAMLab/ORB_SLAM3.git`  

그 다음 ORB_SLAM3 폴더 안으로 들어간다.  

`cd ORB_SLAM3`  

그 다음 Thirdparty 폴더에 들어가 Pangolin 라이브러리를 다운 받는다.  

`cd Thirdparty/`  

그럼 경로가 다음과 같이 수정되었을 것이다.  
`~/Desktop/ORB_SLAM3/Thirdparty$`  
(나 같은 경우 바탕화면에서 진행을 했다.)  

이제 Pangolin을 빌드를 진행해보자.  
순서대로 터미널에 입력하면 빌드가 이뤄진다.  

```
git clone https://github.com/stevenlovegrove/Pangolin.git  
cd Pangolin  
mkdir build  
cd build  
cmake ..  
make -j8  
sudo make install  
```




