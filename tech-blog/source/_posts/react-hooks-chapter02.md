---
title: React-2부 [상태관리]
date: 2019-12-29 13:29:55
thumbnail : /images/react-hooks.png
tags: [react]
category : [IT Tech, 3. React]
---

> 작성자 : 플랫폼 개발실 R&D팀 유주빈

## 들어가기 앞서

- 본글은 React의 hooks문법으로 작성이 되었습니다. 또한 기본적인 리액트 환경셋팅에 관한 내용은 다루지 않습니다. 리액트의 기본적인 환경셋팅에 관한 내용은 아래의 리액트 1부를 참고해 주세요.

###### [React 1부 : 개발환경 구축하기](http://.com)

- 1부에서 리액트는 단방향 데이터 흐름을 갖고 있다고 하였습니다. 부모 컴포넌트가 자식 컴포넌트에게 데이터를 전달하는 것을 의미합니다. 그렇다면 자식 컴포넌트가 부모 컴포넌트에게 데이터를 전달하는 방법은 없을까요?

## 간략한 예제

- 본 글에서는 위의 해답을 예제를 갖고 설명을 하도록 하겠습니다. 해당 예제는 깃허브에서 프로젝트를 다운로드 받으실 수 있습니다.

###### [깃허브주소](http://.com)

- 프로젝트의 파일 구조에서 chapter01_react_setting 프로젝트는 1부 프로젝트이며 chapter02_react_component는 본글의 2부 프로젝트입니다. 해당 프로젝트의 핵심 파일구조는 아래와 같습니다.

```
chapter02_react_component(본 글의 root  경로)
      └─src
         └─ 01. Parameters&Callback 
         └─ 02. Context API 
         └─ 03. Redux 
         └─ 04. Mobx
         └─ app.css (01,02,03,04에서 공통적으로 사용하는 css)
         └─ RootContainer.jsx (01,02,03,04의 결과를 묶기위한 jsx 파일)
      ... 이하 생략 ... 
```

- 프로젝트를 보시면 아시겠지만 01~04번까지 UI상으로는 동일한 프로젝트입니다. 해당 소스를 실행하면 아래와 같은 예제를 보실 수 있습니다.

![예제](/images/react/chapter-02-gif.gif)

- 위의 그림은 부모 컴포넌트와 왼쪽, 오른쪽 자식 컴포넌트간의 공을 주고 받는 예제입니다. 각 컴포넌트들은 서로에게 공을 넘겨줄 수 있는 버튼을 갖고 있습니다. 해당 예제를 통해 컴포넌트들 간에 상태를 어떤 방식으로 전달할 수 있는지 보도록 하겠습니다.

## 컴포넌트들의 상태관리 및 전달

- 리액트에서 상태관리 및 전달을 할 경우 대표적으로 사용되는 방법은 아래와 같이 4가지가 존재합니다. 위의 프로젝트 번호와 매칭되어 생각하시면 되겠습니다.

```
01. 단방향 props 전달 및 부모 컴포넌트의 Call Back 함수
02. Context API를 활용한 전역상태 관리
03. Redux를 활용한 전역상태 관리
04. MobX를 활용한 전역상태 관리
```

### 01. 단방향 props 전달 및 부모 컴포넌트의 Call Back 함수
#### [폴더명 : "01. Parameters&Callback"]

- 먼저 파일구조를 보도록 하겠습니다.

```
chapter02_react_component(본 글의 root  경로)
      └─ src
          └─ 01. Parameters&Callback 
                        └─ Root.jsx
                        └─ components
                                 └─ Parent.jsx (부모)
                                 └─ Left_Child.jsx (왼쪽 자식)
                                 └─ Right_Child.jsx (오른쪽 자식)
      ... 이하 생략 ... 
```

- Root.jsx는 명목상 01번의 전체 결과를 묶기 위한 컴포넌트입니다. 여기서 Parent.jsx가 존재하며 해당 자식 컴포넌트로  Left_Child , Right_Child를 갖고 있습니다. 자세한 코드는 아래와 같습니다.

##### Root.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
import React from 'react';
import Parent from "./components/Parent";
import '../app.css';

