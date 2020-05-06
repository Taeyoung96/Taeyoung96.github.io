---
title : "Qt를 이용하여 Histogram specification(Equalization) 구현하기"
category :
    - Image_processing
tag :
    - qt
    - raw data
    - image processing
    - histogram
toc: true
comments : true
toc_sticky : true
---

**Qt Creator**를 이용하여 **Histogram specification**를 구현해보자.

먼저, **Histogram specification**이란 무엇일까?


## 1. Histogram specification이란?

영상의 Histogram을 사용자가 원하는 모양의 Histogram으로 변형하고 싶을 때 사용하는 방법이다.

명암 대비를 개선 시키는 것은 Histogram Equalization과 비슷하다.

하지만 **특정 부분을 향상**시키거나, **원하는 모양의 Histogram**을 만들 때 사용한다.

Histogram specification과 Histogram matching은 서로 비슷한 의미이다.

그렇다면, Histogram specification을 하기 위해서는 어떤 과정을 거쳐야 할까?

## 2. Histogram specification을 하기 위한 단계

1. Input Image의 Histogram과 Target Image의 Histogram을 각각 구한다.

2. 각각 구한 Histogram에 각각 누적 Histogram을 구한다.

3. 누적 Histogram을 이용하여 정규화된 누적 Histogram을 만든다.

정규화 된 Histogram을 만드는데 쓰이는 식은 다음과 같다.

$$





