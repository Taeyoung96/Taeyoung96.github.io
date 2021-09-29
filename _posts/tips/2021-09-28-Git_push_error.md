---
title : "[Git push error] remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead."
category :
    - tips
tag :
    - Git
    - tips
toc : true
toc_sticky: true
comments: true
---  

Git push를 할 때 나는 에러 해결 법을 알아보자.  

어느 순간부터 Github ID를 활용해서 `git push origin master`를 진행하면 다음과 같은 에러가 나온다.  

```
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
fatal: Authentication failed for 'https://github.com/Taeyoung96/SLAM-Resources-for-Beginner.git/'
```

Github ID가 아닌 token을 활용해서 Push를 진행해야한다.  

## Github Token 생성하기  

우선 Github에 들어가서 Token을 생성하자.  

Token을 생성하는 법은 아래의 블로그를 참고하면 쉽게 만들 수 있다.  

- [GitHub 토큰 인증 로그인: Personal Access Token 생성 및 사용 방법](https://curryyou.tistory.com/344)  
- [깃허브 GitHub 토큰 access tokens 생성하기](https://devmg.tistory.com/166)  

Personal Access Token에서 Token을 Copy하자.  


<p align="center"><img src="https://user-images.githubusercontent.com/41863759/135227288-bb97c0b8-29d2-4609-a4c6-3bb5d3fcc8e8.png" width = "500" ></p>

**Token 번호는 한번 밖에 보이지 않으니 꼭 복사를 하자!**  

## Token 번호를 활용하여 git remote 등록하기

터미널에서 token 번호를 이용하여 다시 저장소를 지정하자!  

터미널에서 다음과 같이 입력하면 된다.  
- `git remote set-url <stream> https://<token>@github.com/<username>/<repo>`  

예시를 들어 설명해보면,  

'stream'의 경우 보통 origin인 경우가 많다.  

`git remote -v`를 활용하여 stream의 상태를 확인하는 것도 좋은 방법이다.  

내 경우 상태를 확인해보니 다음과 같은 식으로 출력이 됐다.  

```
origin	https://github.com/Taeyoung96/SLAM-Resources-for-Beginner.git (fetch)
origin	https://github.com/Taeyoung96/SLAM-Resources-for-Beginner.git (push)
```

'token'의 경우 아까 복사해둔 Token 번호를 그대로 입력하면 된다.  

예시 같은 경우 'username'이 'Taeyoung96'  
그리고 'repo'같은 경우 'SLAM-Resources-for-Beginner.git'이다.  

따라서 아래와 같이 명령어를 입력하면 token번호를 활용하여 저장소가 등록된다.  

- `git remote set-url origin https://<복사한 Token 번호>@github.com/Taeyoung96/SLAM-Resources-for-Beginner.git`  

---

등록을 완료 했으면, 다시 `git push`를 진행하자.  

- `git push origin master`  

```
Counting objects: 3, done.
Writing objects: 100% (3/3), 243 bytes | 243.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0)
To https://github.com/Taeyoung96/SLAM-Resources-for-Beginner.git
 * [new branch]      master -> master
```  

😆정상적으로 push가 된 것을 확인할 수 있다!😆  
