# 2. React Testing Library

## 강의 요약 및 노트

### React Testing Library에 대해

- 리액트 컴포넌트를 사용자 입장에 가깝게 테스트할 수 있는 도구 -> 사용자 입장에 가깝다는게 어떤 의미인지를 알아보자. E2E 테스트에 가까운 느낌은 있지만 실제로 E2E 테스트는 아님.
- given-when-then. given은 우리가 테스트를 위해 사전에 준비해서 주는 것, when과 then은 각각 가정과 결과
- mock 한 것이 test 밖에 위치하고 있을 때 하위 테스트가 실행되면 mock이 어디서 불리었던 간에 호출된 것으로 인식하기 때문에 매 테스트 때마다 clear를 해주어야 함
- ** 강의 36분 부근에 fixture 폴더 생성하여 진행하는 부분 확인해서 다음 과제부터 적용해보기. 또 해당 내용부터 40분까지 내용 적용해보기

---

## 키워드 정리

### React Testing Library

- 구현 기반의 테스트 도구인 Enzyme의 대안으로 자리잡은 테스트 도구로서 '실제 사용자 경험과 유사한 방식'의 테스트를 작성할 것을 권고한다. 이에 따라 구현보다 그 '기능'에 초점을 맞춘다.
    - Enzyme은 Implementation Driven Test, 즉 '구현'에 집중하는 테스트 작성에 적합. 실제 브라우저 DOM이 아니라 리액트가 만들어내는 가상 DOM을 기준으로 테스트를 작성
    - 반면 RTL은 Behavior Driven Test, '행위'에 집중하는 테스트 작성에 적합. RTL은 `jsdom`이라는 라이브러리를 통해 실제 브라우저 DOM을 기준으로 테스트를 작성하며, 사용자 브라우저에서 렌더링하는 실제 HTML 마크업의 모습이 어떤지에 대해 테스트 하기 용이함
- 주요 API
    - `render()`
        - `@testing-library/react` 모듈로 import 가능
        - 인자로 렌더링 할 리액트 컴포넌트를 넘김
        - RTL에서 제공하는 모든 쿼리함수와 기타 유틸리티 함수를 담고 있는 객체를 리턴. 따라서 아래와 같이 destructuring 문법으로 리턴한 객체로부터 원하는 쿼리 함수만 얻어올 수도 있음

        ```js
        import { render, fireEvent } from "@testing-library/react";

        const { getByText, getByLabelText, getByPlaceholderText } = render(
        <YourComponent />
        );
        ```

    - `fireEvent()`
        - 쿼리 함수로 선택된 영역을 대상으로 특정 이벤트를 발생시키기 위한 이벤트 함수들을 담음
    - 쿼리함수
        - `getByOOO()`, `queryByOOO()`, `findByOOO()` 등

- 참고
    - [React Testing Library 홈페이지](https://testing-library.com/docs/react-testing-library/intro/)
    - [테코블 - 초심자를 위한 React Testing Library](https://tecoble.techcourse.co.kr/post/2021-10-22-react-testing-library/)
    - [DaleSeo - RTL 사용법](https://www.daleseo.com/react-testing-library/)
    - [리액트 공식문서의 Testing Overview](https://reactjs.org/docs/testing.html)

</br>

### given - when - then 패턴

- BDD(Behavior Driven Development)의 일부.
- 기본 아이디어는 시나리오(또는 테스트)를 3개의 섹션으로 분리하는 것
    - **given**: 시나리오에서 지정한 동작이나 행동을 시작하기 전에 상태를 설명. 테스트의 '전제 조건(pre-conditions)'이라고 생각하면 됨
    - **when**: 지정하는 '그 행동'
    - **then**: 그 지정된 동작으로 인한 변경사항에 대한 예상

- 예시. [아샬님 TIL](https://github.com/ahastudio/til/blob/main/blog/2018/12-08-given-when-then.md#%EC%83%81%ED%99%A9%EC%9D%84-%EB%8D%94-%EB%88%88%EC%97%90-%EB%9D%84%EA%B2%8C)

```Ruby
# Given
power = 700.watts
time = 3.minutes
stove = Stove.new(power)

# When
food = stove.cook(time)

# Then
assert food.complete?
```

- 참고
    - [martinfowler.com](https://martinfowler.com/bliki/GivenWhenThen.html)
    - [cucumber](https://cucumber.io/docs/gherkin/reference/)

</br>

### Mocking

- 키워드 정리 중 헷갈리는 점. Mocking과 Test fixture가 어떤 차이가 있는지...? test fixture의 경우 강의에서는 별도로 분리해서 가져와서 사용했던 걸로 기억하는데 mocking은 (경험적으로) 보통 테스트에서 function을 mocking해서 사용하지 않았나. 근데 의미적으로 test fixture랑 뭐가 다른지 잘 모르겠다. 대강 찾아보니 stub이랑도 사람들이 헷갈려 하는 것 같은데. mocking, test fixture, stub의 각각의 의미와 차이점에 대해 정확히 알아보기

- 참고할 것. 해당 블로그에 있는 글 원문으로 보기(https://jaime-note.tistory.com/330)

</br>

### Test fixture

- '테스트를 위해 고정되어 있는 것' 정도로 의역 가능
- (from wiki.) 특정 항목, 장치, 소프트웨어를 일관되게 테스트하는 데 사용되는 환경. 변경되지 않는 상태나 데이터를 미리 만들어두어(미리 만들어 둔 더미 데이터) 사용
- 테스트가 항상 동일한 설정으로 시작되기 때문에 테스트를 반복할 수 있고, test fixture를 사용하면 개발자가 메서드를 여러 함수로 분리하고 각 함수를 다른 테스트에 재사용할 수 있으므로 테스트 코드 설계가 쉬워짐. 또한 이전 테스트 실행에서 남은 데이터로 작업하는 대신 알려진 초기 상태로 테스트를 사전 구성함
