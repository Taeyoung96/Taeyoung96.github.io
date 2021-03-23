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

이벤트 카메라는 기존의 카메라와 달리 고정된 프레임 단위로 이미지를 캡쳐하는 것이 아닌, <font color='ff0000'> 픽셀 간의 밝기 변화를 비동기적(asynchronously)으로 측정하고 기록하는 카메라</font>이다. 

이벤트 카메라는 기존의 카메라(traditional camera)와 비교했을 때 다음과 같은 장점을 지닌다.  

1. Low latency - 낮은 반응 속도  
2. Low power consumption - 낮은 소비 전력  
3. High dynamic range - 가장 밝은 부분과 어두운 부분의 비율이 큼 (빛에 강인함)  

하지만 전통적인 카메라와 output이 다르기 때문에 기존의 연구 중인 computer vision algorithm을 바로 적용하기엔 무리가 있다.😟  

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












