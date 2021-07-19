---
title : "ORB-SLAM3 빌드 및 실행 (Ubuntu 18.04)"
category :
    - SlamTip
tag :
    - ORB-SLAM3
    - tips
toc : true
toc_sticky: true
comments: true
---  
Ubuntu 18.04에서 ORB-SLAM3를 빌드 및 실행해보자!  

좋은 기회로 ORB SLAM3를 스터디할 기회가 생겼다.  

스터디를 진행하기 전 먼저 내 Local computer에서 돌려보려 하는데,  
역시 한번에 빌드가 되지 않아 정리를 하려고 한다.  

ORB SLAM3의 기본적인 구조는 ORB SLAM2와 비슷하다.  

요구하는 라이브러리도 거의 비슷하여 예전에 작성한 [ORB-SLAM2 with WebCam](https://taeyoung96.github.io/slamtip/ORBwithWeb/)를 참고하여  
빌드를 진행했다.  

## Development enviornment  

현재 ORB SLAM3의 Build를 진행한 개발 환경은 다음과 같다.  

- OS : Ubuntu 18.04  
- Gcc : 7.5.0  
- OpenCV : 3.4.5  
- Eigen : 3.3.4  
- OpenSSL : 1.1.1  
- Python : 2.7.17  
- ROS : Melodic  

Github에 이슈를 보니 Eigen 버전에 따라 오류가 많이 생기는 것 같았다.  
참고해서 빌드를 진행하자!  

## How to build ORB SLAM3  

먼저 터미널에서 git clone을 이용해 소스코드를 다운받는다.  

`git clone https://github.com/UZ-SLAMLab/ORB_SLAM3.git`  

그 다음 ORB_SLAM3 폴더 안으로 들어간다.  

`cd ORB_SLAM3`  

그 다음 Thirdparty 폴더에 들어가 Pangolin 라이브러리를 다운 받는다.  

`cd Thirdparty/`  

그럼 경로가 다음과 같이 수정되었을 것이다.  
`~/Desktop/ORB_SLAM3/Thirdparty$`  
(나 같은 경우 바탕화면에서 진행을 했다.)  

이제 Pangolin을 빌드를 진행해보자.  
순서대로 터미널에 입력하면 빌드가 이뤄진다.  

```bash
git clone https://github.com/stevenlovegrove/Pangolin.git  
cd Pangolin  
mkdir build  
cd build  
cmake ..  
make -j8  
sudo make install  
```

Pangolin 빌드까지 완료하였다면, 이제 전체적인 빌드를 진행해보자.  

터미널에서 `cd ..`를 두번 눌러 경로를 다음과 같이 수정하자.  

`~/Desktop/ORB_SLAM3$`  

전체적인 빌드를 위해 
```bash
chmod +x build.sh
./build.sh
```

를 입력하자.  

역시나 한번에 빌드가 되지 않는다...  

내가 발견한 에러 command는 다음과 같다.  

```
CMakeFiles/Makefile2:280: recipe for target 'CMakeFiles/ORB_SLAM3.dir/all' failed
make[1]: *** [CMakeFiles/ORB_SLAM3.dir/all] Error 2
Makefile:83: recipe for target 'all' failed
make: *** [all] Error 2  
```

터미널 창에서 오류가 난 부분을 확인해보니,  
```
/home/taeyoung/Desktop/ORB_SLAM3/src/CameraModels/KannalaBrandt8.cpp:534:41: error: no match for ‘operator/’ (operand types are ‘cv::Matx<float, 3, 1>’ and ‘float’)
         x3D = x3D_h.get_minor<3,1>(0,0) / x3D_h(3);  
```

다행히 많은 사람들이 겪고있던 오류였다.  

[ORB SLAM3 이슈 탭](https://github.com/UZ-SLAMLab/ORB_SLAM3/issues/300)에서 해결 방법을 찾을 수 있었다.  

OpenCV 버전 문제인 것으로 생각이 든다.  

문제를 해결하기 위해 `/src/` 폴더에 있는 `LocalMapping.cc`에서 Line 628번째에 있는  
`x3D = x3D_h.get_minor<3,1>(0,0) / x3D_h(3);` 를 주석 처리하고,  
그 밑에 Line에 
```cpp
x3D = cv::Matx31f(x3D_h.get_minor<3,1>(0,0)(0) / x3D_h(3), x3D_h.get_minor<3,1>(0,0)(1) / x3D_h(3), x3D_h.get_minor<3,1>(0,0)(2) / x3D_h(3));
```
 를 추가하자.  

또 `/src/CameraModels/` 폴더에 있는 `KannalaBrandt8.cpp`에서 Line 534번째에 있는  
`x3D = x3D_h.get_minor<3,1>(0,0) / x3D_h(3);` 를 주석 처리하고, 동일하게  
그 밑에 Line에 
```cpp
x3D = cv::Matx31f(x3D_h.get_minor<3,1>(0,0)(0) / x3D_h(3), x3D_h.get_minor<3,1>(0,0)(1) / x3D_h(3), x3D_h.get_minor<3,1>(0,0)(2) / x3D_h(3));
```
를 추가하자.  

그리고 Build를 다시 진행해주면 되는데 주의할 점이 있다.  

**Build를 다시 진행할 때 `Thirdparty` 폴더에 있는 `DBoW2` 와 `g2o` 폴더에 각각 `build` 폴더를 삭제해준 후 다시 빌드를 진행해야 한다.**  

(다시 확인해보니, 굳이 폴더를 삭제해줄 필요는 없다. 😅)  

빌드를 다시 진행해보면  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/125924757-c832b4b7-e948-4383-a868-42d525439da2.png" width = "600" ></p>  

다음과 같이 빌드가 완료됐다는 문구가 보일 것이다.  

빌드 성공이다! 😃  

❗ 오류를 수정하여 fork를 한 내 [ORB_SLAM3](https://github.com/Taeyoung96/ORB_SLAM3)에 update를 해두었다. 바로 이 repository를 사용하면 오류를 수정할 필요가 없어진다.  

## 데이터셋을 이용한 코드 실행하기  

빌드가 다 됐으니, 이제 데이터 셋을 다운 받고, 코드를 실행시켜보자.  

먼저 EuRoC Dataset을 다운 받을 수 있는 [링크](https://projects.asl.ethz.ch/datasets/doku.php?id=kmavvisualinertialdatasets)에 들어가 데이터 셋을 다운 받자.  

ASL format으로 되어있는 데이터셋을 다운 받아야 하는데,  
<p align="center"><img src="https://user-images.githubusercontent.com/41863759/125930333-6d614556-f95a-4b79-b617-f5466467da12.png" width = "500" ></p>  

나같은 경우는 'Machine Hall 01'을 다운 받아 돌려보도록 하겠다.  

Dataset을 다운을 받으면 Dataset의 경로를 지정해주어야 한다.  

`ORB_SLAM3` 폴더에 `DataSets/EuRoC/MH01` 폴더를 차례로 만들고 Dataset의 압축을 풀어주자.  

Dataset의 압축이 풀리면 `mav0` 라는 이름의 폴더가 생길 것이다.  

다시한번 정리하면 `mav0` 폴더가 있는 경로는 다음과 같다.  

`~/ORB_SLAM3/DataSets/EuRoC/MH01$`  

ORB SLAM3의 Github에 나와있는 `./euroc_examples`를 이용한다면 전체적인 Dataset이 필요하기 때문에 직접 실행을 위한 명령어를 터미널에 작성해보겠다.  

ORB SLAM3에는 크게 4가지 모드가 존재한다.  

1. Monocular mode  
2. Monocular-Inertial mode  
3. Stereo mode  
4. Stereo-Inertial mode  

Inertial mode에서는 IMU를 사용하여 map을 만들고, 그렇지 않은 경우는 Only camera만을 이용하여 map을 만드는 것이라 이해하면 쉽다.  

그리고 ORB SLAM3의 경우 Atals, 즉 Multi map의 경우도 mapping이 가능하기 때문에 여러 sequence를 이용하여 하나의 큰 map을 만드는 것이 가능하다.  

Multi-session 모드도 위와 마찬가지로 총 4가지 모드가 있는데,  
현재 포스팅에서는 Stereo mode로 multi map을 만드는 것을 보여주도록 하겠다.  

ORB SLAM3를 실행할 때는 다음과 같은 규칙으로 command를 입력하면 된다.  

> **[실행하고자 하는 Mode] [장소 인식에 필요한 Vocabulary.txt] [Dataset에 맞는 파라미터를 넣어주기 위한 yaml파일] [데이터셋 경로] [데이터셋에 맞는 Timestamp.txt]**  

위 command를 한 줄에 입력하자.  


### Monocular mode  

`ORB_SLAM3` 폴더에서 `Examples` 폴더로 경로를 이동 후, (`~/Desktop/ORB_SLAM3/Examples$ `)  
다음과 같이 command를 입력하자.  

```bash
./Monocular/mono_euroc ../Vocabulary/ORBvoc.txt ./Monocular/EuRoC.yaml ../DataSets/EuRoC/MH01/ ./Monocular/EuRoC_TimeStamps/MH01.txt
```  

실행시키면 다음과 같은 결과를 볼 수 있다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/125934249-8d2a480e-b32f-4c32-9518-6167b66811b5.png" width = "500" ></p>  

### Monocular-Inertial mode  

위와 마찬가지로 경로를 `~/Desktop/ORB_SLAM3/Examples$`로 이동 후,  
다음과 같이 command를 입력하자.  

```bash
./Monocular-Inertial/mono_inertial_euroc ../Vocabulary/ORBvoc.txt ./Monocular-Inertial/EuRoC.yaml ../DataSets/EuRoC/MH01/ ./Monocular-Inertial/EuRoC_TimeStamps/MH01.txt
```  

결과 사진은 다음과 같다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/125934505-1b79a576-71a3-44ea-b4b6-a2063e8e0f80.png" width = "500" ></p>  

### Stereo mode  

mode만 바뀌었다.  

위와 같은 경로, 그리고 command는 다음과 같이 입력하자.  

```bash
./Stereo/stereo_euroc ../Vocabulary/ORBvoc.txt ./Stereo/EuRoC.yaml ../DataSets/EuRoC/MH01/ ./Stereo/EuRoC_TimeStamps/MH01.txt
```  

결과 사진은 다음과 같다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/125934956-6a7b9a58-9dd7-448f-8e66-90a5abfb7e72.png" width = "500" ></p>  

### Stereo-Inertial mode

mode만 바뀌었다.  

command는 다음과 같이 입력을 해주면 된다.  

```bash
./Stereo-Inertial/stereo_inertial_euroc ../Vocabulary/ORBvoc.txt ./Stereo-Inertial/EuRoC.yaml ../DataSets/EuRoC/MH01/ ./Stereo-Inertial/EuRoC_TimeStamps/MH01.txt
```  

결과 사진은 다음과 같다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/125935792-2fecd67f-90f3-479d-bb1b-4ef0c80233ad.png" width = "500" ></p>  

### Multi-session mode  

Multi-session은 따로 존재하는 mode라기 보단 여러 sequence의 데이터셋을 불러와서 하나의 Multi map을 만드는 것이라 생각하면 된다.  

여러 Dataset을 실행시키기 위해 간단하게 다음과 같은 Rule을 이용하여 command를 작성해보자.  

> **[실행하고자 하는 Mode] [장소 인식에 필요한 Vocabulary.txt] [Dataset에 맞는 파라미터를 넣어주기 위한 yaml파일] [데이터셋 sequence1 경로] [데이터셋 sequence1에 맞는 Timestamp.txt]   [데이터셋 sequence2 경로] [데이터셋 sequence2에 맞는 Timestamp.txt] [데이터셋 sequence3 경로] [데이터셋 sequence3에 맞는 Timestamp.txt]**  

되게 복잡한 듯 보이지만 사실 실행할 sequece와 Timestamp만 추가된 것이다.  

우선 Multi-session을 돌리기 위해 다시 Dataset을 다운 받자.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/125930333-6d614556-f95a-4b79-b617-f5466467da12.png" width = "500" ></p>  

Vicon Room2에 대한 ASL format dataset을 모두 다운 받고, 각각 다른 폴더에 다른 sequence를 저장하자.  

다음과 같이 데이터셋을 저장하면 된다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/125936311-49b987fc-9659-48fe-8218-09925e55da66.png" width = "300" ></p> 

V201 폴더엔 Vicon Room2 01에 대한 파일들이 들어있고,  
V202 폴더엔 Vicon Room2 02에 대한 파일, V203 폴더엔 Vicon Room 2 03에 대한 파일들이 들어있다.  

데이터 셋이 모두 준비가 되었다면 다시 경로를  
`~/Desktop/ORB_SLAM3/Examples$ ` 로 이동 후, 다음과 같이 command를 입력한다.  

```bash
./Stereo/stereo_euroc ../Vocabulary/ORBvoc.txt ./Stereo/EuRoC.yaml ../DataSets/EuRoC/V201/ ./Stereo/EuRoC_TimeStamps/V201.txt ../DataSets/EuRoC/V202/ ./Stereo/EuRoC_TimeStamps/V202.txt ../DataSets/EuRoC/V203/ ./Stereo/EuRoC_TimeStamps/V203.txt
```  

모두 한 줄에 입력해야 하는 것을 주의하자!  

그럼 다음과 같은 결과 화면이 나오게 된다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/125936893-f7e3cc11-a9ad-4641-8e6a-1a2248c818f4.png" width = "500" ></p>  

Multi session의 경우, 부분적으로 Map을 여러개 만들어 놓았다가 나중에 합쳐지는 과정을 거친다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/125936932-d8ffabbe-1f58-4ff7-a2cd-2bc6c9eb6ab1.png" width = "500" ></p>  

이렇게 Dataset을 활용한 ORB SLAM3 코드를 실행해봤다.  


## Evaluation Code 돌려보기  

ORB_SLAM3 v0.4 부터는 evaluation에 대한 코드도 제공한다.  

Readme.md에 설명이 자세히 나와있기 하지만, 내가 돌려봤던 코드를 정리하고자 글을 남겨보려 한다.  

Readme.md에 나와있는 EuRoC Dataset을 활용한 [설명](https://github.com/UZ-SLAMLab/ORB_SLAM3#evaluation)을 기준으로 evaluation 코드를 실행해보도록 하겠다.  

먼저 터미널에서 경로는 다음과 같이 설정해준다.  

`~/Desktop/ORB_SLAM3/Examples$`  

그 다음 Readme.md에 나와있는대로 다음과 같은 명령어로 실행하면,  
```
python ../evaluation/evaluate_ate_scale.py ../evaluation/Ground_truth/EuRoC_left_cam/MH01_GT.txt f_dataset-MH01_stereo.txt --plot MH01_stereo.pdf
```

다음과 같은 오류가 뜨는 것을 볼 수 있다.  

```
Traceback (most recent call last):
  File "../evaluation/evaluate_ate_scale.py", line 151, in <module>
    second_list = associate.read_file_list(args.second_file, False)
  File "/home/taeyoung/Desktop/ORB_SLAM3/evaluation/associate.py", line 64, in read_file_list
    file = open(filename)
IOError: [Errno 2] No such file or directory: 'f_dataset-MH01_stereo.txt'
```

Readme.md에서 하라는대로 했는데 왜 안되지 싶었다. 😑  

확인을 해보니, `f_dataset-MH01_stereo.txt`를 직접 만들고 만들어진 `.txt`파일을 argument로 넣어주어야 한다.  

`f_dataset-MH01_stereo.txt`는 ORB SLAM3를 돌리면 만들어지는 'CameraTrajectory.txt'를 의미한다.  

먼저 'Stereo-Inertial' 모드를 실행시켜, 'CameraTrajectory.txt'를 만든다.  
.txt가 만들어진 경로는 `~/Desktop/ORB_SLAM3/Examples$` 이다.  

그리고 나서 Evaluation code를 돌려준다.  

다음과 같이 command를 입력해준다.  

```
python ../evaluation/evaluate_ate_scale.py ../evaluation/Ground_truth/EuRoC_left_cam/MH01_GT.txt CameraTrajectory.txt --plot MH01_stereo.pdf
```

그 결과 터미널 창에  
`0.068498,1.011646,0.047055` 이렇게 숫자가 출력되고,  
`MH01_stereo.pdf`라는 pdf 파일이 생긴 것을 볼 수 있다.  

숫자의 의미는 trans_error , scale, trans_error_GT이다.  

trans_error는 예측 값에서 하나의 point와 Ground truth의 하나의 point를 비교하여 Translation error를 표현한 것이다. trans_error에서 scale 값을 곱한 것이 trans_error_GT이다.  

`evaluate_ate_scale.py`에 여러 매게변수들이 존재하니 참고하면 좋을 것이다.  

내가 얻은 `MH01_stereo.pdf` 결과는 다음과 같다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/126162863-af45de73-bb43-4b3d-b17a-9dc783d5f4fd.png" width = "500" ></p>  

직접 코드로 돌려보니 또 새로운 느낌을 받았다. 👍  








