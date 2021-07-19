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

뎁스 카메라를 사용하여 Point cloud화를 시키거나, SLAM 등 여러 알고리즘을 사용할 때 생각했던 것 보다 정확도가 높지 않았던 경우가 있다.  

이 때 **Depth filtering**을 활용하여 Depth 값의 값만 보정을 시켜도 정성적인 평가로 정확도가 올라갔던 경험이 있어 포스팅을 진행하고자 한다.  

Depth filtering을 진행한 뎁스 카메라는 **Real Sense D435**이며 [librealsense-ROS Wrapper](https://github.com/IntelRealSense/realsense-ros)를 활용하여 ROS에서 쉽게 Depth filtering을 적용할 수 있다.  

