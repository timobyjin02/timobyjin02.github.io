---
layout: default
title: 기본문법
parent: React
nav_order: 1
---

# jsx

⇒ js + xml/html

## html 안에 js 코드 넣기

```jsx
const name = '소품';
const element - <h1>h1, {name} </h1>; // html 안에 js 코드 넣고 싶으면 {} 사용

ReactDOM.render(
	element,
	document.getElementById('root');
)
```

## 태그 속성에 값을 넣는 방법

```jsx
// 큰따옴표 사이에 문자열을 넣거나
const element = <div tabIndex="0"></div>

// 중괄호 사이에 자바스크립트 코드를 넣는다
const element = <img src={user.avatarUrl}></img>;
```

## 자식을 정의하는 방법

```jsx
 const element = (
	<div>
		<h1>안녕하세요</h1>
		<h2>열심히 리액트를 공부해 봅시다!</h2>
	</div>
)
```

## jsx  코드를 자바스크립트로 변환하는 함수

```jsx
// createElement 함수의 파라미터를 나타낸 함수
React.createElement(
	type,    // 첫 번째는 element의 유형, type -> html tag or reacr component
	[props], // 속성들
	[...children]  // 현재 element가 포함하고 있는 자식 element
)
```

- jsx를 사용하면 장점들이 많은 → 생산성, 가독성 높음, 간결해짐

```jsx
// jsx
const element = (
    <h1 className="greeting">
        Hello, world!
    </h1>
)

// no jsx
const element = React.createElement(
    'h1',
    { className: 'greeting' },
    'Hello, world!'
)

// no jsx result
const element = {   // -> React.createElement() 의 결과로 아래와 같은 객체가 생성됨
    type: 'h1',
    props: {
        className: 'greeting',
        children: 'Hello, world!'
    }
}
```

## jsx 사용 코드

## jsx 사용 x 코드

```jsx
class Hello extends React.Component {  // Hello 라는 이름을 가진 react component가 나옴
	render()
	 {
			return <div>Hello {this.props.toWhat}</div>; // js + html => jsx
	}
}

ReactDOM.render(  // ReactDOM에 render 함수를 이용해서 화면에 렌더링
		<Hello toWhat="World" />
		document.getElementById('root')
);
```

```jsx
class Hello extends React.Component {  // Hello 라는 이름을 가진 react component가 나옴
	render()
	 {
			return React.createElement('div', nulll, 'Hello ${this.props.toWhat}'); 
	}
}

ReactDOM.render(  // ReactDOM에 render 함수를 이용해서 화면에 렌더링
		React.createElement(Hello, { toWhat: 'World' }, null), // createElement 이용 -> 자바스크립트 객체가 나옴
		document.getElementById('root')
);
```

# Rendering Elements

## Rendering

```jsx
// Book.jsx

import React from "react";

function Book(props) {
    return (
        <div>
            <h1>{`이 책의 이름은 ${props.name}입니다.`}</h1>
            <h2>{`이 책은 총 ${props.numOfPage}페이지로 이루어져 있습니다.`}</h2>
        </div>
    )
}

export default Book;  // Book component 내보내기
```

```jsx
// book 상위

import React from "react";
import Book from "./Book";  // Book component 받기

function Library(props) {
    return (
        <div>
            <Book name="처음 만난 파이썬" numOfPage={300}/>
            <Book name="처음 만난 AWS" numOfPage={300}/>
            <Book name="처음 만난 리액ㅌ" numOfPage={300}/>
        </div>
    )
}

export default Library;
```

```jsx
// 우리가 만든 컴포넌트 실제로 화면에 랜더링 하기 위해서 index.js 파일 수정

import React from "react";
import ReactDOM from "react-dom/client";
import "./index.css";
import App from "./App";
import reportWebVitals from "./reportWebVitals";

import Library from "./chapter03/Library"; // Library component 가져오기

ReactDOM.render(  // ReactDOM을 이용하여 rootDOM 노드에 랜더링 하는 코드
  <React.StrictMode>
    <Library />
  </React.StrictMode>,
  document.getElementById("root")
);

reportWebVitals();
```

## Elements

- Elements → 리액트 앱을 구성하는 가장 작은 블록들
    
    자바스크립트의 객체 형태로 존재 
    

```jsx
const element = <h1>Hello, world</h1>;
// -> react의 createElement 함수를 사용하여 element 생성
// -> react는 이 element를 이용해 실제 우리가 보게 될 DOM element를 생성
```

