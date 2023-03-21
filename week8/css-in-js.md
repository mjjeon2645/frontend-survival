# 3. CSS in JS

## 강의 요약 및 노트

- 핵심은 style component

- 디자인 시스템은 여러 페이지와 채널을 통틀어 언어와 시각적인 일관성을 유지하면서 중복을 줄여 규모 있는 디자인을 관리하기 위한 일련의 표준
- 디자인 시스템이란 재사용이 가능한 컴포넌트와 패턴들을 사용하여 대규모 디자인을 관리하기 위한 완전한 표준 set
- [아토믹 디자인에 대한 참고-3주차](../week3/react-component.md/#atomic-design)

<hr />

## 키워드 정리

### CSS in JS란?

- 자바스크립트를 사용하여 컴포넌트의 스타일을 지정하는 스타일링 기법. 스타일 정의를 CSS 파일이 아닌 JS로 작성된 컴포넌트에 바로 삽입하는 기법
- 2014년 페이스북 개발자인 Christophere Chedeau aka Vjeux가 처음 소개
- 자바스크립트가 구문 분석되면 CSS가 생성되어 DOM에 첨부됨. 이를 통해 CSS를 컴포넌트 수준 자체로 추상화할 수 있고, JS를 사용하여 선언적이고 유지 관리 가능한 방식으로 스타일을 기술할 수 있음
- 라이브러리 예
    - emotion
    - styled components
    - JSS
    - Tailwind CSS

<img src="https://i0.wp.com/css-tricks.com/wp-content/uploads/2021/05/aKsPahlPZ8qr6R8aVCancNsC_LOuKlcpBo-Ys44a1ya3QDvoLabbiBTYf36xX90hAfgMxgvBjMxxuBgIGnzH-_NId-71NfK7hh-ZFBJizZF6l3A4sLgb2vyYKgwnod86YBoLsE4.png?w=1600&ssl=1">

- CSS를 작성하는 어려움. CSS-in-JS로 이 이슈들을 모두 해결할 수 있다고 강조(from Vjeux)
    - Global namespace: 글로벌 공간에 선언된 이름의 명명 규칙 필요
    - Dependencies: CSS간의 의존 관계를 관리
    - Dead Code Elimination: 미사용 코드 검출
    - Minification: 클래스 이름의 최소화
    - Sharing Constants: JS와 CSS의 상태 공유
    - Non-deterministic Resolution: CSS 로드 우선 순위 이슈
    - Isolation: CSS와 JS의 상속에 따른 격리 필요 이슈

- CSS-in-JS 접근법을 사용했을 때 얻을 수 있는 실질적인 이익(장점) from [All You Need To Know About CSS-in-JS (2017)](https://blog.rhostem.com/posts/2017-06-24-unified-styling-language)

1. Scoped styles: 범위가 지정된(scoped) 스타일

- 라이브러리가 선택자를 자동으로 만들고 레퍼런스를 관리하므로 전역에서 클래스 이름이 충돌할 일이 없음
- 또한 이 선택자 범위(scope)는 주변 코드의 범위 규칙을 따름
- 코드가 모듈화 되어있고 이 선택자들을 앱의 다른 영역에서 사용하고 시픙ㄹ 때 자바스크립트 모듈로 변환하여 필요한 곳에서 import 가능 -> 코드 베이스를 지속해서 유지 보수성이 있게 한다는 측면에서 강력
- 스타일의 범위가 기본적으로 지정되도록 강제하게 됨으로 스타일 코드의 기본 품질을 향상시킴
- 앱의 모든 것을 컴포넌트로 표현할 수 있듯이 스타일도 컴포넌트, 즉 앱의 기본적인 구성 요소의 일부가 되어버림 -> 범위를 '컴포넌트'로 지정

2. Critical CSS: 필수적인(critical) CSS

- CSS-in-JS는 기본적으로 필수적인 CSS가 우선해서 작동하도록 요구함

3. Smarter optimisations: 더 똑똑한 최적화

- 클래스 자체의 이름을 관리하는 낮은 레벨의 작업을 최소화하고 필요한 스타일을 작성에만 집중 -> 작성한 스타일을 재활용 가능한 클래스들로 분리하는 것을 완전히 자동화할 수 있음

4. Package management: 패키지 관리
5. Non-browser styling: 브라우저가 아닌 환경의 스타일링

- 참고
    - [위키 CSS-in-JS](https://en.wikipedia.org/wiki/CSS-in-JS)
    - [삼성SDS 인사이트 리포트 웹 컴포넌트 스타일링 관리 : CSS-in-JS vs CSS-in-CSS](https://www.samsungsds.com/kr/insights/web_component.html)

<hr />

- CSS-in-JS 사용 시의 장점(from [All You Need To Know About CSS-in-JS (2017)](https://d0gf00t.tistory.com/22))
    - 컴포넌트로 생각하기— 더이상 스타일시트의 묶음을 유지보수 할 필요가 없습니다. CSS-in-JS는 CSS 모델을 문서 레벨이 아니라 컴포넌트 레벨로 추상화합니다(모듈성).
    - CSS-in-JS는 JavaScript 환경을 최대한 활용하여 CSS를 향상시킵니다.
    - "진정한 분리 법칙"—스코프가 있는 선택자로는 충분하지 않습니다. CSS에는 명시적으로 정의 하지 않은 경우, 부모 요소에서 자동으로 상속되는 속성이 있습니다. jss-isolate 플러그인 덕분에 JSS 규칙은 부모 요소의 속성을 상속하지 않습니다.
    - 스코프가 있는 선택자—CSS는 하나의 전역 네임스페이스만 있습니다. 복잡한 애플리케이션 내에서 선택자 충돌을 피할 수 없습니다. BEM과 같은 네이밍 컨벤션은 한 프로젝트 내에서는 도움이 되지만, 서드파티 코드를 통합할 때는 도움이 되지 않습니다. JSS는 JSON으로 표현된 것을 CSS로 컴파일 할 때, 기본적으로 고유한 이름을 생성합니다.
    - 벤더 프리픽스—생성된 CSS 규칙은 자동적으로 벤더 프리픽스가 붙어있으므로 생각할 필요가 없습니다.
    - 코드 공유—JavaScript와 CSS사이에 상수와 함수를 쉽게 공유할 수 있습니다.
    - 현재 화면에 사용중인 스타일만 DOM에 있습니다(react-jss).
    - 죽은 코드 제거
    - CSS 유닛 테스트!

- 단점
    - 러닝 커브
    - 새로운 의존성
    - 신규 팀원이 코드베이스에 적응하기 어렵게 만듭니다. 프론트엔드를 처음 접하는 사람들은 "더" 많은 것을 배워야합니다.
    - 현상 유지를 위한 도전 (꼭 단점은 아니다.)

</br>

### CSS

- Cascading Style Sheet
- 문서가 사용자에게 표시되는 방식(스타일, 레이아웃 등)을 지정하는 언어
    - 여기서 말하는 문서란 일반적으로 마크업 언어를 사용하여 구조화 시킨 텍스트 파일임. HTML이 가장 일반적인 마크업 언어지만 SVG나 XML과 같은 다른 마크업 언어도 사용 가능
- 문법적 특징
    - 시작은 선택자(selector)로!
    - 그 다음 중괄호( `{ }` ) 세트로 감쌈
    - 중괄호 안에 속성-값 쌍의 형태를 취하는 하나 이상의 선언이 있음

- 참고
    - [MDN - What is CSS?](https://developer.mozilla.org/en-US/docs/Learn/CSS/First_steps/What_is_CSS)
