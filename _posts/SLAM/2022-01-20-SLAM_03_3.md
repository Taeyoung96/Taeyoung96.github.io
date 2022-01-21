---
title : "Slam 3-3강 (ICP & Point Cloud Registration - Non-linear Least Squares) 요약"
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

- Least Squares Approach 소개 및 장점  
- 2D point-to-point registration  
- Least Squares Registration using Point-to-Plane Metric  
- Robust Least Squares  
- Non-Rigid Registration  

## 1. 강의 및 강의 자료  

강의 List는 [SLAM KR](https://www.youtube.com/channel/UCXvT7auo7xUd7v0B2pmvwIA)의 SLAM DUNK seson 2에서 정해준 강의를 따라 정리를 할 것이다.  

SLAM DUNK season 2의 Reference는 [여기](https://youtube.com/playlist?list=PLubUquiqNQdMYwQVftUSFEWhJgzBErO9N)서 확인해 볼 수 있다.  

SLAM KR에서 이 강의를 듣고 요약을 세미나 형식으로 준비를 해준다!

이번 포스팅은 다음의 강의들을 참고하여 요약을 해보았다.  

Point Cloud Registration & ICP의 경우 2021 강의에서 3번에 걸쳐 나눠 강의를 진행했기 때문에  
그 강의를 듣고 요약을 진행했다.  

- [ICP & Point Cloud Registration - Part 3: Non-linear Least Squares (Cyrill Stachniss, 2021)](https://youtu.be/CJE59i8oxIE)    
- [SLAM DUNK Season 2 - Iterative Closest Points](https://youtu.be/BiQx5ISVdxU)  

Cyrill Stachniss의 강의 자료는 pdf로 [다운](https://drive.google.com/file/d/1tBHavj2v8GgsdOx4UMIYRN9lGvcDWejQ/view?usp=sharing) 받을 수 있다.  

## 2. Least Squares Approach 소개 및 장점  

현재 Point cloud registration을 3번에 걸쳐 진행하고 있다.  

[첫 번째 강의](https://taeyoung96.github.io/slam/SLAM_03_1/)에선 두 Point cloud 집합의 Correspondence를 알고 있을 때, Registration을 하는 방법을 배웠고, [두 번째 강의](https://taeyoung96.github.io/slam/SLAM_03_2/)에선 두 Point cloud 집합의 Correspondence를 모를 때 추정하는 방법과 조금 더 Robust한 ICP 알고리즘에 대해 배웠다.  

이번엔 Parameter를 찾는 방법을 SVD 알고리즘을 사용하는 것이 아닌 Least squares 접근 방법을 사용하여 Rotation matrix $R$과 Translation vector $t$를 구하는 방법에 대해서 배운다.  

Least squares는 [Slam 2강 (Least Squares - An Informal Introduction) 요약](https://taeyoung96.github.io/slam/SLAM_02/#2-least-squares%EB%9E%80) 에서 다루었으니 참고해보자.  

그렇다면 SVD로 푸는 방법보다 Least squares 접근 방법으로 푸는 것이 어떤 장점이 있을까?  

이에 대해 강의에서는 다음과 같은 이유를 말하고 있다.  

1. SVD solution은 point-to-point correspondences를 가정한다.  
2. Error function이 복잡해지면 복잡해질 수록, Least squares 접근 방법이 요구된다.  
3. 불확실성(Uncertainties)에 대해서 더 좋은 결과를 보인다. (Robust하다.)  
4. Alignment를 진행할 때 서로 다른 Weight를 활용하여 Alignment를 더 잘 할 수 있다.  

그렇다면 어떠한 방식으로 Least squares를 사용해서 Point cloud registration이 되는지  
2D point-to-point registration을 통해서 이해해보자.  

## 3. 2D point-to-point registration  

우리가 결국 구해야 하는 것은 **Rotation matrix $R$과 Translation vector $t$** 이다.  
SVD를 사용하는 것이 아닌 Gauss Newton Minimization을 사용하여 최적화를 진행해보자.  

우선 사용한 수식은 아래와 같다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/150261684-3f277d19-9ba7-4413-bdb3-53518b8d1929.png" width = "600" ></p>  

위 식에서 우리가 사용하는 error function은 Rotation Matirx에 Sine과 Cosine에 관한 값이 들어가 있기 때문에, **Non-linear한 error function**이라고 할 수 있다.  

따라서 이를 선형화해주는 작업이 필요하다.  
Gauss Newton Method에서 했던 것과 마찬가지로 먼저 1차 미분에 대한 식을 구하기 위해 Jacobian을 알아야 한다. 여기서 Jacobian의 형태는 2x3 행렬이다. 2는 x성분 y성분을 의미하고 3은 parameter의 갯수$(t_x, t_y, \theta)$를 의미한다.  

Jacobian에 대한 식은 아래와 같다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/150274615-24c55383-f3a7-47dc-b3f1-d1ed9cfc4c7e.png" width = "400" ></p>  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/150277640-c720e4c8-2c68-4b75-b7ea-8b96bd531e1f.png" width = "500" ></p>  

바로 위에 있는 강의 자료에서 맨 마지막에 행렬을 표현할 때 $x_n, y_n$은 Error function에 있는 $x_n$의 성분을 x성분과 y성분으로 나누어서 쓴 것이니 주의하자.  

다음 Gauss Newton Method를 이용해서 최적의 해를 구하는 과정을 거친다.  

Gauss Newton Method를 사용하기 위해서 $H, b$ 값을 구해준다.  
$H, b$의 의미는 $x$를 선형화를 할 때 $x = x + \Delta x$ 값으로 구하게 되는데 $\Delta x$만을 변수로 $x$를 상수로 봤을 때, $\Delta x$에 대한 이차식을 행렬로 표현한 것이 $H$, 1차식을 벡터로 표현한 것이 $b$이다.  

위에서 구한 Jacobian을 이용하여 필요한 $H, b$를 구하는 식을 구하고 Gauss Newton Method를 쓴 방법은 강의자료에 아래와 같이 나와있다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/150297149-46eee52d-b461-4389-a159-a359b45381e3.png" width = "550" ></p>  

이 방법을 사용하여 최적화를 진행하면 어떻게 될까?  

**SVD based method와는 다르게 매 step(또는 iteration)마다 Correspondence가 달라질 수도 있다는 것을 확인했다!**  

## 4. Least Squares Registration using Point-to-Plane Metric  

위와 같은 Least Squares Method는 기본적으로 Point-to-point metric을 사용했다.  

[이전 포스팅](https://taeyoung96.github.io/slam/SLAM_03_2/#4-%EA%B0%9C%EC%84%A0%EB%90%9C-icp-2--%EB%98%90-%EB%8B%A4%EB%A5%B8-data-association-%EC%A0%84%EB%9E%B5)에서도 설명했지만 data association을 진행할 때 Point-to-plane association 전략이 point-to-point보다 좋은 성능을 보이고 있기 때문에 최근 data association은 대부분 point-to-plane metric을 사용한다.  

따라서 Point-to-plane을 least squares method를 사용해보자.  

먼저 Error function부터 다시 설계해야 하는데, error function은 아래와 같다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/150300300-6409d3be-3274-4041-9f0b-985ca63abaa9.png" width = "550" ></p>  

Normal vector $n_n$을 구할 때는 Eigenvalue와 eigenvector가 활용되거나, 외적을 활용한다.  

Error function이 달라졌기 때문에 Jacobian도 당연히 달라진다. 달라지는 형태는 아래 강의자료에서 확인할 수 있다.  

Error fuction을 구할 때, 벡터의 내적이 활용되므로 최종적인 형태는 1x3형태의 vector임을 주의하자.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/150302729-5464c2ae-6770-4529-b499-f15359f88206.png" width = "550" ></p>  

Jacobian을 구하면 아까 Gauss Newton method를 진행하기 위해 구했던 $H,b$를 구하고 동일한 과정을 거쳐 최적의 값을 찾으면 된다.  

**여기서 Point-to-plane 기반의 방법을 사용할 때 주의할 점은 결과값이 대칭(symmetric)이 아니라는 것이다!** 말을 풀어서 설명하자면 Point cloud $x$를 $y$에 대해서 registration을 계산한 결과와 Point cloud $y$를 $x$에 대해서 진행한 결과 값이 달라진다는 것이다. 즉, 기준에 따라 결과 값이 다르게 나올 수 있다. 왜냐하면 Normal vector값이 달라지기 때문이다.  

<u>따라서 Noraml vector를 구할 때, $x$에 대한 Noraml vector 성분과 $y$에 대한 Noraml vector 성분을 더하는 과정을 거쳐 대칭이 안되는 문제를 해결하려 한다.</u>  

이러한 방법을 **Symmetric Point-to-plane** 방법이라고 한다.  
Noraml vector를 각각 구하는 과정이 있어야 하지만 성능이 확실히 좋아졌다고 한다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/150304760-f268c0f9-14af-4532-a829-4fea6c1e3549.png" width = "550" ></p>  

## 5. Robust Least Squares  

SVD based method나 Least squares method나 Robust하게 만드려면  
결국 Data association을 잘해야 한다.  

Outlier를 제거하기 위해 Robust한 kernal을 사용하거나 / M-estimator같은 것들을 사용한다. 이러한 Kernal을 만드는 연구도 활발히 진행중인 것 같다. 또한 Heuristics한 방법이나 초기화를 잘 해서 outlier를 제거하려는 노력도 많다.  

지금까지 강의에서 나온 내용을 정리하면 다음과 같다.  

- Initial guess는 중요하다! (odometry나 constant velocity로 제공)  
- Point-to-plane 방법이 point-to-point보다 성능이 좋다.  
- Point-to-plane 방법을 사용할 때, Symmetric metric을 사용하면 성능이 더 올라갈 수 있다.  
- Outlier을 제거하려는 노력이 중요하다.  

<font color='ff0000'>Cyrill 교수님의 경험에 의해면 Point cloud에 uncertainties가 존재할 때, 현재 설명한 Point cloud registration 방법은 결과가 좋지 않다고 말씀하셨다.</font>  

따라서 Point cloud값이 확실할 때 이러한 방법들을 사용해야한다. 이 말을 듣고 보니 Lidar SLAM을 할 때는 ICP를 많이 활용하지만, Visual SLAM의 경우 그렇지 않은데 아마 point cloud의 uncertainties 때문이라고 생각이 들었다.  

## 6. Non-Rigid Registration  

지금까지는 Rigid body일 때 point cloud registration을 하는 방법에 대해서 알아봤다.  
하지만 물체가 Non-rigid일 경우는 어떻게 달라질까?  

예를 들어 움직이는 사람을 Point cloud registration을 진행하거나 시간에 따라 길이가 변하는 식물을 Point cloud registration을 진행하려면 어떻게 할까?  

심화 내용이기 때문에 방법만 나열은 해보면 다음과 같다.  

- Skeleton Deformation을 활용해서 Data association을 만든다.  
- 시간축을 기준으로 Interpolation을 진행한다.  
- 좀더 정교한 Data associaton 방법을 활용한다.  

---

ICP와 관련해서 [Jupyter notebook by Igor Bogoslavski](https://nbviewer.org/github/niosus/notebooks/blob/master/icp.ipynb)를 적극 추천해주셨다. 이전 포스팅에서도 언급했지만 정말 좋은 자료이기 때문에 다시 한번 언급을 했다. 이렇게 3강에 걸쳐 ICP에 대해서 알아봤다. 꼼꼼히 정리하는 것이 나중에 볼 땐 좋지만, 시간이 너무 많이 걸린다.. 😥  시간 배분을 잘해서 포스팅을 꾸준히 진행해야겠다.  

