---
title: React-3-1부 [JWT와 React Router]
date: 2020-04-04 19:29:55
thumbnail : /images/react-hooks.png
tags: [react]
category : [IT Tech, 3. React]
---

> 작성자 : 플랫폼 개발실 R&D팀 유주빈

## 들어가기 앞서

- 본글은 React의 hooks문법으로 작성이 되었습니다. 또한 기본적인 리액트 환경셋팅 및 상태관리에 대한 내용은 다루지 않습니다. 해당 내용은 아래의 링크를 참고해주세요.

###### [React 1부 : 개발환경 구축하기](https://tech-people.github.io/2019/12/12/react-hooks-chapter01/)
###### [React 2부 : 상태관리](https://tech-people.github.io/2019/12/12/react-hooks-chapter02/)

 - 본 글에서는 react를 활용하여 로그인 인증을 위해 추가적으로 알아야 하는 JWT 및 React Router에 대해 알아보도록 하겠습니다.

## 로그인 및 인증

- 웹 / 앱 개발을 하면 로그인과 관련되서 필연적으로 만나게 되는 개념이 쿠키와 세션입니다.
 
```
쿠키 : 브라우저, 사용자의 컴퓨터(FRONT-END) 에 저장되는 데이터를 의미. 클라이언트 사이드에 있기 때문에 속도가 빠름
세션 : '시간' 을 의미. 이 시간은 사용자와 서버(BACK-END)의 통신이 지속되는 시간을 의미함. 서버에 접속한 사용자별로 고유한 ID를 가진 세션이 생성됨. 서버측에 저장되기 때문에 보안이 쿠키에 비해 뛰어남.
```

- 쿠키는 보안상의 문제로 적용하기가 쉽지 않고 세션의 경우 쿠키에 비해 보안이 안전하지만 서버쪽 부하가 증가하게 됩니다. 이 때문에 본 리액트 3부에서는 JWT를 활용하여 간단하게 로그인을 하는 실습을 진행할 것입니다.

## JWT

> #### 개요

- JWT는 "JSON Web Token"의 약자이며 웹표준(RFC7519) 으로서 두 개체에서 JSON 객체를 사용하여 가볍고 자가수용적인(Self-Contained) 방식으로 정보를 전달할 수 있습니다.

- 자가 수용적이기 때문에 Token에 모든 정보를 지니고 있습니다. 그렇기 때문에 특별히 인증과 과련되서 토큰을 저장하거나 하는 부하를 줄일 수 있습니다.

> #### 구조

- JWT (JSON Web Token)는 헤더(Header) ,내용(Payload) , 서명(Signature) 인 3가지 부분으로 나눠집니다.

![JSON Web Token 구조](/images/react/jwt_structure.png)

> #### 헤더(Header)

- 헤더는 typ와 alg라는 두가지 정보를 갖고 있습니다. 데이터의 예시는 아래와 같습니다. 

```
{
    typ : "JWT"
    alg : "HS256"
} 
```

- typ : 토큰의 형식을 정보로 갖고 있습니다.
- alg : 해싱 알고리즘 정보를 갖고 있습니다. 대게 HMAC SHA256 혹은 RSA가 사용됩니다. 해당 알고리즘은 토큰을 검증할 때 사용되는 서명(Signature) 부분에 적용됩니다.

> #### 내용(Payload)

- 내용 부분은 토큰에 담을 정보가 들어가 있습니다. 정보는 name과 value의 한쌍으로 구분되며 이런 한 조각을 "클레임(claim)"이라고 일컫습니다.

- 클레임은 아래와 같이 3가지로 분류가 됩니다.

1. 등록된 (registered) 클레임
2. 공개 (public) 클레임
3. 비공개 (private) 클레임

> ##### 등록된 (registered) 클레임

- 등록된 클레임은 토큰에 대한 정보를 지정하기 위해 이미 정해진 클레임입니다. 해당 클레임은 필수가 아닌 선택이며 항목들은 아래와 같습니다.

1. iss: 토큰 발급자 (issuer)
2. sub: 토큰 제목 (subject)
3. aud: 토큰 대상자 (audience)
4. exp: 토큰의 만료시간 (expiraton), NumericDate 형식이며 현재 시간보다 이후로 설정되어 있어야 합니다.
5. nbf: Not Before 를 의미, 토큰의 활성화 시간입니다.NumericDate 형식이며 지정한 날짜가 지나기 전까지는 토큰이 처리되지 않습니다.
6. iat: 토큰이 발급된 시간 (issued at)
7. jti: JWT의 고유 식별자

