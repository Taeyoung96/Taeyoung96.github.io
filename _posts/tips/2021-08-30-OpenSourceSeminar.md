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

터미널을 활용하여 다음과 같은 명령어를 활용하여 오픈소스를 읽어보자.  

1. 오픈소스에서 누가 가장 개발을 많이 했는지 확인  
    - `$ git shortlog -sn | nl` : nl은 파일의 line number 명시 (순위표시용으로 사용)  

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

우선, 오픈소스 프로젝트 기여를 할 때는 **새로운 Branch를 생성해서 소스코드를 바꾼 후,  
Add, Commit을 진행하자.**  

만약 Push를 진행하려고 할 때, 
```
! [rejected] master -> master (fetch first)
error: failed to push some refs to 'https://github.com/dalso~~'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull …') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

다음과 같은 에러가 뜬다면, Base에 대한 동기화가 필요한 경우다.  

`git pull`을 활용할 수도 있지만, `fetch`와 `rebase`를 활용하여 fork 뜬 repository를 동기화를 시킬 수도 있다.  

```
# 공식 upstream 저장소에서 최신 commit history 가져오기     
$ git fetch upstream master

# 최신 commit history 기준으로 베이스 갱신 (rebase)
$ git rebase upstream/master

# Fork 한 저장소(GitHub)도 수정하기 (PR 자동 갱신) 
$ git push --force origin master
```

Commit을 쪼개거나 합칠때도 `rebase`라는 명령어가 사용된다.  

예제를 활용하여 rebase를 좀 더 이해해보자.  





정리를 잘해둔 [블로그 - Git 상황별 자주쓰는 명령어 정리](https://steemit.com/kr/@yjiq150/git)가 있으니 참고해보자.  
또한 오픈소스 프로젝트에 PR을 보낼 때, 그 과정에 대해 잘 나타낸 [블로그 - git 초보를 위한 풀리퀘스트(pull request) 방법](https://wayhome25.github.io/git/2017/07/08/git-first-pull-request-story/)도 있으니 참고해보자.  