export default function Root() {
    return(
        <Parent/>
        // Parent 컴포넌트를 렌더링 합니다.
    );
}
```

##### Parent.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
import React , {useState} from 'react';
import Left_Child from "./Left_Child";
import Right_Child from "./Right_Child";
// React 관련 모듈과 Left_Child , Right_Child 컴포넌트를 불러옵니다.

export default function Parent() {

    const [owner , setOwner] = useState('parent');
    // useState 함수는 react hooks에서 state를 설정하는 함수입니다.
    // useState의 파라메터는 초기값을 의미합니다.
    // const [owner , setOwner] 에서 첫번째 owner는 상태명 , setOwner는 상태변환 함수명입니다.

    const onClickParentToLeft = () => {
    // 공을 부모 컴포넌트가 갖고 있다면 공의 주인을 left 컴포넌트로 변경
        if(owner == 'parent')
            setOwner('left');
    };

    const onClickParentToRight = () => {
    // 공을 부모 컴포넌트가 갖고 있다면 공의 주인을 right 컴포넌트로 변경
        if(owner == 'parent')
            setOwner('right');
    };

    return(
        <table className={'parent-table'}>
            <tbody>
                <tr>
                    <td colSpan={2}>
                        Parent
                        <br/>
                        <div className={'ball'} style={{visibility: owner == 'parent' ? 'unset' :'hidden'}}>ball</div>
                        // owner 값을 통해 ball 모양인 div의 visibility 속성을 제어합니다.
                        <br/>
                        <button onClick={onClickParentToLeft} className={'btn-parent-to-left'}>Pass to Left_Child</button>
                        // 부모 컴포넌트에서 Left_Child 컴포넌트로 공을 전달하는 버튼입니다.
                        <button onClick={onClickParentToRight} className={'btn-parent-to-right'}>Pass to Right_Child</button>
                        // 부모 컴포넌트에서 Right_Child 자식 컴포넌트로 공을 전달하는 버튼입니다.
                    </td>
                </tr>
                <tr>
                    <td className={'left-table'}>
                        <Left_Child owner={owner} setOwner={setOwner}/>
                        // Left_Child 컴포넌트에 owner 상태와 상태변화 함수인 setOwner를 넘깁니다.
                        // setOwner 함수를 통해 owner의 값이 변경되면 다시 렌더링이 됩니다.
                    </td>
                    <td className={'right-table'}>
                        <Right_Child owner={owner} setOwner={setOwner}/>
                        // Left_Child 컴포넌트에 owner 상태와 상태변화 함수인 setOwner를 넘깁니다.
                        // setOwner 함수를 통해 owner의 값이 변경되면 다시 렌더링이 됩니다.
                    </td>
                </tr>
            </tbody>
        </table>
    );
}
```

##### Left_Child.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
import React from 'react';
// 리액트 모듈을 불러옵니다.

export default function Left_Child({owner , setOwner}) {
// 부모로부터 props를 전달받습니다. props는 부모에게 받는 값을 의미합니다.
// 현재 owner와 setOwner를 받았습니다. owner를 통해 현재 공의 주인을 알 수 있으며
// 만약 해당 컴포넌트가 공을 갖고 있는 상태에서 다른 컴포넌트에게 공을 줄 경우
// Call Back 함수인 setOwner를 호출하여 부모의 owner상태를 변경할 수 있습니다.

    const onClickChildToParent = () => {
    // 공을 왼쪽 자식 컴포넌트가 갖고 있다면 공의 주인을 부모 컴포넌트로 변경
        if(owner == 'left')
            setOwner('parent');
    };

    const onClickRightToLeft = () => {
    // 공을 왼쪽 자식 컴포넌트가 갖고 있다면 공의 주인을 오른쪽 자식 컴포넌트로 변경
        if(owner == 'left')
            setOwner('right');
    };

    return(
        <div className={'child'}>
            <button onClick={onClickChildToParent} className={'btn-to-parent'}>Pass to Parent</button>
            // 왼쪽 자식 컴포넌트에서 부모 컴포넌트로 공을 전달하는 버튼입니다.
            // onClick으로 onClickChildToParent를 호출하며 해당 메서드는 부모로 부터 받은 상태변환 함수인
            // setOwner 메서드를 내부적으로 호출하고 있습니다.
            <br/>
            Left Child
            <br/>
            <div className={'ball'} style={{visibility: owner == 'left' ? 'unset' :'hidden'}}>ball</div>
            // owner 값을 통해 ball 모양인 div의 visibility 속성을 제어합니다.
            <button onClick={onClickRightToLeft} className={'btn-left-to-right'}>Pass to Right</button>
            // 왼쪽 자식 컴포넌트에서 오른쪽 자식 컴포넌트로 공을 전달하는 버튼입니다.
            // onClick으로 onClickChildToParent를 호출하며 해당 메서드는 부모로 부터 받은 상태변환 함수인
            // setOwner 메서드를 내부적으로 호출하고 있습니다.
        </div>
    );
}
```

##### Right_Child.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
import React from 'react';

export default function Right_Child({owner , setOwner}) {
// Left_Child 컴포넌트 설명과 동일합니다.

    const onClickChildToParent = () => {
    // 공을 오른쪽 자식 컴포넌트가 갖고 있다면 공의 주인을 부모 컴포넌트로 변경
        if(owner == 'right')
            setOwner('parent');
    };

    const onClickRightToLeft = () => {
    // 공을 오른쪽 자식 컴포넌트가 갖고 있다면 공의 주인을 왼쪽 자식 컴포넌트로 변경
        if(owner == 'right')
            setOwner('left');
    };

    return(
        <div className={'child'}>
            <button onClick={onClickChildToParent} className={'btn-to-parent'}>Pass to Parent</button>
            // 오른쪽 자식 컴포넌트에서 부모 컴포넌트로 공을 전달하는 버튼입니다.
            // onClick으로 onClickChildToParent를 호출하며 해당 메서드는 부모로 부터 받은 상태변환 함수인
            // setOwner 메서드를 내부적으로 호출하고 있습니다.
            <br/>
            Left Child
            <br/>
            <div className={'ball'} style={{visibility: owner == 'right' ? 'unset' :'hidden'}}>ball</div>
            // owner 값을 통해 ball 모양인 div의 visibility 속성을 제어합니다.
            <button onClick={onClickRightToLeft} className={'btn-right-to-left'}>Pass to Left</button>
            // 오른쪽 자식 컴포넌트에서 왼쪽 자식 컴포넌트로 공을 전달하는 버튼입니다.
            // onClick으로 onClickChildToParent를 호출하며 해당 메서드는 부모로 부터 받은 상태변환 함수인
            // setOwner 메서드를 내부적으로 호출하고 있습니다.
        </div>
    );
}
```

