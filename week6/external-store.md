# 1. External Store

## 강의 요약 및 노트

### 관심사의 분리(Separation of Concerns)

- 기능적으로도 분류할 수 있을 것이고 분류의 기준은 여러가지가 될 수 있을 것
- 흔히 사용되는 Layered Architecture에서는 사용자에게 가까운 것-먼 것으로 구분(가까운 것 순으로 UI - Business Logic - 데이터 접근과 저장)
- 아키텍처의 관점 까지는 아니겠으나 프로세스에 관점에서도, 단계를 'Input - Process - Output' 3단계로 적절하게 구분만 하더라도 코드를 이해하고 유지보수 하는데 큰 도움이 됨
- 널리 알려진 MVC로 거칠게 매핑하면 아래와 같음
    - Model -> Process
    - View -> Output
    - Controller -> Input

** MVC에 대해 정확히 확인하기

### Flux

- MVC의 대안으로 페이스북이 발표

### External Store란?

- 리액트가 아닌, 리액트의 바깥. -> 스토어가 리액트의 안에 있지 않다는 의미
- 전체적인 아키텍처 관점에서 보았을 때 '리액트' 자체는 가장 바깥임. core가 아니라 가장 바깥의 view에 속하기 떄문. 따라서 여기서 external의 의미란 안이냐 바깥이냐가 아니라, 리액트 안에 속헤있지 않다는 의미(아키텍처의 관점이 아닌 리액트의 입장에서!)
- 리액트에서 상태를 관리할 때 `useState`를 사용했었는데 이제는 그게 아닌 external store로 상태를 관리할 것. 따라서 상태가 업데이트 되었을 때 별도의 조치가 없다면 변경된 state가 view에 반영되지 않음. (교재에 있는 관련문서 찾아보기)

- `useReducer`가 기본형이고 `setState`가 내부적으로 `useReducer`를 사용한다. (그래서 setState를 사용했을 때 리렌더링이 되는거!)

** 다시 이해해야 할 곳.

- 강의 28분의 괄호 안에 객체 넣는 부분
- 29분 부터. 항상 같은 함수? 이게 무슨소린지.. useCallback 뭔얘긴지 모르겠음.

---

## 키워드 정리

### 관심사의 분리

- Separation of Concerns. SoC. 컴퓨터 프로그램을 구별된 부분으로 분리시키는 디자인 원칙으로 각 부문은 개개의 관심사를 해결
- 관심사란? -> 컴퓨터 프로그램 코드에 영향을 미치는 정보의 집합
- 관심사의 분리는 정보를 잘 정의된 인터페이스가 있는 코드 부분 안에 '캡슐화' 시킴으로써 달성함
- 특징
    - 관심사 분리를 이용하면 프로그램의 설계, 디플로이, 이용의 일부 관점에 더 높은 정도의 자유가 생김
    - 독립적인 개발, 업그레이드 외에도 모듈 재사용을 위한 더 높은 정도의 자유가 있음
    - 모듈이 인터페이스 뒤에서 관심사의 세세한 부분을 숨기므로 다른 부분의 상세를 모르더라도, 또 해당 부분들에 상응하는 변경을 하지 않더라도 하나의 관심사의 코드 부분을 개선하거나 수정이 가능함
- 관심사의 분리는 추상화의 일종임