```jsx
// elements 실제 모습 (버튼을 나타내기 위한 elements)
{
  type: 'button',  // html 태그가 문자열로 들어감 -> elements는 해당 태그 이름을 가진 DOM 노드를나타냄
  props: { // 속성
    className: 'bg-green',
    children: {
      tye: 'b',
      props: {
        children: 'Hello, element!'
      }
    }
  }
}
```

```jsx
// elements가 실제로 렌더링 되는 모습
<button class='bg-green'>
  <b>
    Hello, element!
  </b>
</button>
```

```jsx
// createElement 함수가 동작하는 과정
function Button(props) {
  return (
    <button className={`bg-${props.color}`}>
      <b>
        {props.children}
      </b>
    </button>
  )
}

function ConfirmDialog(props) { // Button 컴포넌트 포함
  return (
    <div>
      <p>내용을 확인하셨으면 확인 버튼을 눌러주세요.</p>
      <Button color='green'>확인</Button>
    </div>
  )
}
```

```jsx
// confirmDialog 컴포넌트 elements
type : 'div',
props: {
  Children: [
    {
      type: 'p',  // html 태그여서 바로 렌더링
      props: {
        children: '내용을 확인하셨으면 확인 버튼을 눌러주세요.'
      }
    },
    {
      type: Button,  // 리액트 컴포넌트 이름인 Button -> 이 경우 리액트는 버튼 컴포넌트 elemenst를 생성해서 합침 -> 최종적으로 밑의 코드로 변환
      props: {
        color: 'green',
        children: '확인'
      }
    }
  ]
}
=================
{
  type: 'button',  
  props: {
    className: 'bg-green',
    children: {
      type: 'b',
      props: {
        children:'확인'
      }
    }
  }
}
```

## Rendering Elements

<aside>
💡 컴포넌트 렌더링을 위해서 모든 컴포넌트가 createElement 함수르 통해 element로 변환한다.

</aside>

- React Elements → 화면에서 보이는 것들을 기술
    - 특징 1: immutable (불변성) → elements 생성 후에는 children이나 attributes 바꿀 수 없다 → 변경 하고 싶은 경우 새로운 element 만들어서 기존과 변경(virtual DOM 사용)

Rendering

```jsx
// root DOM Node -> React DOM에 의해 관리

<div id="root"></div>  //-> div 태그 안에 elements들이 렌더링
```

```jsx
// root div에 실제로 React elements 렌더링 하기 위해서

const element = <h1>안녕,리액트!</h1>;  // element 먼저 생성
ReactDOM.render(element, document.getElementById('root')); 
// 생성된 elememts를 root div에 렌더링 하는 코드 -> ReactDOM에 render라는 함수 사용 -> 첫 번째 파라미터인 react elements를 두 번째 파라미터인 html elements, 즉 DOM elements에 렌더링 하는 역할
```

- React elements와 DOM elements는 다른 개념!!
    - React elements는 React virtual DOM에 존재, DOM elements는 실제 브라우저 DOM에 존재 !!
- React elements가 렌더링 되는 과정은 virtual DOM → 실제 DOM으로 이동하는 과정

## Rendering Elements를 업데이트 하기

```jsx
function tick() {
  const element = (  // 현재 시간을 포함하고 있는 element 생성하여 root div에 렌더링하는 역할하고 있다
    <div>
      <h1>안녕, 리액트!</h1>
      <h2>현재 시간: {new Date().toLocaleTimeString()}</h2>
    </div>
  );

  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);  // 자바스크립트 함수를 이용하여 tick함수를 1000마다 호출 -> 매초 새로운 시간이 화면에 보임 -> 내부적으로는 기존 element가 변경되는 것이 아니라 새로운 element를 생성해서 바꿔치기
```

### 실습 - 시계 만들기

```jsx
// react 함수 컴퍼넌트 만들기

import React from "react";

function Clock(props) { // Clock 컴퍼넌트
    return (
        <div>
            <h1>안녕, 리액트!</h1>
            <h2>현재 시간: {new Date().toLocaleTimeString()}</h2>
        </div>
    )
}

export default Clock;
```

```jsx
import React, { Children } from "react";
import ReactDOM from "react-dom";
import "./index.css";
import reportWebVitals from "./reportWebVitals";

import Clock from "./chapter_04/Clock";

setInterval(() => {
  ReactDOM.render(
    <React.StrictMode>
      <Clock />
    </React.StrictMode>,
    document.getElementById("root")
  );
});

reportWebVitals();
```

