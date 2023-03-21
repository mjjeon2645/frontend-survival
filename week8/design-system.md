# 1. Design System

## 강의 요약 및 노트

- 어떻게 하면 우리가 좀 더 일관적인 사용자 경험, 시각적인 디자인을 전달할 수 있는가에 대한 고민
- atomic design의 경우 리액트를 이 아토믹 디자인에 모두 맞추려고 하기 보다는 디자인 시스템을 만들기 위한 하나의 '방법론'이자 기본 개념으로 이해할 것. 단순히 해당 틀에 끼워맞춘다는 생각으로 접근할 경우 향후 리팩터링의 가능성까지 차단할 수 있음

<hr />

## 키워드 정리

### 반응형 웹 디자인(Responsive web design)

- 하나의 웹사이트에서 PC, 스마트폰, 태블릿 PC 등 접속하는 디스플레이의 종류에 따라 화면의 크기가 자동으로 변화도록 만든 웹 페이지 접근 기법
- 각 디바이스 별로 웹사이트를 별개 제작하지 않고, 하나의 공용 웹사이트를 만들어 다양한 디바이스에 대응할 수 있음
- 디바이스에 따른 URL이 동일하여 검색 포털 등 광고를 통한 사용자 접속을 효율적으로 관리할 수 있음
- 내용 수정 시 하나의 페이지만 수정하면 다양한 디바이스에 동일하게 반영

</br>

### 디자인 시스템(Design System)

- 디자인 시스템이란? **"디자인 시스템은 다양한 페이지와 채널을 걸쳐 공통의 언어와 시각적 일관성을 만들고 반복되는 작업을 줄임으로써, 규모에 맞게 디자인을 관리하기 위한 표준 집합이다."** (from Nielsen Norman Group)
- 반응형 웹 디자인과 관련해서 뷰포트에 따라 동일/또는 차별화 할 수 있는 컴포넌트
    - 동일하게 유지: 서체(typeface), 기본 유닛(base unit), 색상(colour), 모양이나 형태(shape/form)
    - 차별화: 그리드(grids), 레이아웃(layout), 글꼴 크기(font size), measure(line length), leading(line height)
- 디자인 시스템의 핵심은 '콘텐츠'를 최적으로 표시하는 것. 즉 콘텐츠는 **항상 동일**해야 함. 모든 디바이스에서 동일한 콘텐츠를 공유하고, 디자인 시스템 구성 요소들을 통해 콘텐츠를 가장 잘 표시하고 표현하는 법에 집중해야 함 -> 핵심!!

- 참고
    - [Laura Kalbag의 “Design Systems” 소개](https://24ways.org/2012/design-systems/)
    - [sk telecom devocean 블로그](https://devocean.sk.com/blog/techBoardDetail.do?ID=163710)

</br>

### Atomic Design

- [아토믹 디자인에 대한 참고-3주차](../week3/react-component.md/#atomic-design)
