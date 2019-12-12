---
title: react[hooks]-개발환경 구축하기
date: 2019-12-12 14:56:55
thumbnail : /images/react-hooks.png
tags: [react]
category : [IT Tech, 2. etc]
---
## 들어가기 앞서

- 기존의 HTML , CSS를 이용하여 충분히 웹 사이트를 만들 수 있습니다. 또한 자바스크립트까지 활용한다면 동적인 요소들도 제어할 수 있습니다. 그러나 앞서 말한 HTML , CSS , 순수한 자바스크립트로만 프론트 엔드를 개발한다는 것은 상당히 복잡하고 어렵습니다. 이러한 부분을 이 글에서 설명할 React를 통해 체계적으로 프론트 엔드 개발을 할 수 있게 됩니다.

## React란 

- 리액트는 페이스북에서 제공을 하고 있는 프론트 엔드 라이브러리 입니다. 이는 컴포넌트 기반으로 개발자가 UI를 설계하도록 도와줍니다.

## React의 특징

### 1. 컴포넌트 기반
- 위에서 언급했듯이 React는 컴포넌트 기반이라고 하였습니다. 이를 설명하기 위하여 아래의 사진을 보도록 하겠습니다.
![dom 트리](/images/react/domTree.png)
- 위의 사진은 이 글에서 실습할 React dom 구조 및 컴포넌트 구조입니다. 일반적인 태그로 아주 간단한 dom 트리구조를 갖고 있습니다. 그러나 빨강색 박스로 한개 이상의 dom을 묶은 컴포넌트 또한 확인할 수 있습니다. 즉, 컴포넌트는 여러개의 태그를 갖을 수 있으며 해당 태그에 대한 로직과 UI 표현을 제어할 수 있습니다.


### 2. virtual dom(가상 dom)
- React를 통해 개발을 하는 개발자는 상황에 따라 다르지만 대게 직접적인 Dom 검색을 통하여 UI에 접근하지 않습니다. 개발자는 컴포넌트의 상태만을 관리하며 이러한 상태를 React의 적절한 로직을 통해 변경하면 가상 돔은 업데이트되며 실제 돔과의 비교를 하게 됩니다. 이를 통해 변동이 생긴 부분을 아예 삭제하고 변경된 dom을 해당 부분으로 변경하게 됩니다.
![가상 돔](/images/react/virtualDom.png)


### 3. One-way Data Flow (단방향 데이터)
- 위에서 설명한 리액트의 특징을 종합해보면 , dom을 컴포넌트 단위로 분리할 수 있으며 해당 컴포넌트들은 각각의 state(상태)를 갖을 수 있습니다. 이렇게 다수의 컴포넌트들은 데이터를 서로에게 전달할 수 있습니다. 다만, 이러한 데이터의 흐름은 기본적으로 부모 컴포넌트에서 자식 컴포넌트로 단방향 흐름만이 가능합니다.
![단방향 데이터 흐름](/images/react/oneFlow.png)



## React의 개발환경 설정

### 1. 경로 설정 및 관련 라이브러리 다운로드
하위 설명은 node가 설치되어 있다는 전제조건하에 설명에 들어가도록 하겠습니다. 이 글은 v10.16.3 버전으로 작성되었습니다.
- React의 본격적인 개발에 들어가기 앞서 프로젝트 폴더와 관련 라이브러리를 다운로드 받도록 하겠습니다. 현재 저는 Window 운영체제를 사용중이기 때문에 cmd창을 이용하여 설정을 하겠습니다. 아래의 명령어를 통해 디렉토리를 생성합니다. "pntbiz_react"는 개발자가 하고자 하는 이름으로 변경하셔도 됩니다.
 ```
 mkdir pntbiz_react
 ```