##### 정리

![01. 단방향 props 전달 및 부모 컴포넌트의 Call Back 함수 전체 개념도](/images/react/parameters_callback_summary.png)

- Parent는 Left_Child , Right_Child 컴포넌트에게 본인의 상태인 owner와 owner 상태를 변경할 수 있는 setOwner를 자식들에게 props를 전달하며 특정 로직에 의해 setOwner를 호출하여 하나의 상태값을 3개의 컴포넌트가 공유하며 본인의 공인지 아닌지의 여부를 판단하여 UI상 사용자에게 공을 이동시키는 표현을 할수 있게 됩니다.

### 02. Context API를 활용한 전역상태 관리
#### [폴더명 : "02. Context API"]

- 위의 예제는 컴포넌트들 간의 상태를 전달 및 공유하는 방법에 대해 알아보았습니다. 그러나 위의 방식은  컴포넌트들간에 공유를 위한 Call Back 함수를 props로 전달해야하며 이는 컴포넌트의 부모와 자식 관계가 점차 깊어질 경우 상당히 복잡해지며 코드가 난잡해진다는 단점이 있습니다.

![Component로 상태공유](/images/react/context_API_01.png)
 
- 위의 사진을 보면 A ~ G까지 컴포넌트가 존재합니다. 여기서 C와 G가 특정 상태를 공유하는 방법으로 위와 같은 방법으로 한다면 A 컴포넌트에서 state와 관련된 정보를 내려줘야 합니다. 이런 경우 중간에 존재하는 컴포넌트들에게도 코드상 영향을 미치게 되며 관리가 어려워집니다.

![Component로 상태공유](/images/react/context_API_02.png)

- 만약 전역적으로 컴포넌트들에게 상태를 전달하며 해당 상태를 변경할 수 있다면 중간에 존재하는 컴포넌트들에게 영향이 없을 것입니다. 이러한 전역적인 상태관리 방법으로 Context API가 존재합니다.

- 먼저 파일구조를 보도록 하겠습니다.

```
chapter02_react_component(본 글의 root  경로)
      └─ src
          └─ 02. Context API 
                        └─ Root.jsx
                        └─ components
                                 └─ Parent.jsx (부모)
                                 └─ Left_Child.jsx (왼쪽 자식)
                                 └─ Right_Child.jsx (오른쪽 자식)
                        └─ context
                                 └─ OwnerContext.jsx (전역상태 컴포넌트)
      ... 이하 생략 ... 
```

- 01번 예제와는 다르게 "context"라는 폴더가 추가되었으며 아래에 OwnerContext.jsx 파일이 추가가 되었습니다. OwnerContext 컴포넌트는  Parent, Left_Child, Right_Child 컴포넌트가 전역적으로 사용하는 상태값을 갖고 있습니다. 코드를 통해 알아보겠습니다. 

##### Root.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
import React from 'react';
import Parent from "./components/Parent";
import OwnerContext from "./components/context/OwnerContext"
// OwnerContext 컴포넌트를 import합니다. 해당 컴포넌트에서 전역으로
// 사용하는 상태값을 갖고 있습니다.

import '../app.css';

export default function Root() {
    return(
        <OwnerContext>
            <Parent/>
        </OwnerContext>
        // 위의 01번 예제와는 다르게 OwnerContext 컴포넌트 태그로 Parent 컴포넌트가
        // 감싸져 있습니다. 이는 OwnerContext의 props 중에 children 이라는 프로퍼티로
        // Parent 컴포넌트가 전달됩니다.
    );
}
```

##### OwnerContext.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
import React ,{ createContext , useState } from 'react';
// createContext 를 import 합니다. 이는 전역적으로 사용할 Context를 생성할 수
// 있도록 해줍니다.

const Context = createContext({
    owner : undefined,
    setOwner : undefined
});
// Context라는 변수명으로 Context를 생성하였습니다.

export default function OwnerContext({children}) {
// Root.jsx 파일에서 Root 컴포넌트가 OwnerContext 컴포넌트 태그로 감싸져 있었습니다.
// 해당 Root 컴포넌트는 children 이라는 필드명으로 값이 전달됩니다.

    const [owner , setOwner] = useState('parent');
    // OwnerContext 컴포넌트는 owner라는 상태값을 갖게 됩니다.
    // 이 상태값을 Context의 값을 변경하여 Context를 사용중인 전역의 컴포넌들에게
    // 값의 변화를 줍니다.

    return(
        <Context.Provider value={{owner : owner , setOwner : setOwner}}>
            {children}
        </Context.Provider>
        // Context.Provider 는 해당 내부에 있는 컴포넌트들에게 value를 전달합니다.
        // 현재 상태값인 owner와 onwer를 변화시킬 수 있는 setOwner를 전역상태 값으로 전달합니다.
        // 이는 각 컴포넌트에서 React의 useContext 혹은 Context의 Consumer 태그를
        // 통해 값을 사용할 수 있습니다. 해당 강좌에서는 useContext를 사용하였습니다.
    );
};

export {Context};
// 해당 export는 다른 컴포넌트에서 Context 값에 접근하기 위해 export를 해줍니다.
```

