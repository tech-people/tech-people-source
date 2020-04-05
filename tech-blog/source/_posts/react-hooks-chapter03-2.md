---
title: React-3-2부 [서버 연동]
date: 2020-04-05 15:24:55
thumbnail : /images/react-hooks.png
tags: [react]
category : [IT Tech, 3. React]
---

> 작성자 : 플랫폼 개발실 R&D팀 유주빈

## 들어가기 앞서

- 본글은 React의 hooks문법으로 작성이 되었습니다. 또한 기본적인 리액트 환경셋팅 및 상태관리에 대한 내용은 다루지 않습니다. 해당 내용은 아래의 링크를 참고해주세요.

###### [React 1부 : 개발환경 구축하기](https://tech-people.github.io/2019/12/12/react-hooks-chapter01/)
###### [React 2부 : 상태관리](https://tech-people.github.io/2019/12/12/react-hooks-chapter02/)
###### [React 3-1부 : JWT와 React Router](https://tech-people.github.io/2019/12/12/react-hooks-chapter03-1/)

## 실습할 구조

![구조](/images/react/react_node_mysql.png) 

- 위의 사진은 react로 작성한 프론트에서 node로 작성한 백엔드로 로그인을 요청하여 데이터베이스인 mysql에서 해당 회원의 정보를 검사하여 토큰(jwt)을 반환하는 예제입니다.
 
## react 개발

> #### view 모듈 준비

- 모든 것을 새롭게 만들기에는 시간이 많이 소요되기 때문에 "React 2부 : 상태관리"에 사용된 프로젝트를 그대로 복사하여 사용하도록 하겠습니다. 또한 추가적으로 "MATERIAL-UI"라는 react 모듈을 이용하도록 하겠습니다. 추가적으로 아래의 모듈을 설치해주세요. 

```
npm install @material-ui/core @material-ui/icons
```

- MATERIAL-UI는 UI를 구현하는데 있어서 이미 라인업된 UI 컴포넌트를 통해 손쉽게 프론트르를 개발할 수 있도록 도와주는 모듈입니다.

```
@material-ui/core : "MATERIAL-UI"에서 제공하는 react 컴포넌트를 사용할 수 있도록 해줍니다.
@material-ui/icons : "MATERIAL-UI"에서 제공 icon을 사용할 수 있도록 해줍니다.
```

- 다수의 페이지를 라우팅 하고 서버에 http 요청을 하기 위해 아래의 모듈도 설치합니다.

```
npm install axios react-router-dom
```

```
axios : 서버 사이드에 http 요청을 하기 위한 모듈
react-router-dom : 리액트 라우터를 사용하기 위한 모듈
```

> #### view 템플릿 준비

- 아래의 링크를 통하면 "MATERIAL-UI"에서 제공하는 샘플 페이지를 볼 수가 있습니다. 

###### [MATERIAL-UI 템플릿 링크](https://material-ui.com/getting-started/templates)

- 해당 실습에서 이용할 페이지는 "Sign In"과 "Album"입니다. 각 샘플의 "SOURCE CODE"를 클릭합니다.

![MATERIAL-UI 템플릿](/images/react/material_ui_material_select.png)

- 해당 링크는 깃허브로 연결이 됩니다. 링크로 이동되면 README.md 파일을 제외하고 컴포넌트 js 파일이 있습니다. 해당 내용을 복사를 합니다.

> ##### 프로젝트 구조

- 실습할 React는 전역상태 관리로 mobx를 사용합니다.  프로젝트 구조는 아래와 같습니다.

```
chapter03_react_view
        /index.html
        /index.jsx
        /package.json
        /package-lock.json
        /webpack.config.js
        /src
           └─ components
                        └─ Album.jsx       : MATERIAL-UI 템플릿의 Album 을 수정
                        └─ AuthRoute.jsx   : 권한을 확인하는 컴포넌트
                        └─ Container.jsx   : 전체 컴포넌트를 갖고 있는 컨테이너 
                        └─ SignIn.jsx      : MATERIAL-UI 템플릿의 Sign In 을 수정
           └─ store
                        └─ LoginStore.js   : 로그인과 관련된 전역상태
           └─ app.css
           └─ Root.jsx
        /dist
           └─ app.js
```

