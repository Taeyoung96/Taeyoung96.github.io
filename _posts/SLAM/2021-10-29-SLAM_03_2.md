---
title : "Slam 3-2강 (ICP algorithm & Unknown Data Association) 요약"
category :
    - Slam
tag :
    - Slam
toc : true
toc_sticky: true
comments: true
---

Cyrill 교수님의 SLAM 강의를 듣고 정리해보자!  

## 0. Today's Goal

- ICP 알고리즘이란?  
- 개선된 ICP (1) : Sampling을 통한 sub point set 사용   
- 개선된 ICP (2) : 또 다른 Data Association 전략  
- 개선된 ICP (3) : 가중치가 부여된 대응관계 활용  
- 개선된 ICP (4) : 잠재적인 Outlier Pairs 제거하기  
- ICP 알고리즘 최종 정리    

## 1. 강의 및 강의 자료  

강의 List는 [SLAM KR](https://www.youtube.com/channel/UCXvT7auo7xUd7v0B2pmvwIA)의 SLAM DUNK seson 2에서 정해준 강의를 따라 정리를 할 것이다.  

SLAM DUNK season 2의 Reference는 [여기](https://youtube.com/playlist?list=PLubUquiqNQdMYwQVftUSFEWhJgzBErO9N)서 확인해 볼 수 있다.  

SLAM KR에서 이 강의를 듣고 요약을 세미나 형식으로 준비를 해준다!

이번 포스팅은 다음의 강의들을 참고하여 요약을 해보았다.  

Point Cloud Registration & ICP의 경우 2021 강의에서 3번에 걸쳐 나눠 강의를 진행했기 때문에  
그 강의를 듣고 요약을 진행했다.  

- [ICP & Point Cloud Registration - Part 2: Unknown Data Association (Cyrill Stachniss, 2021)](https://youtu.be/ktRqKxddjJk)    
- [SLAM DUNK Season 2 - Iterative Closest Points](https://youtu.be/BiQx5ISVdxU)  

Cyrill Stachniss의 강의 자료는 pdf로 [다운](https://drive.google.com/file/d/1J_QCE1CLwJ5d7ugY1wnx-DWNaWR-FQTr/view?usp=sharing) 받을 수 있다.  

## 2. ICP 알고리즘이란?  

[지난 포스팅](https://taeyoung96.github.io/slam/SLAM_03_1/)에 이어서 Point Cloud Registration에 대해서 강의를 진행해주셨다.  

두 Point Cloud 간의 대응관계(Correspondences)가 주어져 있을 때는 SVD 알고리즘을 활용해 Rotation Matrix $R$ 과 Translation Vector $t$를 찾을 수 있었다.  
한장의 슬라이드로 정리하면 다음과 같다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/139383703-45a610be-3aff-473f-acd6-1cfd2403f7f8.png" width = "600" ></p>  

(자세한 내용은 [지난 포스팅](https://taeyoung96.github.io/slam/SLAM_03_1/)을 참고하자.)  

이번 강의에서는 **대응관계(Correspondences)가 주어져 있지 않을 때,  
즉 Unknown Data Association일 때 Point Cloud Registration을 하는 방법에 대해서 알아보자.**  

아쉽게도 Unknown Data Association일 때는 Direct하고 Optimal한 Solution이 존재하지는 않는다.  

그러나 두 Point Cloud Set(집합)의 대응 관계를 잘 추정(Estimate)하고, 그 후 Rotation Matrix $R$ 과 Translation Vector $t$를 찾는 방법으로 Point Cloud Registration을 진행한다면 최적의 해를 찾을 수 있다.  

이것이 ICP Algorithm이다.  
> ICP Algorithm이란 Point Cloud Registration을 진행할 때 Data Association에 대해서 모를 경우,  
대응관계를 추정하고 그 대응관계를 통해 두 Point Cloud를 Align을 하는 과정 중 하나이다.  
대응관계를 만들 때는 가장 가까운 점을 활용하며 반복적인 과정을 통해 오차를 최소화한다.  



<p align="center"><img src="https://user-images.githubusercontent.com/41863759/139392961-9b65fb42-3872-47e0-8b56-0c49ac852a57.png" width = "500" ></p>  

과정을 다시한번 설명하자면,  
Data Association을 추정하고 추정된 Data Association을 활용하여 Rigid body transformation을 구한다.  
이를 반복적으로 수행하여 최적의 Parameter (Rotation matrix & Translation vector)를 찾는다.  

ICP 알고리즘에서는 Direct로 해를 찾을 수 없어 **초기값(Initial guess)을 가정하고 문제를 풀어나간다.**  
예를 들어 Reference frame에서의 point cloud의 위치나 정확하지는 않지만 대략적인 대응관계  
(Correspondences)를 초기값으로 가정한다.  

Point Cloud Set $x_n, y_n$이 있다고 할 때 ICP 알고리즘의 흐름을 대략적으로 설명하면 다음과 같다.  

1. Point Cloud $x_n$의 대략적인 대응관계를 초기값으로 설정한다.  
보통 대응관계를 구할 때는 $x_n$에서 각각의 point에 대해, 가장 가까운 거리에 있는 $y_n$의 하나의 점과 매칭을 통해 대응관계를 만든다.       
2. 대응관계를 활용하여 Rotation Matrix $R$과 Translation vector $t$를 구한다.  
3. $R,t$를 활용하여 다시 $\bar{x_n} = R * x_n + t$ 으로 만들어 $y_n$과 align 시킨다.  
4. $\bar{x_n}$과 $y_n$의 차이를 Error로 정의하고 Error 값이 원하는 Threshold 값보다 적어질 때까지 1~3번 과정을 진행한다.  

❗ICP를 jupyter notebook으로 작성한 [링크](https://nbviewer.org/github/niosus/notebooks/blob/master/icp.ipynb)가 있는데 Visualize도 굉장히 잘 되어있다!  

**위와 같은 ICP를 Vanila ICP라고 부른다. 즉, 가장 기본이 되는 ICP Algorithm이다.**  

ICP 알고리즘은 초기에 Point Cloud가 충분히 가까울 때 잘 동작을 한다.  
Lidar를 활용하여 Point cloud를 뽑아냈을 때, Scan한 Data간의 시간차이가 짧으면 짧을수록 좋다.  

Vanila ICP의 경우 구현이 간단하지만 몇 가지 단점들이 존재한다.  

1. Iteration이 많이 필요할 수도 있다. 즉, iteration과 비례하여 시간이 오래 걸릴 수 있다.  

2. 초기 대응관계(Correspondences)를 완전히 잘못 구하거나  
나쁜 대응관계(Correspondences)를 이용할 경우 결과가 매우 나쁠 수 있다.  

따라서 이러한 단점들을 보완하려는 연구들이 많이 진행되었고,  
ICP Algorithm은 다음과 같은 내용들에 Focus를 맞추어 연구가 진행되었다.  

- 전체 Point Cloud가 아닌 부분적인 Point Cloud (points subset)만 사용  
- 또 다른 Data Association 전략을 사용  
- 가중치가 부여된 대응관계(Weight the Correspondences)를 활용  
- Outlier를 제거 후 point pairs를 맞추는 방법  

이러한 ICP 알고리즘은 다양한 Performance에 초점이 맞추어져 있다.  
Speed에 초점이 맞추어 졌을 수도 있고, Stability, 또는 Outlier나 Noise에 굉장히 강인하도록  
연구가 될 수도 있다.

좀 더 발전된 ICP 알고리즘에 대해서 한 번 알아보자.  

## 3. 개선된 ICP (1) : Sampling을 통한 sub point set 사용 

Vanila ICP 알고리즘에서는 모든 point를 사용한다. 이런 방법도 물론 경우에 따라서 필요하고 정확하게 두 Point Cloud의 관계를 구할 수 있지만, 조금만 생각해봐도 계산랑이 많다는 것을 알 수 있다.  

따라서 Point Cloud 전체를 사용하는 것이 아닌 Sampling을 통해서 일부분(subset)의 Point Cloud를 사용하여 ICP 알고리즘을 사용하자는 것이 개선된 ICP 알고리즘의 한 방법이다.  

Sampling을 하는 방법은 다음과 같다.  

1. Use all points  
2. Uniform sub-sampling  
3. Random sampling  
4. Feature-based sampling  
5. Normal-space sampling  

강의에서는 Uniform sub-sampling과 Normal-space sampling에 대해서 한 장의 슬라이드로 정리했다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/141739832-35466d8c-8c86-4a82-921e-b518e6b50141.png" width = "500" ></p>  

Uniform sampling은 간격이 균일하게 sampling을 하는 반면,  
Normal-space sampling은 굴곡진 부분에 대해서 조금 더 많은 sampling을 하는 것을 확인할 수 있다.  
Normal-space sampling 방법은 Smooth한 지형에서 조금씩 보이는 curvture나 조그마한 object가  
붙어 있는 경우 sampling을 하고 point cloud registration을 하는데 효율적인 방법이다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/141740594-43bdfd57-2a1c-46e4-a254-166f4923214e.png" width = "500" ></p>  

위 슬라이드에서도 알 수 있듯이 Random sampling과 비교를 했을 때도 Normal-space sampling이  
굴곡진 부분에 대해서 Alignment를 더 잘 수행했다는 것을 확인할 수 있다.  

Feature를 쉽게 찾을 수 있는 경우 Feature-based sampling도 강력한 방법이 될 수 있다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/141741219-87dc5852-95ce-49a6-ab0d-d65e8be56e5a.png" width = "500" ></p>  

Full 3D scan과 Feature만 뽑았을 때를 비교하면, 추출된 point의 수가 훨씬 적어졌다는 것을 알 수 있고, 계산량적인 측면에서 ICP 알고리즘을 훨씬 개선시켰다고 이야기할 수 있다. 

대응관계(correspondences)를 찾는데 훨씬 간소화할 수 있고, Feature들이 잘 뽑혔다고 가정할 때 높은  
효율성과 때로는 높은 정확도까지 보일 수 있다.  

## 4. 개선된 ICP (2) : 또 다른 Data Association 전략  

Data Association을 잘 찾으면 우리는 Optimal한 Solution을 쉽게 찾을 수 있다.  
**따라서 Data Association을 잘하는 것이 ICP 알고리즘의 결과에 엄청난 영항을 미친다.**  

Vanila ICP 알고리즘에서는 가장 가까운 point를 활용해서 Data Association을 구축했지만,  
이 방법이 잘 통하지 않는 환경도 존재할 것이다.  

따라서 수 많은 Data Association 방법이 제안되었다.  
1. Closest point  
2. Closest compatible point  
3. Normal shooting  
4. Projection-based approaches  
5. Point-to-plane  

이외에도 정말 많다...  

첫번째로 **Closest point** 방법은 Vanila ICP 알고리즘에서도 사용을 하는 Data association 방법이다.  
가장 가까운 점을 찾을 때 KD tree와 같은 구조를 사용하기도 하며, 간단한 방법이지만  
오차를 최소화 하기 위해 수렴(Convergence)을 할 때 굉장히 많은 Iteration을 돌아야 한다.  

Closet point 방법으로 Data association을 하기 위해서는 초기값(Initial guess)을 알아야한다.  
만약 초기값을 알 수 없을 경우, 질량 중심(Center of mass)을 이용하여 두 Point cloud를 이동 시킨 후,  
가장 가까운 point들을 활용하여 대응관계를 만들면 된다.  

두번째로 **Closest compatible point** 방법은 설명할 수 있는(Compatible) Point를 사용하여 Data association을 하는 방법이다. 보통의 points들은 (x,y,z) 좌표를 가지고 있는데 뎁스 카메라나 스테레오 카메라를 활용하거나 카메라 & 라이다 퓨전을 할 경우 point에 여러 정보들이 추가될 수 있다.  

Color, Other local features, Normals 등등 points를 설명할 수 있는 정보들을 활용하여  
Data association을 만들어 조금 더 효율적으로 ICP 알고리즘을 수행할 수 있다.  

세번째로 **Normal shooting** 방법은 한 point에서 Normal vector를 구하고 그 방향과 만나는 또 다른 Point set에서의 점을 활용하여 Data association을 하는 방법이다. 이 방법은 Smooth한 평면에서는 굉장히 유용한 Data association 방법이지만, Noise가 많거나 곡선이 많은 복잡한 환경일 경우 좋지 않을 수도 있다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/141748123-300a90c2-c9ce-4ebb-aa0f-683dd43b02ce.png" width = "450" ></p>  

네번째로 **Projection-based approaches** 방법은 Point cloud를 RGB-D camera와 같은 센서에 Projection 시켰을 때, 서로 같은 점에 projection이 된 Point들을 대응시켜 Data association을 하는 방법이다. 

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/141748675-2db46d1e-90f0-4a7f-8d7a-3b32f19e65cf.png" width = "450" ></p>  

최근 가장 많이 사용을 하고 있는 방법은 바로 **Point-to-plane** 방법이다. 앞서 소개한 방법은 Point와 point를 Direct하게 이어서 유클리디안 거리를 만들었다면, <u>Point-to-plane은 source point에서 Target point를  
Direct하게 잇는 것이 아니라, Target point의 점과 점 사이에 있는 가상의 평면(또는 직선)을 만들고   Normal vector를 구해 가장 가까운 점을 고르고 유클리디안 거리를 계산하여 Data association을 만드는 방법이다. 아래 그림을 보면 훨씬 이해가 잘 될 것이다.</u>  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/141755919-ac63ef34-e3bf-40b8-b9bd-42f1b65ea92e.png" width = "450" ></p>  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/141756392-2267ab97-9eac-491a-b3de-401b2e206ae8.png" width = "500" ></p>  

Point-to-plane 방법은 최소화해야하는 함수 또한 조금 달라지는데, Normal vector를 내적시킨 값을 최소화 시킨다고 이해하면 된다.    

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/141757113-01afbff7-035d-4fc6-98f5-18a9d2cc7274.png" width = "500" ></p>  

**Point-to-plane 방법은 최근 ICP 알고리즘을 사용하는데 가장 유용하게 사용하는 Data Association 방법임을 기억해두자!**  

동일한 횟수로 Iterator를 돌았을 때, Point-to-plane 방법은 이미 정합이 완료된 반면, Point-to-point는 최적화가 조금 더 진행되어야 한다는 것을 아래 그림을 통해서 확인할 수 있다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/141757943-e732dd47-fed2-4294-b32c-297eb8de17f9.png" width = "400" ></p>  

## 4. 개선된 ICP (3) : 가중치가 부여된 대응관계 활용  

Point의 쌍을 만들 때, Points들의 신뢰도에 따라서 Weight를 부여하는 방법도 존재한다.  

보통 사용하는 센서에서 나온 값이 불확실한 경우 Weight를 사용하여 얼마나 Noise가 있는지  
알 수 있도록한다.  

이미 우리는 앞서 ICP 알고리즘 및 Point cloud registration을 설명할 때, Weight의 개념을 사용하였다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/141764269-30232894-cb58-40a4-aaf4-02a9f9b6a2f4.png" width = "500" ></p>  

## 5. 개선된 ICP (4) : 잠재적인 Outlier Pairs 제거하기

좋은 Data Association은 곧 ICP 알고리즘의 결과에 영향을 미친다.  
따라서 Outlier pairs를 제거하는 것이 ICP 알고리즘 결과에 크게 영향을 주고  
이를 제거하려는 연구들이 많이 진행되었다.  

간단히 생각해볼 수 있는 방법은 Point끼리의 유클리디안 거리가 일정 Threshold를 넘을 경우,  
Outlier pairs로 생각하고 이를 제거해주는 방법이다.  

또 다른 방법은 인접한 Points에서 대응관계로 묶인 Points들의 거리가 크게 차이가 난다면 Outlier pairs로  
생각하고 이를 제거해주는 방법이다. 이 방법에 따른다면 아래 그림에서 왼쪽 대응관계가 Outlier pair로  
판명될 것이다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/141766387-7b15e062-6e27-4b9a-9512-9e354386cd8c.png" width = "300" ></p>  

이 방법을 발전시켜 가장 대응관계가 나쁜 $t \%$를 제거하고 ICP 알고리즘을 수행하는 방법이 제안되었는데,  
이를 **Trimmed ICP**라고 부른다.  

여기서 $t$를 지정해주는 것 또한 하나의 문제인데, 주로 얼마나 Outlier가 많을 것 같은지 얼마나 많은 Points들이 Overlap 됐는지에 따라 결정된다.  

마지막으로 조금 더 일반적인 방법으로 Outlier를 제거하고 싶다면 Kernal을 활용하는 방법도 있다.  
Kernal function을 활용하게 된다면 잠재적으로 큰 에러 값에 대해 적은 영향을 받도록 할 수 있다.  

Kernal function에 대해 자세한 설명은 다크프로그래머님의 블로그에 [Kernel Density Estimation(커널밀도추정)에 대한 이해](https://darkpgmr.tistory.com/147) 포스팅을 참고하도록 하자.  

이런 Outlier를 제거하는 방법은 ICP 알고리즘 뿐만아니라, "Moving object segmentation"과 같은 다양한 분야에서 활용될 수 있다.  

## 6. ICP 알고리즘 최종 정리  

이렇게 Vanila ICP 알고리즘에서부터 개선된 ICP까지 다양한 알고리즘을 살펴봤는데,  
이를 정리해보면 다음과 같다.  

1. 정합을 하기에 좋은 Point cloud를 Sub-sampling을 진행한다.  
2. Points들의 대응관계를 결정한다. (상황에 따라 맞는 대응관계를 채택)  
3. Robust한 성능을 보이기 위해, 가중치를 부여하거나 Outlier의 후보군을 제거한다.  
4. $SVD$ 알고리즘을 활용하여 Rotation Matrix $R$과 Translation vector $t$를 구한다.  
5. 모든 점에 대해서 Rotation Matrix $R$과 Translation vector $t$를 구한다.  
6. Error 값을 구한다.  
7. 일정 Threshold보다 Error가 작아질 때까지 반복한다.  
8. 최종적인 Alignment를 만들어낸다.  

---

이렇게 Data Association을 알 수 없을 때 Point cloud registration을 하는 방법인 ICP 알고리즘에 대해  
알아봤다. 다음 강의에서는 $SVD$ 알고리즘을 사용할 수 없을 때, Least Squares를 활용하여 Point cloud registration 하는 방법을 알아보자.  