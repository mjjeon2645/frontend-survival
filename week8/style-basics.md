# 2. Style Basics

## 강의 요약 및 노트

- `id`로 선택자를 지정할 경우 오로지 하나만 존재해야 함. 물론 여러개가 있어도 브라우저 상에서 눈으로 볼 때 큰 문제가 생기지는 않음. -> 강의를 들으며 간과하고 있었던 부분, 즉 새롭게 알게된 부분은 우리가 컴포넌트를 재사용하거나 또는 반복하여 사용할 경우 id는 중복 사용될 가능성이 높아진다는 점.
- `class`가 `className`으로 자동 변경되는 이유는 자바스크립트에서 `class`가 이미 예약어이기 때문
- inline style -> 어찌 보면 CSS를 자바스크립트 객체로 표현

```js
export default function Greeting() {
  const style = {
    color: '#00F',
  };

  return (
    <p style={style}>
      Hello, world!
    </p>
  );
}
```

```js
export default function Greeting() {
  return (
    <p
      style={{
        color: '#00F',
      }}
    >
      Hello, world!
    </p>
  );
}
```

<hr />

## 키워드 정리

### className

- JSX 문법. 자바스크립트에서 사용하는 예약어 중 `class`가 존재하므로 `className`으로 대체하여 사용
- 원래 리액트 환경에서 JSX 문법을 사용하지 않는다면 `React.createElement('div', null)`과 같은 형태로 컴포넌트를 생성해야 하나 JSX 문법적 설탕으로 마치 html을 작성하듯 편리하게 사용해옴
- `className`은 특정 엘리먼트의 클래스 속성의 값을 가져오거나 설정할 수도 있음(프로퍼티 명으로서의 className)

- 참고
  - [MDN Element.className](https://developer.mozilla.org/ko/docs/Web/API/Element/className)
  - [코딩애플-리액트에서 레이아웃 만들 때 쓰는 JSX 문법 3개](https://codingapple.com/unit/react2-jsx-classname-html/)

</br>

### 의미있는 마크업

- [내 노션페이지에 정리해 둔 것-의미있는 마크업](https://www.notion.so/aa346b19649845a689ef625d2b30c808?pvs=4#da0b156ee21d41e48f3aab8a28aa60c4)