> ##### Container.jsx 코드

```
import React from 'react';
import Album from "./Album";
import SignIn from "./SignIn";
import {useObserver} from "mobx-react/dist/mobx-react";
import { BrowserRouter as Router, Route, Switch} from 'react-router-dom';
import AuthRoute from './AuthRoute';

export default function Container() {

    return(
        useObserver(() => (
            <>
                <Router>
                    <Switch>
                        <AuthRoute path="/" exact={true}/>
                        <Route path="/login" exact={true} component={SignIn} />
                        <AuthRoute path="/album" exact={true} component={Album} />
                    </Switch>
                </Router>
            </>
        ))
    );

}
```

- "react-router-dom" 모듈의 BrowserRouter 컴포넌트를 이용하여 브라우저 경로에 따라 다른 컴포넌트가 렌더링이 되도록 설정을 했습니다.

- 라우터를 사용하기 위해 Router 컴포넌트 안에 Switch 컴포넌트를 넣습니다. 필수는 아니지만 Switch 컴포넌트는 하위의 Route 컴퍼넌트 중 1개만 렌더링이 되도록 해줍니다.

- AuthRoute 컴포넌트는 권한을 검사하는 컴포넌트 입니다. 내부에는 Route 컴퍼넌트와 Redirect 컴포넌트가 존재합니다.

> ##### AuthRoute.jsx 코드

```
import React from 'react';
import {Route, Redirect} from 'react-router-dom';
import {useObserver} from "mobx-react/dist/mobx-react"
import LoginStore from '../store/LoginStore';


export default function AuthRoute({component : Component , ...rest}) {
    return(
        useObserver(() => (
            <Route {...rest} exact={true} render={(props) => {
                return LoginStore.loggedIn ? <Component /> : <Redirect to={'/login'} />
            }}/>
        ))
    );
}
```

- AuthRoute 컴포넌트는 component로 렌더링할 컴포넌트를 받으며 나머지인 ...rest를 받습니다.

- prop으로 들어온 component를 Component 변수에 할당하며 전역상태인 LoginStore 에서 로그인 여부를 확인하여 true(로그인을 한 경우)일 경우 Component 컴포넌트를 렌더링합니다.

- 전역상태에서 로그인 여부에 false일 경우 Redirect 컴포넌트를 이용하여 "/login" 로 라우팅을 합니다.

> ##### SignIn.jsx 코드
###### 중요 부분만 코드에 포함하도록 하겠습니다. 

```
import React, {useRef} from 'react';
import Axios from 'axios'
import {useObserver} from "mobx-react/dist/mobx-react"
import loginStore from "../store/LoginStore";
import {Redirect} from 'react-router-dom';
... 생략


export default function SignIn() {

    const classes = useStyles();
    const form = useRef();

    const onClickSignIn = () => {
        Axios.post('http://localhost:3000/auth/login', new FormData(form.current) , {
            headers: {
                'Content-Type': 'application/json'
            }
        })
            .then(function ({data}) {
                if (data.result == 'success'){
                    loginStore.setLoggedIn(true);
                    loginStore.setToken(data.token);
                    console.log('로그인 여부 : ' +loginStore.loggedIn);
                    console.log('토큰 : ' + loginStore.token);
                }
            })
    };

    return(
        useObserver(()=> (
            <Container>
            ... 생략
                        <Button
                            type="button"
                            fullWidth
                            variant="contained"
                            color="primary"
                            className={classes.submit}
                            onClick={onClickSignIn}
                        >
                            Sign In
                        </Button>
            ... 생략
                {loginStore.loggedIn ? <Redirect to={'/album'}/> : null}
            </Container>
        ))
    );
}
```

- MATERIAL-UI로 복사한 코드를 SignIn.jsx에 복사하여 넣습니다. 그리고 mobx를 렌더링 할 수 있도록 코드를 수정 후 로그인 버튼에 onClick 이벤트를 통해 이벤트를 등록합니다.

- 로그인 버튼에 등록한 onClickSignIn 메서드는 Axios 모듈을 통해 서버 사이드에 요청을 합니다. 요청시 아이디와 비밀번호는 post로 넘깁니다.

