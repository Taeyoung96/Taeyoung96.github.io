---
title : "2020 CVPR SLAM Challenge 요약"
category :
    - Research
tag :
    - Slam
toc : true
toc_sticky: true
comments: true
---  

2020 CVPR SLAM Challenge를 보고 요약해보자.  

대회가 열린지 시간이 꽤 지났지만 실제 환경에서 Robust한 SLAM을 만들고자 하는 연구자들이 보면 흥미로운 영상이 있어 보고 짧게 요약을 하고자 한다.  

바로 2020 CVPR에서 열린 [2020 CVPR SLAM Challenge](https://sites.google.com/view/vislocslamcvpr2020/slam-challenge)이다!  

이 대회에 대해서 간단히 소개하고 1등 솔루션 및 여러 솔루션들을 공개해주기 때문에 솔루션들을 언급하며 포스팅을 마무리해보자 한다.  

## 대회 개요  

### TartanAir Dataset 소개  

이 대회는 우선 [TartanAir](https://theairlab.org/tartanair-dataset/) 라는 Dataset을 이용하여 극한의 환경에서 SLAM을 수행하는 것을 목표로 하고 있다.  

결국 연구에 TartanAir Dataset을 많이 사용하라고 홍보(?)도 하고, 자신들이 만든 Dataset이 추후 Visual SLAM 연구에 도움이 되고자 이런 대회를 개최한 것 같다.  

짧게 TartanAir Dataset을 만든 이유를 이야기하면 다음과 같다.  

우선 현재 Visual SLAM 연구에서 자주 쓰이는 Dataset은 다음과 같다.  

- KITTI dataset  
- TUM RGB-D dataset  
- EuRoC dataset  

Dataset을 활용해서 SLAM 알고리즘 자체를 개선시키는 것도 물론 중요하지만, <u>문제는 이러한 Dataset이 현실(Real enviornment)와 살짝 거리감이 있다는 것을 지적했다.</u>  

1. 기존의 Dataset의 경우, 한 가지나 두 가지의 유형의 scene에서만 가지고 있다.  
2. Ground truth의 Translation이나 Rotation이 제한 적이다.  
3. 기존의 Dataset의 경우, Train / Test set이 제대로 나누어져 있지 않다.  
4. Day-night, 극한의 조도 환경 변화 등 실제 환경에서 일어날 법한 장면들을 기존의 Dataset은 담고 있지 않다.  

3번째 의견 같은 경우, 최근에 딥러닝을 SLAM에 활용한 연구들이 많이 이루어 지고 있는데, 기존의 Dataset의 경우 Train / Test set이 제대로 나누어져 있지 않아, 연구자들이 제시한 SLAM 알고리즘의 공정성 문제가 제기되고 있다는 의미이다.  

**따라서 이러한 기존의 Dataset의 한계를 극복할 방법으로 TartanAir Dataset을 제시한 것이다.**  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/155265243-6d7fbe55-e4cd-49a1-a2eb-2dd3f639ed28.png" width = "600" ></p>  

위 사진처럼 다양한 환경에서 만들어진 Dataset을 제시하고 있다.  

이러한 Dataset은 실제 환경이 아니라 시뮬레이션 환경에서 만들고 Ground truth를 만들었다. 하지만 시뮬레이션의 환경이 좋아 실제 세계와 시뮬레이션의 Gap을 최소화했다고 주장하고 있다.  

다른 Dataset과 달리 시뮬레이션 환경에서 Ground truth를 제작했기 때문에 굉장히 많은 Label 및 환경을 제시하고 있다.  

제공하는 센서 및 환경은 다음과 같다.  

- Stereo image  
- Optical Flow  
- Depth  
- Lidar  
- Pose  
- Light(illumination)  
- Weather  
- Season  
- Dynamic Object  
- Random Motion Patteren  

정말 많은 정보들을 제공하기 때문에 이 Dataset을 활용하여 좀 더 좋은 Visual SLAM 알고리즘을 연구 및 개발할 수 있을 것이라는게 TartanAir Dataset 저자들의 주장이다.  

### 대회 트랙 및 평가 방법

이 대회에서는 TartaAir Dataset을 활용하고 Monocular Track과 Stereo Track 두 트랙으로 나누어 대회를 진행했다.  

두 트랙의 자세한 정보는 아래의 링크에서 확인할 수 있다.  

- [TartanAir Visual SLAM - Mono Track](https://www.aicrowd.com/challenges/tartanair-visual-slam-mono-track)  
- [TartanAir Visual SLAM - Stereo Track](https://www.aicrowd.com/challenges/tartanair-visual-slam-stereo-track)  

Baseline Code로는 ORB-SLAM이 제공되고 평가 지표는 다음과 같다.  

기존의 KITTI dataset에서 Error를 측정하는 것과 비슷하게 normalized Relative Pose Error를 사용한다.  

다만 KITTI dataset과 달리 (5, 10, 15, ...,40m)와 같은 모든 가능한 부분 시퀀스에 대해서 translational and rotational errors를 계산한다.  

총 Error의 경우, $E = E_{rot} + \beta E_{trans}, (\beta = 7)$이라는 식으로 계산이 되는데 이유는 translational and rotational errors간 벨런스를 맞추기 위해서다. TartaAir Dataset에서 평균적인 Rotation speed(in degree)는 Translation speed보다 7배 컸기 때문이다.  

이 대회에서 Test할 때 쓰이는 Sequence의 특징은 다음과 같다.  

- Day-night  
- Illumination changes  
- Weather  
- Dynamic objects  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/155268200-b640601e-8785-496f-8902-816bc9c9fe59.png" width = "600" ></p>  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/155268243-7242fb65-32e2-4af3-adc7-bb60a20ca261.png" width = "600" ></p>  

처음 드는 생각은 이런 환경에서 SLAM이 된다고...?였다.  

그렇다면 대회에 참가한 사람들의 solution은 어떤지 간단히 살펴보자.  

## 대회 솔루션  

트랙은 Monocular 그리고 Stereo, 두 트랙으로 나누어 진행을 했지만 우승자는 동일한 팀이였다.  

솔루션에 대한 영상은 [CVPR SLAM Challenge Awards Ceremony](https://youtu.be/jNhPD4oO6xA)에서 확인할 수 있다.  

우선 Baseline으로 제공된 ORB-SLAM의 Score와 우승자의 Score는 다음과 같다.  

- **Monocular Track**

||Score|Secondary Score|
|:---:|:---:|:---:|
|ORB-SLAM-Mono|3.57|17.7|
|Best Team|0.34|0.45|  

- **Stereo Track**
  
||Score|Secondary Score|
|:---:|:---:|:---:|
|ORB-SLAM-Stereo|1.64|2.9|
|Best Team|0.12|0.48|  

정말 엄청난 결과가 아닐 수 없다.. 😳😳  

### 1등 솔루션  

Mono Track에서 1등이 공개한 솔루션은 다음과 같다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/155272135-aaf0aba0-a1b2-4fd8-95a2-9ea2100f9d96.png" width = "600" ></p>  

- [COLMAP](https://colmap.github.io/)을 baseline으로 잡는다.  
- [SuperPoint](https://github.com/rpautrat/SuperPoint)와 [SuperGlue](https://github.com/magicleap/SuperGluePretrainedNetwork)를 활용하여 특징점 추출 및 특징점 매칭  
- 환경에 따른 Dynmaic한 Threshold 설정  
- BoW를 활용한 Loop detection  



Stereo Track에서도 비슷한 솔루션을 가져갔다.  
<p align="center"><img src="https://user-images.githubusercontent.com/41863759/155272979-70239d09-cb2e-4e29-848f-9c1fb4876b05.png" width = "600" ></p>  

- [COLMAP](https://colmap.github.io/)을 baseline으로 잡는다. 다만 기존의 COLMAP은 stereo에 대한 pipeline이 없기 때문에 아래와 같은 pipeline을 설계했다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/155272640-31cab3e2-658c-4f9d-a1b8-17572a852787.png" width = "600" ></p>  

- Monocular Track과 동일하게 [SuperPoint](https://github.com/rpautrat/SuperPoint)와 [SuperGlue](https://github.com/magicleap/SuperGluePretrainedNetwork)를 활용하여 특징점 추출 및 특징점 매칭을 진행한다.  
- SIFT feature와 SuperPoint feature들로 Mapping을 진행한다.  
- BoW를 활용한 Loop detection   

이 저자들은 Low한 texture에서는 Line과 같은 feature를 활용하면 더욱 더 특징이 많이 추출 될 것이라고 했다. 이 솔루션의 아쉬운 점은 Offline으로 동작한다는 것이다. 1000장의 이미지를 활용해 Map을 만드는데 30분이 소요된다고 언급했다.  


### 2등 솔루션  

2등 솔루션은 논문으로 작성이 되어있고 Github에 코드도 공개가 되어있다.  
논문은 CVPR 2020에 accept 되었다.  

논문 제목은 **"VOLDOR: Visual Odometry from Log-logistic Dense Optical flow Residuals"** 이다.  

조금 더 자세한 내용을 알고 싶으면 논문 및 코드를 참고하면 좋을 것 같다. ([AriXiv](https://arxiv.org/abs/2104.06789) , [Code](https://github.com/htkseason/VOLDOR))  

VOLDOR의 카테고리를 분류하면 Indirect method를 사용하고 Dense한 Map을 활용하여 odometry를 추정하는 방법이라고 설명한다.  

VOLDOR의 Workflow를 알아보면 아래와 같다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/155307249-efb2c56a-ea5e-4692-bfac-8cd7d15ebf59.png" width = "700" ></p>  

우선 전처리 과정으로 Optical Flow와 Disparity를 얻어내야 한다.    

먼저 연속적인 이미지 sequence를 Dense하게 Optical Flow를 추정하는 estimator에 넣어준 후 Optical Flow를 얻어낸다.  

Depth map은 Input으로 넣어주는 Disparity를 통해 얻어낸다.  

이렇게 두 가지 Input을 가지고, Dense한 Optical flow와 rigid flow를 최소화 하는 것을 목표로 한다. Rigid flow는 Camera pose를 활용해서 warping된 Depth map을 통해 얻는다.  

여기서 발표자가 말하는 Rigid flow라는 말이 너무 이해가 안되서 논문도 찾아보고 주관적인 의견을 덧붙여 정리를 하자면, Optical Flow는 보통 연속 되는 두 이미지에서 얻어내는데 이때 움직이는 물체가 있을 경우, 추정하는 optical flow값이 완전히 바뀌게 된다. 따라서 <u>얼마나 정적인 환경에서 얻어낸 Optical flow 값인가를 확률적으로 나타내고 이를 pixel마다 각각 나타내 하나의 Dense한 Optical Flow와 비슷하게 2D 이미지처럼 얻어낸 것이 Rigid flow</u>라고 이야기를 하는 것 같다.  

Dense한 Optical flow와 rigid flow의 차이를 Rigidness map이라고 하여 얻어낸 다음, Fisk Residual Model에 넣어준다. Fisk Residual Model은 deep learning model이 아니라 확률밀도함수(PDF)와 같은 확률론적인 모델이다. 다만 여기서 쓰이는 파라미터들이 Optical flow estimation에서 쓰이는 학습된 파라미터들이다.      

Fisk Residual Model의 output을 활용해서 Depth map, Rigidness Map을 update하고, camera pose도 update하는 식으로 Visual odometry를 계산하는 것이다.  

Depth map을 업데이트 할 때는 Sampling을 진행하고 propagation 방법을 활용하여 업데이트를 진행한다. Maximum Inlier Estimation이라는 수식을 제안하는데 Rigidness map을 활용해서 조금더 매끄러운 Depth map을 얻고자 했다. (사실 수식이 정확하게 어떤 것을 이야기 하고 있는지는 조금 더 봐야 할 것 같다.)  

Rigidness Map을 업데이트 할 때는 Markov chain smoothing이라는 알고리즘을 사용한다. (이 부분은 이해가 부족하다.)  

Camera pose를 업데이트 할 때는 이전 time에서 얻은 Depth map을 활용하고 P3P solver를 최소화하도록 maximum a posteriori(MAP) approximation한다.  

결과에 대한 지표는 따로 없고, 영상으로 발표를 마무리한다. 오른쪽 아래 보이는 그림에서 빨간색으로 나온 것이 Trajectory를 시각화 한 것이다.   

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/155318987-03d84836-3d46-474d-ae61-1f7aadc4a16d.png" width = "500" ></p>  

### 3등 솔루션   

3등 솔루션 역시 논문으로 작성되어 있고 코드도 공개되어 있다.  
논문은 ICRA 2021(with RA-L)에 제출되었다.  

논문 제목은 **"A Fully Online and Versatile Visual SLAM for Real-Time Applications"** 이다.  

조금 더 자세한 내용을 알고 싶으면 논문 및 코드를 참고하면 좋을 것 같다. ([AriXiv](https://arxiv.org/abs/2102.04060) , [Code](https://github.com/ov2slam/ov2slam))  

우선 발표자의 소개를 잠깐 했는데 발표자가 소속되어 있는 ONERA( 프랑스 국립 항공우주 연구 센터)에서는 Real-time, reliability 그리고 robustness에 대해서 연구를 진행하고 있다고 이야기 했다.  

따라서 이런 연구에 맞춰 새로운 SLAM system을 제시했는데 특징은 다음과 같다.  

- 크게 Feature-based SLAM으로 분류 : 3개의 threads(front-end, mapping, and state estimation thread)로 구성  
- Visual Front-end thread : 들어오는 이미지에 대해서 real time processing이 가능하도록 설계  
- Mapping thread : real time이 나오도록 하면서 정확도를 최대로 할 수 있도록 유연하고 모듈화를 해 놓았다.  
- State estimation thread : Keyframes와 3D map에 대해서 중복인 상태를 filtering을 진행하며 최적화(Bundle Adjustment)를 수행  

자세하게 알고리즘에 대해서 알아보자.  

우선 Visual Front-End thread부터 설명하면 다음과 같다.  

Visual Front-End thread도 크게 4가지로 구분을 하고 있다.  

- Preprocessing  
- Tracking  
- Pose Estimation  
- New KeyFrame Creation  

> Preprocessing 단계에서는 모든 이미지에 대해 CLAHE 알고리즘을 적용하고, coarse-to-fine tracking을 위해 Pyramid 구조를 만든다. CLAHE 알고리즘이란 Contrast Limited Adaptive Histogram Equalization의 줄임말로 간단히 설명하면 히스토그램 평활화를 할 때 이미지의 대비효과를 부각시키는 방법을 적용한 알고리즘이다. [CLAHE - 네이버 블로그](https://m.blog.naver.com/samsjang/220543360864)에 조금 더 자세한 설명이 있다.  

> Tracking 단계에서는 첫번째로 이전의 camera pose의 Motion model을 이용하여 현재의 camera pose를 추정한다. 이전의 camera pose와 추정한 현재의 camera pose를 이용하여 Optical Flow tracking을 적용한다. 이 때 Lukas Kanade Method를 사용한다. 그 다음 2D-2D RANSAC filtering을 적용하여 outlier를 제거한다.  

> Pose Estimation 단계에서는 첫번째로 P3P-based Pose Refinement를 진행한다. 최적화를 진행할 때 map point는 고정을 시키고 camera의 pose만 최적화를 진행한다. Non-linear optimization 방법이기 때문에 Levenberg-Marquardt 알고리즘을 적용하여 최적화를 진행한다. 미리 추정한 Pose에서 KeyPoint의 절반 미만이 성공적으로 추적이 되면 미리 추정한 Pose를 버리고, P3P RANSAC 알고리즘에서 계산된 카메라 pose를 활용한다.  

> New KeyFrame Creation 단계에서는 새로운 KeyFrame을 생성하게 되는데 3D keypoints의 수가 일정 threshold보다 높거나 새로운 keypoints를 그리드 전략으로 찾게된다. 그리고 각각 keypoints들마다 Descriptor도 찾게된다.  

이제 Mapping thread에 대해서 알아보자.  

Mapping thread도 크게 3가지로 구분하고 있다.  

- Stereo Triangulate  
- Temporal Triangulation  
- Local Map Tracking  

> Stereo Triangulate 단계에서는 Tracking에서 사용했던 Optical Flow 방법을 동일하게 사용하고 있다. 미리 추정된 pose를 활용해서 2D-2D Epipolar filtering을 활용하고 Inliers에 대해 Triangulate를 적용하여 새로운 3D Map points를 얻게 된다.  

> Temporal Triangulation 단계에서는 Monocular 모드에서 3D map을 초기화할때만 사용하거나 Stereo 모드에서 Matching이 이뤄지지 않았을 때 사용한다. Temporal Triangulation 단계에서 생성된 3D Keypoints를 활용하여 다음 단계에 카메라 pose 추정을 더 잘할 수 있게 되고, 또한 더 좋은 카메라 pose가 추정이 되면 다음 단계에 더 정확한 3D Keypoints의 위치를 얻을 수 있게 된다. 즉, 초기화를 잘 해놓으므로 선순환이 일어난다.  

> Local Map Tracking 단계에서는 현재 KeyFrame이나 인접한 KeyFrame에서 보이는 3D points의 세트를 나타내는 과정이다. KeyFrame에 대해 Local map을 만들고 마지막에는 covisibility graph를 update한다. Real-time을 보장하기 위해 이 단계를 모듈화하였다.  

마지막으로 State estimation thread에 대해서도 알아보자.  

State estimation thread는 크게 두가지로 구분한다.  

- Local Bundle Adjustment  
- Keyframes Filtering  

> Local Bundle Adjustment 단계에서는 3D Points와 Camera pose를 동시에 최적화를 수행한다. 최적화를 수행할 KeyFrames들을 고르고 KeyFrame에서 관찰되는 3D Points들을 가지고 Bundle Adjustment를 수행한다.  

> Keyframes Filtering 단계에서는 covisibility graph에서 불필요한 KeyFrame들을 제거하는 역할을 수행한다. 불필요한 KeyFrame들을 제거하므로써 다음 Bundle Adjustment를 수행할 땐 조금 더 빠르게 수행을 하는 것을 목적으로 한다.  

---

이렇게 총 3가지의 솔루션을 요약해보았다. 개인적으로 Real-time을 만족하면서 딥러닝을 사용하지 않은 세번째 솔루션이 인상깊었다. 로봇을 고려하다보면 컴퓨팅 파워나 Real-time이 항상 고려되어야 한다고 생각하기 때문이다. 이러한 대회로 다양한 SLAM 알고리즘을 알 수 있어 흥미로웠다. 또한 TartaAir Dataset이 나오면서 SLAM에 딥러닝을 적용한 논문이 많이 나올 것으로 예상된다. 😆😆   

