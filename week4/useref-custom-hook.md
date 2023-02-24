# 4. useRef & Custom Hook

## 강의 요약 및 노트

- useRef를 사용하면 컴포넌트 생애주기 전체에 걸쳐서 동일한 객체가 유지된다.
- 객체 자체가 값은 아니고, 값을 참조하기 위한 객체. 즉 언제든 값을 변경할 수 있다.
- 주요 용도
    - 컴포넌트가 사라질 때까지 동일한 값을 사용해야 하는 경우 -> input 등의 ID를 관리할 때
    - (특히 useEffect 등과 함께 쓰면서 만나게 되는) 비동기 상황에서 현재 값을 제대로 쓰고 싶은 경우
---

## 키워드 정리

### useRef

- [useRef 정리본 바로가기](./react-hook.md/#4-useref)

</br>

### Hook의 규칙

- [react-hook이란 바로가기](./react-hook.md/#react-hook)