##### Parent.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
import React, { useContext } from 'react';
import Left_Child from "./Left_Child";
import Right_Child from "./Right_Child";
import {Context} from "./context/OwnerContext"
// 나머지는 01번 예제와 같습니다. 다만 OwnerContext.jsx에서 마지막에
// export를 해준 Context를 불러와 줍니다. 해당 Context와 import 한
// useContext를 이용하여 값을 불러옵니다. 

export default function Parent() {

    const context = useContext(Context);
    // 생성한 Context를 useContext 함수의 파라메터로 전달하면 해당 Context를
    // 활용할 수 있습니다.

    const onClickParentToLeft = () => {
    // Provider의 value로 전달한 owner 값을 비교를 합니다.
    // owner를 변경할 경우 같이 넘긴 setOwner 메서드를 활용합니다.
        if(context.owner == 'parent')
            context.setOwner('left');
    };

    const onClickParentToRight = () => {
        if(context.owner == 'parent')
            context.setOwner('right');
    };

    return(
        <table className={'parent-table'}>
            <tbody>
                <tr>
                    <td colSpan={2}>
                        Parent
                        <br/>
                        <div className={'ball'} style={{visibility: context.owner == 'parent' ? 'unset' :'hidden'}}>ball</div>
                        // Context API인 context의 owner 값을 비교하여 visibility 속성을 정합니다.
                        <br/>
                        <button onClick={onClickParentToLeft} className={'btn-parent-to-left'}>Pass to Left_Child</button>
                        <button onClick={onClickParentToRight} className={'btn-parent-to-right'}>Pass to Right_Child</button>
                    </td>
                </tr>
                <tr>
                    <td className={'left-table'}>
                        <Left_Child/>
                        // 위의 01번 예제에서는 props를 전달하였지만 Context API를 활용하면 해당 props 전달 코드를
                        // 사용하지 않고 전역적으로 상태를 관리할 수 있습니다. 
                    </td>
                    <td className={'right-table'}>
                        <Right_Child/>
                    </td>
                </tr>
            </tbody>
        </table>
    );
}
```

##### Left_Child.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.
###### Right_Child.jsx는 Left_Child.jsx에서 판단로직만 "right"로 변경이 되기에 코드를 생략합니다.

```
import React, { useContext } from 'react';
import {Context} from "./context/OwnerContext"
// Parent 컴포넌트와 동일하게 useContext와 Context를 import 합니다.

