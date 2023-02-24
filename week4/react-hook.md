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

### [React Hook이란](#react-hook)

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

- 공식문서에서 hook을 간략하게 나눈 기준은 기본/추가/library
    - 기본: useState, useEffect, useContext
    - 추가: useReducer, useCallback, useMemo, useRef, useImperativeHandle, useLayoutEffect, useDebugValue, useDeferredValue, useTransition, useId
    - library: useSyncExternalStore, useInsertionEffect

#### 1. useState

- useState는 컴포넌트에서 상태 변수를 사용할 수 있도록 한다. 상태 유지 값과 그 값을 갱신하는 함수를 반환하며 기본 사용형태는 아래와 같다.

```javascript
const [state, setState] = useState(initialState);
```

- 파라미터인 initialState는 처음에 상태로 설정할 값을 의미한다. 모든 유형의 값이 들어갈 수 있으며 심지어 function도 초기 상태값으로 지정할 수 있다. 이 파라미터는 초기 렌더링 이후 무시된다.
    - 함수를 initialState로 전달할 경우 '초기화 함수(initializer function)'로 취급된다. 해당 함수는 순수해야 하고, 인자를 받지 않고, 어떠한 타입의 값이라도 반환해야 한다.
    - 리액트가 컴포넌트를 초기화 할 때 이 '초기화 함수'를 호출하고, 그 리턴값을 초기 상태로 저장한다.
- state는 현재 상태이다. 최초 렌더링 시에는 useState의 파라미터인 initialState와 일치한다.
- set 함수는 state를 다른 값으로 업데이트 하고 re-render, 즉 리렌더링을 트리거한다.

