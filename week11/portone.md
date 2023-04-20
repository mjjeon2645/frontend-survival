# 2. 포트원 결제 요청

## 강의 요약 및 노트

- 포트원은 통합 솔루션. 이전에는 아임포트였음 -> 많이 사용되는 것이기 때문에 이번 강의와 학습을 통해 사용법을 익혀두기

- `Reflect.get(window, 'IMP').init('가맹점_식별코드');` 는 1회를 초과하여 실행하지 않도록 주의해야 함. 따라서 `main.tsx` 파일의 main 함수에서 실행하면 main 함수를 여러번 실행할 일은 없을테니 1회만 실행하게 될 것임

- `instance = Reflect.get(window, 'IMP')` 타입을 잡아주는게 복잡해서 강제로 얻는거라고 하는데 무슨 내용인지 모르겠음 -> 찾아보기

## 키워드 정리

### JavaScript SDK

- SDK는 Software Development Kit의 줄임말. 자바스크립트 컨텍스트에서 특정 REST API와 상호작용하기 위한 라이브러리를 의미함

### `Reflect.get()`

- 프로퍼티의 값을 반환함. `Reflect.get(target, propertyKey)` 형태로 사용됨. target이 object가 아닐 때 TypeError 발생함

```javascript
// Object
const obj1 = { x: 1, y: 2 };
Reflect.get(obj1, "x"); // 1

// Array
Reflect.get(["zero", "one"], 1); // "one"
```