# Components and Props

<aside>
💡 Components (붕어빵 틀) → Elements (붕어빵) → Props (붕어빵 속 재료 ex. 팥, 슈크림…)

</aside>

## Components

- props(입력) → React Component → React Element(출력)

## Props

컴포넌트에 전달할 다양한 정보들을 담고 있는 자바스크립트 객체

### 특징

- Read-Only → 값을 변경 할 수 없다 (ex. 붕어빵 다 구웠는데 속재료 못 바꾸듯이..)
    - 새로운 값을 컴포넌트에 전달하여 새로 Element를 생성
    - 모든 리액트 컴포넌트는 Props를 직접 바꿀 수 없고 같은 Props에 대해서는 항상 같은 결과를 보여줄 것

### 사용법

```jsx
// JSX 형태 -> key, value로 이루어짐
function App(props) {
  return (
    <Profiler
      name='소플'
      introduction='안녕하세요, 소플입니다.'
      viewCount={1500}
    />
  );
}
===========
// Props -> 이처럼 자바스크립트 형태의 객체가 된다.
{
  name: '소플',
  introduction: '안녕하세요, 소플입니다.',
  viexCount: 1500
}
```

```jsx
// Props에 중괄호 사용해서 Props에 컴포넌트도 넣을 수 있다 : Layout 컴포넌트
// 이처럼 JSX를 사용하는 경우 간단하게 컴포넌트 안에 props를 넣을 수 있다.

function App(props) {
  return (
    <Layout
      width={250}   // 정수값
      height={1440} // 정수값
      header={<Header title="소플의 블로그입니다." />}  // React element
      footer={<Footer />}  // React element
    />
  );
}
```

```jsx
// JSX 안씀 -> 비추 (쓰지 말자!!)

React.createElement(
  Profile,  // type -> 컴포넌트 이름인 Profile
  {         // props -> 자바스크립트 객체가 들어감
    name: '소플',
    introduction: '안녕하세요, 소플입니다.',
    viewCount: 1500
  },
  null  // children -> 하위 컴포넌트가 없기에 null
);
```

## Component 만들기

