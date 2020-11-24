---
title : "ORB-SLAM2 with WebCam(Monocular & RGBD)"
category :
    - tips
tag :
    - ORB-SLAM2
    - tips
toc : true
toc_sticky: true
comments: true
---  
ORB-SLAM2를 실시간으로 구동해보자!  

Toy project를 진행하면서 ORB-SLAM2를 실시간으로 구동을 하게 되었다.  

Moncular 모드와 RGB-D 모드를 실시간으로 돌릴 수 있도록 소스코드를 조금 변경하고,  
그것을 빌드하는 과정을 정리하고자 한다.  

참고로 실습은 Ubuntu 18.04에서 진행을 했다.  

## 1. 소스 코드 다운 받기  
내가 진행한 toy project에 대한 소스 코드는 [Depth-estimation-with-ORB-SLAM2](https://github.com/Taeyoung96/Depth-estimation-with-ORB-SLAM2)에서 확인할 수 있다.  

먼저 터미널에서  
`git clone https://github.com/Taeyoung96/Depth-estimation-with-ORB-SLAM2.git`  
를 입력하여 소스코드를 다운 받는다.  

사실 레파지토리에 있는 소스코드는 ORB-SLAM2와 거의 동일하다.  

만약 ORB-SLAM2의 소스코드를 이용해서 실시간으로 구동을 원한다면,  
`/Examples/Monocular/mono_tum.cc`의 소스 코드와,  
`/Examples/RGB-D/rgbd_tum.cc`의 소스 코드만 복사하여 빌드를 하면 된다.  

## 2. ORB-SLAM2 설치하기  
ORB-SLAM2는 여러 라이브러리를 사용하기 때문에 실행하는 것도 까다롭다.  

먼저 레파지토리로 디렉토리 경로를 이동해준다.  
(아니면 ORB-SLAM2 디렉토리가 되겠죠?) 
`cd Depth-estimation-with-ORB-SLAM2`  

OpenGL과 관련된 패키지를 다운 받는다.  
`sudo apt-get install libglew-dev`  

USB video devices에 필요한 소스 코드를 git clone 해준다.  
`git clone http://github.com/ktossell/libuvc`  
`cd libuvc`  

libuvc를 빌드를 해줍니다.  
`mkdir build`  
`cd build`  
`cmake ..`  
`make && sudo make install`  

빌드를 완료 하면 다시 디렉토리를 'Depth-estimation-with-ORB-SLAM2'로 이동합니다.  
(아니면 ORB-SLAM2 디렉토리가 되겠죠?)  
`cd ..` (2번)  

Thirdparty 폴더에 들어간 후,  
`cd Thirdparty/`  

Pangolin에 대한 소스 코드를 git clone 해줍니다.  
`git clone https://github.com/stevenlovegrove/Pangolin.git`  

아까와 동일하게 빌드를 하는 과정을 거칩니다.  
`cd Pangolin`  
`mkdir build`  
`cd build`  
`cmake ..`  
`make -j8`  
`sudo make install`  

참고로 저는 이 빌드 과정에서 nvidia 드라이버와 호환이 좋지 않아서 삽질을 엄청 했습니다 😢  
nvidia driver와 호환이 좋지 않았던 것 같은데, 결국 nvidia driver를 다시 지우고 설치했더니  
Pangolin이 정상적으로 빌드가 이루어 졌습니다.  

정상적으로 빌드가 완료가 되었다면!  
다시 'Depth-estimation-with-ORB-SLAM2'로 돌아옵니다.  

`cd ..` (3번 반복)  

전체적으로 빌드를 진행해 줍니다!  
`chmod +x build.sh`  
`sh build.sh`  

만약 빌드 과정에서 **error: usleep is not declared in this scope**라는 오류가 뜬다면,  
`include/` 폴더 안에 있는 `System.h`에 `#include <unistd.h>`를 추가해 줍니다.  

드디어 길고 길었던 ORB-SLAM2 설치가 끝났습니다.  

## 3. ORB-SLAM2에 librealsense 추가하기  

RGB-D 모드에서 depth Map 정보를 이용하기 위해서 librealsense를 이용합니다.  

우선 디렉토리 위치를 'Depth-estimation-with-ORB-SLAM2'로 이동한 후, Thirdparty 폴더에 들어갑니다.  
`cd Thirdparty/`  

librealsense에 해당하는 소스코드를 git clone 해줍니다.  
`git clone https://github.com/IntelRealSense/librealsense.git`  

빌드를 진행합니다.  
`cd librealsense/`  
`mkdir build`  
`cd build`  
`cmake ..`  
`make`  
`sudo make install`  

정상적으로 빌드를 완료 하였다면, 이제 CmakeLists.txt를 수정해줍시다!  

## 4. ORB-SLAM2 CmakeLists.txt 수정하기  
'Depth-estimation-with-ORB-SLAM2' 폴더에서 CmakeLists.txt를 클릭합니다.  

아니면 터미널에 `gedit CmakeLists.txt`를 입력한다.  

 'target_link_libraries'라는 부분에 `${realsense2_LIBRARY}`를 추가합니다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/100112331-76602180-2eb2-11eb-885c-0fa16321a91d.png" width = "500" ></p>  
<p align="center"> 위에 보이는 부분에 추가하면 됩니다. </p>  

또 한 가지를 수정해줘야 하는데 find_package에 librealsense를 추가합니다.  
<p align="center"><img src="https://user-images.githubusercontent.com/41863759/100112302-6e07e680-2eb2-11eb-85b6-0e1f81993c8f.png" width = "350" ></p>  

<p align="center"> 👇👇👇 </p> 
 
<p align="center"><img src="https://user-images.githubusercontent.com/41863759/100112308-6fd1aa00-2eb2-11eb-9c87-169871891ae8.png" width = "350" ></p>  

전체적인 빌드를 다시 해줍니다.  
`sh build.sh`  

## 5. ORB-SLAM2 구동하기  

드디어 길었던 빌드의 과정이 끝났습니다!  

- Monocular 모드로 실시간으로 구동하기  
우선, `/Examples/Monocular/mono_tum.cc`에서 `#define UsingWebCam`을 활성화 시켜줍니다.  

**소스 코드 안에 카메라 index를 자신에 맞게 꼭 변경해주세요!**

그리고 터미널에 `./Examples/Monocular/mono_tum Vocabulary/ORBvoc.txt ./Examples/Monocular/d435.yaml`  

잘 따라오셨다면 정상적으로 작동할 겁니다!

카메라를 리얼센스를 사용했기 때문에 그에 맞는 .yaml 파일을 사용하였습니다.  

- RGB-D 모드로 실시간으로 구동하기  
우선, `/Examples/RGB-D/rgbd_tum.cc`에서 `#define UsingWebCam`을 활성화 시켜줍니다.  

그리고 터미널에 `./Examples/RGB-D/rgbd_tum Vocabulary/ORBvoc.txt ./Examples/RGB-D/d435.yaml `  

RGB-D 카메라를 이용해서도 실시간으로 ORB-SLAM2를 구동할 수 있습니다.  

## Reference 
- [ORB SLAM2 설치](https://robot-vision-develop-story.tistory.com/5)





















