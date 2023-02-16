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

- state는 공식 문서에서 'A component's Memory'로 정의하고 있다. 컴포넌트는 interaction의 결과로 screen을 변경해야 할 필요가 있다. 예를 들어 input field에 텍스트를 입력한다거나, '다음' 버튼을 누른다던가. 이에따라 컴포넌트는 현재의 input 값이나 이미지와 같은 것들을 '기억해야'하는데 이러한 컴포넌트 특정 메모리를 state라고 한다.
- props는 함수 매개변수 처럼 컴포넌트에 전달되는 반면 state는 함수 내에 선언된 변수처럼 컴포넌트 안에서 관리된다는 차이점이 있다.

</br>

### DRY 원칙

- 모든 형태의 정보 중복을 지양하는 원리. '모든 지식은 시스템 내에서 유일하고 중복이 없으며 권이있는 표상만을 가진다'는 말로 기술할 수 있음
- [참고 글](https://thebook.io/006844/part01/ch01/01/03/07/)에서는 SOLID의 원칙 중 일부가 DRY 원칙의 필연적 산물이라고 함. X, Y를 함께 하는 모듈이 있을 때 X 코드가 필요하나 Y 로직을 들어내지 않는 한 모듈 재사용이 불가하여 결국 또다시 X를 코딩할 수 밖에 없는 상황을 맞닥뜨림. 이는 DRY 원칙 위배. 결론적으론 DRY 한 코드를 만드는 과정에 의존성 주입과 단일 책임 문제가 개입됨

</br>

### SSOT(Single Source of Truth)

- 단일 진실 공급원
- [드롭박스 홈페이지 참조](https://experience.dropbox.com/ko-kr/resources/source-of-truth). 정보 시스템 설계 및 이론 중 하나. 모든 비즈니스 데이터를 하나의 공간에 저장하는 것을 말함.
- 비즈니스 데이터를 하나의 공간에 저장하면 회사 내 누구나 이러한 접근 가능 데이터를 바탕으로 중요한 비즈니스 의사 결정을 내릴 수 있을 것이라는 아이디어를 근간으로 함

</br>

### useState

- `useState`는 state를 함수 컴포넌트 안에서 사용할 수 있게 해주는 hook. 컴포넌트에 상태 변수를 추가할 수 있게 해준다.
- hook은? -> React 16.8에 새로 추가된 기능으로 class를 작성하지 않고도(함수형 컴포넌트에서) state와 다른 react의 기능들을 사용할 수 있게 해줌
    - 최상위(at the top level)에서만 hook을 호출해야 함 -> 컴포넌트가 렌더링 될 때마다 항상 동일한 순서로 hook이 호출되는 것을 보장하기 위해서
    - react 함수 내에서 hook을 호출해야 함. 이를 통해 컴포넌트의 모든 상태 관련 로직을 소스코드에서 명확히 볼 수 있음
- 파라미터로 `initialState`. 상태의 초기값. 만약 initialState로 function을 전달하면 initializer function으로 취급된다. pure, 즉 순수함수어야 하고 arguments를 갖지 않으며 어떠한(any) 타입의 값을 return 해야 한다.
- 두 값을 가진 array를 리턴한다.
    - 현재의 state
    - set 함수. state를 다른 값으로 업데이트 하면서 동시에 re-render를 일으킨다.

</br>

### 1급 객체(first-class object)란?

- 다른 객체들에 일반적으로 적용 가능한 연산을 모두 지원하는 객체. 보통 함수에 인자로 넘기고, 수정하고, 변수에 대입하는 것과 같은 연산을 지원할 때 일급 객체라고 함

</br>

### Lifting State Up

- 동일한 데이터에 대한 변경사항을 여러 컴포넌트에 반영해야 할 때, 가장 가까운 공통 조상으로 state를 끌어올리는 것 -> 즉 이를 통해 state를 공유하고 값이 서로 동기화된 상태를 유지할 수 있음 -> 상단 컴포넌트에서 공유될 state를 소유하고 있으면 이 컴포넌트는 source of truth가 됨
