---
title : "Event camera survey 요약"
category :
    - Research
tag :
    - Event camera
toc : true
toc_sticky: true
comments: true
---

Event-based Vision: A Survey를 읽고 요약해보자!  

기존의 카메라와는 다른 생체 모방형 카메라 'Event Camera'에 대해 연구할 기회가 생겼다.  
새로운 분야에 대해 알기 위해서는 보통 Survey 논문을 읽기 마련이다.  

'이벤트 카메라'쪽에서 가장 최신 survey 논문인 [Event-based Vision: A Survey](https://arxiv.org/pdf/1904.08405.pdf)를 읽고 정리를 해보자.  

이벤트 카메라와 관련하여 정보가 많은 [Github repository](https://github.com/uzh-rpg/event-based_vision_resources)도 존재하니 참고해보자.  

## 이벤트 카메라란?  

이벤트 카메라는 기존의 카메라와 달리 고정된 프레임 단위로 이미지를 캡쳐하는 것이 아닌,  
<font color='ff0000'> 픽셀 간의 밝기 변화를 비동기적(asynchronously)으로 측정하고 기록하는 카메라</font> 이다.   

이벤트 카메라는 기존의 카메라(traditional camera)와 비교했을 때 다음과 같은 장점을 지닌다.  

1. Low latency - 낮은 반응 속도  
2. Low power consumption - 낮은 소비 전력  
3. High dynamic range - 가장 밝은 부분과 어두운 부분의 비율이 큼 (빛에 강인함)  

하지만 전통적인 카메라와 output이 다르기 때문에 기존의 연구 중인 computer vision algorithm을  
바로 적용하기엔 무리가 있다.😟  

이벤트 카메라가 가지는 장점 때문에 Object tracking, Object recognition, Depth estimation, 3D scanning, image reconstruction, SLAM 등등 여러 분야에 활용 될 수 있다.  

이벤트 카메라를 소개하는 여러 영상들도 있으니 참고해보자.  

- [Event-Based Camera vs Standard Camera](https://youtu.be/kPCZESVfHoQ)  
- [Event Cameras: Opportunities and the Road Ahead (CVPR 2020)](https://youtu.be/6Sn9-M7qXLk)  
- [High Speed and High Dynamic Range Video with an Event Camera](https://youtu.be/eomALySSGVU)  

## 이벤트 카메라의 작동 원리  

이벤트 카메라는 말 그대로 빛의 변화에 따른 event를 생성해 내는 카메라이다.  
모든 픽셀에 대해서 asynchronously하고 independently하게 생성이 된다.  

각각 픽셀들은 시간에 대해 Log intensity(빛의 세기에 log를 취한 값)을 기록하고 있는다.  
만약 빛의 급격한 변화가 일어나 일정한 threshold 값을 넘어가게 된다면, event가 발생하였다고 신호를 보낸다.  

그 신호에는 보통 location(x,y), time(t), polarity(p)에 대한 정보가 담겨있다.  
Polarity란 빛의 밝기가 증가하는 방향인지 감소하는 방향인지 알려주는 신호이다.  

이벤트 카메라는 Data-driven(데이터를 기반으로 하는) sensor이다.  
더 많은 움직임이 관측되고 더 많은 이벤트들이 발생한다면 그만큼 많은 신호가 전송된다.  

이벤트 카메라는 microsecond resolution으로 timestamp를 진행하고  
sub-millisecond 단위로 정보를 전송하기 때문에 매우 낮은 latency를 가질 수 있다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/112133776-60b7fa00-8c0f-11eb-9e47-f1c66acdf0fa.png" width = "400" ></p>  



---

이벤트 카메라도 여러 종류로 나누어 진다.  

1. DVS event camera  
2. ATIS  
3. DAVIS - 가장 범용적으로 쓰이는 event camera 종류이다.  

이벤트 카메라의 이점을 확실하게 하기 위해서는 기존의 computer vision algorithm에서 다뤄야 할 Issue들이 존재한다.  

- **Coping with different space-time output** :
    기존의 카메라는 연속적이고 촘촘한 이미지를 가지고 있는 것과 달리, 이벤트 카메라의 결과는 Sparce하고 asynchronous하다. 따라서 Image sequence에 의해 만들어진 기존의 algorithm은 이벤트 카메라에 바로 적용이 어렵다.  

- **Coping with different photometric sensing** :  
    기존의 카메라에서는 Grayscale information이 제공되는 것과 달리, 이벤트 카메라는 Binary(increse/decrease)한 이벤트 information만 사용할 수 있다. 빛의 변화는 Scene의 빛의 변화뿐만 아니라 scene과 camera의 움직임에 의해서도 발생할 수 있기 때문에 주의를 해야한다.  

- **Coping with noise and dynamic effects** :  
    Real world에서는 모든 상황에 대해서 Noise가 존재할 수 밖에 없다. 특히 이벤트 카메라 같은 경우 트렌지스터 회로 노이즈도 존재하여 Noise 처리를 잘 하는 것이 특히 중요하다.  

## 이벤트 Processing 과정  

### Event data를 표현할 수 있는 방법  

Event data를 이용하여 의미 있는 information을 뽑아내는 과정은 굉장히 중요한 일이다.  

이벤트 카메라는 asynchronous and sparse한 방법 그리고 high temporal resolution, low latnecy로  
정보를 얻기 때문에, 시간적인 측면에서 특정한 latency는 event를 처리하는데 굉장히 중요한 역할을 한다.  

동시에 얼마나 많은 양의 이벤트 데이터를 처리하는 지에 따라 처리 과정을 구별할 수 있는데,  

1. Methods that operate on an event-by-event basis 

2. Methods that operate on groups or packets of events  

하지만 리뷰 논문의 저자는 처리 과정에 따라 구별하는 것 보다  
Model-based approach와 Model-free approach에 따라 나누는 것이 낫다고 이야기 하고 있다.  

Model-based approach에서는 event data를 최적화 기법을 이용하여 처리하려고 하고,  
Model-free approach에서는 Geometric, temporal, photometric-based의 loss function이나 objective function과 관련해서 이벤트 데이터를 처리하려고 한다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/112594409-399a3c00-8e4c-11eb-9eee-73304255c0ff.png" width = "800" ></p>  

위 그림에서는 Event signal을 Visualization한 모습이다. (a)는 3차원 공간(x,y,t)에서의 event data를 나타낸 모습이고, (b)는 하나의 event frame에 대해서 나타낸 모습이다.  
또한 (c)는 Time surface에서 event signal을 표현, (d)는 Voxel-grid 관점에서 표현하였다.  
(e)는 Motion 보정을 통한 event image를 나타내고, 마지막으로 (f)는 기존의 Computer vision  
방법과 호환을 위한 Grid를 이용하여 event signal을 표현하였다.  

- **Individual events** : $e_k = (x_k, t_k, p_k)$는 event-by-event 처리를 할 때 event signal을 표현하는 방법이다. 주로 filter 기반이나 SNN(Spiking Neural Network)를 처리할 때 사용한다. 이전 event 정보들이나 추가적으로 알고 있는 정보들과 결합을 하여 output을 만들어낸다.  

- **Event packet** : $ \varepsilon = \{e_k\}^{N_e}_{k=1}$은 시,공간적으로 이웃한 event signal을 합쳐 ouput으로 처리하는 방법을 말한다. {N_e}는 packet의 size를 말한다.  

- **Event frame/image or 2D histogram** : 몇몇의 알고리즘은 event data의 통계적인 특징을 원하기도 한다. 기존의 Computer vision algorithm에서 사용되었던 historgam이 event image에도 적용이 되는데 이 방법은 event timestamps를 양자화하고, 공간적인 특징 정보를 무시하기 때문에 이상적인 방법은 아니다.  

- **Time sureface(TS)** : TS란 동일한 시간에서 각각 pixel들의 정보를 2D map으로 표현한 것이다. 이벤트의 강도(Intensity)가 해당 위치의 이미지로 변환이 되며 더 큰 값은 더 최근의 모션에 해당이 된다. TS는 기존의 computer vision 분야에서 Motion History Images라고 불렸다.  

- **Voxel Grid** : Event data를 3차원 histogram으로 표시하려면 Voxel을 이용하는 방법도 있다. Voxel을 이용하면 시간적인 정보(temporal information)를 표현하는데 효과적이다. 각각의 event들이 voxel을 이용해서 축척될 수도 있고, kernal을 사용할 수도 있다.  

- **3D point set** : Voxel과 비슷하게 3차원 point들의 집합으로 event data를 표현할 수 있다. 이렇게 표현할 경우 $(x_k, y_k, t_k) \in {\Reals}^3$으로 쓸 수 있고, point cloud를 처리하는 방식도 Event data에 적용을 해볼 수가 있다.  

- **Point sets on Image plane** : Event data들은 이미지 평면에서 2D points들의 집합으로 간주될 수 있습니다. ICP와 mean-shift algorithm 등 shape tracking 방법을 사용할 때 표현하는 방법이다.  

- **Motion-compensated event image** : Event 정보 뿐만 아니라 Motion compensation 정보도 포함하도록 표현하는 방법이다. 이미지 평면에서 edge들이 움직일 때, event가 발생하는데 그 정보들을 이용해서 event signal을 보정하는데 사용한다. 따라서 이 방법은 event들이 얼마나 motion을 잘 찾는지 평가해볼 수 있는 요소로 사용된다.  

- **Reconstructed Image** : Event 정보에서 polarity(positive and negative)는 Positive와 negative를 각각 사용할 수도 있고, 이 정보들을 합쳐서 사용할 수도 있다. Event data를 이용하여 brightness image를 얻으면 좀 더 motion에 불변한 특징들에 대해서 표현할 수 있다. 이는 Object recognition 같은 motion 방향이 불필요한 task에서 주로 사용되는 방법이다.  

### Event processing의 방법  

Hardware platform과 Event data를 어떻게 처리할 것인지에 따라 Event processing 방법이 많이 달라진다. 또한 어떤 tastk인지, ANN(Artificial neural network)에는 어떻게 통과할 것인지에 따라도 달라진다. 크게 Event-by-event 방법과 Group of Event 방법이 존재한다.  

- **Event by event based methods** : Noise 제거, 특징 추출, Image reconstruction 등을 수행할 때 확률 기반의 Filter(ex 칼만 필터, 파티클 필터)가 주로 사용 된다. 또다른 방법으로는 다양한 형태의 ANN(인공 신경망)이 사용된다.  

- **Methods for Groups of Events** : 




















