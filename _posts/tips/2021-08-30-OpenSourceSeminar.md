---
title : "[2021 오픈소스 컨트리뷰션] Open Source 참여 및 협업을 위한 Github Tip"
category :
    - tips
tag :
    - OpenSource
    - tips
    - Git
toc : true
toc_sticky: true
comments: true
---  

Open Source 참여 및 협업을 위한 Github 활용법을 알아보자!  

이번 [2021 오픈소스 컨트리뷰션 아카데미](https://www.oss.kr/contribution_academy)에 참가하면서 오픈소스에 참여에 관한 강의를 들을 수 있게 되었다.  

강의는 리얼리눅스 [송태웅 대표님](https://github.com/Taeung)이 해주셨으며, 이 포스팅의 내용도 강의 내용을 일부 담고 있어, <u>모든 저작권은 송태웅 대표님께 있다는 것을 먼저 알리고 포스팅을 시작</u>하겠다.  

나같은 경우 Add, Commit, Push 등 기본적인 Git 사용법만 알고 이 강의를 들었는데,  
굉장히 유용한 팁들이 많아 이를 기록하고자 한다.  

Git은 버전 관리를 위한 하나의 Tool이다. 오픈소스 프로젝트의 경우 Github을 활용하여 프로젝트를 진행하는 경우가 많다. 그렇다면 단순히 Git 사용법을 알면 오픈소스에 기여를 할 수 있을까?  

아쉽게도 Git 사용법만 가지고는 오픈소스에 기여를 할 수 없다. 프로젝트마다 가지고 있는 Coding Style이 존재하고, commit을 할 때 어떠한 단위로 commit을 진행하는지도 프로젝트마다 다르다.  

그렇다면 오픈소스를 참여하려고 할 때 가장 먼저 해야하는 일이 무엇일까?  

**우선 오픈소스를 잘 읽어야 한다.**  

## 개발자가 오픈소스를 읽는 방법  

우선 우리가 오픈소스를 참여하고자 할 때, 모든 소스 파일을 전부 이해하기란 사실상 불가능에 가까울 것이다. <u>우선 오픈소스에서 핵심적인 Contributor를 파악하는 것이 중요하다.</u> 그 사람이 가장 오픈소스에 대해서 많은 이해를 하고 있을 가능성이 높기 때문이다.  

터미널을 이용하여 다음과 같은 명령어를 활용하여 오픈소스를 읽어보자.  

1. 오픈소스에서 누가 가장 개발을 많이 했는지 확인  
    - `git shortlog -sn | nl` : nl은 파일의 line number 명시 (순위표시용으로 사용)  

2. 개발자별 commit 개수 요약  
    - `git shortlog -h | grep summary`  

3. 개발자별 commit 개수 순위 정리  
    - `git shortlog -h | grep number`  

또한 commit을 통해 어떤 것이 바뀌였는지도 log를 통해 확인이 가능하다.  

1. 전체 소스파일 수정내역(commit) 리스트  
    - `git log --oneline`  

2. 소스수정 내역(commit) 한가지 확인하기  
    - `git show [Commit ID]` : 단, Merge commit 같은 경우 수정 내역이 없다.    

3. 특정 commit의 수정한 파일들 확인하기  
    - `git show [Commit ID] | grep "diff --git"`  

4. 기간을 정해서 수정내역(commit) 확인하기 (2020년 1월 부터 2020년 6월 30일까지)  
    - `git log --oneline --after=2020-01-01 --before=2020-06-30`  

5. 기간을 정해서 특정 기간 내 commit의 갯수를 확인하기 (6월 한달 간)   
    - `git log --oneline --after=2020-06-01 --before=2020-06-30 | wc -l`  

6. 소스파일 수정내역(commit) 옛날것부터 살펴보기  
    - `git log --reverse`  

오픈 소스에서 특정 폴더에 Contribute를 하고 싶은 경우도 있다.  
이럴 때는 특정 폴더를 기준으로 commit 리스트를 확인하자.  

- 특정 폴더를 기준으로 소스 수정내역(commit) 리스트 확인하기  
    - `git log --oneline -- [특정 폴더]/`  

여러 parameter를 조합하여 자기가 원하는 부분만 쏙쏙 확인할 수도 있다.  

예를 들어, 특정 폴더의 6월 한달간 수정내역 리스트를 확인하고 싶을 경우  

- `git log --oneline --after=2020-06-01 --before=2020-06-30 -- [특정 폴더]/`  

## 커밋 메시지 작성 방법  

git log라는 명령어를 활용하면 Commit을 할 때 ID 그리고 commit messege가 나오는 것을 볼 수 있다.  

혼자 Github을 사용할 땐 아무말 대잔치로 Messege를 작성했지만, 오픈소스에 참여를 할 때는 이 Commit을 통해 어떤 것이 바뀌는지 Commit messege를 통해서 알려줘야 한다.  

확실한 정답은 없지만, <u>단어 하나하나도 이 Commit을 확실하게 나타낼 수 있도록 사용해야 한다.</u> 예를 들어 Add, Fix, Support 등 추가를 한다는 의미도 목적이 다르면 다르게 표현을 해야한다.  

이슈 번호를 이용하는 것도 한 가지의 방법이다.  

- 참고 : [Git commit으로 Issue 종료하기(Closing issue with commit)](https://www.hahwul.com/2018/07/27/closing-git-issue-with-commit/)  

## 자주 쓰이는 Git 명령어  

Add, Commit, Push 말고도 오픈소스에 참여를 하다보면 자주 쓰이는 Git 명령어가 있다.  

**첫번째로 알아볼 명령어는 `rebase`이다.**  

Rebase는 merge와 비슷하게 branch를 합칠 때 많이 사용하게 되는데 Commit history를 좀 더 깔끔하게 남기 때문에 다른 작업자들이 Commit history를 볼 때 조금 더 수월하게 볼 수 있다는 장점이 있다.  

실습을 통해 rebase에 대한 감을 잡을 수 있었다.  

**상황 1 : 가장 오래된 역사 부터 두번째 commit 이후에 새로운 커밋 commit 3개 넣기**  

```
# 가장 오래된 commit 두번째로 되감기 
# 두번째로 가장 오래된 commit 의 "pick" 글자를 "edit" 으로 수정하기
# nano 편집기 저장: ctrl + o 
# nano 편집기 나가기: ctrl + x
$ git rebase -i --root  

# 새로운 파일 'hello_1.c'를 만든 후 Add, Commit
$ touch hello_1.c  
$ git add hello_1.c
$ git commit -m "test: add hello_1.c"

# 새로운 파일 'hello_2.c'를 만든 후 Add, Commit
$ touch hello_2.c  
$ git add hello_2.c
$ git commit -m "test: add hello_2.c"

# 새로운 파일 'hello_3.c'를 만든 후 Add, Commit
$ touch hello_3.c  
$ git add hello_3.c
$ git commit -m "test: add hello_3.c"

# continue 옵션을 이용하여 rebase 진행
$ git rebase --continue

# 참고: rebase 과정을 취소하려면 --abort 옵션을 사용할 수 있다.  
```

**상황 2 : 상황 1에서 만든 3개를 commit을 1개로 합치기**  

(1) hello_3.c 커밋과 hello_2.c 커밋을 합친다.  

```
# hello_3.c commit 의 "pick" 글자를 "edit" 으로 수정하기
# nano 편집기 저장: ctrl + o 
# nano 편집기 나가기: ctrl + x
$ git rebase -i --root  

# reset --soft HEAD~1 설명: --hard와는 다르게 commit 정보만 삭제하고 파일 변경분은 남겨둔다.  
$ git reset --soft HEAD~1
$ git status  

$ git commit --amend
$ git rebase --continue 

# 결과를 확인
$ git log --oneline  
$ git show [합쳐진 commit ID]  
```

(2) hello_2.c 커밋과 hello_1.c 커밋을 합친다.  

```
# hello_2.c commit 의 "pick" 글자를 "edit" 으로 수정하기
# nano 편집기 저장: ctrl + o 
# nano 편집기 나가기: ctrl + x
$ git rebase -i --root  

# reset --soft HEAD~1 설명: --hard와는 다르게 commit 정보만 삭제하고 파일 변경분은 남겨둔다.  
$ git reset --soft HEAD~1
$ git status  

$ git commit --amend
$ git rebase --continue 

# 결과를 확인
$ git log --oneline  
$ git show [합쳐진 commit ID]  
```

Rebase에 대한 자세한 설명은 아래의 블로그를 참고해보자.  

- [Git Rebase (1)](https://suhwan.dev/2018/01/21/Git-Rebase-1/)  
- [rebase 로 병합하기](https://backlog.com/git-tutorial/kr/stepup/stepup2_8.html)  
- [3.6 Git 브랜치 - Rebase 하기](https://git-scm.com/book/ko/v2/Git-%EB%B8%8C%EB%9E%9C%EC%B9%98-Rebase-%ED%95%98%EA%B8%B0)  

---


**두번째로 알아볼 명령어는 `stash`다.**  

파일을 잠깐 수정을 하고, Commit log를 수정하고 싶은 일이 있을 때 유용하게 사용할 수 있는 명령어이다.  

`git stash`를 이용하면 불필요한 commit이나 branch를 새로 만들 일이 없다.  

Stash에 대한 자세한 설명은 아래의 블로그를 참고해보자.  
- [git stash 사용법 - 현재 상태를 저장해보자](https://helloinyong.tistory.com/202)  


---  

**마지막으로 알아볼 명령어는 `blame`이다.**  

`git blame`을 활용하면 해당 소스라인 대해서 누가 마지막으로 수정을 했는지 commit ID 추적이 가능하다.  

만약 내가 기여를 원하는 함수가 있다면, 이 함수를 누가 언제 만들었는지 알 수 있다는 뜻이다.  

마찬가지로 아래의 블로그에 설명이 자세히 나와있으니 참고해보자.  
- [Git - blame 명령어](https://codechacha.com/ko/git-blame/)  

상황별로 친절히 어떤 명령어를 사용하는지 잘 정리된 블로그도 있어 한번 보면 좋을 것 같다.  

- [Git 상황별 자주쓰는 명령어 정리](https://steemit.com/kr/@yjiq150/git)  



