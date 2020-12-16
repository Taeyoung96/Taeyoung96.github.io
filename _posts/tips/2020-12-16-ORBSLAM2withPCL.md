---
title : "ORB-SLAM2 with PCL(Point Cloud Map Library)"
category :
    - tips
tag :
    - ORB-SLAM2
    - tips
toc : true
toc_sticky: true
comments: true
---  
'ORBSLAM2_with_pointcloud_map'를 실행시켜보자!  

ORBSLAM2를 이용하여 Point Cloud Map을 만드는 멋진 Github 저장소가 있다.  
[ORBSLAM2_with_pointcloud_map](https://github.com/gaoxiang12/ORBSLAM2_with_pointcloud_map)에서 Git clone을 이용해서 다운을 받고,  
pcl library를 이용해서 RGB-D Image에 Dense한 Map을 만들 수 있다.  

하지만...  
실행하는 것 조차 너무 복잡해서 정리를 해 놓으려고 한다.

[ORBSLAM2_with_pointcloud_map](https://github.com/gaoxiang12/ORBSLAM2_with_pointcloud_map)의 소스코드를 이용하면  
<p align="center"><img src="https://user-images.githubusercontent.com/41863759/102325980-29f69600-3fc7-11eb-8420-3f2e9f07333e.png" width = "500" ></p>

다음과 같은 Pointcloud Map을 Viewer를 통해서 볼 수 있다!  

## 1. Git clone 및 버전 확인  
이번 repository를 다운을 받고,  
**실행을 시켜보면서 느낀건 라이브러리 버전을 일치하는 것이 정말 중요하다는 것이다.**  

우선 컴퓨터의 Spec은 다음과 같다.  
- OS : Ubuntu 18.04  
- CPU : Intel(R) Core(TM) i9-9900K CPU @ 3.60GHz  
- RAM : 32G  
- GPU : GeForce RTX 2070 (여기서 GPU가 쓰이는지는 모르지만..ㅎ)  

그리고 라이브러리 버전도 적어보면,  
- OpenCV : 3.4.0  
- Eigen : 3.3.4  
- CMake : 3.10.2  

**<font color='#FF0000'>만약 버전이 나와 다를시, 내 방법대로 빌드를 진행해도 실행이 되지 않을 수 있다..</font>**

Github을 통해 다운 받을 오픈소스는  
1. [Pangolin](https://github.com/stevenlovegrove/Pangolin)  
2. [g2o](https://github.com/RainerKuemmerle/g2o)  
3. [DBoW2](https://github.com/dorian3d/DBoW2)  


이 Repository를 실행시키는데 핵심은 **<font color='#0000FF'>CMakeList.txt</font>** 이다.  

우선 터미널을 열고,  
`git clone https://github.com/gaoxiang12/ORBSLAM2_with_pointcloud_map.git`  
를 입력하자.  

## 2. 빌드 진행 (g2o_with_orbslam2/)

Repository 안에 있는 **orbslam2_modified.zip**의 압축 파일을 풀어주자.  
`cd ORBSLAM2_with_pointcloud_map/`  
`unzip orbslam2_modified.zip `  

터미널에 'ORB_SLAM2_modified/CMakeLists.txt'를 바꾸냐는 것을 물어보는데 `A`를 눌러 모두 replace를 해준다.  

[ORBSLAM2_with_pointcloud_map](https://github.com/gaoxiang12/ORBSLAM2_with_pointcloud_map)의 README.md를 참고하면,  
`g2o_with_orbslam2/`를 먼저 빌드한 후,  
`ORB_SLAM2_modified/`를 빌드하라고 설명이 나와있다.  

```
 cd g2o_with_orbslam2
 mkdir build
 cd build
 cmake ..
 make 
```
를 차례로 터미널에 입력하여 `g2o_with_orbslam2/` 빌드를 진행하자.  

---

### 오류 해결법  

빌드를 진행하다보면 `make`에서  
`error: no matching function for call to ‘g2o::SE2::setRotation(Eigen::Rotation2D<double>::Scalar)’
     t.setRotation(t.rotation().angle()+_measurement);`라는 오류가 등장한다.  

당황하지 말고 `g2o_with_orbslam2/g2o/types/slam2d/edge_se2_pointxy_bearing.cpp`에 들어가서  

line 51번째 줄 ` t.setRotation(t.rotation().angle()+_measurement);`를  

<p align="center"> 👇👇👇 </p> 

`t.setRotation((Eigen::Rotation2Dd)(t.rotation().angle()+_measurement));`로 바꿔준다.  

- 출처 : [Github Issue : ERROR Building g2o_modified](https://github.com/gaoxiang12/ORBSLAM2_with_pointcloud_map/issues/3)  

이 문제가 해결 된다면 `make`를 해보자.  
또다른 error를 만날 수 있다. (괜찮다 아직 볼 error가 많다 ㅎㅎ)  

`error: static assertion failed: YOU_MIXED_DIFFERENT_NUMERIC_TYPES__YOU_NEED_TO_USE_THE_CAST_METHOD_OF_MATRIXBASE_TO_CAST_NUMERIC_TYPES_EXPLICITLY     #define EIGEN_STATIC_ASSERT(X,MSG) static_assert(X,#MSG);`라는 오류가 등장한다.  

구글링해본 결과 Eigen의 버전 문제다.  
[libeigen3-dev 3.2.0-8](https://launchpad.net/ubuntu/trusty/amd64/libeigen3-dev/3.2.0-8)에서 `libeigen3-dev_3.2.0-8_all.deb`를 클릭해서 다운받는다.  

또 다른 터미널에서 Downloads를 들어가 
`sudo dpkg -i libeigen3-dev_3.2.0-8_all.deb`
를 입력한다.  

- 출처 : [ubuntu16.04 orbslam ./build.sh 出错eigen](https://www.cnblogs.com/bigzhao/p/6215392.html) - 중국인의 블로그인 것 같다. 번역기를 키고 보자.  

다시 `make`를 눌러보자.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/102332589-a1c8be80-3fcf-11eb-9274-a14bc3cc3852.png" width = "600" ></p>  

위와 같은 문구가 터미널창에 떳다면, 빌드가 성공한 것 이다!  

## 3. 빌드 진행 (ORB_SLAM2_modified/)

다음 단계를 이제 진행해보자.  
터미널에서 `cd ..`를 2번 눌러  
`~ ORBSLAM2_with_pointcloud_map/`로 디렉토리 경로를 변경해준다.  

```
cd ORB_SLAM2_modified
mkdir build
cd build
cmake ..
make
```  
이것도 차례대로 터미널에 입력하여 빌드를 진행한다.  

이 Repository는 ORB_SLAM2가 Base이다.  

따라서 `ORB_SLAM2_modified/Thirdparty`에 들어가 추가적인 빌드를 진행해 주어야 한다.  

`ORB_SLAM2_modified/Thirdparty`안에 들어가보면,  
`DBow2/`, `g2o/` 폴더가 존재하는데 다시 빌드를 진행해주어야 한다.  
(소스 코드 저자의 경로로 빌드가 되어있기 때문!)  

`ORB_SLAM2_modified/Thirdparty/DBoW2`의 폴더에 들어간 후,  
`build/` 폴더를 삭제한 후,  
```
mkdir build
cd build
cmake ..
make
```
를 진행한다.  
<p align="center"><img src="https://user-images.githubusercontent.com/41863759/102343854-6da8ca00-3fde-11eb-810d-173568999126.png" width = "500" ></p>

이와 같이 터미널에 출력이 되면 `DBow2/` 빌드가 성공한 것이다.  

마찬가지로 `g2o/`도 다시 빌드를 진행한다.  

`ORB_SLAM2_modified/Thirdparty/g2o`의 폴더에 들어간 후,  
`build/` 폴더를 삭제한 후,  
```
mkdir build
cd build
cmake ..
make
```
를 진행한다.  
<p align="center"><img src="https://user-images.githubusercontent.com/41863759/102344345-19521a00-3fdf-11eb-8e9a-a491ef7b89c0.png" width = "500" ></p>  

이와 같이 터미널에 출력이 되면 `g2o/` 빌드가 성공한 것이다. 

추가적으로 `Thirdparty/`에 다른 오픈소스도 빌드를 진행해야 한다.  

### Pangolin 설치 

터미널에서 경로를 `~Thirdparty/`로 이동한다.  
터미널에 아래의 명령어들을 순서대로 입력해준다.  
```
git clone https://github.com/stevenlovegrove/Pangolin.git
cd Pangolin
mkdir build
cd build
cmake ..
make -j8
sudo make install
```  

문제 없이 Pangolin을 빌드를 한다면 이제 `ORB_SLAM2_modified/`를 빌드해보자!

---

### 오류 해결법  

첫번째로 만난 오류는  
`cmake ..`를 입력했을 때,  

```
CMake Error at CMakeLists.txt:33 (find_package):
  Could not find a configuration file for package "OpenCV" that is compatible
  with requested version "2.4.3".
```  
라는 오류가 떴다.  


CMakeList.txt에 OpenCV 버전이 맞지 않아 생긴 오류이다.  

`ORB_SLAM2_modified/CMakeLists.txt`를 눌러,  

```c
find_package(OpenCV 2.4.3 QUIET)
```  
를 아래처럼 바꿔준다. (다른 부분은 모두 동일!)  

```c
find_package(OpenCV 3.0 QUIET)
if(NOT OpenCV_FOUND)
   find_package(OpenCV 2.4.3 QUIET)
   if(NOT OpenCV_FOUND)
      message(FATAL_ERROR "OpenCV > 2.4.3 not found.")
   endif()
endif()
```  
내 경험상 이 오류 하나만 잡으면 `cmake ..`는 완료할 수 있다.  

---

다음 `make`를 누르면,  
```
fatal error: g2o/types/sim3/types_seven_dof_expmap.h: No such file or directory
 #include <g2o/types/sim3/types_seven_dof_expmap.h>
          ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

```  

이런 에러가 보이게 된다.  
`g2o/`의 경로가 달라서 생기는 문제인데,  
`ORB_SLAM2_modified/include/LoopClosing.h`에 직접 들어가보면,  

```c
#include <g2o/types/sim3/types_seven_dof_expmap.h>
```

이렇게 선언을 해준 line을 볼 수가 있다.  

이 부분을  
```c
#include <Thirdparty/g2o/g2o/types/types_seven_dof_expmap.h>
```  
이렇게 수정을 해준다.  

---

수정을 하고 다시 `make`를 눌러보면,  
```
fatal error: g2o/types/sba/types_six_dof_expmap.h: No such file or directory
 #include <g2o/types/sba/types_six_dof_expmap.h>
          ^~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
```  

이와 같이 아까와 비슷한 error를 만날 수 있게 된다.  

아까와 같은 방법으로 '#include'의 경로를 바꾸어 준다.  

```c
#include <g2o/types/sba/types_six_dof_expmap.h>
#include <g2o/types/sim3/types_seven_dof_expmap.h>
```  
<p align="center"> 👇👇👇 </p> 
```c
#include <Thirdparty/g2o/g2o/types/types_six_dof_expmap.h>
#include <Thirdparty/g2o/g2o/types/types_seven_dof_expmap.h>
```  

---

똑같은 에러가 반복되는데  
`ORB_SLAM2_modified/include/Optimizer.h`에 들어가서  

```c
#include <g2o/types/sim3/types_seven_dof_expmap.h>
```  
<p align="center"> 👇👇👇 </p> 
```c
#include <Thirdparty/g2o/g2o/types/types_seven_dof_expmap.h>  
```  

---

`ORB_SLAM2_modified/src/Optimizer.cc`에 들어가서  

```c
#include <g2o/core/block_solver.h>
#include <g2o/core/optimization_algorithm_levenberg.h>
#include <g2o/solvers/eigen/linear_solver_eigen.h>
#include <g2o/types/sba/types_six_dof_expmap.h>
#include <g2o/core/robust_kernel_impl.h>
#include <g2o/solvers/dense/linear_solver_dense.h>
#include <g2o/types/sim3/types_seven_dof_expmap.h>
```  
<p align="center"> 👇👇👇 </p> 
```c
#include "Thirdparty/g2o/g2o/core/block_solver.h"
#include "Thirdparty/g2o/g2o/core/optimization_algorithm_levenberg.h"
#include "Thirdparty/g2o/g2o/solvers/linear_solver_eigen.h"
#include "Thirdparty/g2o/g2o/types/types_six_dof_expmap.h"
#include "Thirdparty/g2o/g2o/core/robust_kernel_impl.h"
#include "Thirdparty/g2o/g2o/solvers/linear_solver_dense.h"
#include "Thirdparty/g2o/g2o/types/types_seven_dof_expmap.h"
```  

---

이제 `make`를 하면,  
```
/usr/bin/ld: cannot find -lg2o_core
/usr/bin/ld: cannot find -lg2o_types_slam3d
/usr/bin/ld: cannot find -lg2o_solver_csparse
/usr/bin/ld: cannot find -lg2o_stuff
/usr/bin/ld: cannot find -lg2o_csparse_extension
/usr/bin/ld: cannot find -lg2o_types_sim3
/usr/bin/ld: cannot find -lg2o_types_sba
collect2: error: ld returned 1 exit status
CMakeFiles/ORB_SLAM2.dir/build.make:927: recipe for target '../lib/libORB_SLAM2.so' failed
make[2]: *** [../lib/libORB_SLAM2.so] Error 1
```  
라는 새로운 형태의 error가 나오게 된다.  

타겟을 한 라이브러리의 링크가 달라서 생기는 오류인 것으로 추측이 된다.  


이제 `ORB_SLAM2_modified/CMakeLists.txt`에 들어가서  
```c
include_directories(
${PROJECT_SOURCE_DIR}
${PROJECT_SOURCE_DIR}/include
${EIGEN3_INCLUDE_DIR}
${Pangolin_INCLUDE_DIRS}
${PCL_INCLUDE_DIRS}
)

...

target_link_libraries(${PROJECT_NAME}
${OpenCV_LIBS}
${EIGEN3_LIBS}
${Pangolin_LIBRARIES}
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
g2o_core g2o_types_slam3d g2o_solver_csparse g2o_stuff g2o_csparse_extension g2o_types_sim3 g2o_types_sba
${PCL_LIBRARIES}
)
```
<p align="center"> 👇👇👇 </p> 
```c
include_directories(
${PROJECT_SOURCE_DIR}
${PROJECT_SOURCE_DIR}/include
${EIGEN3_INCLUDE_DIR}
${Pangolin_INCLUDE_DIRS}
${PROJECT_SOURCE_DIR}/Thirdparty/Pangolin/include
${PCL_INCLUDE_DIRS}
)

...

LINK_DIRECTORIES(
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib
${PROJECT_SOURCE_DIR}/Thirdparty/Pangolin/lib
${PROJECT_SOURCE_DIR}/lib)

target_link_libraries(${PROJECT_NAME}
${OpenCV_LIBS}
${EIGEN3_LIBS}
${Pangolin_LIBRARIES}
${PCL_LIBRARIES}
${PROJECT_SOURCE_DIR}/Thirdparty/DBoW2/lib/libDBoW2.so
${PROJECT_SOURCE_DIR}/Thirdparty/g2o/lib/libg2o.so
)
```  

이런 식으로 교체한다. (다른 부분은 건들이지 말자)  

다시 `make`를 진행하면,  
<p align="center"><img src="https://user-images.githubusercontent.com/41863759/102346623-80250280-3fe2-11eb-8446-23f5e3499fb9.png" width = "600" ></p>  

길고 길었던 빌드가 끝났다!  

## 4. Example 돌려보기  

빌드가 끝났으니 ORBvoc.txt와 Dataset을 준비하고 예제소스를 돌려보자.  

[ORB_SLAM2](https://github.com/raulmur/ORB_SLAM2)도 따로 git clone을 진행한 후 받아 놓는다.  

ORBvoc.txt는 원래 ORB_SLAM2 repository에 `Vocabulary/` 폴더에 가보면 압축파일 형태로 존재한다.  

압축을 풀고, `ORBvoc.txt`를 `ORB_SLAM2_modified/Vocabulary/`에 넣어준다.  

Dataset같은 경우 TUM Dataset을 이용하였다. [여기](https://vision.in.tum.de/data/datasets/rgbd-dataset/download)서 원하는 데이터셋을 다운 받을 수 있다.  

링크를 타고 들어가서 `fr1/room`를 다운받았다.  

- [ORB_SLAM2](https://github.com/raulmur/ORB_SLAM2)의 Readme.md를 참고하면 된다.  

`ORB_SLAM2_modified/`폴더에 따로 데이터 셋 폴더를 만든 후, `fr1/room`를 옮겨 놓았다.  

```
mkdir datasets
cd datasets
```
그리고 'rgbd_dataset_freiburg1_room'폴더를 옮긴다.  

경로를 `~ /ORB_SLAM2_modified`으로 이동시킨 후,  
실행 명령어를 입력한다!  
`./Examples/RGB-D/rgbd_tum Vocabulary/ORBvoc.txt Examples/RGB-D/TUM1.yaml datasets/rgbd_dataset_freiburg1_room/ Examples/RGB-D/associations/fr1_room.txt
`
위의 명령어를 한 줄로 입력해야 한다!  

문제 없이 실행이 된다면

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/102348499-57523c80-3fe5-11eb-89d5-a8496328f556.png" width = "700" ></p>  

위와 같은 화면이 나올 것이다.  

**만약 실행 도중**
```
New map created with 854 points
receive a keyframe, id = 1
generate point cloud for kf 1, size=24994
show global map, size=3659
receive a keyframe, id = 2
generate point cloud for kf 2, size=25314
Segmentation fault (core dumped)
```
라는 오류 메세지(혹은 비슷한 오류 메세지)가 뜨고 종료가 된다면,  

PCL 라이브러리의 버전 문제일 가능성이 높다.  

`ORB_SLAM2_modified/src/pointcloudmapping.cc`의 viewer() 함수의 약간의 수정이 필요한데,  
```cpp
...

void PointCloudMapping::viewer()
{
    pcl::visualization::CloudViewer viewer("viewer");
    while(1)
    {
        {
            unique_lock<mutex> lck_shutdown( shutDownMutex );
            if (shutDownFlag)
            {
                break;
            }
        }
        {
            unique_lock<mutex> lck_keyframeUpdated( keyFrameUpdateMutex );
            keyFrameUpdated.wait( lck_keyframeUpdated );
        }
        
        // keyframe is updated 
        size_t N=0;
        {
            unique_lock<mutex> lck( keyframeMutex );
            N = keyframes.size();
        }
        
        for ( size_t i=lastKeyframeSize; i<N ; i++ )
        {
            PointCloud::Ptr p = generatePointCloud( keyframes[i], colorImgs[i], depthImgs[i] );
            *globalMap += *p;
        }
        // 출처 : https://zhuanlan.zhihu.com/p/92642910
        //PointCloud::Ptr tmp(new PointCloud());
        //voxel.setInputCloud( globalMap );
        //voxel.filter( *tmp );
        //globalMap->swap( *tmp );
        viewer.showCloud( globalMap );
        cout<<"show global map, size="<<globalMap->points.size()<<endl;
        lastKeyframeSize = N;
    }
}
```

위와 같이 중간에 주석처리를 해준다.  

- 출처 : [https://zhuanlan.zhihu.com/p/92642910](https://zhuanlan.zhihu.com/p/92642910) - 중국인의 블로그다. 번역기를 켜놓고 보자.  

이렇게 [ORBSLAM2_with_pointcloud_map](https://github.com/gaoxiang12/ORBSLAM2_with_pointcloud_map)의 소스 코드를 돌려봤다.  

다른 분들에게도(미래의 나에게도) 도움이 되었으면 한다.

























