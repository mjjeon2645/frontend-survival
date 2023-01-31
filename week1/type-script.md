# 2. TypeScript

## 강의 요약 및 노트

- 간단하게 REPL 사용하기. ts-node 실행. -> 해당 내용은 node_modules 안에 없음에도 불구하고 실행됨. 반드시 이 폴더 안에 bin 파일이 있어야만 실행 가능한 것은 아니고, 온라인 상에서 다운받아 설치하는 과정을 거쳐서도 실행이 가능

```
npx ts-node
```

- 나만의(ㅎㅎ) 타입 정의하기. 타입을 정의하거나 인터페이스를 정의할 때에는 우리가 변수명을 작성하는 그 위치에 첫글자가 대문자인 형태로 작성해준다.

```typescript
// type을 잡아주는 방법
type Human = {
    name: string;
    age: number;
};

// interface를 잡아주는 방법(마치 함수표현식과 함수선언식 같다. = 붙고 안붙고..ㅎ)
interface Person {
    name: string;
    age: number;
};
```

- 값이 타입일수도 있는가? YES! 아래 형태가 값이 타입인 것이라고 볼 수 있음. Union에서 유용하게 쓰인다고 함 -> 매개변수를 제한하거나 할 때 유용하게 쓸 수 있다고 함!

```
let category: 'food';

category = 'food';
```

---

## 키워드 정리

- REPL
- TypeScript
    - Interface vs Type
    - 타입 추론
    - Union Type vs Intersection Type
    - Optional Parameter