- 서버 사이드에서 응답이 정상적으로 오면 전역상태에서 토큰과 로그인 여부에 대해 업데이트를 수행합니다.

- 렌더링의 마지막에 Redirect 컴포넌트를 통해 전역상태로부터 로그인 여부를 검사하여 로그인이 되었을 경우에는 "/album" 경로로 이동시킵니다. 

> ##### Album.jsx 코드
>###### 중요 부분만 코드에 포함하도록 하겠습니다. 

```
import React from 'react';
import Axios from 'axios'
import {useObserver} from "mobx-react/dist/mobx-react";
import loginStore from "../store/LoginStore";
... 생략

export default function Album() {
    const classes = useStyles();

    const verifyToken = () => {
        let token = loginStore.token;

        Axios.post('http://localhost:3000/auth/verify', null , {
            headers: {
                'Content-Type': 'application/json',
                'token' : token
            }
        })
            .then(function ({data}) {
                if (data.result == 'success'){
                    alert('유효한 토큰입니다.')
                }
            });
    }

    return(
        useObserver(()=> (
            <>
                <CssBaseline />
                ... 생략
                    <Grid container spacing={2} justify="center">
                        <Grid item>
                            <Button variant="contained" color="primary" onClick={verifyToken}>
                                Verify Token
                            </Button>
                        </Grid>
                    </Grid>
                ... 생략
            </>
        ))
    );
}
```

- MATERIAL-UI로 복사한 코드를 Album.jsx에 복사하여 넣습니다. 해당 화면은 로그인을 하면 접근할 수 있는 컴포넌트입니다.

- 중간에 있는 버튼 2개중에 1개를 삭제 후 현재 토큰을 서버에 검증하기 위한 버튼 1개만 남겨두고 삭제합니다. 그리고 검증을 하는 verifyToken 메서드를 이벤트로 연결해줍니다.

![토큰 검증 버튼](/images/react/verify_btn.png) 

- 연결된 "verifyToken" 메서드는 Axios 모듈로 서버 사이드에 검증 요청을 보냅니다. 해당 검증이 정상적일 경우 alert 메서드로 정상임을 나타냅니다.

## node 개발

> #### 서버 사이드 모듈 준비

- 서버 사이드를 node로 구현하기 위해 새로운 폴더를 생성 후 아래와 같은 모듈을 설치합니다.

```
npm install  express cors jsonwebtoken multer sync-mysql body-parser
```

- 각각의 모듈은 아래와 같은 기능들을 도와줍니다.

> #### 서버 사이드 모듈 준비

```
express: 서버 구축을 위한 모듈
cors: CORS 정책으로 인해 요청이 거부가 되는 것을 허용하기 위한 모듈
jsonwebtoken: JWT를 생성 및 검증을 위한 모듈
multer: multipart/form-data 형식으로 데이터를 받기 위한 모듈
sync-mysql: 동기적으로 mysql를 이용하기 위한 모듈
body-parser: 클라이언트측에서 요청을 받을때, url-encoded 쿼리 및 json 형태의 바디를 파싱하는데 도움을 주는 모듈
```

> ##### 프로젝트 구조
  
```
chapter03_node_server
        /database
           └─ UserDao.js       : 회원과 관련되어 mysql에 접근하는 class
        /service
           └─ JwtService.js    : JWT를 생성하고 검증하는 class
           └─ LoginService.js  : 회원정보 인증 및 토큰생성, 토큰 검증을 하는 class
        /config.js             : JWT에 들어가 시크릿
        /package.json
        /package-lock.json
        /server.js             : 웹 서버 설정
```

> ##### server.js 코드

```
const express = require('express');
const multer = require('multer');
const bodyParser = require('body-parser');
const cors = require('cors');

const loginService = require('./service/LoginService');

const port = process.env.PORT || 3000;
const app = express();
const upload = multer();

app.use(bodyParser.urlencoded({extended: true}));
app.use(bodyParser.json());
app.use(upload.array());
app.use(cors());

app.listen(port, () => {
    console.log(`Server is running at ${port}`);
});

app.post('/auth/login', (req, res) => {
    let loginInfo =  req.body;
    let token = loginService.getToken(
        loginInfo.id,
        loginInfo.password
    );

    res.status(200).json({
        result : token ? 'success' : 'fail',
        message : token ? 'Login success' : 'Login Fail',
        token : token
    });
});

app.post('/auth/verify', (req, res) => {
    let token =  req.headers.token;

    let result = loginService.verifyToken(token);

    if(result)
        res.status(200).json({
            result : result ? 'success' : 'fail',
            message : result ? 'Valid' : 'Not valid',
        });
});
```

