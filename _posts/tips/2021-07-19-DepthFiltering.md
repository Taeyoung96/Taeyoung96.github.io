---
title : "[RealSense] Depth Filtering 적용"
category :
    - tips
tag :
    - Realsense
    - tips
toc : true
toc_sticky: true
comments: true
---  

Realsense D435에서 Depth filtering을 적용해보자!  

뎁스 카메라를 사용하여 Point cloud화를 시키거나, SLAM 등 여러 알고리즘을 사용할 때 생각했던 것  
보다 정확도가 높지 않았던 경우가 있다.  

이 때 **Depth filtering**을 활용하여 Depth 값의 값만 보정을 시켜도 정성적인 평가로 정확도가 올라갔던 경험이 있어 포스팅을 진행하고자 한다.  

Depth filtering을 진행한 뎁스 카메라는 **Real Sense D435**이며 [librealsense-ROS Wrapper](https://github.com/IntelRealSense/realsense-ros)를  
활용하여 ROS에서 쉽게 Depth filtering을 적용할 수 있다.  

Realsense Library에서 제공하는 필터의 종류는 총 4가지이다.  

간단하게 이 필터가 어떻게 동작하는지 설명하고,  
ROS를 활용하여 Depth filtering을 적용하는 방법을 알아보도록 하겠다.  

## Depth Filtering  

Depth filtering은 Post-precessing filter이고, Noise를 제거해주며, Depth의 Quality를 높여준다.  

### Decimation filter  

첫 번째로 알아볼 Filter는 'Decimation filter' 이다.  

이 필터를 이용하면 **Depth Image의 Scale을 줄여 Complexity를 줄일 수 있다.**  

필터의 크기에 따라 얼마나 Scale을 줄일 것인지도 정할 수 있다.  

예를 들어 (1280X720)의 Depth image를 Scale factor 3을 이용해 필터를 적용하면,  
[426.6666667, 240] >> [428,240] 이렇게 Scale이 변경된다.  

만약 Hole이 존재한다면, Zero-padding으로 처리한다.  

### Spatial Edge-Preserving filter  

두 번째로 알아볼 Filter는 'Spatial Edge-Preserving filter' 이다.  

이 필터를 적용하면 **Edge는 보존하면서 전체적인 Depth image에는 Smoothing 효과**를 낼 수 있다.  

이 필터에는 Filter Magnitude, Smooth Alpha, Smooth Delta,	 Hole Filling과 같은 여러 Parameter들이 존재한다. 이 Parameter들을 조절해보며 환경에 맞는 Filter을 사용하면 좋을 것이다.  

이 필터같은 경우, [Domain Transform for Edge-Aware Image and Video Processing](https://www.inf.ufrgs.br/~eslgastal/DomainTransform/#Downloads) 논문에 자세한 구현 방법이 나와있으니 좀 더 알고 싶은 사람들은 참고해보자.  

### Temporal filter  

다음 알아볼 Filter는 'Temporal filter'이다.  

이 필터를 적용하면 **이전 frame에 저장되어 있던 Depth value를 활용해, 현재 Depth data를 좀 더 개선시켜주는 Filter**이다.  

만약 pixel의 값이 없거나 Noise가 많이 낀 쓰레기 값이 들어온 경우, 역시 Parameter를 이용하여 조절이 가능하다.  

기본적으로는 Moving Average Filter를 활용하여 이전 frame의 Depth data를 활용한다.  

### Holes Filling filter  

마지막으로 알아볼 Filter는 'Holes Filling filter'이다.  

이 필터를 적용하면 Depth data를 얻지 못한 곳에서 주위 pixel 값을 활용하여 Outlier를 보정하는 필터이다.  

가까운 위치에 있는 pixel에서 값을 보정할 것인지, 먼 위치에 있는 pixel에서 값을 보정할 것인지 Parameter를 통해 정할 수 있다.  

## ROS를 활용한 Depth filtering 사용  

Ubuntu 18.04, ROS Melodic에서 Test를 진행했다.  

앞에서도 말했지만 [librealsense-ROS Wrapper](https://github.com/IntelRealSense/realsense-ros)를 활용하면 쉽게 Depth filtering을 적용할 수 있다.  

`catkin_ws` 폴더 안에 `src` 폴더가 있고, 그 안에 github 소스코드를 다운받는다.  

다음 빌드를 진행한다.  

나같은 경우 Realsense camera를 이용하여 Point cloud를 만들었는데,  
Depth filtering을 적용했을 때와 안했을 때 성능 차이가 확연히 드러났다.  

Point cloud를 만들 때, 사용한 launch 파일은  
```
roslaunch realsense2_camera rs_rgbd.launch
```

launch 파일에서 Parameter를 바꾸어주면 바로 Depth filtering 적용이 가능하다.  

먼저 `rs_rgbd.launch`를 열고,  
Argument에 Filter를 적용할 수 있는 부분에 적용하고 싶은 Filter의 이름을 적으면 된다.  

중복하여 Filter를 사용할 수도 있다.  

Line 84에 보면,

```
 <arg name="filters"             default=""/>
```
위와 같이 되어있는 것을 볼 수 있다.  

나같은 경우 [프로젝트](https://github.com/Taeyoung96/2021-Capstone-Design/blob/master/ROS_Package/src/realsense-ros/realsense2_camera/launch/rs_rgbd.launch#L84)에서 아래와 같이같이 Filter를 적용했다.  


```
<arg name="filters"             default="temporal,decimation,spatial,disparity"/> 
```

launch 파일이 실행되어 있는 동안, `rqt`를 활용하여 매개변수 값을 바꿔줄 수도 있다.  

실시간으로 파악하면서 환경에 맞는 최적의 매개변수 값을 찾아주면 된다.  

Depth filtering을 적용하기 전 Point cloud와 적용한 후 Point cloud의 차이는 아래와 같다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/126198907-3dc5d23b-dd46-46b9-8574-f4480c596d86.png" width = "300" ></p>  

<p align="center">< Depth filtering 적용하기 전 ></p>  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/126198928-6932ce43-a19f-4052-b15c-1a4fc394e552.png" width = "300" ></p>  

<p align="center">< Depth filtering 적용하기 후 ></p>

프로젝트에서 번호판을 Point cloud화 시키는 작업을 진행했는데, Depth filtering을 활용하여 평평한 번호판을 만들 수 있게 되었다. 😆  

## Reference  

- [D415의 이미지 필터링과 깊이 인텔® RealSense™ 어떻게 합니까?](https://www.intel.co.kr/content/www/kr/ko/support/articles/000036539/emerging-technologies/intel-realsense-technology.html)  
- [rs-post-processing Sample](https://github.com/IntelRealSense/librealsense/tree/master/examples/post-processing)  
- [Post-processing filters](https://dev.intelrealsense.com/docs/post-processing-filters#section-decimation-filter)  




