# 1. 개발하기 전 준비

## 강의 요약 및 노트

- 이번 강의에서 쇼핑몰을 개발하기 전 '용어'에 대한 정리를 하고 간다. -> 이 부분이 의미가 있는점. 예전에 포트폴리오를 만들면서 map과 position에 대한 혼동 때문에 결국에 명확하지 않은 단어로 파일 이름을 짓고 백엔드를 만들었다. 개발 문서를 작성할 때 적어도 용어에 대한 정의를 명확히 한다면 커뮤니케이션에서도 오류를 줄일 수 있고, 시스템이 완전히 분리되지 않은 상황에서도 오해의 소지를 줄일 수 있을 것이다.

- 개발 환경 세팅을 스스로 진행해보자.
    - notion에 따로 정리해둔 타입스크립트 프로젝트 세팅 프로세스 진행
    - CodeceptJS 세팅
- 추가로 라이브러리를 설치해보자.
    - react router
    - styled-components
    - styled-reset
    - usehooks-ts
    - axios
    - tsyringe
    - reflect-metadata
    - usestore-ts
    - jest-dom
    - msw

### styled-components 사용

- ThemeProvider
- styled-reset -> <Reset />
- GlobalStyle -> styled-components의 createGlobalStyle 이용
    - 어떤 요소들을 GlobalStyle로 다룰 것인지 곰곰이 생각해보기
    - 단순히 box-sizing만 존재하지는 않을 것
- Theme 활용하기. defaultTheme을 설정해보기
    - defaultTheme을 타입으로 설정하여 Theme으로 활용하기.
    - ThemeProvider로 감싸고 defaultTheme을 theme으로 내려주기
- 잘 이해 안되는 것. props를 사용하기 위해서 styled.d.ts 파일에 아래와 같은 내용이 추가되는데 왜 이걸 안하면 계속 에러가 나는지 모르겠음

```typescript
declare module 'styled-components' {
    export interface DefaultTheme extends Theme {}
}
```

### routes 사용

- /src/routes.tsx를 생성

```javascript
// 기본 형태는 아래와 같음

const routes = [
    {
        element: <Layout />,
        children: [
            { path: '/', element: <HomePage /> },
            // ...
        ],
    },
];

export default routes;
```

- Layout을 만들어주기
    - 궁금했던 점. 왜 Layout은 components 폴더에 들어가있을까? 레이아웃이 정말 컴포넌트일까? -> main.tsx에 들어가는 요소도 아닌데...
    - Layout을 감싸는 container의 CSS 속성 잘 살펴보기. 

- 강의를 두 번 들으니 fixtures 폴더의 강력함을 알게된 것 같다. 단순히 따라 치기만 했을 때는 몰랐는데 fixtures 폴더 내에 index를 만들어주고 index에서 활용하는 각각의 mock data를 파일로 만들어주면 추후에 변경사항이 생겼을 때에도 변경이 용이하고, 특히 fixtures.products, fixtures.cart와 같은 네임스페이스를 사용하면서 real data와의 혼란을 줄여준다.

## 키워드 정리
