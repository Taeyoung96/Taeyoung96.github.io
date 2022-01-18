---
title : "[Paper Review] Kinect Fusion 요약"
category :
    - Research
tag :
    - Paper_Review
toc : true
toc_sticky: true
comments: true
---  
Kinect Fusion을 읽고 요약해보자!  

Kinect Fusion은 2011년에 나온 논문이다.    
최신 논문은 아니지만 현재까지 인용수도 높고 kinect fusion에 나온 알고리즘을 응용하여  
여러 SLAM 알고리즘이 만들어졌기 때문에 간단히 정리해보려 한다.  

논문도 2가지 버전으로 있는데, 하나는 수식적인 내용이 설명되어 있고,  
또 다른 하나는 Application쪽으로 논문이 작성되었다.  

두 논문을 모두 참고해보면 좋을 것 같다. 이번 포스팅은 첫번째 논문을 주 Reference로 삼았다.    

- 수식적인 내용 : [KinectFusion: Real-Time Dense Surface Mapping and Tracking](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/ismar2011.pdf)  
- Application적인 내용 : [KinectFusion: Real-time 3D Reconstruction and Interaction Using a Moving Depth Camera](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/kinectfusion-uist-comp.pdf)  

이 논문에서 하고자 하는 것은 결국 **값싼 RGB-D camera를 이용해서 Dense한 Reconstruction을 함과 동시에 Camera를 Tracking하는 알고리즘을 제안**하는 것이다.  

여기서 말하는 값싼 RGB-D camera는 Kinect camera를 의미한다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/149857466-d2a83170-d9a8-4e3d-bab0-652bea4e85f5.png" width = "400" ></p>  

위 그림이 kinect fusion의 결과를 가장 잘 보여주는 그림이다.  

왼쪽이 kinect camera에서 나온 raw한 depth image이고,  
오른쪽이 kinect fusion을 사용하여 dense한 reconstruction을 한 결과이다.  

그렇다면 어떻게 이러한 알고리즘이 구성되었는지 살펴보자.  

이번 포스팅은 Rigid body Transformation Matrix와 ICP를 대략적으로 안다는 가정하에 요약을 진행했다.  
복잡한 수식적인 부분은 나도 아직 이해를 제대로 못했기 때문에 핵심적인 부분만 이야기를 해보겠다. 😅  

## System workflow  