> ##### 공개 (public) 클레임

- 공개 클레임은 서로 충돌이 일어나지 않도록 이름을 고유하게 갖고 있어야 합니다. 그래서 URL 형태로 작성을 합니다.

```
{ "http://pntbiz.co.kr": true }
```

> ##### 비공개 (private) 클레임

- 비공개 클레임은 대게 클라이언트와 서버에 약속된 클레임입니다. 이는 실제 사용을 하는 개발자가 지정하게 됩니다.

```
{ "comName": "People and Technology" }
```

- 위의 3개의 클레임을 이용하여 내용(Payload)를 작성하였을 때 아래와 같은 예시가 나옵니다.

```
{
    "iss": "pntbiz.co.kr",
    "exp": "1485270000000",
    "http://pntbiz.co.kr": true,
    "comName": "People and Technology",
}
```

> #### 서명(Signature)

- JSON Web Token 의 마지막 부분으로 서명(signature)은 헤더(Header)의 인코딩값과, 내용(Payload)의 인코딩값을 합친후 주어진 비밀키로 해쉬를 하여 생성합니다.

> #### JWT 마무리

- 이러한 JWT를 통해 별도의 토큰과 관련된 저장소 및 부하를 만들지 않고 토큰으로 인증을 할 수 있습니다. 그러나 이것이 장점만을 갖고 있는 것은 아닙니다. 장단점의 요약은 아래와 같습니다.
 
- JWT 장점
 
1. URL  파라미터와 헤더로 사용
2. 수평 스케일이 용이
3. REST 서비스로 제공 가능
4. 내장된 만료
5. 독립적인 JWT
 
- JWT 단점

1. 내용(Payload)은 암호화되지 않기에 담는 데이터 내용이 제한적
2. 클레임이 많을 수록 토큰의 길이가 길어짐
3. 한 번 발급한 Token을 만료되기 전에 폐기가 어려움
 
## React Router
###### 기본적인 react 환경과 관련된 모듈은 설치되어 있다는 가정하에 내용이 진행됩니다.

- React Router는 SPA 어플리케이션의 라우팅 문제를 해결하기 위해 react 진영에서 거의 표준처럼 사용되고 있는 네비게이션 라이브러리입니다. 이는 브라우저의 내장 API인 location과 history와도 연동이 됩니다.

> #### React Router 모듈 설치

- React Router를 사용하기 위해 아래의 모듈을 설치합니다.

```
npm i react-router-dom
```

> #### React Router 주요 컴포넌트

- React Router를 활용하여 프로그램을 작성하기 전에 주요한 컴포넌트들에 대해 한번 살펴보도록 하겠습니다.

> ##### Link 컴포넌트

- Link 컴포넌트는 <a> 태그와 유사하게 브라우저를 href 속성을 통해 URL 변경하여 이동시키 듯이 <Link> 컴포넌트는 to라는 prop을 통해 이동할 URL을 지정합니다.  

- 코드상에서 사용 예시는 아래와 같습니다.

```
<Link to="/pntbiz">피플앤드테크놀러지</Link>
```

> ##### Route 컴포넌트

- Route 컴포넌트는 브라우저의 URL과 일치하는 컴퓨넌트를 브라우저에 렌더링 할 수 있도록 지정합니다. path라는 prop을 통해서 경로를 지정하며 component 라는 prop을 통해 렌더링 될 컴포넌트를 지정합니다.

- 코드상에서 사용 예시는 아래와 같습니다.

```
<Route path="/pntbiz" component={Pnt} />
```

> ##### Redirect 컴포넌트

- Redirect 컴포넌트는 브라우저의 경로를 변경시켜 주는 컴포넌트입니다. to라는 prop을 통해 변경할 경로를 지정할 수 있습니다.

- 코드상에서 사용 예시는 아래와 같습니다.

```
<Redirect to="/login"/>
```


### 마무리

- 이상으로 다음편에서 진행할 react로 로그인 구현하기와 관련되서 미리 알아야 할 내용에 대해 알아 보았습니다.

- 지금까지 실습한 내용을 토대로 간단하게 로그인을 구현해보는 시간을 갖도록 하겠습니다. 

##### 출처

  - https://www.npmjs.com/package/jsonwebtoken
  - https://velopert.com/2448