- 참고하기
    - [Hooks API Reference](https://ko.reactjs.org/docs/hooks-reference.html#usestate)
    - [useState in React Docs beta ver.](https://beta.reactjs.org/reference/react/useState)

#### 2. useEffect

- useEffect는 컴포넌트와 외부 시스템을 동기화해준다. 컴포넌트의 탑 레벨에서 선언되어야 하며, 기본 사용 형태는 아래와 같다.

```javascript
useEffect(setup, dependencies?);
```

- setup 파라미터는 effect의 로직이 포함된 '함수'이다. 옵셔널로 'cleanup 함수'를 리턴할 수 있다. 컴포넌트가 DOM에 처음 추가됐을 때 리액트가 setup 함수를 실행한다. dependencies(의존성 배열)의 변경에 따른 매 회 rerender 후에, cleanup 함수가 제공된 경우 리액트는 이전 value로 cleanup 함수를 실행하고 새로운 value로 setup 함수를 실행한다. 컴포넌트가 DOM에서 제거되고 난 후 리액트가 마지막으로 cleanup 함수를 실행한다.
- dependencies 파라미터는 옵셔널이다. setup 코드 내부에서 참조된 모든 반응형 values들(reactive values)의 목록이다. 이 reactive values들은 props, state 뿐만 아니라 컴포넌트 안에서 선언된 모든 변수들과 function을 포함한다. 만약 단 한번 실행하기 원한다면 의존성에 빈 배열을 전달하면 된다.

- 참고하기
    - [리액트 공식문서: useEffect](https://ko.reactjs.org/docs/hooks-reference.html#useeffect)
    - [리액트 공식문서 useEffect beta ver.](https://beta.reactjs.org/reference/react/useEffect)
    - [Synchronizing with Effects](https://beta.reactjs.org/learn/synchronizing-with-effects)
    - [You Might Not Need an Effect](https://beta.reactjs.org/learn/you-might-not-need-an-effect)
    - [useEffect 완벽 가이드](https://overreacted.io/ko/a-complete-guide-to-useeffect/)

#### 3. useContext

- useContext는 컴포넌트를 중첩하지 않고도 React context를 구독할 수 있게 한다. 
    - context는 props 전달의 대안이다. 보통 상위 컴포넌트에서 하위 컴포넌트로 정보 전달 시 props를 전달하는데, 중간에 여러 컴포넌트를 거쳐야 하거나 앱의 여러 컴포넌트가 같은 정보를 필요로 한다면 props로 내려주는 것이 너무 장황하고 불편할 수 있다. context를 사용하면 부모 컴포넌트가 props를 통해 명시적으로 전달하지 않고도 하위 트리에 있는 모든 컴포넌트가 일부 정보를 사용할 수 있다. (약간 텔레포트 같은 느낌..?)

```javascript
const value = useContext(SomeContext);
```

- SomeContext 파라미터는 이전에 `createContext`로 생성한 컨텍스트를 나타낸다. 컨텍스트 자체는 정보를 갖지 않으며, 컴포넌트엥서 제공하거나 읽는 정보의 종류를 나타낸다.

- 참고하기
    - [리액트 공식문서: 기타 hook](https://ko.reactjs.org/docs/hooks-overview.html#other-hooks)
    - [리액트 공식문서: useContext](https://ko.reactjs.org/docs/hooks-reference.html#usecontext)
    - [useContext 공식문서 베타버전](https://beta.reactjs.org/reference/react/useContext)
    - [Passing Data Deeply with Context](https://beta.reactjs.org/learn/passing-data-deeply-with-context)

#### [4. useRef](#4-useref)

- useRef는 `.current` 프로퍼티로 전달된 인자(initialValue)로 초기화된 변경 가능한 ref 객체를 반환한다. 반환된 객체는 컴포넌트의 전 생애주기를 통해 유지되는데, 본질적으로 useRef는 .current 프로퍼티에 변경 가능한 값을 담고 있는 '상자'와 같다. 
    - `ref`는 컴포넌트가 특정 정보를 '기억'하게는 하지만 해당 정보가 새로운 렌더링을 트리거하지 않도록 할 때 사용한다. (참고. [referencing values with Refs](https://beta.reactjs.org/learn/referencing-values-with-refs))

```javascript
const ref = useRef(initialValue);
```

- 만약 initialValue에 0을 넣었을 경우 useRef는 아래와 같은 객체를 반환한다.

```javascript
const ref = useRef(0);

// useRef가 반환하는 객체의 형태
{
    current: 0 // useRef에 전달한 값
}
```

- 참고하기
    - [리액트 공식문서: useRef](https://ko.reactjs.org/docs/hooks-reference.html#useref)
    - [useRef 공식문서 베타버전](https://beta.reactjs.org/reference/react/useRef)

#### 5. useLayoutEffect

- 가급적 `useEffect`를 먼저 사용하도록. useLayoutEffect는 성능을 저하시킬 수 있다. useLayoutEffect는 브라우저가 화면을 repaint하기 전에 실행되는 useEffect 버전이다. 리턴값은 `undefined`이다.

```javascript
useLayoutEffect(setup, dependencies?);
```

- 파라미터 각각에 대한 설명은 useEffect와 동일하다.

- 참고하기
    - [리액트 공식문서: useLayoutEffect](https://ko.reactjs.org/docs/hooks-reference.html#uselayouteffect)
    - [useLayoutEffect 공식문서 베타버전](https://beta.reactjs.org/reference/react/useLayoutEffect)

</br>

### React StrictMode란

- 개발 중 컴포넌트에서 흔히 발생하는 버그를 빠르게 발견할 수 있으며 사용 형태는 아래와 같다.

```javascript
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'));
root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```
- Strict Mode는 아래와 같은 사항으로 점검을 수행하며 개발 전용이므로 프로덕션 빌드에 전혀 영향을 미치지 않는다.
    - 불완전한 렌더링으로 인한 버그를 찾고자 추가로 리렌더링을 진행
    - efflect cleanup이 누락되어 발생하는 버그를 찾기 위해 effects를 추가로 실행
    - deprecated된 API를 사용하는지 체크

- 참고하기
    - [리액트 공식문서: Strict 모드](https://ko.reactjs.org/docs/strict-mode.html)
    - [Strict 모드 공식문서 베타버전](https://beta.reactjs.org/reference/react/StrictMode)
