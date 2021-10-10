---
title : "[Paper Review] Event camera survey 요약 (2)"
category :
    - Research
tag :
    - Event camera
toc : true
toc_sticky: true
comments: true
---

Event-based Vision: A Survey를 읽고 요약해보자! (2)  

[이전 포스팅](https://taeyoung96.github.io/research/EventSurvey/)에 이어서 이벤트 카메라 Survey 논문을 한 번 살펴보자.  

이번 포스팅에서는 이벤트 카메라를 활용한 Algorithms / Application에는 어떤 것들이 있는지 살펴보겠다.  
Low-level vision부터 (ex. Feature detection, Tracking, Optical Flow estimation), 3D 구조와 관련된 알고리즘 (ex. Depth estimation, Visual Odometry) 그리고 Segmentation, Recognition까지 정말 다양한 분야에 이벤트 카메라가 활용될 수 있다.  

## Feature Detection and Tracking  

이벤트 카메라를 활용해서 Feature detection 그리고 Tracking을 한다면 어떤 어려움이 있을까?  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/125287051-70db1d00-e357-11eb-9cdf-c5475654a609.png" width = "400" ></p>  

위 그림을 보면 (c)가 어떤 장면(Scene), (a)가 대각선으로 움직였을 때 Event 생성 결과, (b)가 위-아래로 움직였을 때 Event 생성 결과이다. <u>같은 scene에 대해서 Motion에 따라 다른 이벤트가 만들어 진다는 것이 Feature detection과 Tracking을 할 때 어려움</u>이다. 또한 센서 Noise에 대한 처리도 해야한다.  

초창기 Event camera를 이용한 feature detection method는 Event data의 Blob (일정한 영역을 찾는 것(?)이라고 이해했다.) 을 Clustering하는 방법으로 알고리즘을 만들었다.  

Tracking을 할 때는 새로 들어오는 event들에 대해 가장 가까이 존재하는 Blob / feature를 찾는 방식으로 알고리즘을 수행한다. 조금 더 나아가 Event 정보들 간에 ICP(Iterative Closetst Point) Algorithm을 사용하거나, Mean-Shift, Particle filtering을 사용하기도 한다.  

Only Event를 이용한 알고리즘도 있는 반면, Event와 Frame을 결합하여 사용한 알고리즘도 존재한다.  

굉장히 많은 알고리즘이 연구되었음에도 불구하고, **Common Dataset으로 평가를 하기가 굉장히 어렵다.** Target에 맞게 알고리즘이 만들어졌기 때문인데, 이 논문의 필자는 Learning-based feature detection 그리고 Tracking 알고리즘 (딥러닝 알고리즘을 의미하는 것으로 보인다.)은 연구가 비교적 덜 진행되었기 때문에 추후 연구 하는 사람들에게 기회일 수 있다고 이야기한다.  

## Optical Flow Estimation  

Optical Flow는 이미지 plane에서 Geometry나 Motion에 대한 정보 없이 물체의 Velocity를 계산하는 방법이다. 기존의 Conventional Cameras(우리가 흔히 사용하는 RGB 카메라) 같은 경우, 두 연속적인 이미지의 계산을 통해 얻을 수 있다.