우선 위에서 언급한 논문 중 [첫번째 논문](https://www.microsoft.com/en-us/research/wp-content/uploads/2016/02/ismar2011.pdf)의 Workflow는 다음과 같다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/149858679-4f4d330c-b145-4784-9de5-dd1644f945ec.png" width = "600" ></p>  

여기서 Input은 kinect camera에서 나온 depth data이다.  

### Measurement  

첫번째로 **Surface Measurement** 단계에서는 Depth data에 대한 전처리 과정을 거친다.  

먼저 Kinect camera로 얻은 raw depth data(혹은 depth map)을 Filtering 과정을 거쳐  
노이즈를 제거하게 된다.  

여기서 사용하는 Filtering은 Bilateral filtering으로 가우시안 필터링보다 Edge부분 보존을 더 잘 시키기 때문에 사용한 것 같다. Bilateral filtering은 [이 블로그 포스팅](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=won19600&logNo=221770735004)을 참고해보자.  

그 다음 Depth data에서 Vertex map(위 그림에서 Nertex map으로 오타가 났다)과  
Noraml map을 구하게 되는데, Vertex map이란 Depth data 하나하나를(픽셀 단위로) 카메라 Intrinsic parameter를 활용하여 3D points로 복원하고 그 점들을 픽셀 위치에 대입하여 3D points 정보들(map 좌표계의 x,y,z에 대한 정보)로 이루어진 하나의 2D map이다.  

map이란 단어는 하나의 image라고 이해하면 쉽다.  
다만 image 안에 들어있는 정보들이 RGB가 아닌 다른 정보들이 들어가 있는 것이다!  

Normal map이란 그 픽셀 위치의 3D points로 복원할 때 Normal vector값을 구할 수 있는데,  
3D points 정보가 아닌 Normal vector의 값이 들어가 있다고 생각하면 이해하기 쉽다.  

아래의 그림을 보면 Normal vector 값이라는 것을 이해하기 쉽다. (출처 : CILAB 발표자료)   

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/149883766-ba6cdd26-f65c-4c8c-b8d0-84e380a48b4f.png" width = "300" ></p>  

여기서 Normal map을 구하는 이유는 우리가 추후에 사용할 ICP 알고리즘에서 Normal vector의 값을 사용하기 때문이다.  

### Pose estimation  

Camera pose estimation은 ICP 알고리즘을 사용한다.  

ICP algorithm은 [이 포스팅](https://taeyoung96.github.io/slam/SLAM_03_2/)에서 조금 더 자세히 알 수 있다.  

여기서는 Point-to-plane 방법을 사용하였다. 여기서 우리가 아까 구한 Vertex map과 Normal map을 사용하게 된다.  

ICP 알고리즘을 통해 구하고자 하는 것은 이전 Frame과 현재 Frame의 Camera pose(Transformation matrix)를 구하는 것이다.  

이전 Frame에서 얻은 Vertex map, Normal map과 현재 Frame에서 얻은 Vertex map, Noraml map간의 대응 관계를 만들어줘야 하는데, 이때 사용하는 대응 관계(Association)은 Projective data association 방법을 사용했다.  

### Update Reconstruction  

연속적인 Depth image가 들어올 경우 카메라 pose를 알고 있다는 가정하에 3D reconstruction을 진행할 수 있다.  

이 때 TSDF(truncated signed distance function)라는 함수를 사용한다.  

TSDF(truncated signed distance function)를 이야기하기 전에 SDF(signed distance function)에 대한 이야기를 먼저 해보겠다.  

3차원의 어떤 모형을 만든다고 한번 가정해보자.  

그렇다면 내가 원하는 3차원의 모형을 그리기 위해서 어느 부분이 경계인지 알려줘야 하게 되는데  
경계 부분을 알려주는 것을 함수로 만들어 경계 부분은 0, 모형의 안쪽 부분은 (-), 모형의 바깥쪽 부분은 (+)로 알려주는 함수가 바로 SDF이다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/149885430-b03c7a66-5ef5-4a24-919f-4198aea3ac01.png" width = "600" ></p>  

위 그림이 SDF를 시각화 한 그림이다. 파란색 점들과 빨간색 점들은 3D point라고 생각하면 이해하기 쉽다.  
(출처: [https://manipulation.csail.mit.edu/pose.html](https://manipulation.csail.mit.edu/pose.html))  

SDF라는 단어를 처음 들었을 때, 나도 이해하기가 조금 힘들었다...  

아래의 동영상도 한번 참고해서 이해해보도록 하면 좋을 것 같다.  

- [Understanding the SDF - Signed Distance Field](https://youtu.be/QgzxBN1m9WE)  

**TSDF(truncated signed distance function)는 SDF에서 나오는 값을 Saturation을 걸어서 무한대로 발산하지 않도록 만든 함수**이다.  

이렇게 되면 계산량이 줄어드는 이점을 가져갈 수 있다. Mesh로 3D model을 표현하는 것보다도 효율적으로 측정하고 새로운 Depth data에 대해 빠르게 Reconstruction을 가져갈 수 있다는 것도 TSDF를 사용하는 이유라고 주장하고 있다.     

### Surface prediction  

3D reconstruction을 진행한 후 표면을 조금 더 매끄럽게 만들어주기 위해 Ray-casting을 활용한다.  

Ray-casting이란 3D 볼륨 데이터에서 2D 이미지를 계산하는 기술이다. (출처 : [위키디피아](https://ko.wikide2.com/wiki/Volume_ray_casting))  
이미지가 어떻게 보이는지 계산하므로써 Camera의 pose도 계산을 할 수 있게 된다.  

Kinect fusion에서는 TSDF에서 나온 3D model을 활용해 Frame과 Frame간 Interpolation을 적용한다.  

Interpolation으로 나온 Camera의 Vertex map과 Noraml map을 Camera pose estination을 하는데 다시 사용하기도 한다.  

## Experiment  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/149895109-b7fed4d2-4aad-4d2a-a2f3-9726bd440e2b.png" width = "500" ></p>  

Kinect fusion 알고리즘을 활용해서 사무실을 3D reconstruction을 한 모습이다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/149895386-3dbfa575-602f-491d-a37a-257c1e71b1fb.png" width = "700" ></p>  

Local loop closure를 사용했을 때, 조금 더 정교한 모양으로 3D reconstruction이 되었다는 모습이다.  

Tracking을 하는 방법을 다르게 하거나, Segmentation, Tracking의 결과에 대한 사진도 논문에 자세히 나와있으니 관심이 있으면 한번 보면 좋을 것 같다.  

## Conclusion

본 논문에서 Contribution으로 이야기하는 것은 다음과 같다.  

1. TSDF를 활용해서 지속적으로 Depth data가 들어올 때 마다 Fusion을 하는 방법을 제안했다.  
2. 정확하고 강인한 Tracking 방법을 제안했다.  
3. 병렬적으로 처리가 가능하도록 알고리즘을 만들어서 GPGPU에서 큰 이점을 가진다.  

이렇게 Kinect fusion에 대한 포스팅을 마치겠다.  

부족하지만 자료가 부족해서 한번 적어보았는데 도움이 되었으면 한다. 😆   