- express 모듈에 CORS 정책과 관련된 이슈 및 요청에 대한 파싱을 정상적으로 수행하기 위해 각 모듈들을 설정해 줍니다.

- 로그인을 하기 위한 "/auth/login", "/auth/verify" 로 post 방식인 경로를 설정해줍니다.

> ##### config.js 코드

```
module.exports = {
    'secret': 'pntbizSecret'
}
```

- JWT에서 사용할 스크릿을 지정합니다.

 > ##### LoginService.js 코드

```
const jwtService = require('./JwtService');
const userDao = require('../database/UserDao');

class LoginService {

    verifyToken(token) {
        return jwtService.verify(token);
    }

    getToken(id, pw) {
        let user = userDao.getUserInfo(id, pw);
        let token = null;

        if(user)
            token = jwtService.create(user);

        return token;
    }

}

module.exports = new LoginService();
```

- LoginService는 계정정보를 조회하여 토큰을 반환(getToken 메서드)하거나 토큰을 받아 토큰을 검증(verifyToken 메서드)하는 역할을 합니다.

> ##### JwtService.js 코드

```
const jwt = require('jsonwebtoken');
const userData = require('../database/UserDao');
const config = require('../config');

class JwtService {

    create(user) {
        let token = jwt.sign({
                id: user.id,
            },
            config.secret,
            {
                expiresIn: '365d',
                issuer: 'pntbiz',
                subject: 'userInfo'
            });
        return token;
    }

    verify(token) {
        return jwt.verify(token, config.secret);
    }

}

module.exports = new JwtService();
```

- JwtService는 토큰을 생성하고 토큰을 검증하는 역할을 합니다. 내부적으로 jsonwebtoken 모듈을 이용하여 회원의 아이디를 클레임으로 추가하여 토큰을 생성합니다.

- JWT에 대한 자세한 내용은 해당 글의 처음에 "React 3-1부 : JWT와 React Router" 를 참고해주세요.

> ##### UserDao.js 코드

```
var mysql = require('sync-mysql');

class UserDao {

    constructor() {
        this.connection = new mysql({
            host     : 'localhost',
            user     : 'root',
            password : '123456',
            port     : 3306,
            database : 'pntbiz_react'
        });
    }

    getUserInfo(id,pw){
        let user = null;
        let result = this.connection.query(`SELECT * from USER WHERE id='${id}' AND pw='${pw}'`);

        if(result.length == 1)
            user = result[0];

        return user;
    }

}

module.exports = new UserDao();
```

- UserDao는 동기적 mysql인 "sync-mysql" 모듈을 이용하여 회원정보를 Select 하는 역할입니다.

## Mysql 설정

- mysql 설정으로는 아래와 같은 db 쿼리를 통해 생성 및 계정 1개를 설정합니다.

```
CREATE DATABASE pntbiz_react;

USE pntbiz_react;

CREATE TABLE USER (
  num INT(11) NOT NULL AUTO_INCREMENT,
  id VARCHAR(10) DEFAULT NULL,
  pw VARCHAR(10) DEFAULT NULL,
  PRIMARY KEY (`num`)
)

INSERT INTO USER
(id, pw)
VALUES( 'pntbiz', '1234');
```

### 마무리

![예제](/images/react/react03_result.gif)

- 이로서 서버 사이드와 데이터 베이스는 mysql를 연동을 하여 백엔드를 구성하고 리액트로 http 요청을 통해 로그인 요청을 후 JWT를 받아보는 실습을 진행하였습니다.

- 해당 실습 내용은 아래 깃허브에서 소스를 받으실 수 있습니다.

###### [React 3부 : JWT와 React Router / 서버 연동](https://github.com/SayRew/pntbiz_react)

##### 출처

  - https://beomy.tistory.com/33
  - https://velopert.com/2448
  - https://www.npmjs.com/package/jsonwebtoken
  
  