export default function Left_Child({owner = null}) {

    const context = useContext(Context);
    // 전역 Context를 활용하기 위해 Parent와 동일하게 설정합니다.

    const onClickChildToParent = () => {
        if(context.owner == 'left')
            context.setOwner('parent');
    };

    const onClickRightToRight = () => {
        if(context.owner == 'left')
            context.setOwner('right');
    };

    return(
        <div className={'child'}>
            <button onClick={onClickChildToParent} className={'btn-to-parent'}>Pass to Parent</button>
            <br/>
            Left Child
            <br/>
            <div className={'ball'} style={{visibility: context.owner == 'left' ? 'unset' :'hidden'}}>ball</div>
            // Context API인 context의 owner 값을 비교하여 visibility 속성을 정합니다.
            <button onClick={onClickRightToRight} className={'btn-left-to-right'}>Pass to Right</button>
        </div>
    );
}
```

##### 정리

![02. Context API를 활용한 전역상태 관리 전체 개념도](/images/react/context_API_03.png)

- 위의 그림을 보시면 모든 컴포넌트들의 부모인 OwnerContext 컴포넌트가 있으며 해당 컴포넌트에서 전역적으로 사용할 상태와 관련된 정보를 설정합니다. 이를 내부적으로 Context API에게 전달하여 다른 컴포넌트들이 전역적으로 사용할 수 있도록 제공을 합니다. 이때 만약 전역적으로 사용할 상태에 대해 변경을 원한다면 Context API에서 제공되는 상태 변환함수를 사용하여 전역상태를 변경시키는 구조입니다. 현재는 컴포넌트가 3~4개 이므로 Context API가 01번 예제 보다 더욱 코드가 복잡해 보이지만 컴포넌트의 수가 늘어나거나 전역적으로 관리해야할 상태가 늘어날 수록 Context API와 같은 전역적으로 상태를 관리해주는 기능들이 빛을 보게 됩니다.

### 03. Redux를 활용한 전역상태 관리
#### [폴더명 : "03. Redux"]

- 지금부터 설명할 Redux는 추가적인 모듈을 설치해야합니다. 만약 github에서 소스를 다운로드 받으셨다면 이미 package.json에 들어있습니다. 만약 안되어 있다면 아래의 명령어를 실행해주세요.

```
npm install redux react-redux;
```

- redux는 react에 종속되어 있는 모듈이 아닙니다. redux는 자바스크립트 애플리케이션 state container 입니다. 때문에 다른 자바스크립트 어플리케이션에서도 사용을 할 수 있습니다. 

- react-redux는 redux와 react UI와의 바인딩을 해주는 모듈입니다.

- 파일구조로 넘어가기 전에 redux의 기본적인 원리에 대해 알아도록 하겠습니다.

![redux 개념도](/images/react/redux_01.png)

- 위의 그림의 화살표는 react 상태를 변화시켜 View를 변경하는 redux의 흐름도입니다. 사용자가 View에서 특정 이벤트를 통하면 Action을 생성하여 Reducer로 전달이 됩니다. 해당 Reducer는 전달받은 Action의 type을 판단하며 해당하는 Store의 값이 변경이됩니다. 이렇게 변화한 Store의 값은 다시 View에 값을 변경시키게 됩니다. 여기서의 redux의 특징은 단일 스토어라는 점입니다.

![redux 개념도](/images/react/redux_02.png)

- 컴포넌트 입장에서 그림은 위와 같습니다. C 컴포넌트에서 특정 action을 dispatch하며 해당 action을 통해 reducer는 store에 있는 어떤 상태를 어떻게 변경해야 할지 알게되며 상태를 변경하게 됩니다. 이에 store는 값이 변경되면 해당 상태를 subscribe(구독)하고 있는 컴포넌트안 G에게 값의 변화를 주게 됩니다. 

- 이제 파일구조를 보도록 하겠습니다.

```
chapter02_react_component(본 글의 root  경로)
      └─ src
          └─ 03. Redux
                └─ Root.jsx
                └─ components
                         └─ Parent.jsx (부모)
                         └─ Left_Child.jsx (왼쪽 자식)
                         └─ Right_Child.jsx (오른쪽 자식)
                └─ reduxModule
                         └─ ballReducer.js (ball 관련된 리듀서)
                         └─ index.js (여러 리듀서를 통합하는 파일)
      ... 이하 생략 ... 
```

- 위의 01번 02번 예제와 다르게 파일구조적으로 "reduxModule"라는 폴더가 존재합니다. 해당 폴더 아래에는 액션 , 액션생성 함수 , 리듀서가 존재합니다. 바로 코드를 보도록 하겠습니다.

##### Root.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
import React from 'react';
import Parent from "./components/Parent";
import { createStore } from "redux";
import reduxModule from './reduxModule'
// 파일구조에서 "reduxModule"라는 폴더가 있었습니다.
// 해당 폴더로 import를 하면 기본적으로 index 파일을 import 합니다.
import {Provider} from 'react-redux';
// react-redux의 Provider를 통해 redux의 값이 전달됩니다.
import '../app.css';

const store = createStore(reduxModule);
// "reduxModule" 폴더 아래에는 액션 ,액션생성 함수 , 리듀서가 정의가 되어 있습니다.
// 해당 정보로 store를 생성합니다.

export default function Root() {
    return(
        <Provider store={store}>
        // Provider 로 전달할 값들이 있는 store를 지정합니다.
            <Parent/>
        </Provider>
    );
}
```

##### ballReducer.js

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
const CHANGE_OWNER = 'BALL/CHAGNE/OWNER';
// 액션 type을 지정합니다. 해당 타입은 리듀서에서 체크를 합니다. 

export const getActionChangeOwner = (owner) => ({
    type: CHANGE_OWNER,
    owner : owner
});
// 액션 생성함수 입니다. type이라는 필드명으로 위에서 정의한 액션 type이 들어갑니다.
// 파라메터로 owner를 받아 owner에 설정되고 있습니다. 해당 액션 정보로 리듀서에서
// 값을 변경합니다. 해당 생성함수는 컴포넌트들 파일에서 사용하도록 export 해줍니다.

const initialState = {
    owner: 'parent'
};
// redux의 초기값 설정입니다.

