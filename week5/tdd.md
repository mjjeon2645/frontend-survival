# 1. TDD

## 강의 요약 및 노트

### TDD에 대해

- 테스트코드를 먼저 작성해야 함
- TDD Cycle
    - Red 에서는 실패하는 테스트 코드를 작성하며 **인터페이스**와 **스펙**에 집중한다.
    - Green. 빠르게 테스트를 통과시키고 올바른 방법이 아니어도 괜찮음. 때로는 스텁(stub)이라고 불리는 것들로 통과시키기도 함(stub: 테스트 중인 모듈이 의존하는 소프트웨어 구성요소의 동작을 시뮬레이션 하는 프로그램). 또는 막무가내로 brute force로도 통과.
    - Refactor. 리팩터링을 통해 코드를 바르게 만듦. 클린코드의 지향점은 '동작'은 바뀌지 않되 '설계'는 바뀌는 것. 즉 테스트를 작성함으로써 테스트가 터지지 않는 범위 내에서 리팩터링을 진행

### Jest에 대해

- 제스트가 영향을 받은 모카 역시도 RSpec의 영향을 받았음
- test 파일 생성 시 `파일 명.test.ts` 또는 BDD 스타일로 테스트 시 `파일 명.spec.ts`로 이름을 잡아주면 되는데 아샬님의 경우 스타일과는 관계 없이 `파일 명.test.ts`로 통일해서 사용
- ** 추가로 조사해볼 것. BDD 스타일이란 무엇을 의미하는건지.

### describe-context-it

- describe에 들어가는 것은 '주어'가 됨
- it에 들어가는 내용은 자연스럽게 그 상위 내용과 붙어서 서술을 해줌. ~~을 한다. 라는 식으로
- context는 describe이나 별도로 지원하지 않기 때문에 `const context = describe;`로 상위 선언 후에 사용 가능. 말 그대로 context이며 with, 또는 when 구문으로 시작됨

---

## 키워드 정리

### TDD란

- Test-driven developtment의 약자. 매우 짧은 개발주기(보통 몇 분에서 몇 시간)의 반복에 의존하는 개발 프로세스로서, Extreme Programming(XP)의 "Tdst-First" 개념에 기반을 둔 단순한 설계를 중요시한다. TDD를 가장 쉽게 표현한 것이 "Red-Green-Refactor" 개발 주기이다.
    - Red: 실패하는 테스트 코드를 먼저 작성
    - Green: 테스트를 통과시키기 위한 코드를 작성(범죄 수준의 것이라도 괜찮으니 우선은 통과시킬 것)
    - Refactor: 테스트 통과를 유지하면서, 생겨나는 모든 중복을 제거(refactoring이란 코드의 외적 행위는 그대로 유지하면서 내부 구조를 변경하는 작업을 의미. TDD에서는 '중복을 제거'하기 위해 리팩터링을 함)
- Red-Green-Refactor 개발 주기를 TDDBE에서 이야기하는 TDD 리듬으로 풀어낸다면 아래와 같다.
    1. 재빨리 테스트를 하나 추가
    2. 모든 테스트를 실행하고 새로 추가한 것이 실패하는지 확인
    3. 코드를 조금 바꿈
    4. 모든 테스트를 실행하고 전부 성공하는지 확인
    5. 리팩터링을 통해 중복을 제거
- TDD의 장점
    - **빠른 피드백**: 기능 단위로 진행하는 테스트로 인해 프로그래머의 손을 떠나기 전에 피드백을 받을 수 있음
    - **작성한 코드가 가지는 불안정성 개선 & 생산성 향상**: 코드가 사용자에게 도달하기 전 문제가 없는지를 먼저 진단받을 수 있으므로 코드가 지니는 불안정성, 불확실성을 지속적으로 해소
    - **프로그래머의 오버 엔지니어링 방지**: TDDBE에서 언급하는 원칙 중 하나. 즉 '자동화된 테스트가 실패할 경우에만 새로운 코드 작성'으로 인해 오버 코딩을 방지할 수 있음
    - **개발 과정이 테스트로 남기 때문에 과거의 인과관계, 의사결정 과정을 상기할 수 있음**: 테스트 작성 과정에서 히스토리가 남기 때문에 과거의 나 자신과 프로그래머가 협업하는 것을 용이하게 만들어줌
  
#### TDDBE 책에서 발췌한 내용

- 테스트 주도 개발(TDD)의 궁극적인 목표는 **작동하는 깔끔한 코드(clean code that works)**
- TDD가 따르는 단순한 규칙 2가지
    - 오직 자동화된 테스트가 실패할 경우에만 새로운 코드를 작성
    - 중복 제거