- 항상 대문자로 시작 (소문자로 시작하는 컴포넌트는 DOM태그로 인식하기 때문

```jsx
// DOM 태그를 이용하여 만든 element -> HTML div 태그로 인식

const element = <div />;

* DOM 태그: div, span, h1... 소문자로 이루어짐
```

```jsx
// Welcome이라는 리액트 Component로 인식

const element = <Welcome name="인제" />;
```

- Function Component
- Class Component

```jsx
function Welcome(props) {
  return <h1>안녕, {props.name}</h1>;
}
```

```jsx
class Welcome extends React.component {  // 모든 컴포넌트는 React.component를 상속 받아 만든다
  render() {
    return <h1>안녕, {this.props.name}</h1>;
  }
}
```

## Component 렌더링

```jsx
// DOM 태그를 사용한 element
const element = <div />;

// 사용자가 정의한 Component를 사용한 element
const element = <Welcome name="인제" />;
```

```jsx
function Welcome(props) {             // 1. Welcome이라는 함수 Component 선언
  return <h1>안녕, {props.name}</h1>; // 3. react는 Welcomponent Component에 {nane: "인제"}를 넣어서 호출하고, 그 결과로 react element가 생성된다
}                                     // 4. 이렇게 생성된 react element는 최종적으로 ReactDOM을 통해 실제 DOM에 효과적으로 업데이트되고 우리가 브라우저를 통해서 볼 수 있다

const element = <Welcome name="인제" />;
ReactDOM.render(                      // 2. "Welcome namw = 인제" 라는 값을 가진 element를 파라미터로 해서 ReactDOM.render 함수를 호출
  element,
  document.getElementById('root')
);
```

## Component 합성과 추출

### 합성

```jsx
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

function App(props) { // App component는 Welcome component를 세개 포함한 => component 합성 
  return (           
    <div>
      <Welcome name="Mike"/> // props 값을 다르게 해서 Welcom component 여러번
      <Welcome name="Steve"/>
      <Welcome name="Jane"/>
    </div>
  )
}

ReactDOM.render(
  <App />,
  document.getElementById('root')
);
```

### 추출

⇒ 기능 단위로 추출하면 좋다

```jsx
function Comment(props) {
  return (
    <div className="comment">
        <div className="user-info">
          <img className="avatar"
            src={props.autor.avatarUrl}
            alt={props.author.name}
          />
          <div className="user-info-name">
            {props.author.name}
          </div>
        </div>

        <div className="comment-text">
          {props.text}
        </div>
        
        <div className="comment-date">
          {FormData(props.data)}
        </div>
    </div>
  );
}
===================
props = {
  author: {
    name: "소플",
    avatarUrl: "https://...",
  },
  text: "댓글입니다.",
  date: Date.now(),
}
```

1. Avatar 추출

```jsx
function Avatar(props) {
  return (
    <img className="avatar"
      src={props.user.avatarUrl}
      alt={props.user.name}
    />
  );
}
```

- Comment component에 적용

```jsx
function Comment(props) {
  return (
    <div className="comment">
        <div className="user-info">
            <Avatar user={props.author} />
          />
          <div className="user-info-name">
            {props.author.name}
          </div>
        </div>

        <div className="comment-text">
          {props.text}
        </div>
        
        <div className="comment-date">
          {FormData(props.data)}
        </div>
    </div>
  );
}
```

1. UserInfo 추출

```jsx
function UserInfo(props) {
  <div className="user-info">
    <Avatar user={props.user} /> // Avatar component 포함
    <div className="user-info-name">
      {props.user.name}</div>
  </div>;
}
```

- Comment component에 적용

```jsx
function Comment(props) {
  return (
    <div className="comment">
      <UserInfo user={props.author} />
      <div className="comment-text">
        {props.text}</div>
      <div className="comment-date">
        {FormData(props.data)}</div>
    </div>
  );
}
```

# State and Lifecycle

## State

<aside>
💡 리액트 Component의 변경 가능한 데이터

</aside>

## Lifecycle

<aside>
💡 리액트 클래스 컴포넌트의 생명주기                                                                                                                                        ⇒ “Component가 계속 존재하는 것이 아니라, 시간의 흐름에 따라 생성되고 업데이트 되다가 사라진다”

</aside>

- 개발자가 직접 정의
- 렌더링이나 데이터 흐름에 사용되는 값만 state에 포함시켜야 한다!
- 자바스크립트의 객체
- state는 직접 수정 할 수 없다 (하면 안됨)

### Mounting (출생)

**컴포넌트가 생성되는 시점**

1. 컴포넌트가 실행된다 → 생성자에서는 컴포넌트의 state를 정의한다
2. 컴포넌트가 렌더링된다
3. `componentDidMount` 함수가 호출된다

### Updating (인생)

1. 컴포넌트의 props가 변경, setState() 함수 호출에 의해 state가 변경되거나, forceUpdate()라는 강제 함수 호출로 인해 컴포넌트가 다시 렌더링된다
2. `componentDidUpdate` 함수가 호출된다

### Unmounting (사망)

**상위 컴포넌트에서 현재 컴포넌트를 화면에 더이상 화면에 표시하지 않게 될 때**

1. Unmout 직전에 `componentWillUnmount` 함수가 호출된다

# Hooks

component에는 두 가지 종류가 존재한다.  1. `Function Component`   2. `Class Component`  (component에는 state라는 중요 개념 들어감)

- `Class Component` (state의 관련된 기능 뿐만 아니라 컴포넌트의 생명 주기 함수도 모두 명확하게 정의되어 있기 때문에 잘 가져다 쓰기만 하면 된다)
    - 생성자에서 state를 정의
    - setState() 함수를 통해 state 업데이트
    - Lifecycle methods 제공
- `Function Component`
    - state 사용 불가, 생명주기 함수에 따른 기능 구현 불가 ⇒ `Hooks` : 관련 함수를 갈고리를 걸어 원하는 시점에 정해진 함수를 실행하도록 만든 것
        
        ```jsx
        function MyComponent(props) {
         
                                     => state 관련 함수 (Hooks)
        														 => Lifecycle 관련 함수 (Hooks)
        														 => 최적화 관련 함수 (Hooks)
        	return (
        		<div>
        			안녕하세요, 소플입니다.
        		</div>
        	)
        }
        ```
        

## 대표적인 Hooks에 대해 살펴보자

### useState()

<aside>
💡 state를 사용하기 위한 Hook (함수 컴포넌트에서는 기본적으로 state를 제공하지 않아서 state 사용하고 싶을 때 사용한다)

</aside>

- useState() 사용법
    
    ```jsx
    import React, { useState } from "react";
    
    function Counter(props) {
        var count = 0;
    
        return (
            <div>
                <p>총 {count}번 클릭했습니다.</p>
                <button onClick={() => count++}>
                    클릭
                </button>
            </div>
        );
    }
    // 문제점
    -> 이처럼 count를 함수의 변수로 선언하게되면 버튼 클릭시 카운트 값을 증가시킬 수 있지만 재렌더링이 일어나지 않아 새로운 카운트 값이 화면에 보이지 않게됨, 
       이런 경우에는 state를 사용해서 값이 바뀔 때마다 재렌더링 되어야 하는데 함수 컨포넌트에는 해당 기능이 따로 없기 때문에 useState를 사용하여 state를 선언하고 update해야 한다.
    ```
    
    - `const [변수명, set함수명] = usestate(초기값);`  ⇒ 변수 각각에 대해 set함수가 따로 존재 !!!
    - return값이 배열로 → state으로 선언된 변수, 해당 state의 set함수가 들어온다

### useMemo()

<aside>
💡 Memozied value를 리턴하는 Hook

</aside>

- useMemo() 사용법
    
    ```jsx
    const memoizedValue = useMemo(
        () => {
            // 연산량이 높은 작업을 수행하여 결과를 반환
        return computeExpensiveValue(의존성 변수1,의존성 변수2);
        }, 
        [의존성 변수, 의존성 변수2]
    );
    // 파라미터로 memoizedValue를 생성하는 create 함수와 의존성 배열을 받는다
    // memozation 개념처럼 배열에 들어있는 변수가 변했을 경우에만 새로create 함수를 호출하여
    // 결과값을 반환하며, 그렇지 않은 경우에는 기존 함수의 결과값을 그대로 반환
    ```
    
    - 컴포넌트가 다시 렌더링 될 때마다 연산량이 높은 작업을 반복하는 것을 피할 수 있다 => 빠른 렌더링 속도를 얻을 수 있다
    - useMemo로 전달된 함수는 렌더링이 일어나는 동안 실행된다 → 렌더링이 일어나는 동안 실행되어서는 안되는 작업을 useMemo 함수에 넣으면 x
        
        ex) useEffect Hook에서 실행되어야 할 side effect같은 것 (서버에서 데이터를 받아오거나 수동으로 DOM을 변경하는 작업 등은 렌더링이 일어나는 동안 실행되어서는 안됨)
        
    - 의존성 배열을 넣지 않을 경우, 매 렌더링마다 함수가 실행
        
        ```jsx
        const memoizedValue = useMemo(
            () => computeExpensiveValue(a, b)
        );
        // 매번 create 함수가 실행된다
        ```
        
    - 의존성 배열이 빈 배열일 경우, 컴포넌트 마운트 시에만 호출 됨
        
        ```jsx
        const memoizedValue = useMemo(
            () => {
                return computeExpensiveValue(a, b)
            },
            []
        );
        // 마운트 될 때만 create 함수가 호출된다 -> 마운트 이후에는 값이 변경 안됨
        ```
        

