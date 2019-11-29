---
title: Hexo로 블로그 만들기
date: 2019-10-12 14:56:55
thumbnail : /images/hexo.png
tags: [hexo]
category : [IT Tech, 3. etc]
---
## Hexo 

- Hexo는 Node.js 기반 정적 사이트 생성기(Static site generator)의 일종이다. 여기서는 Hexo 와 hueman 테마를 이용하여 기업 IT 블로그를 구성해 보려고 한다. 맨 마지막에는 하나의 로컬 폴더에서 Hexo 데이터를 블로그로 배포하고, 소스를 백업하는 방법을 설명한다.



### 1. 설치
- 사전준비
  - [node.js](https://nodejs.org/ko/) 설치
  - [git](https://git-scm.com/book/ko/v2/%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-Git-%EC%84%A4%EC%B9%98) 설치
  - 깃허브에 블로그로 사용할 repository 생성 (예 : tech-people.github.io)
  - 깃허브에 hexo 소스를 저장할 repository 생성 (예 :  tech-people-source)



### 2. hexo 설치
2.1. 설치

- 아래와 같이 hexo 를 설치한다.
 ```
 npm install -g hexo-cli
 ```
- hexo 소스를 저장하게될 github 리포지토리(tech-people-source) 를 clone 한후에 해당 위치에서 디렉토리를 생성하면 추후에 관리하기가 편한다.
- tech-people-source 를 clone 한후에 해당 위치에서 아래와 같이 디렉토리를 생성한후 init 을 실행한다.
```
 mkdir <디렉토리명>
 hexo init <디렉토리명>
```
- 제대로 설치되었는지 테스트해기 위해 아래와 같이 서버를 실행한다.
```
hexo s (or server)
```
- 서버를 실행한 후에 웹브라우저에서 http://localhost:4000 에 접속해서 정상적으로 페이지가 나오면 설치가 완료된 것이다.

2.2. 글 작성하기

- 아래 명령어를 실행하면 /source/_post/ 아래에 .md 파일이 생성이 된다.
```
hexo new <글 제목>
```


2.3 작성한 글을 html로 만들기

- 아래 명령어를 실행하면 public 이라는 폴더가 생성되고, 글이 html 로 변환된다.
```
hexo g  (or generate)
```


2.4  깃허브로 푸시하기 (글 배포하기)

- 글을 작성한 후 깃허브에 푸시해야 웹에서 블로그를 확인할수 있다. hexo에서 깃허브의 리포지토리(tech-people.github.io)로 바로 배포하려면 플러그인을 설치해야 한다. 아래의 명령어로 플러그인을 설치한다.
```
npm install hexo-deployer-git --save
```
- _config.yml을 아래와 같이 수정한다.
```
deploy:
  type: git
  repo: https://github.com/tech-people/tech-people.github.io
  branch: master
```
- 아래의 명령어로 깃허브로 블로그를 배포(푸시)한다.
```
hexo d -g 
or
hexo deploy -generate
```
- 마지막으로 https://tech-people.github.io 에 접속하여 배포된 블로그 내용을 확인한다. 만약 수정한 내용이 반영이 안되면 아래 명령어로 기존 데이터를 삭제하고 다시 배포한다.
```
hexo clean
```



### 3. Hueman 테마 설치 

- Hexo 설치가 마무리되었으면, Hueman 테마를 설치해 보자.

3.1. hexo init를 이용하여 만든 폴더로 이동한다.
3.2. 해당 폴더에서 아래의 명령어로 hueman 테마 파일을 clone 한다.
```
git clone https://github.com/ppoffice/hexo-theme-hueman.git themes/hueman
```
3.3. _config.yml에서 theme 부분을 landscape 에서 hueman 으로 수정한다.
```
theme: hueman
```
3.4. themes/hueman 폴더에 있는 _config.yml.example를 _config.yml로 파일명을 변경한다.
3.5. Hueman 테마의 Insight Search 검색엔진을 사용하기 위해 hexo-generator-json-content 를 설치한다.
```
npm install -S hexo-generator-json-content
```
3.6. hexo s (or server)를 이용하여 로컬(http://localhost:4000) 테스트를 해본다.



### 4. 깃허브로 블로그로 배포(푸시)하기
- 위에 언급한것 처럼 아래의 명령어로 배포하면 된다. 실행하면 _config.yml에 명시된 리포지토리로 배포된다.
```
hexo d -g 
or
hexo deploy -generate
```



### 5. Hexo 소스 깃허브에 저장

- hexo d 명령어로 배포하면 public 폴더에 있는 내용만 _config.yml에 설정된 tech-people.github.io 리포지토리로 푸시되기 때문에, 원본소스 모두를 리포지토리에 올리려면 위에서 clone 한 tech-people-source 리포지토리로 소스트리 또는 git 명령어를 이용하여 푸시하면 된다. **결론적으로 하나의 폴더의 내용을 hexo d 와 git 명령어를 통해서 각기 다른 깃허브 리포지토리를 푸시한다고 생각하면 된다.**


> 작성자 : 플랫폼 개발실

##### 출처 
- https://hyunseob.github.io/2016/02/23/start-hexo/
- https://taetaetae.github.io/2016/09/18/hexo_github_blog/