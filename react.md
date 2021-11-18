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



