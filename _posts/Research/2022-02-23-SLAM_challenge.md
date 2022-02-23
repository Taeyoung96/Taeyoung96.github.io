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

논문 제목은 **"VOLDOR: Visual Odometry from Log-logistic Dense Optical flow Residuals"** 이다.  

조금 더 자세한 내용을 알고 싶으면 논문 및 코드를 참고하면 좋을 것 같다. ([AriXiv](https://arxiv.org/abs/2104.06789) , [Code](https://github.com/htkseason/VOLDOR))  