- 참고
    - [위키백과: 관심사 분리](https://ko.wikipedia.org/wiki/%EA%B4%80%EC%8B%AC%EC%82%AC_%EB%B6%84%EB%A6%AC)


</br>

### Layered Architecture

- 가장 일반적인 아키텍처 패턴으로 n-tier, n계층 아키텍처 패턴이라고도 함
- 대부분의 회사에서 볼 수 있는 전통적인 IT 커뮤니케이션 및 조직구조와 거의 일치
- 대부분의 레이어드 아키텍처는 네 가지 표준 계층으로 구성됨(presentation, business, persistence, database) - 경우에 따라 business 계층과 persistence 계층이 단일 business 계층으로 결합되기도 함
- 각 계층은 애플리케이션 내에서의 특정 역할과 책임이 있음. 또한 아키텍처의 각 계층은 특정 비즈니스 요청을 충족하기 위해 수행해야 하는 작업을 중심으로 추상화를 형성함
- 가장 강력한 특징은 **컴포넌트 간의 관심사를 분리**하는 것. 특정 계층 내의 컴포넌트는 해당 계층과 관련된 로직만 처리함. 이로 인해 잘 정의된 컴포넌트 인터페이스와 제한된 컴포넌트 범위로 애플리케이션을 쉽게 개발, 테스트, 관리, 유지할 수 있음
- 주요 컨셉
    - **폐쇄형 레이어(closed layer)**. 어떤 요청(request)이 레이어에서 이동할 때 그 바로 아래 레이어를 통과해야만 다음 레이어로 이동할 수 있음. 왜 굳이 레이어를 단계별로 거쳐야 하는가? 이는 **layers of isolation**. 즉 계층 격리와 관련있음. -> 아키텍처의 한 계층에서 변경한 내용이 다른 계층의 요소들에 영향을 미치지 않고 해당 계층 내의 요소와 SQL을 포함하는 다른 계층에만 제한적으로 적용된다는 것을 의미함. 레이어가 다른 요소들 간의 상호 의존성이 높아질 경우 결합도가 높아지며 이런 유형의 아키텍쳐는 변경이 어렵고 비용도 많이 들어감
    - 계층 격리에 따라 각각의 계층은 서로의 내부 작동에 대한 지식이 거의, 또는 전혀 없어야 함. 
    - 때로는 특정 계층을 개방하는 것이 합당한 경우도 있음. 

- 주의점
    - 싱크홀 방지 패턴: 요청이 아키텍처의 여러 계층을 통과하면서 각 계층 내에서 로직이 거의, 전혀 수행되지 않고 단순 통과 처리되는 상황
    - 프레젠테이션, 비즈니스 계층을 별도의 배포 가능한 단위로 분할하더라도 모놀리식 애플리케이션에 적합한 경향

- 참고
    - [OREILLY S/W Architecture Patterns](https://www.oreilly.com/library/view/software-architecture-patterns/9781491971437/ch01.html)
    - [블로그 글 - 계층화 아키텍처](https://hudi.blog/layered-architecture/)

</br>

### Flux Architecture

- Facebook에서 발표했으며 클라이언트 사이드 웹 어플리케이션을 만들기 위해 사용하는 아키텍처. 단방향 데이터 흐름을 활용해 리액트를 보완하는 역할을 함
- Model-View-Controller와 혼동에 유의할 것. flux는 MVC와 다르게 단방향으로 데이터가 흐름
    - 사용자가 리액트 뷰와 상호작용
    - 뷰는 중앙의 dispatcher를 통해 애플리케이션의 데이터, 비즈니스 로직을 보유한 다양한 store로 액션을 전파
    - store는 액션에 영향이 있는 모든 뷰를 갱신시킴 -> 리액트의 '선언형 프로그래밍' 스타일로 인해 뷰가 어떤 방식으로 갱신해야 하는지를 일일이 작성하지 않고서도 데이터 변경이 가능함

- 아래의 핵심적인 세 가지 부분으로 구성되어 있음
    - Dispatcher
    - Stores
    - Views(React 컴포넌트)
- flux 어플리케이션에도 Controller는 존재하지만 위계의 최상위에서 controller-views - views 관계로 존재하고 있음
    - controller-views: store에서 데이터를 가져와 그 데이터를 자식에게 보내는 역할
    - action creator 메소드: dispatcher 헬퍼 메서드. 애플리케이션에서 가능한 모든 변경을 표현하는 시맨틱 API를 지원하는데 사용됨. flux 업데이트 주기의 4번째 부분

- 참고하기
    - [flux 공식](https://facebook.github.io/flux/docs/in-depth-overview/)
    - [flux 번역](https://haruair.github.io/flux/docs/overview.html#content)

</br>

### useReducer

- (베타버전 문서) 컴포넌트에 'reducer'를 추가하도록 하는 리액트 훅
    - reducer란? 많은 상태 업데이트가 여러 이벤트 핸들러에 분산되어 있는 컴포넌트는 과부하가 걸릴 수 있음. 이럴 경우 컴포넌트 외부의 모든 상태 업데이트 로직을 'reducer'라고 하는 단일의 함수로 통합할 수 있음
- `useState`의 대체함수로 `(state, action) => newState`의 형태로 reducer를 받고 `dispatch` 메서드와 짝의 형태로 현재 state를 반환함
- `const [state, dispatch] = useReducer(reducer, initialArg, init?);`
    - reducer: 상태가 업데이트 되는 방식을 지정. 순수함수여야 하며 상태와 액션을 인자로 받고 다음 상태를 반환해야 함
    - initialArg: 초기 상태
    - init(옵셔널): 초기 함수. 지정하지 않으면 초기 상태가 initialArg로 설정되며, 그렇지 않으면 초기 상태는 init을 호출한 결과로 설정

- 참고하기
    - [리액트 베타버전](https://beta.reactjs.org/reference/react/useReducer)
    - [리액트 extracting state logic into a reducer](https://beta.reactjs.org/learn/extracting-state-logic-into-a-reducer)
    - [리액트 과거 버전](https://ko.reactjs.org/docs/hooks-reference.html#usereducer)
    - [블로그-React Hooks: useReducer 사용법](https://www.daleseo.com/react-hooks-use-reducer/)

</br>

### useCallback

- 리렌더링 사이에서 함수 정의를 캐시(cache)할 수 있는 리액트 훅
- `const cachedFn = useCallback(fn, dependencies)`
    - 함수를 메모이제이션(memoization) 하기 위해 사용되는 hook. 첫 번째 인자로 넘어온 함수를, 두 번째 인자로 넘어온 배열 내의 값이 변경될 때까지 저장해놓고 재사용할 수 있게 해 줌

- 참고하기
     - [리액트 베타문서-useCallBack](https://beta.reactjs.org/reference/react/useCallback)
     - [블로그 React hooks: useCallback 사용법](https://www.daleseo.com/react-hooks-use-callback/)
