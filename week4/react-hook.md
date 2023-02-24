# 3. React의 Hook

## 강의 요약 및 노트

### useEffect

- 어떤 컴포넌트는 외부 시스템과 싱크로나이즈, 즉 '동기화'를 해주어야 할 필요가 생긴다. -> 하지만 공식 문서에서는 '정말로 외부와 싱크로나이즈 하는 것이 아니라면 `useEffect` 쓰지 마라'고 하고 있다. > 이 얘기인 즉슨, 정말로 useEffect를 써야 하는 상황을 강조하는 것임
- useEffect는 거칠게 이야기하면 '렌더링 이후 해야 할 일, 즉 React의 외부와 관련된 일을 정해줄 수 있'다.
- 기본적으로는 렌더링 때마다 실행됨
- 강의에서 예시로 든 title 변경을 생각해보자. 사실 아래와 같이 변경해도 문제가 있는 것은 아니다.

```javascript
document.title = 'xxx';
```

- 하지만 react의 외부에 접근할 때는 useEffect를 사용하는 것이 '우아하게(?)' 접근하는 것이므로 이렇게 쓰는 습관을 들이도록 한다.

```javascript
useEffect(() => {
    document.title = 'xxx';
});
```

- 처음에 한 번만 실행하도록 할 때는 의존성 배열을 빈 배열로 넣어주면 된다. 

```javascript
const [products, setProducts] = useState<Product[]>([]);

useEffect(() => {
	const fetchProducts = async () => {
		const url = 'http://localhost:3000/products';
		const response = await fetch(url);
		const data = await response.json();
		setProducts(data.products);
	};

	fetchProducts();
}, []);
```

- 위 코드는 즉시실행 함수 형태로 아래와 같이 동일하게 사용할 수 있다.

```javascript
const [products, setProducts] = useState<Product[]>([]);

useEffect(() => {
    (async () => {
        const url = 'http://localhost:3000/products';
        const response = await fetch(url);
        const data = await response.json();
        setProducts(data.products);
    })();
}, []);
```

### React.StrictMode

- 개발 모드에서는 두 번 작동. 프로덕션 모드에서는 한번만 작동함. 라이프사이클에 영향을 받는 부분들에 대해서는 두 번 작동하여 비교함. 예상치 못한 부작용을 검사함. 아샬님은 이 모드를 사용할 것을 추천


** 강의 들으면서 이해 안갔던 것 두 군데. (1) 왜 useEffect 안에 return을 넣고 콜백함수를 넣었을 때 클릭하면 title 변동&타이머가 멈추는지, (2) 의존성 배열을 이용해 fetch 할 때 주의사항 내용 전부

---

## 키워드 정리

### React Hook이란

- Hook은 React 16.8부터 새로 도입된 요소로 기존 Class 바탕의 코드 작성 필요 없이 상태값과 여러 리액트 기능을 사용할 수 있음
- hook은 리액트 컨셉을 대체하는 것이 아니라 props, state, context, refs, lifecycle과 같은 리액트 개념에 좀 더 직관적인 API를 제공하는 것
- hook을 사용하면 컴포넌트로부터 상태 관련 로직을 추상화할 수 있어 독립적인 테스트가 가능하고 컴포넌트 재사용이 가능. 결론적으로 **hook은 계층 변화 없이 상태 관련 로직을 재사용할 수 있도록 도움**
- 또한 hook을 통해 class 없이 react 기능들을 사용할 수 있음
- hook을 사용할 때는 아래 사용 규칙을 지켜야 함
    - 최상위(at the top level)에서만 hook을 호출해야 함. 반복문, 조건문, 중첩된 함수 내에서 hook을 실행하지 말 것
        - 이유: 이 규칙을 따라야 컴포넌트가 렌더링 될 때마다 항상 동일한 순서로 hook이 호출되는 것이 보장됨. 또한 useState, useEffect가 여러번 호출되는 중에도 hook의 상태를 올바르게 유지할 수 있음
    - react 함수 컴포넌트 내에서만 hook을 호출해야 함(일반 JS 함수에서 hook 호출 X). 다만 custom hook 내에서는 hook 호출 가능
        - 이유: 이 규칙을 지킬 때 모든 상태 관련 로직을 소스코드에서 명확하게 보이도록 할 수 있음

- 참고
    - [리액트 공식문서: hook의 개요](https://ko.reactjs.org/docs/hooks-intro.html)
    - [리액트 공식문서: hook의 규칙](https://ko.reactjs.org/docs/hooks-rules.html)

</br>

### Hooks

1. useState

- test

2. useEffect
- test

3. useContext

4. useRef

5. useLayoutEffect

</br>

### React StrictMode란
