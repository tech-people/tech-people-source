---
title: React[hooks]-개발환경 구축하기 1부
date: 2019-12-12 14:56:55
thumbnail : /images/react-hooks.png
tags: [react]
category : [IT Tech, 3. React]
---

> 작성자 : 플랫폼 개발실 R&D팀 유주빈

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
- React를 통해 개발을 하는 개발자는 상황에 따라 다르지만 대게 직접적인 Dom을 제어하여 UI를 수정하지 않습니다. 개발자는 컴포넌트의 상태만을 관리하며 이러한 상태를 React의 적절한 로직을 통해 변경하면 가상 돔은 업데이트가 되고 실제 돔과의 비교를 하게 됩니다. 이를 통해 변동이 생긴 부분을 아예 삭제하고 변경된 dom을 해당 부분으로 변경하게 됩니다.
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
- React는 프로젝트 규모가 커질수록 많은 컴포넌트들이 생기며 해당되는 파일들이 무수히 많이 생성됩니다. 게다가 관련된 자바스크립트 , css , 이미지 등까지 합친다면 양이 엄청나게 불어납니다. webpack은 이러한 수 많은 파일들의 대규모 의존성을 파악하고 브라우저가 이해할 수 있도록 번들로 묶고 컴파일하는 역할을 합니다. webpack-cli는 webpack을 사용한 프로젝트를 더욱 유연하게 사용할 수 있도록 커맨드라인으로 제어할 수 있도록 도와줍니다. webpack-dev-server는 webpack을 통해 개발 서버를 열어 개발중인 결과물을 확인할 수 있도록 도와줍니다.  
```
 npm install -D webpack webpack-cli webpack-dev-server
```
- 위에서 언급한 webpack은 다수의 파일을 하나의 번들링으로 묶고 컴파일을 한다고 하였습니다. 여기서 말하는 컴파일이란 브라우저가 이해할 수 있는 언어로 변환되는 것을 의미합니다. 결국 프론트 엔드 개발이기 때문에 최종 결과물은 브라우저가 이해를 해야합니다. 이때 사용되는 모듈이 babel 입니다. 자세히는 자바스크립트 컴파일러이며 최신 자바스크립트 문법인 ES6, ES7 , React 등의 문법을 브라우저가 이해할 수 있도록 도와주며 이러한 환경은 preset가 들어간 모듈들이 도와줍니다.
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
/dist  (해당 폴더와 app.js는 만들지 않아도 됩니다.)
   └─ app.js
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
- 웹팩의 기본적인 설정은 아래와 같습니다.
###### 참고 : React에서 jsx란 자바스크립트의 확장판 문법입니다. 확장자를 jsx나 jsx로 하나 상관은 없으나 webpack에서 확장자를 이용하여 해당 파일을 어떤 방법으로 컴파일할지 정하기 때문에 해당 글에서는 jsx라고 정하겠습니다.
```
const path = require('path');

module.exports = {
    mode: 'development',
    entry: {
        app: './index.jsx',
    },
    module: {
        rules: [{
            test: /\.jsx?$/,
            loader: 'babel-loader',
            options: {
                presets: ['@babel/preset-env', '@babel/preset-react']
            }
        }],
    },
    output: {
        path: path.join(__dirname, 'dist'),
        filename: 'app.js'
    },
};
```
- 해당 설정에서 가장 중요한 부분은 entry와 module, output 입니다.
- entry : 컴파일을 시작할 파일을 명시합니다. 현재는 indexjsx 파일을 통해 컴파일이 시작되며 하나 이상의 파일을 설정할 수도 있습니다.
- module : entry에서 시작한 컴파일의 규칙을 정하는 부분입니다. 해당 내용에서는 jsx 확장자를 option의 presets에 명시한 설정을 babel-loader가 사용하여 컴파일 하라는 내용입니다.
- output : 컴파일이 완료된 산출물을 생성하는 부분입니다. 현재 dist폴더에 app.js로 생성되도록 설정을 했습니다. 해당 산출물을 index.html에서 호출합니다.

#### 3) index.jsx
```
import React from 'react';
import ReactDOM from 'react-dom';
import UserList from './src/components/UserList.jsx'

ReactDOM.render(<UserList/> , document.querySelector('#root'));
```
- index.jsx는 webpack.config.js에서 컴파일의 시작점으로 지정한 파일이며 react와 react-dom 모듈을 불러오고 있습니다. 또한  dom의 id로 root인 dom에 UserList라는 컴포넌트를 주입하겠다는 설정입니다.
- 여기서 생소한 문법이 있습니다. 바로 <UserList/>입니다. 해당 컴포넌트를 화면에 렌더링을 하기 위해서는 컴포넌트를 태그화하여 표현을 하면 렌더링을 하게 됩니다.

