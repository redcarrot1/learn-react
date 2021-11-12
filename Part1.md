html, css -> 정적인 콘텐츠

JS -> 동적으로 무언가를 하고 싶을 때... 데이터를 불러오거나 실시간 화면을 바꾸거나...

html + css +  JS + Server -> 기본적인 웹!



보통 js는 <head>안에 <script>에 넣는다. -> 편의상, 임시로, 아주 간단할때 이렇게 사용

브라우저는 html을 위에서부터 읽으면서 화면에 띄어주기 때문에 body를 빨리 보여주기 위해 script를 body의 마지막부분에 넣기도 한다.

또는 .js파일을 따로 만든 뒤 <script src="{파일경로}"></script>를 하던지. -> 대부분 이렇게 사용



명령문이 끝날 땐 `;`을 붙이는게 좋다.

안붙여도 정상 실행은 되지만.. 가급적 붙이는걸로?



html주석은 `<!--주석-->`

js주석은 `//`(한줄), `/* */`(여러줄)





변수

`var`, `let`, `const`

변수타입 구분이 없다. 처음에 숫자로 시작했지만 중간에 문자열로 바꿀 수도 있다.

```js
var a = 100;
a = 'ㅋㅋㅋ';
```

변수명으로 한글, 영어, 일본어 등이 모두 가능하다.

데이터타입을 알고싶다면 `console.log(typeof a);`

변수를 만들고 값을 넣지 않으면 `undefined`가 뜬다.

배열에 서로 다른 데이터타입이 들어와도 괜찮다. `var a = [1, 2, "안녕", true]`

`배열.length`하면 배열의 길이가 리턴되다.





if, for 등은 모두 타언어와 동일





함수

```js
function sample(a, b) {
    
    return a+b;
}

var result = sample();
```







객체

```js
var person = {};
person.userName = '동네';
person.age = 18;
person.introduce = function () { //익명함수
    //객체 내에 있는 변수 사용을 위해 this. 사용
    console.log('안녕하세요?'+this.userName+"입니다."); 
}

var person2 = { //객체 내에선 = 대신 : 사용
    userName: '제나',
    age: 20,
    intorudce: function() {
    console.log('안녕하세요?'+this.userName+"입니다.");
    }
};

person.introduce();
person2.intorudce();



//생성자(Constructor)
function Person(userName, age){
    this.userName=userName;
    this.age=age;
    this.introduce = function(){
        console.log('안녕하세요?'+this.userName+"입니다.");
    }
}

//인스턴스(instance)
var p1 = new Person('동네', 18);
var p2 = new Person('제나', 20);

p1.introduce();
p2.introduce();
```









### DOM (Document Object Model)

 ```js
 //전체 페이지가 로드 후 실행
 //js는 위에서 읽은 순서대로 바로 실행하기 때문에 아래 요소를 잡기 위해선 필요하다.
 window.onload = function (){
     var title=document.getElementById('main-title');
     console.log(title);
 }
 
 //요즘엔 addEventListener(명령, 함수) 사용
 window.addEventListener('load', function(){
     var title=document.getElementById('main-title');
     console.log(title);
 });
 //load : 무조건 전체 페이지가 로드된 후 실행
 //DOMContentLoaded: DOM이 로드 후 실행
 
 //요즘에는 이런식의 코드를 잘 사용하지 않음
 var title = document.getElementById('main-title');
 var titleSpan = title.getElementsByTagName('span');
 
 //요즘은 이렇게 사용
 //가장 먼저 찾은 객체를 리턴해줌
 var mainTitle=document.querySelector('#main-title'); //#{id}
 var mainTitle=document.querySelector('#main-title span'); //#{id}의 자식 span
 
 var mainTitle=document.querySelector('.main-title'); //.{class}
 
 //해당하는 모든 객체를 모두 리턴 (배열로)
 var mainTitle=document.querySelectorAll('.main-title'); //.{class}
 console.log(mainTile[2]); //배열처럼 리턴 가능
 for(var i=0; i<mainTitle.length; i++){
     mainTitle[i].style.border = '1px solid red';
 }
 
 
 
 //특정 태그의 클래스나 속성을 가져오기
 var link = document.querySelector('.ilbuni a');
 console.log(link.getAttribute('href')); //.getAttribute( {속성 이름} )
 link.setAttribute('href', 'https://www.naver.com'); //.SetAttribute({속성 이름},{바꿀정보})
 
 link.classList.add('special', 'foo'); //기존 클래스에 추가, 여러개 가능
 link.classList.remove('special', 'foo'); //클래스 삭제, 여러개 가능
 link.classList.contains('special') //true, false
 
 
 
 //동적으로 화면 그리기
 var mom=document.querySelector('.mom'); //태그 추가는 부모가 해줌
 var listElem = document.createElement('li'); //태그 생성
 listElem.innerHTML='<span>새로 추가된 li 입니다.</span>'; //태그안에 쓸 내용
 mom.appendChild(listElem);
 ```













### 이벤트(Event)

```js
var btn=document.querySelector('.btn');
var mainTitle = document.querySelector('#main-title');
function btnClickHandler() { //js에서 직접 css 값 넣기
    mainTitle.style.background='dodgerblue';
}
btn.addEventListener('click', btnClickHandler); //이벤트 추가
```

```js
var currentMenu;
var menuLinks = document.querySelectorAll('.menu-link');

function clickMenuHandler() {
    if(currentMenu) currentMenu.classList.remove('menu-active');
    this.classList.add('menu-active');
    currentMenu=this;
}

for (var i = 0; i < menuLinks.length; i++) {
    menuLinks[i].addEventListener('click', clickMenuHandler);
}



VS

//이렇게 이벤트 위임을 사용하자!
var currentMenu;
var menu = document.querySelector('.menu');

function clickMenuHandler(e) { //이벤트 핸들러로 사용되면 자동으로 이벤트 정보 e가 들어옴
    //this == e.currentTarget -> menu
    //e.target == 이벤트가 실행된 객체
    if(currentMenu) currentMenu.classList.remove('menu-active');
    e.target.classList.add('menu-active');
    currentMenu=e.target;
}

menu.addEventListener('click', clickMenuHandler); //부모에게 이벤트 위임
```

- 이벤트 함수는 최대한 단순하게...
  - 길어지면 함수를 분리하자