export default function ballReducer(state = initialState, action) {
// 마지막으로 리듀서이며 해당 리듀서는 default로 export 해줍니다.
// 파라메터로 state와 action이 있습니다.
// state은 현재 store에 있는 값이 들어오며 action은 위에서 정의한 액션 생성함수에 의해
// 만들어진 액션이 넘어옵니다.

    switch (action.type) {
        case CHANGE_OWNER:
            return {
                ...state,
                owner: action.owner,
            };
        default:
            return state;
    }
    // switch문으로 action의 type을 체크합니다. 해당 체크에 따라 return 되는 값이 다릅니다.
    // 여기서 return 되는 로직에 의해 store의 값이 변경됩니다.
}
```

##### index.js

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
import { combineReducers } from 'redux';
// redux 모듈에 combineReducers를 import 합니다.
// 프로젝트 규모가 커지며 많은 리듀서를 제작하게 됩니다.
// 이때 combineReducers를 통해 여러 리듀서를 하나로 합칩니다.
import ballReducer from './BallReducer';
// 제작한 리듀서를 import 합니다.

export default combineReducers({
    ballReducer,
});
```

##### Parent.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
import React from 'react';
import Left_Child from "./Left_Child";
import Right_Child from "./Right_Child";
import {connect} from "react-redux";
// react와 redux를 연결해주는 connect 모듈을 import 합니다.
import  * as ballReducer from "../reduxModule/ballReducer";
// "reduxModule" 폴더 아래에 ballReducer.jsx 파일의 export된 모든 요소를 import 합니다.
// ballReducer.xxxx 처럼 해당 요소에 접근할 수 있습니다.

function Parent({owner , changeOwner}) {
// 파라메터 값으로 owner와 changeOwner가 넘어오고 있습니다. 해당 부분의 넘어오는 값은 코드 밑 부분에 있습니다.
// owner는 store로 부터 subscribe(구독)하고 있는 상태입니다. changeOwner는 해당 상태를 변경할 dispatch가 넘어옵니다.

    const onClickParentToLeft = () => {
    // 공을 parent가 갖고 있다면 changeOwner 함수 . 즉 , dispatch 함수를 통해 left로
    // store에 있는 owner라는 상태를 left로 변경하라는 로직입니다.
        if(owner == 'parent')
            changeOwner('left');
    };

    const onClickParentToRight = () => {
    // 공을 parent가 갖고 있다면 changeOwner 함수 . 즉 , dispatch 함수를 통해 left로
    // store에 있는 owner라는 상태를 right 변경하라는 로직입니다.
        if(owner == 'parent')
            changeOwner('right');
    };

    return(
        <table className={'parent-table'}>
            <tbody>
                <tr>
                    <td colSpan={2}>
                        Parent
                        <br/>
                        <div className={'ball'} att={owner} style={{visibility: owner == 'parent' ? 'unset' :'hidden'}}>ball</div>
                        <br/>
                        <button onClick={onClickParentToLeft} className={'btn-parent-to-left'}>Pass to Left_Child</button>
                        <button onClick={onClickParentToRight} className={'btn-parent-to-right'}>Pass to Right_Child</button>
                    </td>
                </tr>
                <tr>
                    <td className={'left-table'}>
                        <Left_Child/>
                    // store에서 상태를 관리하므로 props로 내려줄 필요가 없습니다.
                    </td>
                    <td className={'right-table'}>
                        <Right_Child/>
                    </td>
                </tr>
            </tbody>
        </table>
    );
}

const mapDispatchToProps = dispatch => ({
    changeOwner : (owner) => {
        dispatch(ballReducer.getActionChangeOwner(owner));
    }
});
// dispatch를 개발자가 원하는 로직별로 Mapping하는 설정부분입니다.
// 파라메터로 dispatch가 넘어옵니다. 해당 부분에 수행하고자 하는 action을 넘기면
// reducer에서 해당 action을 판단하여 store의 상태를 업데이트 합니다.
// 해당 메서드는 아래에 connect 함수의 파라메터로 들어갑니다.
// 여기서 정의된 changeOwner가 위의 컴포넌트 파라메터로 다시 들어가게 됩니다.

const mapStateToProps = (state) => {
    return {
        owner: state.ballReducer.owner
    };
};
// subscribe(구독) 을 통해 받을 값을 정의합니다.
// state 파라메터를 통해 넘어온 값을 onwer에 값을 다시 셋팅합니다.
// 셋팅한 onwer는 위에서 props로 받고 있는 owner로 값이 들어오게 됩니다.
// 위의 state.ballReducer.owner는 state.[combinreducer를 한 reducer].[설정한 상태값] 입니다.

export default connect(mapStateToProps,mapDispatchToProps)(Parent);
// 구독 및 디스패치 함수의 셋팅과 parent 컴포넌트의 설정을 통해 store와 연결됩니다.
```


##### Left_Child.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.
###### Right_Child.jsx는 Left_Child.jsx에서 판단로직만 "right"로 변경이 되기에 코드를 생략합니다.

```
import React from 'react';
import {connect} from "react-redux";
import  * as ballReducer from "../reduxModule/ballReducer";
// Parent 컴포넌트와 동일하게 ballReducer와 connect 모듈을 import 합니다.

