# 2. Style Basics

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



## 강의 요약 및 노트

<hr />

## 키워드 정리

### className

</br>

### 의미있는 마크업
