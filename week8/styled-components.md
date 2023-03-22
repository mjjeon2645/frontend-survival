# 4. styled-components

## 강의 요약 및 노트

- HTML 요소가 아니라 기존에 적용한 styled component에 추가로 적용하고 싶을 경우 `.` 이 아니라 `()` 괄호를 사용하면 됨

<hr />

## 키워드 정리

### styled-components

- 리액트 컴포넌트 시스템에 대한 스타일링을 위해 어떻게하면 CSS를 개선할 수 있을지에 대한 결과물! 'single use case', 즉 단일 사용 사례에 집중함으로써 최종 사용자와 개발자를 위한 경험을 최적화함
- 얻을 수 있는 장점들
    - **Automatic critical CSS**: styled-components는 페이지에서 렌더링 되는 컴포넌트를 추적하여 자동으로 스타일을 삽입. 코드 분할(code splitting. 여러 코드 혹은 컴포넌트를 분리하는 것으로 필요에 따라 특정한 컴포넌트만 로딩하거나 병렬로 로딩이 가능)과 결합하면 사용자가 필요한 최소한의 코드를 로드할 수 있음
    - **No class name bugs**: 스타일에 고유한 클래스 이름을 자동으로 생성하므로 중복, 오버랩, 오타에 대한 걱정 없음
    - **Easier deletion of CSS**: 모든 스타일이 특정 컴포넌트에 연결되어 있어 사용 여부를 명확히 알 수 있음
    - **Simple dynamic styling**: 프롭스나 global theme을 기반으로 컴포넌트의 스타일링을 간단하고 직관적으로 관리할 수 있음
    - **Painless maintenance**: 컴포넌트에 적용되는 스타일링을 찾기 쉬워 코드베이스 규모와 관계 없이 유지, 관리, 보수가 편함
    - **Automatic vendor prefixing**: 현재 표준에 맞춰 CSS를 작성하고 나머지는 styled-components가 핸들링

- 참고
    - [styled-components 공식 홈페이지](https://styled-components.com/docs/basics)
    - [MDN code splitting](https://developer.mozilla.org/ko/docs/Glossary/Code_splitting)