- 방금 생성한 디렉토리로 이동하여 npm 초기화를 합니다. 명령어 init을 하면 package name 및 version 등 입력해달라는 정보가 나옵니다. 본 글에서는 전부 엔터를 입력하여 해당 값을 입력하지 않았습니다.
```
 cd pntbiz_react
 npm init
```
- npm을 이용하여 react 라이브러리를 다운로드 합니다. 또한 react-dom 모듈 또한 설치를 합니다. react-dom은 react 모듈을 이용해 만든 내용을 실제 UI 렌더링에 사용되는 모듈입니다.
```
 npm install --save react react-dom
```
- React는 프로젝트 규모가 커질수록 많은 컴포넌트들이 생기며 해당되는 파일들이 무수히 많이 생성됩니다. 게다가 관련된 자바스크립트 , css , 이미지 등까지 합친다면 양이 엄청납니다. webpack은 이러한 수 많은 파일들의 대규모 의존성 파일들을 브라우저가 이해할 수 있도록 번들로 묶고 컴파일하는 역할을 합니다. webpack-cli는 webpack을 사용한 프로젝트를 더욱 유연하게 사용할 수 있도록 커맨드라인으로 제어할 수 있도록 도와줍니다. webpack-dev-server는 webpack을 통해 개발 서버를 열어 개발중인 결과물을 확인할 수 있도록 도와줍니다.  
```
 npm install -D webpack webpack-cli webpack-dev-server
```
- 위에서 언급한 webpack은 다수의 파일을 하나의 번들링으로 묶고 컴파일을 한다고 하였습니다. 여기서 말하는 컴파일이란 브라우저가 이해할 수 있는 언어로 변환되는 것을 의미합니다. 결국 프론트 엔드 개발이기 때문에 최종 결과물은 브라우저가 이해를 해야합니다. 이때 사용되는 모듈이 babel 입니다. 자세히는 자바스크립트 컴파일러이며 최신 자바스크립트 문법인 ES6, ES7 , React 등의 문법을 브라우저가 이해할 수 있도록 도와주며 이러한 설정은 prest이 들어간 모듈들이 도와줍니다.
```
npm install -D @babel/core @babel/preset-env @babel/preset-react babel-loader
```
###### 참고 사항 : npm install에서 D 옵션을 사용함은 개발시에만 필요한 모듈을 분리하기 위함입니다.

### 2. 개발환경 설정
해당 부분에서는 위에서 내려받은 여러 모듈을 통해 React 개발환경을 구축해보도록 하겠습니다.
- 아래는 생성할 디렉토리의 미리보기 구조입니다. 해당 경로와 동일하게 폴더와 파일들을 생성해주세요. 아래 구조의 오른쪽 숫자는 내용을 수정할 파일의 순서입니다.
```
( / : 프로젝트 root 디렉토리 혹은 package.json이 있는 경로 )
/index.html (1)
/index.jsx (3)
/package.json
/package-lock.json
/webpack.config.js (2)
/src
   └─ components
                └─UserList.jsx (4)
                └─User.jsx  (5)
```

#### 1) index.html
- 해당 파일은 실제 서버로 접속하면 처음으로 보이게 되는 html 파일입니다. 해당 소스는 아래와 같습니다.
```
<html>
<head>
    <meta charset="UTF-8"/>
    <title>pntbiz_react</title>
</head>
<body>
<div id="root"></div>
<script src="./dist/app.js"></script>
</body>
</html>
```
- 위 내용은 크게 특별한 점은 없습니다. 일반적은 head와 body로 분리가 되어 있습니다. 약간의 특이점은 하나의 div가 id로 "root"라는 속성을 갖고 있으며 script 파일로 "./dist/app.js" 경로의 app.js 파일을 호출하고 있습니다. id 속성의 값인 "root"는 꼭 "root"일 필요는 없지만 실습과 동일하게 해주세요. 

#### 2) webpack.config.js
> 작성자 : 플랫폼 개발실

##### 출처 
- https://hyunseob.github.io/2016/02/23/start-hexo/
- https://taetaetae.github.io/2016/09/18/hexo_github_blog/