function Left_Child({owner , changeOwner}) {
// Parent 컴포넌트와 동일하게 owner에는 구독중인 상태값이 넘어오며 
// changeOwner에는 dispatch를 할 수 있는 메서드가 넘어옵니다.

    const onClickChildToParent = () => {
        if(owner == 'left')
            changeOwner('parent');
    };

    const onClickLeftToRight = () => {
        if(owner == 'left')
            changeOwner('right');
    };

    return(
        <div className={'child'}>
            <button onClick={onClickChildToParent} className={'btn-to-parent'}>Pass to Parent</button>
            <br/>
            Left Child
            <br/>
            <div className={'ball'} style={{visibility: owner == 'left' ? 'unset' :'hidden'}}>ball</div>
            <button onClick={onClickLeftToRight} className={'btn-left-to-right'}>Pass to Right</button>
        </div>
    );
};

const mapDispatchToProps = dispatch => ({
    changeOwner : (owner) => {
        dispatch(ballReducer.getActionChangeOwner(owner));
    }
});
// Parent와 동일하게 dispatch의 Mapping 정보를 정의합니다.

const mapStateToProps = (state) => {
    return {
        owner: state.ballReducer.owner
    };
};
// Parent와 동일하게 구독 정보를 설정합니다.

export default connect(mapStateToProps,mapDispatchToProps)(Left_Child);
// Parent와 동일하게 store와 연결합니다.
```

##### 정리

![03. Redux를 활용한 전역상태 관리 전체 개념도](/images/react/redux_03.png)

- 위의 그림은 03번 예제에 적용한 redux 및 컴포넌트의 전체 개념도 입니다. 단일 Store에 각 컴포넌트가 액션을 디스패치를 함과 동시에 owner에 대해 구독을 하고 있는 상황입니다. 단일 Store에서 owner에 대한 상태를 관리하므로 각 컴포넌트는 각자의 상태를 갖고 있을 이유가 없습니다. 이러한 redux는 Contex API와 동일하게 부모 자식관계가 깊어지더라도 Store에 디스패치 및 구독을 통해 서로 상태를 공유할 수 있습니다.

### 04. MobX를 활용한 전역상태 관리
#### [폴더명 : "04. Mobx"]

- Mobx를 사용하기 위해서는 Mobx와 관련된 모듈들을 설치해줘야 합니다. 깃허브로 프로젝트를 다운로드 받으셨다면 package.json에 이미 들어가 있습니다. 만약 있지 않다면 아래의 명령어를 실행해주세요.

```
npm install mobx mobx-react
```

- 기본적으로 mobx는 install를 해줘야하며 react와 같이 사용하기 위해서는 mobx-react를 추가로 설치해줍니다. mobx-react는 mobx로 설정된 값과 react를 연결하여 값이 변경되면 자동으로 rendering 되도록 해줍니다.

![mobx 개념](/images/react/mobx_01.jpg)

 - Mobx도 마찬가지로 Action 발생하면 관리하는 state가 변경되며 이는 state와 관련된 정보들도 같이 업데이트가 됩니다.
 
 - 파일구조는 아래와 같습니다.
 
```
chapter02_react_component(본 글의 root  경로)
      └─ src
          └─ 03. Redux
                └─ Root.jsx
                └─ components
                         └─ Parent.jsx (부모)
                         └─ Left_Child.jsx (왼쪽 자식)
                         └─ Right_Child.jsx (오른쪽 자식)
                └─ store
                     └─ store.js (Mobx 스토어를 정의)
      ... 이하 생략 ... 
```

- 01번 02번 03번 예제와 다르게 "store" 폴더 아래에 store.js 파일이 존재합니다. 해당 파일은 전역으로 사용할 상태를 Mobx로 정의를 해놓은 파일입니다. Mobx는 redux와 다르게 다수의 Store를 정의할 수 있으며 설정이 매우 간단하고 자유롭습니다. 그렇기 때문에 다수의 사람들이 공동으로 프로젝트를 사용할 경우 mobx-state-tree와 같은 틀이 조금더 구체화되어 있는 모듈을 사용하는 것이 좋습니다. 이 강좌에서는 mobx-state-tree를 따로 다루지 않습니다.

###### 본 글에서 Mobx에 대해 데코레이터를 사용하지 않습니다.

##### Root.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
// 해당 파일은 01번 예제와 동일합니다. 즉 , 설정할 부분이 없습니다.
// 물론 mobx-react 모듈에서 Provider를 통해 Contex API 예제 처럼
// 값을 전달할 수 있습니다. 하지만 본 글에서는 간단하게 store에 대해
// import를 통해 사용합니다.
import React from 'react';
import Parent from "./components/Parent";
import '../app.css';

export default function Root() {
    return(
        <Parent/>
    );
}
```

##### store.js

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
import { observable } from 'mobx'
// mobx 모듈에서 observable 모듈을 import 합니다.
// observable 모듈은 관리할 상태에 대해 정의할 수 있게
// 도와줍니다.

