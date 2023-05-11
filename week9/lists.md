# 2. 목록보기

## 강의 요약 및 노트

### 상품 목록

- 상품 목록을 얻어서 표시하는 화면을 만들자
  - 이 말 자체가 정답. 두가지로 나눌 수 있음
  - 1. 상품 목록 얻기 from API 서버
  - 2. 상품 목록 보여주기 => 무엇으로? 리액트로
- 이 과정에서도 각각 책임을 나눠준다.
  - 상품 목록을 얻는 것 => `useFetchProducts` 훅으로
  - 상품 목록을 보여주는 것 => `Products` 컴포넌트로 구현
  - 이 둘을 조합하는 것은? => `ProductListPage`
  - 물론 컴포넌트에서도 훅을 부를 순 있지만 위와 같이 할 때 테스트가 쉬워지고 책임을 분명하게 나눌 수 있음
- 프로젝트 사이즈가 클 때는 components 하위에 주제별로 폴더를 만들어서 component들을 관리할 수 있다.

- li 하나의 항목의 width르 20%로 잡으면 5개의 li가 채워지게 됨

```CSS
li {
  width: 20%;
}
```

- 아샬님의 경우 컴포넌트 -> 휵 -> 타입 -> 유틸리티 순으로 상위 정리

- 이미지의 가로 - 세로 비율을 1대 1(1:1)로 설정할 때

```CSS
img {
  display: block;
  width: 100%;
  aspect-ratio: 1/1;
}
```

- img 태그에 직접 alt 속성을 지정하지 않고 styled-components로 스타일 지정 시 attrs로 alt 속성을 지정하는 방법

```js
const Thumbnail = styled.img.attrs({
  alt: 'Thumbnail',
})`
  // CSS
`;
```

- 숫자에 콤마 찍는 방법 => 내가 하던 방법과는 다르므로 익혀두기. 단 반환값은 string 타입으로 반환됨

```ts
export default function numberFormat(value: number) {
  return new Intl.NumberFormat().format(value);
}
```

- ProductsStore를 만들 떄 왜 Singleton과 useStore-ts가 필요한걸까? 알아보기

### 카테고리 목록

## 키워드 정리
