---
title: 깃허브 블로그 협업하기
date: 2019-10-12 15:45:30
thumbnail : /images/github.jpg
tags: [github,Pull requests]
category : [IT Tech, 3. etc]
---
## 깃허브 블로그 협업하기

- 블로그를 멤버들이 작성하고자 할때, 깃허브라는 협업이 가능한 버전관리 시스템을 최대한 이용해야 합니다. 그럼, 개발 프로젝트와 동일한 프로세스로 블로그 작성법에 대해서 알아보도록 하겠습니다.
---
1. Hexo 소스 리포지토리(https://github.com/tech-people/tech-people-source) 에서 fork 한다.
2. fork 한 소스를 로컬로 clone 한다.
3. 로컬에서 Hexo 작성 환경을 셋팅한다. ['Hexo로 블로그 만들기 참고'](https://tech-people.github.io/2019/10/12/hexo/)
4. 글 작성후 로컬에서 테스트한다.
5. 작성 및 테스타가 완료되면, 깃허브로 푸시하고, 원본 리포지토리 master 브랜치로 PR (Pull requests) 을 날린다.
6. 관리자는 PR 요청을 받은 내용을 확인하고, 머지한후 블로그에 배포한다.


참고 : [Pull requests 이란?](https://wayhome25.github.io/git/2017/07/08/git-first-pull-request-story/)

> 작성자 : 플랫폼 개발실