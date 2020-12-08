---
title : "Docker 명령어 및 RUN command option 정리"
category :
    - tips
tag :
    - docker
    - tips
toc : true
toc_sticky: true
comments: true
---  

자주 쓰이는 docker 명령어 및 Run command option에 대해서 정리해보자!  

도커로 환경 Setting하는 일이 많아지면서, docker 명령어에 대해 정리를 해보려 한다.  

### Docker 명령어 정리

**Docker 명령어는 항상 root 권한으로 실행해야 한다!**  

- **Docker 접속**(바로 docker 안에 workspace로 접속한다.) :  
    `docker exec -it [컨테이너 이름] bash`
    `docker attach [컨테이너 이름]` 

- **Docker Image 목록 확인하기** : 
    `docker images`

- **Docker Container 시작하기** :  
    `docker start [컨테이너 이름]`

- **Docker Container 끄기** :  
    `docker stop [컨테이너 이름]`  

- **Docker Container 목록 보기** (현재 실행 중인 컨테이너만) : 
    `docker ps`

- **Docker Container 목록 보기** (꺼져 있는 컨테이너까지 전부 다) : 
    `docker ps -a`  

- **Docker Container 삭제** : 
    `docker rm [컨테이너 이름]`  

- **Docker Image 삭제** : 
    `docker rmi [이미지 이름]`  

- **Docker Container에서 Image 생성하기** : 
    `docker commit [options] [컨테이너 이름] [이미지 이름[:tag name]]`  
    -   옵션 정보  
        - `-a, --author=""` : 생성자 정보  
        - `-m, --message=""`: 이미지 메시지  
        - `-p, --pause=true/false` : 이미지를 생성할 때 컨테이너를 중지(stop) 한 뒤 commit 여부


- **Docker에서 Jupyter notebook 접속하기** : 
    `jupyter notebook --ip=0.0.0.0 --port=[포트 번호] --allow-root` - 포트 번호는 보통 8888이다.  

- **Docker에서 Tensorboard 접속하기** :  
    `tensorboard --logdir=[log 경로] --port [포트 번호] --host=0.0.0.0` - 포트 번호는 보통 6006이다.  


