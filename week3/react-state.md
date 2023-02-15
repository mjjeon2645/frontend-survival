# 2. React State

## 강의 요약 및 노트

- __step 3. Find the minimal but complete representation of UI state__
    - 작지만 최소한의 것(minimal), 하지만 '완벽한' UI 상태의 표현을 찾기

- __step 4. Identify where your state should live__
    - 그리고 그 상태가 '어디에' 있게 할 것인지를 명확하게 결정

- __step 5. Add inverse data flow__
    - data flow를 역으로 더하기

### React의 State

- 오해, 또는 성급히 일반화 하지 말 것. 과연 '모든' 데이터는 state를 가지는가? 데이터라고 해서 항상 state를 가지는 것이 아님. state는 리액트에서 '변경'을 다루는 요소인 것이지 모든 데이터가 state를 가진다고 볼 수는 없음
- 상태가 변화하면 해당 컴포넌트와 그 하위 컴포넌트를 re-rendering함
- 아무렇게나 만들어도 되겠지만(ㅎㅎ) 리액트에서는 효율성, 일관성을 위하여 DRY(Don't Repeat Yourself) 원칙을 따르는 SSOT를 만듦

### React State의 조건

- 변경돼야 함. (ㅇ ㅏ...) 변경되지 않는 것은 state로 다룰 가치가 없음 -> 만약 모든 '데이터'가 state를 가지고 있다고 오해한다면 변경되지 않는 것 조차 state처럼 다루게 됨. state로서의 가치를 가지지 않는 일반적인 data를 마치 state처럼 다루는 이상한 상황 발생
- 부모 컴포넌트가 props를 통해 전달한다면 state가 아님 -> 무슨 소리..?
- 다른 state나 props를 이용해 계싼 가능하다면 state가 아님 -> 읭?

=> 위 내용에서 2, 3번 잘 이해가 안가는데 특히 3번 같은 경우에는 useEffect()를 쓸 일이 많지 않을거란 내용의 공식문서가 있으니 참고해볼 것

</br>

### 찾아보기*****!!

- 강의 18분부터 나온 내용의 event 타입 잡아주는 방법 왜 그런지 알아보기. event가 일어난 건 알겠는데 어디서 일어난거야? 라는 부분이 명확하지 않아서 계속 `value`에 에러가 떴었는데 `ChangeEvent<HTMLInputElement>`로 명확하게 잡아주니 해결됨

```typescript
const handleChange = (event: ChangeEvent<HTMLInputElement>) => {
    const { value } = event.target;
    setFilterText(value);
};
```

---

## 키워드 정리

### React state란?

</br>

### DRY 원칙

</br>

### SSOT(Single Source of Truth)

</br>

### useState

</br>

### 1급 객체(first-class object)란?

</br>

### Lifting State Up
