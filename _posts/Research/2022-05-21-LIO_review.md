---
title : "내맘대로 LIO(LiDAR-inertial odometry) survey (Feat.2018~2022)"
category :
    - Research
tag :
    - survey  
    - LiDAR-inertial
toc : true
toc_sticky: true
comments: true
---  

최근 나온 LIO(LiDAR-inertial odometry)을 정리해보자!  

2022년을 기준으로 오픈소스로 공개된 LIO 계열 알고리즘이 많이 등장했다.  
이쪽 연구에 관심을 가지고 시작하는 입장으로써 잘 정리된 문서가 없어 이를 정리하고자 한다.  

**❗️ 공부를 이제 막 시작한 입장으로써 잘못 된 내용이 있을 수 있습니다.  
틀린 부분이 있거나 놓친 논문이 있다면 댓글로 알려주시면 감사드리겠습니다! 😆**  

최근 [Fast-LIO](https://github.com/hku-mars/FAST_LIO) 계열 시리즈가 주목을 많이 받고 있기 때문에 여기 나와 있는 Related works 및 구글링을 통해 조사를 진행했다.  

논문의 핵심 아이디어만 이야기하는 식으로 정리를 하고자 한다.  

## LIO-Mapping (IRCA 2019)  
- 논문 링크 : [Arxiv](https://arxiv.org/abs/1904.06993), [IRCA](https://ieeexplore.ieee.org/abstract/document/8793511?casa_token=JhE9dkqEDnIAAAAA:hM87BOCnSA7_O2QLpqzVM3fjwEX9Jb2UyG527J0nXqH8Pvt1j_uDecAADcDxWTBBliLgLnrb8r8) /  공식 페이지 - [Link](https://sites.google.com/view/lio-mapping)  
- 코드 공개 여부 : O ([Github](https://github.com/hyye/lio-mapping))  
- 실험 환경 : Velodyne VLP-16 (LiDAR) + Xsens MTi-100 (IMU)  
Indoor - handheld / UAV, Outdoor -  golf car / KAIST Urban dataset  
- **Main Contribution / Key IDEA**  
    1. Tightly coupled lidar-IMU fusion method  
    2. Initialization 방법과 Optimization 방법을 [VINS-MONO](https://taeyoung96.github.io/research/VINS_mono/)에서 많이 영감을 받음  
    3. Global map을 다시 구성할 때 Rotational constraints를 추가하여 Gauss-newton 방법으로 최적화를 진행  

## LIO-SAM (IROS 2020)  

- 논문 링크 : [Arxiv](https://arxiv.org/abs/2007.00258), [IROS](https://ieeexplore.ieee.org/abstract/document/9341176?casa_token=-VvWqtE7GGQAAAAA:sWAz4VR5VbI0ffXNOwnKtlyFdpmaV__5gqO61UPDjVFP4tqmn7aqlK8GdiPbDBNbf5cS-sz8YBw)  
- 코드 공개 여부 : O ([Github](https://github.com/TixiaoShan/LIO-SAM))  
- 실험 환경 : Velodyne VLP16, Ouster OS1-128 (LiDAR) + Microstrain 3DM-GX5-25 (IMU)  
Outdoor - Jackal platform or handheld로 실험, MIT campus dataset 구축 
- **Main Contribution / Key IDEA**  
    1. factor graph로 문제 해결 : IMU-preintegration, lidar odometry, loop closure, GPS factor 사용  
    2. 휴리스틱하게 이전 robot pose보다 급격하게 커진 경우를 keyframe으로 선별,  local sliding window-based scan-matching으로 n개의 LiDAR scan에 대해서 최적화를 수행  
    3. GTSAM 사용하며 Euclidean distance로 constraint를 만들어 Loop closing을 수행, 대부분 LOAM & LeGO LOAM의 방법을 많이 사용  

## LINS (ICRA 2020)  

- 논문 링크 : [Arxiv](https://arxiv.org/abs/1907.02233), [ICRA](https://ieeexplore.ieee.org/abstract/document/9197567?casa_token=6yu-wNE_uX0AAAAA:79GiPPtW-bWALGHfCGtj1LE0TbmLlfQ4PVH5_RiF5ZJ5zoR-dhIcY5RrhoM_8tsEcxCt1cMZMWs)  
- 코드 공개 여부 : O ([Github](https://github.com/ChaoqinRobotics/LINS---LiDAR-inertial-SLAM))  


## KFS-LIO (ICRA 2021)  

- 논문 링크 : [IRCA](https://ieeexplore.ieee.org/document/9561324)  
- 코드 공개 여부 : X  
- 실험 환경 :   Velodyne VLP16 (LiDAR) + Xsens MTi-300 (IMU)  
KAIST Urban dataset  
- **Main Contribution / Key IDEA**  
     1. Feature 뽑는 방법은 LOAM과 동일, GPS를 평가할 때 쓰이는 개념인 Dilution of precision (DOP)에서 영감을 받아 Key-Feature Selection Algorithm을 제안    
     2. IMU preintegration을 진행할 때 구한 Rotation과 Translation 값을 활용하여 LiDAR de-skewing을 같이 진행함으로써 계산량을 줄이는데 초점  
     3. LOAM에서 Random으로 feature를 sampling하는 것 보다 제안하는 방법이 좋다고 강조  
     4. Loop closure detection에서 LiDAR, IMU의 residual을 활용한 새로운 Threshold를 제안, uncertainty를 관리하기 위함  

