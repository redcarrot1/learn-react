컴포넌트

화면은 결국 컴포넌트들의 결합

재사용과 결합도 가능성을 낮추기 위해 컴포넌트 사용

코드를 작은 코드들로 분해



컴포넌트=html+css+js

리액트=컴포넌트+컴포넌트+...





### 프로젝트 시작하기

- 먼저 Node.js 설치

```bash
npx create-react-app my-app
cd my-app
npm start
```

- 패키지 설치만(ex `node_modules`) 원한다면 `npm install`





react는 결국 JS의 라이브러리이다.

하지만 우리가 작성한 코드가 그대로 브라우저에 전달되는게 아니라, JS에 맞게 변환 후 브라우저에 실행된다.

따라서 기본 JS 문법이나 작성법과 조금 다를 수 있다. (JSX 문법)





Index.js가 가장 먼저 실행되는 파일이다.





public>index.html 을 열어보면 `<div id="root"><div>`가 있다.

src>index.js를 열어보면 `ReactDOM.render(<App />, document.getElementById('root'));`가 있다.

`ReactDom.render()`함수에 `<App/>`과 index의 rootId를 넘겨주면  index.html의 div 부분이 우리가 작성하는 `<App/>`의 내용으로 바뀐다. 또한  `<App/>` 과 같은것을 컴포넌트라고 한다.







다른 라이브러리나 파일을 imort할 때, css는 확장자를 붙이고 나머지는 js는 붙이지 않는다.

```react
import ReactDOM from 'react-dom';

import './index.css';
import App from './App';
```







JSX문법을 이용함

```react
//js안에 html 문법이 있음
function App() {
  return (
    <div>
      <h2>Let's get started!</h2>
    </div>
  );
}
```





react의 파일명은

- 대문자 시작
- 새로운 서브 단어의 경우 대문자로 시작







컴포넌트를 결합할 때는 파일을 imort한 후 html 태그처럼 사용하면 된다.

```react
function ExpenseItem() {
    return <h2>Expense item!</h2>
}
export default ExpenseItem;




import ExpenseItem from './components/ExpenseItem';
function App() {
  return (
    <div>
      <h2>Let's get started!</h2>
      <ExpenseItem></ExpenseItem>
    </div>
  );
}
export default App;
```









- react의 중요한 문법 중 하나는 리턴될 때 루트 태그는 하나이어야 한다. 따라서 바깥에 큰 태그를 두고 시작해야한다.
- CSS를 사용할 땐 import 하고 `className`으로 사용한다.
- html 내부에서 `{}`을 이용하면 js의 값을 사용할 수 있다.
  - 주로 하드코딩보다 동적 방식을 더 많이 사용한다.

```react
import "./ExpenseItem.css";

function ExpenseItem() {
  const expenseDate = new Date(2021, 2, 28);
  const expenseTitle = "Car Insurance";
  const expenseAmount = 294.67;

  return (
    <div className="expense-item">
      <div>{expenseDate.toISOString()}</div>
      <div className="expense-item__description">
        <h2>{expenseTitle}</h2>
        <div className="expense-item__price">${expenseAmount}</div>
      </div>
    </div>
  );
}

export default ExpenseItem;
```









컴포넌트 속성으로 값을 넘길 수 있다. 사용하는 컴포넌트에선 `props`란 객체로 모든 속성을 받게 된다.

```react
import ExpenseItem from "./components/ExpenseItem";
function App() {
  return (
    <div>
      <h2>Let's get started!</h2>

      <ExpenseItem
        title="title"
        amount="amount"
        date=new Date(2021, 2, 28)
      ></ExpenseItem>
    </div>
  );
}
export default App;




import "./ExpenseItem.css";
function ExpenseItem(props) {
  return (
    <div className="expense-item">
      <div>{props.date.toISOString()}</div>
      <div className="expense-item__description">
        <h2>{props.title}</h2>
        <div className="expense-item__price">${props.amount}</div>
      </div>
    </div>
  );
}

export default ExpenseItem;

```









react는 형제 컴포넌트간 데이터 교류가 불가능

반드시 부모 <-> 자녀 관계로 데이터를 주고 받아야 함.

부모->자녀: props

자녀->부모: 부모 컴포넌트에서 매개변수 받는 함수를 만든 후 자녀에게 props로 함수 전달, 자녀에서 props.전달된함수에 데이터를 넣어서 호출하면 됨.





useState를 사용하는 컴포넌트 -> statefull 컴포넌트, controller 컴포넌트

단지 화면을 표시하기 위한 컴포넌트 -> presentation 컴포넌트, dump 컴포넌트







props.children

예를 들어 카드 모양의 ui를 위해서 다음과 같은 코드를 짠다고 해보자

```react
const AddUser = () => {

  return (
    <Card>
      <form>
        ...
      </form>
    </Card>
  );
};
```



그럼 `<Card>`태그 사이에 있는 내용이 그대로 박혀야 한다.

이럴때 Card.js 에서 props.children을 사용한다.

```react
const Card = props => {
    return <div>
        {props.children}
    </div>
};
```