#### 4) UserList.jsx
```
import React from 'react';
import User from "./User.jsx";

export default class UserList extends  React.Component{
    render() {
        return (
            <div>
                <tabe>
                    <User name={'김아무개'} age={25}/>
                    <User name={'이아무개'} age={26}/>
                </tabe>
            </div>
        )
    }
};
```
- 자바스크립트의 class 문법을 통해 React의 Component를 extends 키워드를 통해 상속받아 컴포넌트를 생성하게 됩니다. render는 위에서 언급한 컴포넌트를 태그화하여 표현하게 되면 호출이 되어 return으로 dom을 반환하여 렌더링을 합니다.
- return 내부에는 위의 import 키워드로 불러온 User 컴포넌트를 사용하고 있습니다. 게다가 User 태그의 내부에는 name과 age의 속성을 명시해주고 있으며 '{'를 통해 값이 들어가 있는 것을 확인할 수 있습니다.
- '{}'를 이용하면 return으로 돌려주는 dom에 자바스크립트 문법을 작성을 할 수 있습니다. 이를 위에서 언급한 React애서 jsx를 뜻합니다. 여기서는 User에 name과 age의 속성에 데이터를 내려주는 단방향 흐름을 확인할 수 있습니다.

##### 참고 : Javascript + XML를 뜻하는 jsx는 xml형식인 dom 내부에 자바스크립트를 사용할 수 있는 문법을 일컫습니다.

- 리액트는 v16.8 부터 hooks라는 문법을 지원합니다. 해당 문법은 class를 통해서가 아닌 함수와 같은 형태로 함수형 컴포넌트를 만들 수 있도록 해줍니다. 위의 코드를 hooks로 변환하면 아래와 같습니다.
```
import React from 'react';
import User from "./User.jsx";

export default function UserList() {
    return (
        <div>
            <tabe>
                <User name={'김아무개'} age={25}/>
                <User name={'이아무개'} age={26}/>
            </tabe>
        </div>
    );
};
```
- 상단에 react 모듈을 불러오며 class와 동일하게 컴포넌트를 export를 합니다. 다만 함수를 선언하여 내부에 return으로 dom을 반환하면 알아서 React의 Component를 상속받아 생성된 class와 동일한 기능을 하게 됩니다.



#### 5) User.jsx (hooks)
```
import React from 'react';

export default class User extends React.Component{
    constructor(props) {
        super(props);
    };
    render() {
        return (
            <tr>
                <td>{this.props.name}</td>
                <td>{this.props.age}</td>
            </tr>
        )
    }
};
```
- UserList.jsx에서 name과 age 속성을 전달하였습니다. 해당 전달받은 값은 props로 전달이 됩니다. 이는 this.props라는 키워드를 통해 접근할 수 있습니다. 이를 통해 render함수 내부에 값의 위치를 설정할 수 있습니다.
- User.jsx 컴포넌트 또한 hooks 문법으로 표현하면 아래와 같습니다.
```
import React from 'react';

export default function User({name,age}) {
    return (
        <tr>
            <td>{name}</td>
            <td>{age}</td>
        </tr>
    );
};
```
- 함수형 컴포넌트의 파라메터로 값을 받아 개발자가 원하는 dom 위치에 설정을 하면 됩니다.
- class를 기반한 컴포넌트가 좋은지 , 함수형 컴포넌트인 hooks가 좋은지는 개발자별로 취향이 다릅니다. 다만 개인적은 의견으로는 hooks가 코드가 더욱 간결하므로 hooks를 권장드립니다.


#### 6) 빌드 및 실행하기
- cmd 창을 열거나 혹은 개발툴을 이용하여 root 경로로 이동합니다. 이동 후 아래의 명령어를 실행합니다.
```
webpack
```
- 해당 명령어를 통하면 자동으로 webpack.config.js 파일을 찾아 컴파일을 시작합니다. 컴파일이 완료가 되면 /dist/app.js가 있는 것을 확인할 수 있습니다.
- 해당 파일을 확인했다면 webpack 개발 서버를 통해 결과물을 확인할 수 있습니다. 아래의 명령어를 입력합니다.
```
webpack-dev-server
```
- 개발 서버 명령어를 통해 서버가 열리며 기본 포트인 8080으로 서버가 개설됩니다. 이제 브라우저를 열어 localhost:8080을 입력하면 아래와 같은 결과가 브러우저에 나타납니다.
```
김아무개	25
이아무개	26
```
- 비록 css 및 동적인 자바스크립트로 UI를 수정하여 보기 좋은 결과물은 아니지만 기본적인 React의 셋팅 및 구조에 대해서 알아보았습니다.


- 소스 깃허브 주소 : https://github.com/SayRew/pntbiz_react.git
