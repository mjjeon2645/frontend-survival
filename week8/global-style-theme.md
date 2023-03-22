# 6. Global Style & Theme

## 강의 요약 및 노트

- styled-reset을 설치하여 모든 속성값을 초기화시킴

```bash
npm i styled-reset
```

- 글로벌 스타일을 지정하는 것은 styled-components에 포함되어 있고 `createGlobalStyle`을 통해 만들 수 있음

<hr />

### theme

- defaultTheme을 이용해서 type을 강제로 잡아주는 것이 좋다. 다른 theme을 건드리다가 항목을 추가했을 때 누락하는 일이 없도록

<hr />

## 키워드 정리

### Reset CSS

- 목적: browser inconsistencies, 즉 브라우저 간 불일치를 줄이는 것(각 브라우저마다 기본적으로 설정하는 CSS가 있는데 이를 reset 즉 초기화시켜 개발자가 일괄적으로 CSS를 적용할 수 있도록 함)
- 모든 HTML 태그를 리셋시키는 기본 코드는 공식 페이지에도 제공되고 있는데 이를 별도의 CSS 파일로 만들어 적용하는 것보다 npm으로 설치하여 최상위 컴포넌트에 적용하는 것이 더 빠름

</br>

### box-sizing 속성

- `box-sizing` 속성은 CSS 속성 중 하나로 요소의 저비와 높이를 계산하는 방법을 지정할 수 있음. 지정 가능한 속성은 2가지로 나뉘며 (1) content-box, (2) border-box임
- content-box
    - CSS 표준이 정의한 초기 기본값이며 width, height 속성이 말 그대로 '콘텐츠 영역'만 포함하며 안팎 여백, 테두리를 포함하지 않음. 
    - (개인 의견) 다만 테두리와 안쪽 여백(padding)을 넣지 않을 경우 일반적으로 인지하는 '가로 길이', '세로 길이'를 count 하는데 헷갈리기 때문에 border-box로 설정하는 것이 타당
- border-box
    - width와 height 속성이 안쪽 여백(padding), 테두리(border)는 포함하고 바깥 여백(margin)은 포함하지 않음
- 참고
    - [MDN CSS box-sizing](https://developer.mozilla.org/ko/docs/Web/CSS/box-sizing)

</br>

### word-break 속성

- 텍스트가 자신의 콘텐츠 박스 밖으로 over-flow 될 때 줄을 바꿀지를 지정함
- 키워드 값
    - normal: 기본 줄 바꿈 규칙 사용
    - break-all: overflow를 방지하기 위해 어떤 두 문자 사이에서도 줄바꿈 발생(한, 중, 일 텍스트 제외)
    - keep-all: 한, 중, 일 텍스트에서는 줄을 바꿀 때 단어를 끊지 않으나 이에 해당하지 않는다면 normal과 동일하게 작용
- 전역 값
    - inherit: 부모 요소의 속성값을 상속받음
    - initial
    - unset

</br>

### Theme

- styled-components는 `<ThemeProvider>` 래퍼 컴포넌트로 테마 지원. 이 컴포넌트는 context API를 통해 그 아래에 있는 모든 리액트 컴포넌트에 테마를 제공
- (개인생각) 디자인 시스템. 즉 한 곳에서 theme을 관리하면 추후에 디자인 시스템에 변동사항이 생겼을 때 일일이 바꾸지 않아도 되므로 유지보수가 간편. 또한 디자인 시스템의 목적 중 하나인 '일관된 사용자 경험'을 제공하기 위해서는 반드시 theme이라는 영역이 중앙 집중식으로 관리되어야 할 것

</br>

### ThemeProvider

- theme을 위한 헬퍼 컴포넌트로, context API를 통해 트리 내에 있는 모든 styled-components에 동일한 theme을 적용

```js
// import styled, { ThemeProvider } from 'styled-components'

const Box = styled.div`
  color: ${props => props.theme.color};
`

render(
  <ThemeProvider theme={{ color: 'mediumseagreen' }}>
    <Box>I'm mediumseagreen!</Box>
  </ThemeProvider>
)
```
