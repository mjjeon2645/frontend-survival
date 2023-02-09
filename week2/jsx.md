# 1. JSX

## 강의 요약 및 노트

- JSX. XML같은 ECMAScript(JS) 문법 확장이다. 
- from 리액트 공식문서: JSX는 JavaScript를 확장한 문법!
- 오해 -> JSX를 React에서만 사용하느냐? NO. React의 부산물 같은 것이기는 하나 반드시 리액트에서만 쓰이는 것은 아님. Vue.js나 다른 프레임워크에서도 사용하기도 함.
- c.f. XML(eXtensible Markup Language) => 데이터를 정의하는 규칙을 제공하는 마크업 언어.

- 근본적으로 JSX는 `React.createElement(component, props, ...children)` 함수에 대한 syntactic sugar를 제공할 뿐이라고 함. (from [리액트 공식문서]((https://ko.reactjs.org/docs/jsx-in-depth.html)))

- JSX는 XML처럼 작성된 부분을 React.createElement를 쓰는 JavaScript 코드로 변환함.  
중괄호를 써서 JS 코드를 그대로 쓸 수 있음(ex. `<p>Hello, {name}!</p>`)  
결국은 JS 코드와 1:1로 매칭됨

- example로 제시된 코드 뜯어보기

### example 1

```javascript
//JSX 코드

<p>Hello, world!</p>
```

```javascript
// 변환된 JS 코드. null 자리는 속성 

React.createElement("p", null, "Hello, world!");
```

- createElement의 첫번째 인자가 일반적인 HTML 태그라면 쌍따옴표("")로 감싼다는 부분 체크하기
- jsx 코드가 `<Button>sample</Button>` 인 것과 `<button>sample</button>` 인 것의 첫 번째 인자는 다름(전자는 리액트의 컴포넌트, 후자는 HTML의 기본 태그인 button이기 때문에 후자의 것이 쌍따옴표로 감싸짐)

### example 2

```javascript
//JSX 코드

<Greeting name="world" />
```

```javascript
// 변환된 JS 코드

React.createElement(Greeting, { name: "world" });
```

- Greeting은 사실 function임. p와 같이 태그가 아닌 function인데 그 자체로 전달이 되었음 -> 이를 통해 JS에서는 function, 즉 함수가 고차함수. 1급 객체로 취급된다는 사실을 알 수 있음(function 그 자체를 object 취급하여 return 하거나 또는 다른 함수의 인자로 제공할 수 있음)

- **궁금한 점.** createElement의 2번째 인자가 비어있을 땐 null을 줬는데, 3번째 인자가 비어있어 보이는데 여기서는 null을 안주고 그냥 비운채로 끝내버리네? -> Greeting 자체가 함수라서 태그로 감싼 text-node가 없기 때문인가? 여튼 굳이 써주지는 않는가보다..

- example 1에서 속성 자리가 null이었던 것과는 달리 이번에는 name 속성에 world가 있으므로 해당 자리가 채워짐. 눈여겨 볼 것은 해당 자리에는 객체(key-value)가 들어옴. 속성이 여러개일 경우 객체 안에 key-value가 콤마로 구분되어 늘어남

### exmaple 3

```javascript
// JSX 코드

<Button type="submit">Send</Button>
```

```javascript
// 변환된 JS 코드

React.createElement(Button, { type: "submit" }, "Send");
```

- 위 내용을 통해 가장 마지막 인자는 텍스트-노드가 들어간다는 것을 확인할 수 있었음.

- 참고. `<React.fragment> </React.fragment>`는 `<> </>`로 동일하게 사용 가능

### example 4

```javascript
// JSX 코드

<div className="test">
	<p>Hello, world!</p>
	<Button type="submit">Send</Button>
</div>
```

```javascript
// 변환된 JS 코드

React.createElement(
	"div",
	{ className: "test" },
	React.createElement("p", null, "Hello, world!"),
	React.createElement(Button, { type: "submit" }, "Send")
);
```

- 리액트 컴포넌트와 같은 형태의 function(예시 2)을 넘겨줬을 때와의 차이점 눈여겨보기.
- div로 감싼 것을 넘겨주면 해당 div 안에 있는 내용들이 3번째 인자에 나타나는데 그 내용들을 또다시 `React.createElement`로 쪼개줌

- 질문. 프리셋이 뭐지?. 바벨의 프리셋을 리액트로 변경하면 `react/jsx-runtime`이라고 들어가면서 내용이 좀 바뀌는데. 이 플러그인 같은 경우에는 우리가 리액트 프로젝트 세팅할 때 extends로도 넣어줬던 내용임. 이걸 왜 넣는지, 뭐가 바뀌는건지 좀 더 찾아볼 것 -> 강의에서는 이게 요즘(?) JSX이긴 하다고 함...

- 강의 26:50 까지의 내용 정리.

---

## 키워드 정리

### React에서 JSX를 사용하는 목적

- JSX를 사용하지 않고 자바스크립트 코드로도 작성할 수 있음. JSX는 문법적 확장이기 때문에 일정 변환 절차를 거쳐 자바스크립트 코드로 1:1 대응을 시킬 수 있기 때문
- 공식문서에서는 UI가 어떻게 생겨야 하는지를 설명하기 위해 리액트와 사용할 것을 권장하고 있음
- 대체로 JS 코드로 UI 작업을 할 떄보다 JSX로 작업할 때 시각적으로 더 도움이 된다고 함
- 또한 반복적으로 React.createElement() 호출을 하지 않아도 됨

</br>

### Syntactic sugar

- 더 쉽게 읽거나 표현할 수 있도록 설계된 프로그래밍 언어 내의 구문
- 예. 자바스크립트의 `for...of` loop은 for룹과 동일한 기능! -> 자바에서의 for문도 enhance for loop으로 변경 가능할 때가 있는데 이 역시 syntactic sugar로 이해할 수 있을까?
- descructuring
