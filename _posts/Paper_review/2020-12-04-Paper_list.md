---
title : "The paper list what I have read"
category :
    - Paper_Review
tag :
    - Paper_Review
toc : true
comments: true
toc_sticky: true
---

읽은 논문을 정리하여 짧게 comment를 남겨보자! 

딥러닝 분야를 공부하다 보니 정말 봐야할 논문이 끊임없이 나오는 것 같다....  
모든 논문을 세세하게 review하고 싶지만, 시간 관계상 그러기란 쉽지 않다.  

읽은 논문들을 간략하게 Comment 및 Main Idea를 기록해두면 효과적일 것이라 생각한다.  

논문 링크를 걸어 두고, 간단하게 Comment를 남기는 식으로 정리를 해보자.  

## Classiciation  

- [EfficientNet: Rethinking Model Scaling for Convolutional Neural Networks](https://arxiv.org/abs/1905.11946)  

> ICML 2019에 소개된 논문이며 2021년 1월 기준으로 인용수가 약 1500회 정도 될만큼 여러 딥러닝 모델의 Backbone network로 사용되고 있다.  
보통 딥러닝 모델 연구는 Channel Scale, Depth Scale, Resolution(Width & Height) Scale이 각각 발전되는 경우가 많았다.  
하지만 **이 논문은 이 모든 Scale을 모두 고려하여(Compound Scale) 딥러닝 모델을 설계하자는 것이 핵심 아이디어**다.  
또한 NAS(Neural Architecture Search)기반으로 Baseline network를 직접 설계하였다.



## Depth Estimation  

- [Unsupervised monocular depth estimation with left-right consistency](https://arxiv.org/abs/1609.03677)  
> CVPR 2017에 소개된 Unsupervised learning으로 Depth estimation model을 제시한 논문이다.  
**이 논문의 핵심은 단일의 이미지(Left View Image)로 다른 시점의 이미지(Right View Image)를 만들어 내고,  
그 두 이미지의 disparity를 이용하여 Depth를 구한다는 것**이다.  

- [Learning the Depths of Moving People by Watching Frozen People](https://arxiv.org/abs/1904.11111)  
> CVPR 2019에 소개된 논문으로 단일의 카메라를 이용해서 객체의 Depth를 추정하는 시나리오를 제시하였다.  
    **이 논문의 main idea는 카메라도 움직이고, 물체가 움직인다는 상황에서 물체의 깊이를 추정할 수 있는 방법을 제시**하였다. 또한 이 모델을 이용해서 새로운 DataSet도 만들어 내었다. 다만 모델의 Input으로 사진뿐 아니라 여러 추가적인 정보들이 들어가야 하고, 자체적으로 만든 평가지표에서 좋은 성능을 냈다고 주장하고 있다.  

## 3D Object Detection  

- [Total3DUnderstanding: Joint Layout, Object Pose and Mesh Reconstruction for
Indoor Scenes from a Single Image](https://arxiv.org/abs/2002.12212)  
> CVPR 2020에 소개된 논문으로 실내 공간의 단일의 이미지에서 Object를 3차원으로 찾고, 찾은 물체를 Mesh를 통해 reconstruction하는 방법을 제시한 논문이다.  
NIPS 2018에서 소개된 **'Cooperative Holistic Scene Understanding: Unifying 3D Object, Layout, and Camera Pose Estimation'** 와  
CVPR 2018에서 소개된 **'Relation Networks for Object Detection'** 그리고 ICCV 2019에서 소개된 **'Deep Mesh Reconstruction from Single RGB Images via Topology Modification Networks'** 를 한번에 처리를 할 수 있도록 네트워크를 설계했다.  

## Segmentation  

- [BiSeNet V2: Bilateral Network with Guided Aggregation for Real-time Semantic Segmentation](https://arxiv.org/abs/2004.02147)  
> CVPR 2020에 소개 된 논문으로 Real time으로 Semantic Segmentation하는 방법을 제시한 논문이다.  
**두 개의 Branch를 만들어 low-level detail(Edge, Corner)과 high-level semantic(어떤 물체인지 구별)을 동시에 진행하는 모델을 제안**했다.  
$\gamma$를 통해 Detail branch와 Semantic branch의 비율을 조절하고 있다.  
새로운 Backbone network를 만들어 제안했기 때문에 논문에서 비교 실험을 많이 진행했다.

