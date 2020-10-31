---
title : "Qt를 이용하여 Histogram specification(Equalization) 구현하기"
category :
    - Computer_Vision
tag :
    - qt
    - raw data
    - Computer_Vision
    - histogram
toc: true
toc_sticky: true
comments : true
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

1 . Input Image의 Histogram과 Target Image의 Histogram을 각각 구한다.

2 . 각각 구한 Histogram에 각각 누적 Histogram을 구한다.

3 . 누적 Histogram을 이용하여 정규화된 누적 Histogram을 만든다.

<center> 정규화 된 Histogram을 만드는데 쓰이는 식은 다음과 같다. </center>
<br>
<center> $H(i)_n=Qmax*H(i)/n$ </center>
<br>

- $H(i)_n$은 정규화된 히스토그램 값,
- $Qmax$는 명도의 최대값 (여기서는 255)
- $H(i)$는 누적 히스토그램 값, n은 픽셀의 수(가로*세로)를 뜻한다.

자세한 설명은 아래의 동영상을 참고하자. (사진을 클릭하면 링크로 넘어갑니다.)

[![histogram](https://user-images.githubusercontent.com/41863759/82750397-da623a00-9dea-11ea-9f5c-aa6e7c1fe544.JPG)](https://www.youtube.com/watch?v=WXHFmJVHvag)

4 . 정규화된 누적 Histogram을 형변환을 통해 unsigned char 형식으로 바꾼다.

5 . QImage, QPixmap, QLabel을 이용하여 Histogram specification을 한 이미지를 출력한다.

## 3. Histogram specification 정리 및 소스 코드

내가 구현한 Histogram specification은 raw 파일을 기준으로 Grayscale 이미지에 대해서 구현을 하였다.

Histogram equalization의 단점을 보완할 수 있는 알고리즘이라고 생각한다.

내가 구현한 이미지에 대해서는 Histogram equalization 과 Histogram specification의 차이를 눈으로 확인하기는 어려웠다.

![image](https://user-images.githubusercontent.com/41863759/82750811-c835cb00-9ded-11ea-89f5-9be532048c33.png)

<center>< Histogram Equalization 결과 출력 화면 ></center>

<br>


![image](https://user-images.githubusercontent.com/41863759/82750822-d8e64100-9ded-11ea-8826-0fd0dd8f2fe3.png)

<center>< Histogram Specification 결과 출력 화면 ></center>

<br>

전체 코드는 [Histogram Specification](https://github.com/Taeyoung96/Histogram_Matching_Qt) 를 참고하면 된다.



















