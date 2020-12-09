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


## Depth Estimation  

- [Unsupervised monocular depth estimation with left-right consistency](https://arxiv.org/abs/1609.03677)  
> CVPR 2017에 소개된 Unsupervised learning으로 Depth estimation model을 제시한 논문이다.  
이 논문의 핵심은 단일의 이미지(Left View Image)로 다른 시점의 이미지(Right View Image)를 만들어 내고,  
그 두 이미지의 disparity를 이용하여 Depth를 구한다는 것이다.  

- [Learning the Depths of Moving People by Watching Frozen People](https://arxiv.org/abs/1904.11111)  
> CVPR 2019에 소개된 논문으로 단일의 카메라를 이용해서 객체의 Depth를 추정하는 시나리오를 제시하였다.  
    이 논문의 main idea는 카메라도 움직이고, 물체가 움직인다는 상황에서 물체의 깊이를 추정할 수 있는 방법을 제시하였다. 또한 이 모델을 이용해서 새로운 DataSet도 만들어 내었다. 다만 모델의 Input으로 사진뿐 아니라 여러 추가적인 정보들이 들어가야 하고, 자체적으로 만든 평가지표에서 좋은 성능을 냈다고 주장하고 있다.  

## 3D Object Detection  

- [Total3DUnderstanding: Joint Layout, Object Pose and Mesh Reconstruction for
Indoor Scenes from a Single Image](https://arxiv.org/abs/2002.12212)  
> CVPR 2020에 소개된 논문으로 실내 공간의 단일의 이미지에서 Object를 3차원으로 찾고, 찾은 물체를 Mesh를 통해 reconstruction하는 방법을 제시한 논문이다.  
NIPS 2018에서 소개된 **'Cooperative Holistic Scene Understanding: Unifying 3D Object, Layout, and Camera Pose Estimation'** 와  
CVPR 2018에서 소개된 **'Relation Networks for Object Detection'** 그리고 ICCV 2019에서 소개된 **'Deep Mesh Reconstruction from Single RGB Images via Topology Modification Networks'** 를 한번에 처리를 할 수 있도록 네트워크를 설계했다.  

