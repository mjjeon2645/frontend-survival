# 1. 관리자 웹 사이트 개발 시작

## 강의 요약 및 노트

- 먼저 관리자 웹 사이트에 필요한 기능들의 목록을 정의한다
  - 목록 정의 시에는 기능의 큰 범주와 하위 세부 기능들을 분리하면 좋은 것 같다.
  - 추후에 화면을 설계하고 URL, REST API를 설계할 때 기능을 참고할 수 있으므로.
- 그 다음 화면을 정의한다.
  - 화면은 기능들을 표현해주는 역할
- REST API
- 준비
  - 큰 방향성: 기존 작업 코드 재활용 + 상품 등록 및 수정을 제외한 대부분은 단순 CRUD로 구성
  - 단순 작업을 위해 활용하는 2가지: SWR, React Hook Form
  - 즉 기능과 방향성에 따라 활용할 라이브러리 등을 고려하고 결정
- 타입 준비
  - 어드민 API에 맞는 타입을 준비
** ApiService
  - 위에서 말한 방향성에서 이번에는 '프론트가 백엔드의 그림자' 역할을 하는, 즉 캐싱을 활용하게 되므로 CRUD 중 대부분 CUD => Command(또는 Mutation)에 대한 코드가 주를 이룬다. Read는 대부분 SWR를 이용하기 때문

## 키워드 정리

### SWR

- [4주차 SWR 정리내용 바로가기](../week4/usehooks-ts.md)
