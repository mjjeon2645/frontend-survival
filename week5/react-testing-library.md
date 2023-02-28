# 2. React Testing Library

## 강의 요약 및 노트

### React Testing Library에 대해

- 리액트 컴포넌트를 사용자 입장에 가깝게 세트슽할 수 있는 도구 -> 사용자 입장에 가깝다는게 어떤 의미인지를 알아보자. E2E 테스트에 가까운 느낌은 있지만 실제로 E2E 테스트는 아님.
- given-when-then. given은 우리가 테스트를 위해 사전에 준비해서 주는 것, when과 then은 각각 가정과 결과
- mock 한 것이 test 밖에 위치하고 있을 때 하위 테스트가 실행되면 mock이 어디서 불리었던 간에 호출된 것으로 인식하기 때문에 매 테스트 때마다 clear를 해주어야 함
- ** 강의 36분 부근에 fixture 폴더 생성하여 진행하는 부분 확인해서 다음 과제부터 적용해보기. 또 해당 내용부터 40분까지 내용 적용해보기

---

## 키워드 정리

### React Testing Library

### given - when - then 패턴

### Mocking

### Test fixture