Return 루트 태그는 1개이어야 한다.

이를 위해 보통 `<div>`로 감싸는데, 이렇게 되면 큰 프로젝트에선 로딩될 때 엄청나게 겹쳐져있는것을 볼 수 있다.

리액트는 `<React.Fragment>`를 지원한다. 또는 임폴트에 `Fragment`를 작성 후 `<Fragment>`만 사용

```react
import React, { Fragment} from 'react';

function App() {
  return (
    <React.Fragment>
      <h1> </h1>
      <h2> </h2>
    </React.Fragment>
  );
}

또는

function App() {
  return (
    <Fragment>
      <h1> </h1>
      <h2> </h2>
    </Fragment>
  );
}
```









포탈

원하는 html 장소에 코드를 넣는 것

`ReactDOM.createPortal(jsx, 원하는 위치)`

```react
import React from "react";
import ReactDOM from "react-dom";

const Backdrop = (props) => {
  return <div>{props.message}</div>
};

const ModalOverlay = (props) => {
  return (
    <div> ok </div>
  );
};

const ErrorModal = (props) => {
  return (
    <React.Fragment>
      {ReactDOM.createPortal(
        <Backdrop message={props.message} />,
        document.getElementById("backdrop-root")
      )}
      {ReactDOM.createPortal(
        <ModalOverlay />,
        document.getElementById("overlay-root")
      )}
    </React.Fragment>
  );
};

export default ErrorModal;
```













ref

사용자의 input을 전송 시 받을 수 있다.

useRef로 생성 후 `<input id="username" type="text" ref={nameInputRef} />`



onChange와 useState를 함께 써서 입력값을 받아도 좋지만, 그럴경우 불필요한 로깅을 계속 하게 된다. 하지만 사용 기능은 더 다양하짐

ref를 사용하면 쉽게 사용자 input값을 불필요한 로그 없이 받을 수 있지만, 조작하는데 한계가 있다.

```react
import React, { useState, useRef } from "react";

const AddUser = (props) => {
  const nameInputRef = useRef();
  const ageInputRef = useRef();

  const addUserHandler = (event) => {
    event.preventDefault();
    const enteredName = nameInputRef.current.value;
    const enteredUserAge = ageInputRef.current.value;
    console.log(enteredName);
    console.log(enteredUserAge)l

    //ref를 직접 수정하는 것은 추천하지 않는다.
    nameInputRef.current.value='';
    ageInputRef.current.value='';
  };

  const errorHandler = () => {
    setError(null);
  };

  return (
        <form onSubmit={addUserHandler}>
          <label htmlFor="username">Username</label>
          <input id="username" type="text" ref={nameInputRef} />
      
          <label htmlFor="age">Age (Years)</label>
          <input id="age" type="number" ref={ageInputRef} />
      
          <Button type="submit">Add User</Button>
        </form>
  );
};

export default AddUser;
```











useEffect(함수, 의존변수)

첫 리로드 된 후 실행 + 의존변수의 값이 변할때마다 함수가 실행됨

- 의존 인자를 넣지 않으면 첫실행 + 컴포넌트가 다시 그려질 때마다 실행된다
- 빈 객체(`[]`를 첫으면 첫 실행에만 실행
- 의존 변수를 넣으면 첫 실행+그 변수가 업데이트 될 때만 실행



return 으로 함수를 넘길 수 있는데, 일명 cleanup 함수이다.

cleanup함수는 useEffect가 다시 실행되기 전에 실행된다.

useEffect1 -> cleanup useEffect2 -> cleanup useEffect3 ->...



```react
useEffect(() => {
  const identifier=setTimeout(() => { //특정 시간이 지난 후에 실행
    console.log('Checking form validity!')
  }, 500);

  return ()=>{ //useEffect가 다시 실행되기 전에 실행됨
    console.log('CLEANUP');
    clearTimeout(identifier); //내장함수. 실행 대기 중인 setTimeout 함수를 없앨 수 있다.
  };
}, [enteredEmail, enteredPassword]);
```















useReducer()

- useState가 너무 복잡할 경우에 사용

`const [state, dispatchFn] = useReducer(reducerFn, initialState, initFn);`

- `dispatchFn`에 객체를 넣어서 함수를 실행할 수 있음
- `reducerFn`에서 현재 상태인 `state`와 파라미터로 받은 `action`을 인자로 받는데, `action`의 값에 따라 `state`를 처리하는 로직을 사용
  - 리턴값은 업데이트 되는`state` 정보

```react
const emailReducer = (state, action) => {
  if (action.type === "USER_INPUT") {
    return { value: action.val, isValid: action.val.includes("@") };
  }
  if (action.type === "INPUT_BLUR") {
    return { value: state.value, isValid: state.value.includes("@") };
  }
  return { value: "", isValid: false };
};

 const [emailState, dispatchEmail] = useReducer(emailReducer, {
    value: "",
    isValid: false,
  });

dispatchEmail({ type: "USER_INPUT", val: event.target.value });
```









Rules of Hooks

1. Only call React Hooks In **React Functions**

2. Only call React Hooks at the Top Level
3. 
