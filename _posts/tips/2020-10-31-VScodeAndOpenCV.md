---
title : "[Ubuntu 18.04] Visual Studio Code(VSCode)에서 OpenCV 연동하기"
category :
    - tips
tag :
    - VSCode
    - tips
    - OpenCV
toc : true
toc_sticky: true
comments: true
---

VSCode에서 OpenCV를 사용해보자!  

이번 포스팅에서는 <u>Github에서 소스코드를 받은 다음 Visual Studio Code를  
이용해 빌드를 해볼 것</u>이다.   

Ubuntu 18.04를 사용하면서 Visual Studio Code를 접해볼 수 있었다.  

처음에 Visual Studio Code를 다루다 보면 빌드 환경을 직접 설정해주어야 하기 때문에  
까다롭다고 느낄 수 있다.  


여러 블로그를 참고하면서 이번에 Visual Studio Code에서 OpenCV 연동하는 법을 정리하고자 한다.  

## 1. VS Code Install & OpenCV Install
먼저 Visual Studio Code 및 OpenCV를 설치해 주어야 한다.  
설치가 안되었다면 [Visual Studio Code 설치하는 방법( Windows / Ubuntu )](https://webnautes.tistory.com/1197?category=752911)를 참고하여 설치를 하길 바란다.  

나같은 경우 포스팅에 나온 단축기 설정까지 동일하게 설정해 주었다.

그리고 OpenCV도 설치를 해주어야 하는데 Ubuntu에서 OpenCV 깔기가 정말 힘들다...  
다음과 같은 블로그를 참고하여 OpenCV를 설치하였다.  
Anaconda나 ROS같이 경로를 바꾸어주는 친구들이 있다면 더욱 더 골치 아프다. 😭  

- [Ubuntu 18.04에 opencv 3.4.0 설치](https://www.youngmaker.net/22)
- [우분투(Ubuntu) 16.04.1 LTS 에 OpenCV 3.2.0 설치하기](https://developer-thislee.tistory.com/15)
- [Ubuntu 16.04 에 Caffe 설치하기(GPU with CUDA)](https://kyubot.tistory.com/93)

만약 OpenCV 설치가 끝난 후 터미널 창에  
`pkg-config --modversion opencv`
를 입력하고 다음과 같은 결과가 나오면 설치해 성공한 것이다.

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97781958-21423000-1bd2-11eb-9297-98e67516a313.png" width = "500" ></p>

나는 OpenCV 3.4.0 Version을 설치했다.  

## 2. 소스 코드 다운 받기

[OpenCV 4로 배우는 컴퓨터 비전과 머신 러닝](https://sunkyoo.github.io/opencv4cvml/)에 나오는 소스 코드를 이제 다운 받아보자!  

터미널을 열고  
`git clone https://github.com/gilbutITbook/006939.git`
를 입력하고 엔터를 누르면 파일이 다운 받아진다.  

## 3. VSCode에서 파일 열고 난 후 Setting  

우리가 VSCode에서 건들여야 할 부분은 크게 3가지이다.

> - CMakeLists.txt
> - task.json
> - 컴파일러 경로  

그럼 차근차근 설정해보자!

이제 그렇다면 VSCode를 열고 좌측 상단에 있는 `파일(F)`를 클릭한다.
그 후 `폴더 열기` 버튼을 누른 후 내가 빌드하고 싶은 소스코드 파일을 Open한다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782168-54d18a00-1bd3-11eb-90a9-b17dc2dea1a2.png" width = "500" ></p>

그럼 이제 컴파일러를 설정하라는 문구가 나온다.  
여기서 GCC 7.5.0을 클릭한다. (어차피 바꿀꺼라 아무거나 해도 상관없다.)  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782198-864a5580-1bd3-11eb-93bd-3f0dc2b43cf8.png" width = "500" ></p>  

그 다음 Ctrl + Shift + P를 눌러서 보이는 입력 창에 c/c++을 입력한 후,   
C/C++: 구성 편집(UI)를 선택한다.  
<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782306-1ab4b800-1bd4-11eb-8e03-d463b1dc8c59.png" width = "500" ></p>  

다음과 같이 컴파일러 경로를 설정한다.
<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782310-1dafa880-1bd4-11eb-9e0f-8a9eb58892b3.png" width = "500" ></p>

그럼 왼쪽에 있는 탐색기 부분이 아래와 같은 모습으로 바뀐 것을 확인할 수 있다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782346-59e30900-1bd4-11eb-991b-99e5ef160cd1.png" width = "300" ></p>

이렇게 컴파일러 경로를 설정한 후, 그 다음으로 건들여야할 녀석은 빌드 작업 구성이다.  

먼저 상단에 있는 `터미널(T)`를 클릭한다. 그리고 `기본 빌드 작업 구성`을 누른다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782568-8ea39000-1bd5-11eb-8ca0-e3b59cef7f6a.png" width = "500" ></p>  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782581-a844d780-1bd5-11eb-8bb3-d92e3d161244.png" width = "500" ></p> 

맨 밑에 `Others`를 클릭한다.  

그리고 `task.json`파일을 아래의 코드를 복사해서 바꾸어준다.  
```json
{
    "version": "2.0.0",
    "runner": "terminal",
    "type": "shell",
    "echoCommand": true,
    "presentation" : { "reveal": "always" },
    "tasks": [
          //C++ 컴파일
          {
            "label": "compile for C++",
            "command": "cd ${fileDirname} && cmake . && make",
            "group": "build",

            //컴파일시 에러를 편집기에 반영
            //참고:   https://code.visualstudio.com/docs/editor/tasks#_defining-a-problem-matcher

            "problemMatcher": {
                "fileLocation": [
                    "relative",
                    "${workspaceRoot}"
                ],
                "pattern": {
                    // The regular expression. 
                   //Example to match: helloWorld.c:5:3: warning: implicit declaration of function 'prinft'
                    "regexp": "^(.*):(\\d+):(\\d+):\\s+(warning error):\\s+(.*)$",
                    "file": 1,
                    "line": 2,
                    "column": 3,
                    "severity": 4,
                    "message": 5
                }
            }
        },
        // 바이너리 실행(Ubuntu)

        {

            "label": "execute",

            "command": "cd ${fileDirname} && ./${workspaceFolderBasename} ",

            "group": "test"

        }

    ]
}

```

이 소스 코드는 [Visual Studio Code에서 CMake 사용하여 OpenCV 코드 컴파일 하기](https://webnautes.tistory.com/933?category=704653)를 참고하였다.  

여기까지 완료하면 왼쪽에 있는 탐색기 부분이 아래와 같이 바뀌었을 것이다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782680-42a51b00-1bd6-11eb-9070-11d2eebf0a93.png" width = "300" ></p>

그럼 이제 CMakeLists.txt파일을 클릭한 후 수정해보자!  

먼저 나와 동일한 소스코드를 빌드한다면  
`get_filename_component(ProjectId ${CMAKE_CURRENT_LIST_DIR} NAME) `만 추가해주면 된다.  

하지만 그렇지 않다면 다음의 소스 코드를 복사하자.  
```c
get_filename_component(ProjectId ${CMAKE_CURRENT_LIST_DIR} NAME)  

string(REPLACE " " "_" ProjectId ${ProjectId})

project(${ProjectId} C CXX)

set (CMAKE_CXX_STANDARD 11)
cmake_minimum_required(VERSION 2.8)
find_package( OpenCV REQUIRED )
 
file(GLOB SOURCES  *.cpp)
 
add_executable(${PROJECT_NAME} ${SOURCES}  )
target_link_libraries( ${PROJECT_NAME} ${OpenCV_LIBS} )
```
## 4. 컴파일 및 빌드

이제..  
모든 준비는 끝났다.  

`main.cpp` 코드를 클릭하고 컴파일 및 실행을 시켜보자! 

단축기를 잘 설정했다면, Ctrl + Alt + C를 눌러 컴파일을 진행할 수 있다.  

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782973-0d013180-1bd8-11eb-84dd-2659a9f5fc3e.png" width = "500" ></p>  

설정이 잘 되었다면 컴파일이 무리 없이 진행될 것이다!  
<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782974-0f638b80-1bd8-11eb-8b10-7551c534a7ad.png" width = "500" ></p> 

왼쪽에 있는 탐색기도 아래와 같이 바뀌었다!

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782976-125e7c00-1bd8-11eb-9a3f-3aa22f44c3a4.png" width = "300" ></p>  

그럼 이제 Ctrl + Alt + R을 눌러 실행을 해보자.  
<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782982-1b4f4d80-1bd8-11eb-9ff1-993db4c105f0.png" width = "500" ></p>  

어떤 파일이냐에 따라 다르겠지만 문제 없이 실행이 된다면 성공이다. 😆

<p align="center"><img src="https://user-images.githubusercontent.com/41863759/97782985-1e4a3e00-1bd8-11eb-8753-d13b6c12d3d0.png" width = "500" ></p>  

야호~