export default observable ({
    owner : 'parent',
    setOwner (owner) {
        this.owner = owner;
        // redux에서는 디스패치로 액션을 넘겨 상태를 변화시켰지만
        // mobx에서는 "=" 키워드를 통해 값을 변경하면 자동으로
        // 해당 상태에 대한 Action이 발생합니다.
    }
});
// observable 모듈을 통해 owner와 setOwner에 대해 정의를 했습니다.
// 하지만 실제로 관리가 되는 상태는 owner이며 setOwner는 단순히
// owner를 변경하기 위한 메서드입니다.
```

##### Parent.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.

```
import React from 'react';
import Left_Child from "./Left_Child";
import Right_Child from "./Right_Child";
import Store from "../store/store";
// owner라는 상태를 갖고 있는 store 모듈을 import 합니다.
import {useObserver} from "mobx-react"
// mobx와 react를 이어주는 모듈입니다.
// store에서 observable로 설정했던 상태들을 useObserver와 연결해주면
// 해당 값이 변경시 react 에도 반영이 됩니다.

export default function Parent() {

    const onClickParentToLeft = () => {
    // 현재 store에 owner 가 parent이면 left로 변경하라는 로직입니다.
        if(Store.owner == 'parent')
            Store.owner = 'left';
    // "=" 키워드를 통해 값을 직접 변경하여도 알아서 Action이 발생하여 값을 변경합니다.
    // 이는 아까 store에서 정의한 setOwner('left')를 사용하여도 됩니다.
    };

    const onClickParentToRight = () => {
        if(Store.owner == 'parent')
            Store.owner = 'right';
    };

    return(
        // return에서 useObserver 모듈을 사용하여 mobx의 store에 있는 상태값이 변경되면 rendering을 도와줍니다.
        useObserver(() => (
            <table className={'parent-table'}>
                <tbody>
                <tr>
                    <td colSpan={2}>
                        Parent
                        <br/>
                        <div className={'ball'} style={{visibility: Store.owner == 'parent' ? 'unset' :'hidden'}}>ball</div>
                        // store에 있는 owner 값을 통해 visibility 속성을 제어합니다.
                        <br/>
                        <button onClick={onClickParentToLeft} className={'btn-parent-to-left'}>Pass to Left_Child</button>
                        <button onClick={onClickParentToRight} className={'btn-parent-to-right'}>Pass to Right_Child</button>
                    </td>
                </tr>
                <tr>
                    <td className={'left-table'}>
                        <Left_Child/>
                    </td>
                    <td className={'right-table'}>
                        <Right_Child/>
                    </td>
                </tr>
                </tbody>
            </table>
        ))
    );
}
```

##### Left_Child.jsx

###### 코드상에 "//" 키워드를 사용하여 설명합니다.
###### Right_Child.jsx는 Left_Child.jsx에서 판단로직만 "right"로 변경이 되기에 코드를 생략합니다.

```
import React from 'react';
import Store from "../store/store";
// owner라는 상태를 갖고 있는 store 모듈을 import 합니다.
import {useObserver} from "mobx-react"
// Parent 컴포넌트와 마찬가지로 store와 이어주는 모듈을 import 합니다.

export default function Left_Child() {

    const onClickChildToParent = () => {
        if(Store.owner == 'left')
            Store.setOwner('parent');
    // Parent 모듈과 다르게 store에 있는 setOwner를 호출해서 사용합니다.
    // setOwner 메서드에는 "this.owner = XXX" 라는 로직이 있으므로 자동으로
    // Action을 합니다.
    };

    const onClickLeftToRight = () => {
        if(Store.owner == 'left')
            Store.setOwner('right');
    };

    return(
        // return에서 useObserver 모듈을 사용하여 mobx의 store에 있는 상태값이 변경되면 rendering을 도와줍니다.
        useObserver(()=> (
            <div className={'child'}>
                <button onClick={onClickChildToParent} className={'btn-to-parent'}>Pass to Parent</button>
                <br/>
                Left Child
                <br/>
                <div className={'ball'} style={{visibility: Store.owner == 'left' ? 'unset' :'hidden'}}>ball</div>
                <button onClick={onClickLeftToRight} className={'btn-left-to-right'}>Pass to Right</button>
            </div>
        ))
    );
}
```

##### 정리

![mobx 개념](/images/react/mobx_02.jpg)

- 위의 그림은 예제 04번 mobx를 적용한 전체 개념도입니다. Store는 mobx로 정의한 상태값을 갖고 있습니다. 각 컴포넌트는 mobx-react 모듈의 Observer를 통해 해당 값에 감시를 하며 컴포넌트 내에서 직접 변경되거나 혹은 store의 setOwner를 호출하여 Action을 발생시킵니다. 이에 변경된 store 상태가 react 컴포넌트들에게 적용이 됩니다. 

> 작성자 : 플랫폼 개발실

##### 출처
  - 책 : 리액트를 다루는 기술(개정판)
  - https://velog.io/@velopert/react-redux-hooks
  - https://medium.com/@jsh901220/mobx-%EC%B2%98%EC%9D%8C-%EC%8B%9C%EC%9E%91%ED%95%B4%EB%B3%B4%EA%B8%B0-a768f4aaa73e