- 참고자료
    - [삼성SDS 홈페이지 인사이트 리포트 - 코드 품질을 높여주는 테스트 주도 개발 알아보기](https://www.samsungsds.com/kr/insights/test-driven-development.html)
    - TDDBE 책
    - [패스트캠퍼스 강의내용 일부 - TDD란? 테스트주도개발에 대한 편견과 실상, 방법론](https://media.fastcampus.co.kr/knowledge/dev/tdd/)

</br>

### Jest

- 페이스북에서 개발, 관리하는 자바스크립트 테스트 프레임워크. 단순함에 집중한다는 점을 표방함(Jest is a delightful JavaScript Testing Framework with a focus on **simplicity**.)
- 바벨, 타입스크립트, 노드, 리액트, 앵귤러, 뷰 등의 환경을 지원함

- 참고
    - [Jest 공식 홈페이지](https://jestjs.io/)

</br>

### Describe - Context - It 패턴

- BDD 테스트 코드 작성 패턴으로 코드의 행동을 설명하는 테스트 코드를 작성할 수 있다. 상황 설명 보다는 테스트 대상을 주인공 삼아 행동을 더 섬세하게 설명하는 데 적합하다.
    - 영어로 context 구문 작성 시 with, when으로 시작할 것
    - it 구문은 심플하게 설명할수록 좋음

| 키워드 | 설명 | 예시 |
|---|---|---|
| Describe | 설명할 테스트 대상 명시 | 테스트 대상이 되는 클래스, 메서드 이름 명시 |
| Context | 테스트 대상이 놓인 상황 설명 | 테스트할 메서드에 입력할 파라미터 설명 |
| It | 테스트 대상의 행동을 설명 | 테스트 대상 메서드가 무엇을 리턴하는지 설명 |

- 만약 한국어로 작성한다면 아래의 원칙을 참고하여 작성한다.
    - `Describe`는 테스트 대상을 명사로 작성
    - `Context`는 '~인 경우', '~할 때', '만약 ~ 하다면'과 같이 상황 또는 조건을 기술
    - `It`은 위에서 명사로 작성한 테스트 대상의 행동을 작성
        - 테스트 대상의 행동은 ~이다, ~한다, ~를 갖는다가 적절
        - ~된다 같은 수동형 표현은 피할 것

- 장점
    - 테스트 코드를 계층 구조로 만들어줌
    - 테스트 코드 추가, 또는 읽을 때 스코프 범위만 신경쓰면 됨
    - 빠뜨린 테스트 코드를 찾기 쉬움
    - 잘 읽히고 재미있음

- 참고
    - [기계인간 John Grib - JUnit5로 계층 구조의 테스트 코드 작성하기](https://johngrib.github.io/wiki/junit5-nested/)

</br>

### 단위테스트란

- 단위 테스트는 인수 테스트의 단점을 보완하는 검증 방식으로, 시스템의 일부(하위 시스템)를 대상으로 기능을 검증하는 테스트이며 특징은 아래와 같다.
    - **시스템의 일부(하위 시스템)을 대상으로 검증**: 전체 시스템을 배치해놓고 진행하지 않는다. 이에 따라 비용 역시 낮아진다.
    - **비용이 낮음**: 전체 시스템을 배치하여 진행하지 않으므로 작성, 관리, 실행에 대한 비용이 낮아진다.
    - **피드백 품질이 높음**: 인수 테스트와 비교했을 때 단위 안에서 버그가 발생한다는 점을 상대적으로 자세히 알 수 있다. 프로그래머 입장에서는 문제 해결을 위한 특정 부분의 피드백을 적절히 받을 수 있기 때문에 피드백 품질이 높다고 할 수 있다.
    - **전체 시스템 이상 여부에 대한 신뢰도는 낮음**: 단위 테스트에서 문제가 없더라도 전체 시스템이 유기적으로 연결될 때의 오류는 확인되지 않으므로 신뢰도가 낮다고 볼 수 있다.
  
- 단위 테스트의 정의가 위와 같다면... [아샬님 TIL](https://github.com/ahastudio/til/blob/main/blog/2016/12-03-tdd-faq.md)의 단위 테스트와 TDD 내용을 요약해보기
    - 단위 테스트란? '구현'에 대한 테스트
    - TDD는 자동화된 단위 테스트 코드를 먼저 작성함으로써 테스트가 개발을 이끌어나가도록 하는 방식
        -  자동화된 단위테스트 -> 항상 우리가 기대한 올바른 결과가 나오는 것을 전제로 함
        - TDD는 인터페이스, 예제, 기대하는 결과를 먼저 작성하는 것으로 시작
        - 단위 테스트 코드를 작성한다는 것은 이미 설게가 어느정도 된 상태, 즉 인터페이스와 예제가 만나는 지점을 묘사할 수 있는 상태를 전제로 함