### useEffect()

<aside>
💡 side effect를 수행하기 위한 Hook

</aside>

- side effect = 효과, 영향
    - 다른 컴포넌트에 영향을 미칠 수 있으며, 렌더링 중에는 작업이 완료될 수 없기 때문에 렌더링 되는 동안 실행x
- useEffect() 사용법
    
    ```jsx
    useEffect(() => {
    	// 컴포넌트가 마운트 된 이후,
    	// 의존성 배열에 있는 변수들 중 하나라도 값이 변경되었을 때 실행됨
    	// 의존성 배열에 빈 배열([])을 넣으면 마운트와 언마운트시에 단 한 번씩만 실행됨
    	// 의존성 배열 생략 시 컴포넌트 업데이트 시마다 실행됨
    ```
    	retutn () => {
    		// 컴포넌트가 마운트 해제되기 전에 실행됨
    		```
    	}
    }, [의존성 변수1, 의존성 변수2, ....]);
    ```
    
    `useEffect(이펙트 함수, 의존성 배열);` ⇒ 의존성 배열 안에 있는 변수 중에 하나라도 값이 변경되면 이펙트 함수가 실행된다. 기본적으로 이펙트 함수는 처음 컴포넌트가 렌더링된 이후와 update로 인한 재렌더링 이후에 실행된다.
    
    - Effect function이 mount, unmount 시에 단 한 번씩만 실행되고 싶을 때
        
        `useEffecr(이펙트 함수, [])` ⇒ 빈 배열
        
    - 의존성 배열 생략 가능
        
        `useEffect(이펙트 함수);` ⇒ 컴포넌트가 업데이트 될 때마다 호출된다
        
        ```jsx
        // 의존성 배열 없이 useEffect 사용 => effect는 DOM이 변경된 이후에 해당 이펙트 함수를 실행
        import React, { useState, useEffect} from "react";
        
        function Counter(props) {
            const [count, setCount] = useState(0);
        
        		// componentDidMount, componentDidUpdate와 비슷하게 작동
        		useEffect(() => {
        			// 브라우저 API를 사용해서 document와 title을 업데이트 한다.
        			document.title = 'yoy clicked ${count} times';
        		});
            return (
                <div>
                    <p>총 {count}번 클릭했습니다.</p>
                    <button onClick={() => count++}>
                        클릭
                    </button>
                </div>
            );
        }
        // effect는 함수 컴포넌트 안에서 선언되기 떄문에 해당 컴포넌트 props와 state에 접근 할 수 있다.
        ```
        

### useCallback()

<aside>
💡 useMemo() Hook과 유사하지만 값이 아닌 함수를 반환 ⇒ 컴포넌트가 렌더링 될 때마다 매번 함수를 새로 정의하는 것이 아닌 의존성 배열에 값이 바뀐 경우에만 함수를 새로 정의해서 리턴해준다

</aside>

- useCallback() 사용법
    
    ```jsx
    const memoizedCallback = useCallback(
        () => {
            doSomething(의존성 변수1, 의존성 변수2);
        },
        [의존성 변수1, 의존성 변수2]
    );
    // 함수와 의존성 배열을 파라미터로 받는다 -> 파라미터로 받는 함수를 callback이라 부른다
    // 의존성 배열에 있는 변수 중 하나라도 변경되면 memoization된 callback 함수를 반환한다
    ```
    
    - 의존성 배열에 따라 memoized 값을 반환한다는 점이 useMemo Hook과 동일
        
        ```jsx
        // 동일한 역할을 하는 코드
        useCallback(함수, 의존성 배열);
        useMemo(() => 함수 의존성 배열);
        ```
        

### useRef()

<aside>
💡 Reference(특정 컴포넌트에 접근할 수 있는 객체)를 사용하기 위한 Hook ⇒ 반환한다

</aside>

- refObject.current
    
    *current : 현재 참조하고 있는 Element
    
- useRef() 사용법
    
    ```jsx
    const refContainer = useRef(초기값);
    // 파라미터로 초기값을 넣으면 초기값으로 초기화된 레퍼런스 객체를 반환한다
    // 만약 초기값이 null이라면 current의 값이 null인 레퍼런스가 반환된다
    // 반환된 레퍼런스 객체는 컴포넌트 lifetime 전체에 걸쳐서 유지된다
    // 컴포넌트가 마운트 헤드 전까지는 계속 유지
    ```
    
    - => useRef Hook은 변경 가능한 current라는 속성을 가진 하나의 상자라고 생각
    - 매번 렌더링 될 때마다 항상 같은 레퍼런스 객체를 반환
    - 내부의 데이터가 변경되었을 때 별도로 알리지 않는다 → current 속성을 변경한다고 해서 재렌더링이 일어나지 않는다
    
- DOM 노드의 변화를 알기 위해 가장 기초적인 방법
    - Callback ref
        - 리액트는 ref가 다른 노드에 연결될때마다 callback을 호출하게 된다
        

## Hook의 규칙과 Custom Hook 만들기

### Hook의 규칙

- Hook은 무조건 최상위 레벨에서만 호출해야 한다.
    - Hook은 컴포넌트가 렌더링될 때마다 매번 같은 순서로 호출되어야 한다.
- 리액트 함수 컴포넌트에서만 Hook을 호출해야 한다.

### Custom Hook

여러 컴포넌트에서 반복적으로 사용되는 로직을 Hook으로 만들어 재사용하기 위함

**상황**

```jsx
import React, { useState, useEffect } from "react";

function UserStatus(props) {
    const [inOnline, setIsOnline] = useState(null);

    useEffect(() => {
        function handleStatusChange(status); {
            setIsOnline(status.isOnline);
        }

    ServerAPI.subscribeUserStstue(props.user.id, handleStatusChange);
    return() => {
        ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
    };
    });

    if (isOnline === null) {
        return '대기중....';
    }
    return isOnline ? '온라인' : '오프라인';
}
```

```jsx
import React, { useState, useEffect } from "react";

function UserListItem(props) {
    const [inOnline, setIsOnline] = useState(null);

    useEffect(() => {
        function handleStatusChange(status); {
            setIsOnline(status.isOnline);
        }

        ServerAPI.subscribeUserStstue(props.user.id, handleStatusChange);
        return() => {
            ServerAPI.unsubscribeUserStatus(props.user.id, handleStatusChange);
        };
    });

   return (
    <li style={{ color: isOnline ? 'green': 'black' }}>
        {props.user.name}
    </li>
   );
}
```

### Custom Hook 추출하기

Custom Hook은 이름이 use로 시작하고 내부에서 다른 Hook을 호출하는 하나의 자바스크립트 함수

```jsx
import { useState, useEffect } from "react";

function useUserStatus(userId) {  // userUserStatus 추출하기
    const [inOnline, setIsOnline] = useState(null);

    useEffect(() => {
        function handleStatusChange(status); {
            setIsOnline(status.isOnline);
        }

        ServerAPI.subscribeUserStstue(userId, handleStatusChange);
        return() => {
            ServerAPI.unsubscribeUserStatus(userId, handleStatusChange);
        };
    });

   return isOnline;
}
```

### Custom Hook 사용하기

- 여러 개의 컴포넌트에서 하나의 Custom Hook을 사용할 때 컴포넌트 내부에 있는 모든 state와 effect는 전부 분리되어 있다
- 리액트 컴포넌트는 각각의 Custom Hook 호출에 대해서 분리된 state를 얻게 된다
- 각 Custom Hook의 호출 또한 완전히 독립적이다

```jsx
function UserStatus(props) {
    const isOnline = useUserStatus(props.user.id);

    if (isOnline === null) {
        return '대기중...'
    }
    return isOnline ? '온라인' : '오프라인';
}

function UserListItem(props) {
    const isOnline = useUserStatus(props.user.id);

    return (
        <li style = {{ color : isOnline ? 'green' : 'black ' }}>
            {props.user.name}
        </li>
    );
}
```

# Handling Events

## Event - 사건

### DOM의 Event

### 리액트의 Event

```jsx
<button onclick="activate()">
	Activate
<button>
```

```jsx
<button onclick={activate}>
	Activate
</button>
```

## Event Handler (=Event Listener)

어떤 사건이 발생하면, 사건을 처리하는 역할

### 사용 방법

```jsx
function Toggle(props) {
    const [isToggleOn, setIsToggleOn] = useState(true);
    
    // 방법 1. 함수 안에 함수로 정의
    function handleClick(){
        setIsToggleOn((isToggleOn) => !isToggleOn);
    }

    // 방법 2. arrow function을 사용하여 정의
    const handleClick = () => {
        setIsToggleOn((isToggleOn) => !isToggleOn);
    }

    return (
        <button onClick={handleClick}>
            {isToggleOn ? "켜짐" : "꺼짐"}
        </button>
    );
}
```

### argument 전달하기

argument : 함수(Event Handle)에 전달할 데이터

parameter : 매개변수

```jsx
function MyButton(props) {
	const handleDelete = (id, event) => {
		console.log(id, event.target);
	};

	return (
		<button onClick={(event) => handleDelete(1, event)}>
				삭제하기
		</button>
	);
}
```

### 클릭 이벤트 처리하기

```jsx
import React, { useState } from "react";
// arrow function
function ConfirmButton(props) { 
    const [isConfirmed, setIsConfirmed] = useState(false);

    const handleConfirm = () => {
        setIsConfirmed((preIsConfirmed) => !prevIsConfirmed);
    };

    return (
        <button onClick={handleConfirm} disabled={isConfirmed}>
            {isConfirmed ? "확인됨" : "확인하기"}
        </button>
    );
}

export default ConfirmButton;
```

# Conditional Rendering

> 어떠한 조건에 따라서 렌더링이 달라지는 것
> 

## Element Variables (조건부 렌더링 하는 방법 1)

<aside>
💡 리액트 element를 변수처럼 다루고 싶을 때

</aside>

```jsx
function LoginControl(props) {
    const [isLoggedIn, setIsLoggedIn] = useState(false);

    const handleLoginClick = () => {
        setIsLoggedIn(true);
    }

    const handleLogoutClick = ()=> {
        setIsLoggedIn(false);    
    }

    let button;
    if (isLoggedIn) {  // isLoggedIn 값에 따라서 버튼이라는 변수에 컴포넌트를 대입
        button = <LogoutButton onclick={handleLogoutClick} />;
    } else {
        button = <LogoutButton onClick={handleLoginClick} />;
    }

    return (
        <div>
            <Greeting isLoggedIn={isLoggrdIn}/>
            {button} // 컴포넌트가 대입된 변수를 return에 넣어 실제로 컴포넌트가 렌더링이 되도록 만든다
                     // 실제로는 컴포넌트로부터 생성된 리액트 element -> 이처럼 element를 변수처럼 저장해서 사용하는 것을 element variables
        </div>
    )
}
```

## Inine Conditions (조건부 렌더링 하는 방법 2)

<aside>
💡 조건문을 코드 안에 집어넣는 것

</aside>

### Inline If

<aside>
💡 if문의 경우 && 연산자를 사용

</aside>

- 양쪽에 나오는 조건문이 모두 true인 경우에만 결과도 true

→ 첫 번째 조건문이 true면 두번째 조건문 평가 / 첫 번째 조건문이 false이면 두 번째 조건문이 false이므로 평가 x

→ 그래서 `react에서는 첫 번째 조건문이 true면 오른쪽에 나오는 element가 결과 값이` 되고, `false이면 false가 결과 값`이 된다.

```jsx
function Mailbox(props) {
    const unreadMessages = props.unreadMessages;

    return (
        <div>
            <h1>안녕하세요</h1>
            {unreadMessages/length > 0 && // 앞에 조건문이 true면 밑의 내용의 결과 값이 나오고, false면 아무것도 렌더링 되지 않는다 
            <h2>
                현재 {unreadMessages.length}개의 읽지 않은 메시지가 있습니다.
            </h2>
            }
        </div>
    );
}
```

### Inline If-Else

<aside>
💡 if-else가 필요한 곳에 직접 넣어줘야 한다 → 조건문의 값에 따라서 다른 element를 보여줄 때 사용 → ?(삼항 연산자) 사용

</aside>

- 삼항 연산자
    
    → condition ? true : false ⇒ 조건문이 참이면 첫 번째 항목을 리턴하고 거짓이면 두 번째 항목을 리턴
    
    ```jsx
    function UserStatur(props) {
        return (
            <div>
                이 사용자는 현재 <b>{props.idLoggedIn ? '로그인' : '로그인하징 않은'}</b> 상태입니다.
            </div>
        )
    }
    ```
    
    ```jsx
    // 문자열이 아닌 경우
    return (
        <div>
            <Greeting isLoggedIn={isLoggedIn}/>
            {isLoggedIn
                ? <LogoutButtin onClick={handleLogoutClick} />
                : <LoginButton onClick={handleLoginClick} />
            }
        </div>
    )
    ```
    

## Component 렌더링 막기

<aside>
💡 null을 리턴하면 렌더링되지 않음

</aside>

```jsx
function WarningBanner(props){
    if (!props.warning) {
        return null;  // props.warning의 값이 false인 경우에 리턴
    }

    return (
        <div>경고!</div>  // props;.warning의 값이 true인 경우에 리턴
    );
}
```

# List and Keys

## List

<aside>
💡 사용하는 자료구조가 Array(배열) → 자바스크립트의 변수나 객체들을 하나의 변수로 묶어 놓은 것

</aside>

## Key

<aside>
💡 각 객체나 아이템을 구분할 수 있는 고유한 값

- 리액트에서는 아이템들을 구분하기 위한 고유한 문자열
</aside>

## 여러 개의 Component 렌더링 하기

### map()

```jsx
const doubled = numbers.map((number) => number * 2); 
// map() 이용하여 numbers 배열에 들어 있는 각 숫자에 2를 곱한 값이 들어간 doubled라는 배열을 생성 
```

```jsx
const numbers = [1, 2, 3, 4, 5];
const listItems = numbers.map((number) =>
	<li>{number}</li>  // return ->listItems 배열응 총 다섯 개의 element를 갖는다
);

ReactDOM.render( // 화면에 렌더링 하기 위해서 ReactDOM에 render() 사용
	<ul>{listItems}</ul>,
  // <li>{1}</li>
  // <li>{2}</li>....
	document.getElementById('root')
);
```

### 기본적인 List Component

```jsx
function NumberList(props) {  // NumberList 컴포넌트는 props로 숫자가 들어 있는 배열인 numbers를 받아서
    const { numbers } = props;

    const listItems = numbers.map((number) => 
        <li>{number}</li>
    );

    return (
        <ul>{listItems}</ul>
    );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.rende(
    <NumberList numvers={numbers} />,
    document.getElementById('root')
);
```
