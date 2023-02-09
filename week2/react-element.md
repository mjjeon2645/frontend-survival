# 2. React Element

## 강의 요약 및 노트

- JSX는? React에 있는 createElement를 쓰도록 코드를 바꿔주는 것?!
- 거듭 이야기하지만 JSX는 필수가 아님. JSX는 빌드환경에서 컴파일 설정을 해야 변환이 되어서 자바스크립트 코드처럼 되는 것이기 때문에 이 부분이 귀찮다면 JSX를 사용하지 않고도 React를 사용할 수 있음
- JSX로 할 수 있는 모든 것은 순수 JS로도 할 수 있음 -> 강의 초반에 이야기했듯이 JSX와 JS 코드는 1:1로 매칭된다!!!
- `createElement`는? `createElement` lets you create a **React element**. 즉 createElement는 React element를 만들어낸다.

- 참고. react 17에서 바뀐 점이 있다면 JSX 변환 모습. (강의 초반부에서 babel의 플러그인을 classic에서 다른 것으로 바꿨을 때 나온 형태)

---

## 키워드 정리

### React.createElement

- 호출의 기본 형태는 아래와 같음

```javascript
React.createElement(component, props, ...children)
```

- 해당 호출을 통해 react element를 생성할 수 있음 -> React Element 트리를 갱신하는 데 쓸 수 있음

</br>

### React Element

- React 애플리케이션을 구성하는 블록이라고 볼 수 있음
- 컴포넌트라는 개념과는 혼동하기 쉽지만 구분해야 함!
- 엘리먼트는 화면에 보이는 것들을 기술하며, React Element는 변경되지 않음
- 또한 일반적으로 엘리먼트는 직접 사용되지 않고 컴포넌트로부터 반환됨

</br>

### React StrictMode

- Fragment와 같이 UI를 렌더링하지 않으며, 자손들에 대한 부가적인 검사, 경고를 활성화 함
- 개발모드에만 활성화 되므로 배포 빌드에 영향 없음
- [공식문서 상](https://ko.reactjs.org/docs/strict-mode.html) StrictMode의 도움 영역은 아래와 같음
    - 안전하지 않은 생명주기를 사용하는 컴포넌트 발견
    - 레거시 문자열 ref 사용에 대한 경고
    - 권장되지 않는 findDOMNode 사용에 대한 경고
    - 예상치 못한 부작용 검사
    - 레거시 context API 검사
    - Ensuring reusable state(react 18에서 추가된 부분인듯 -> 개발 전용 검사). 개발 모드에서 의도적으로 마운팅을 두 번 일으켜 두 번 모두 문제 없이 동작해야 버그가 없다고 인식
