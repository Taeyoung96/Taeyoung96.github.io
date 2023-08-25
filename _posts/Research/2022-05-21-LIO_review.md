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

개인적인 생각으론 최근 [Fast-LIO](https://github.com/hku-mars/FAST_LIO) 계열 시리즈가 주목을 많이 받고 있기 때문에 여기 나와 있는 Related works 및 구글링을 통해 조사를 진행했다. 논문의 핵심 아이디어만 이야기하는 식으로 정리를 하고자 한다.  

**❗️ 공부를 한 것을 정리해놓은 포스팅으로써 잘못된 내용이 있을 수 있습니다.  
틀린 부분이 있거나 중요한데 놓친 논문이 있다면 댓글로 알려주시면 감사드리겠습니다! 😆**  

## Paper List  
- [LIPS (IROS 2018)](#lips-iros-2018)  
- [LIO-Mapping (IRCA 2019)](#lio-mapping-irca-2019)  
- [LIO-SAM (IROS 2020)](#lio-sam-iros-2020)  
- [LINS (ICRA 2020)](#lins-icra-2020)  
- [KFS-LIO (ICRA 2021)](#kfs-lio-icra-2021)  
- [Fast-LIO (RA-L 2021)](#fast-lio-ra-l-2021)  
- [Fast-LIO2 (T-RO 2022)](#fast-lio2-t-ro-2022)   
- [Globally Consistent and Tightly Coupled 3D LiDAR Inertial Mapping (ICRA 2022)](#globally-consistent-and-tightly-coupled-3d-lidar-inertial-mapping-icra-2022)  
- [Wildcat (Arxiv 2022)](#wildcat-arxiv-2022)  

## LIPS (IROS 2018)  
- 논문 링크 : [Pdf](http://udel.edu/~yuyang/downloads/geneva_iros2018.pdf), [IROS](https://ieeexplore.ieee.org/abstract/document/8594463)  
- 코드 공개 여부 : O ([Github](https://github.com/rpng/lips))  
- 실험 환경 : custom LiDAR IMU simulator,  
    Quanergy M8(LiDAR) + Microstrain 3DM-GX3-25 (IMU)  
- **Main Contribution / Key IDEA**  
    1. 수 많은 point cloud 중 평면(Plane)을 추출하는 방법을 채택, closest point (CP) plane 표현 방법을 새롭게 정의 및 제안  
    2. "Continuous Preintegration Theory for Graph-based Visual-Inertial Navigation" [논문](http://udel.edu/~ghuang/papers/tr_cpi.pdf)에서 제안한 Continuous preintegration factor 사용  
    3. Plane factors를 정의하고 graph-based optimization을 활용하여 최적화를 진행  
    4. 과연 plane만으로 constraints를 만들고 최적화를 진행하면 정확하게 LiDAR의 pose가 추정이 될지는 의심이 든다...  

## LIO-Mapping (IRCA 2019)  
- 논문 링크 : [Arxiv](https://arxiv.org/abs/1904.06993), [IRCA](https://ieeexplore.ieee.org/abstract/document/8793511) /  공식 페이지 - [Link](https://sites.google.com/view/lio-mapping)  
- 코드 공개 여부 : O ([Github](https://github.com/hyye/lio-mapping))  
- 실험 환경 : Velodyne VLP-16 (LiDAR) + Xsens MTi-100 (IMU)  
Indoor - handheld / UAV, Outdoor -  golf car / KAIST Urban dataset  
- **Main Contribution / Key IDEA**  
    1. 최적화를 진행할 때 optimization-based method를 사용   
    2. Initialization 방법과 Optimization 방법을 [VINS-MONO](https://taeyoung96.github.io/research/VINS_mono/)에서 많이 영감을 받음  
    3. Global map을 다시 구성할 때 Rotational constraints를 추가하여 Gauss-newton 방법으로 최적화를 진행  
    4. 계산량이 조금 많다는 것이 단점  

## LIO-SAM (IROS 2020)  

- 논문 링크 : [Arxiv](https://arxiv.org/abs/2007.00258), [IROS](https://ieeexplore.ieee.org/abstract/document/9341176)  
- 코드 공개 여부 : O ([Github](https://github.com/TixiaoShan/LIO-SAM))  
- 실험 환경 : Velodyne VLP16 or Ouster OS1-128 (LiDAR) + Microstrain 3DM-GX5-25 (IMU)  
Outdoor - Jackal platform or handheld로 실험, MIT campus dataset 구축 
- **Main Contribution / Key IDEA**  
    1. factor graph로 문제 해결 : IMU-preintegration, lidar odometry, loop closure, GPS factor 사용  
    2. 휴리스틱하게 이전 robot pose보다 급격하게 커진 경우를 keyframe으로 선별,  local sliding window-based scan-matching으로 n개의 LiDAR scan에 대해서 최적화를 수행  
    3. GTSAM 사용하며 Euclidean distance로 constraint를 만들어 Loop closing을 수행, 대부분 LOAM & LeGO LOAM의 방법을 많이 사용  

## LINS (ICRA 2020)  

- 논문 링크 : [Arxiv](https://arxiv.org/abs/1907.02233), [ICRA](https://ieeexplore.ieee.org/abstract/document/9197567)  
- 코드 공개 여부 : O ([Github](https://github.com/ChaoqinRobotics/LINS---LiDAR-inertial-SLAM))  
- 실험 환경 : Velodyne VLP-16 (LiDAR) + Xsens MTiG-710 (IMU)  
Ground Vehicle에서 실험, 주로 Outdoor에서 실험하고 GroundTruth는 GPS로 취득  
- **Main Contribution / Key IDEA**  
    1. [LIO-Mapping(ICRA 2019)](https://taeyoung96.github.io/research/LIO_review/#lio-mapping-irca-2019)과 저자들이 많이 겹침, LIO-Mapping보다 속도가 빠르다고 주장    
    2. 최초로 IEKF(iterated EKF)를 사용하여 LIO 문제를 해결하려 함  
    3. "Quaternion kinematics for the error-state Kalman filter" [논문](https://arxiv.org/abs/1711.02508)의 방법을 활용하여 error state를 표현함으로써 짐벌락 문제(gimbal lock problem)가 일어나지 않도록 해결  
    4. Feature extraction 및 Mapping 모듈은 LOAM & LeGO-LOAM의 방법을 사용, Odometry를 계산하는 모듈만 IMU를 추가하여 IEKF로 바꿨다고 이해하면 편함  


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

## Fast-LIO (RA-L 2021)  

- 논문 링크 : [Arxiv](https://arxiv.org/abs/2010.08196), [RA-L](https://ieeexplore.ieee.org/abstract/document/9372856)  
- 코드 공개 여부 : O ([Github](https://github.com/hku-mars/FAST_LIO)) - 현재 Fast-LIO2로 update 되어있음  
- 실험 환경 : UAV platform을 활용, Livox Avia (LiDAR) + Livox Avia built in IMU,  
Velodyne VLP-16 (LiDAR) + Xsens MTiG-710 (IMU) : [LINS (ICRA 2020)](#lins-icra-2020)와 비교를 위함  
- **Main Contribution / Key IDEA**  
    1. [LINS (ICRA 2020)](#lins-icra-2020)와 동일하게 IEKF(iterated EKF)를 사용하여 state estimation  
    2. 새로운 Kalman Gain 공식을 제안하여 measureent dimension(LiDAR point cloud)이 아닌 state dimension(Pose)에 기반하여 Kalman Gain을 구함.  
    3. IMU measurement를 기반으로 LiDAR point cloud의 motion compensation을 진행  
    4. Odometry에서 예측한 pose를 기반으로 Map 형성, scan to map 방식으로 Correspondense를 구함.  

FAST-LIO의 간략한 논문 리뷰를 [다른 포스팅](https://taeyoung96.github.io/research/Fast_LIO/)에도 적어보았다!

## Fast-LIO2 (T-RO 2022)  

- 논문 링크 : [Arxiv](https://arxiv.org/abs/2107.06829), [T-RO](https://ieeexplore.ieee.org/document/9697912)  
- 코드 공개 여부 : O ([Github](https://github.com/hku-mars/FAST_LIO))  
- 실험 환경 : 여러 SOTA 알고리즘들과 비교 (LILI-OM, LIO-SAM, LINS...)  
NCLT datast, UTDM dataset, Urbanloco dataset  
- **Main Contribution / Key IDEA**  
    1. [Fast-LIO (RA-L 2021)](#fast-lio-ra-l-2021)의 저널 확장판  
    2. Fast-lio와 달리 feature extraction 부분이 없음. Raw한 point cloud를 바로 활용  
    3. LiDAR feature를 뽑지 않으므로 새로운 Measurement model 제안하여 residual 계산 (G-ICP와 유사)  
    4. LiDAR-IMU extrinsic parameter를 state vector에 넣어 계산을 진행함  
       (Fast-LIO의 경우 extrinsic parameter는 안다고 가정)  
    5. Map을 효율적으로 관리할 수 있는 자료구조인 iKD tree라는 구조를 제안  


## Globally Consistent and Tightly Coupled 3D LiDAR Inertial Mapping (ICRA 2022)  

LIO(LiDAR-inertial odoemtry)가 아닌 LI-SLAM(LiDAR-inertial SLAM) 논문이다!  

- 논문 링크 : [Arxiv](https://arxiv.org/abs/2202.00242)  
- 코드 공개 여부 : X  
- 실험 환경 : KAIST urban dataset / Newer College Dataset  
- **Main Contribution / Key IDEA**   
    1. 일반적인 LIO system의 경우 front-end 부분에서만 IMU를 사용하지만, 이 논문에서는 front-end부터 back-end까지 IMU를 활용하여 최적화를 진행  
    2. Odometry estimation, Local mapping, Global mapping 총 3가지 모듈로 구성  
       (전부 IMU 활용)  
    3. Visual SLAM 논문인 DSO(Direct Sparse Odometry)에서 영감을 받아 front-end를 keyframe-based fixed-lag smoothing method 사용  
    4. GPU 가속이 가능하도록 알고리즘 설계  
    5. GTSAM을 활용하여 non-linear optimziation를 진행  

## Wildcat (Arxiv 2022)  

LIO(LiDAR-inertial odoemtry)가 아닌 LI-SLAM(LiDAR-inertial SLAM) 논문이다!   

- 논문 링크 : [Arxiv](https://arxiv.org/abs/2205.12595)  
- 코드 공개 여부 : X  
- 논문 소개 및 설명 동영상 : [Tartan SLAM Series - CSIRO's Wildcat SLAM](https://youtu.be/YCE1Aj0k1UA)  
- 실험 환경  : DARPA Subterranean Environments / MulRan / QCAT dataset  
             Velodyne VLP16 (LiDAR) + Microstrain 3DM-CV5 IMU (IMU)  
             Ouster OS1-64 (LiDAR + built in IMU)  
- **Main Contribution / Key IDEA**   
    1. DARPA Subterranean Challenge에서 우승한 알고리즘  
    2. Continuous-Time을 활용한 최적화 방법을 사용 (이 부분에 대한 지식이 아직 부족하다..)  
    3. LiDAR로 voxelization을 multi-scale로 하면서 surfel을 뽑아냄  
    4. LiDAR Odometry는 Sliding window optimization을 사용  
    5. PGO(Pose graph optimization)을 사용, Loop closure detection 같은 경우는 point-to-plane ICP,  Mahalanobis distance search radius 기반으로 진행  

