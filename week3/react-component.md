# 1. React Component

## 강의 요약 및 노트

- __step 1. Break the UI into a component hierarchy__
    - UI를 컴포넌트 계층구조로 쪼개기. 조금 더 거칠게 이야기하자면 컴포넌트 트리 같은 구조로 쪼개는 것

- __step 2. Build a static version in React__
    - 리액트로 정적인 버전을 만들기
  
### 컴포넌트 계층 구조

- React의 강력한 특징을 생각해보았을 때, '복잡한 UI'를 만들기 위해서 '간단한 component'를 조립해야 한다는 점을 상기해야 함 => 컴포넌트는 '자신의 상태를 캡슐화 하고 있는' 상태여야 함
- 위 말을 반대로 뒤엎어보면, component를 복잡하게 만들고 있는 순간부터 '아, 내가 react의 장점을 제대로 살려서 만들고 있는건가?' 라는 의심을 해봐야 한다는 것

### 컴포넌트를 나누는 몇 가지 기준 -> 컴포넌틐 하이어아키를 만들기 위해 활용할 수 있는 기준들

- SRP(Single Responsibility Principle)
    - 단일 책임 원칙(객체지향 프로그래밍의 SOLID 중 하나)
- CSS -> 이미 알고 있는 기준을 재활용(예. class명으로 할 수 있는 무언가를 하기)
- Design's Layer
- Information Architecture(JSON Schema의 영향)

</br>

- TS 사용할 때는 props를 넘겨줄 때 그 안에 어떤 요소들이, 어떤 타입으로 들어가는지 알기 때문에 굳이 예전에 JS 사용할 때처럼 분해해서 다 넘겨주는 방식이 아니어도 된다. 두 방법 모두 맞다, 틀리다의 관점이 아닌 어떤 방법이 더 '강력한가'에 대해 고민해보면 좋을 것
- props가 분해되어 넘겨졌을 때는 물론 하나 하나, 개별적인 요소를 고칠 때 편할 수 있음
- 

```typescript
// product를 통쨰로 넘겨주었을 때
<ProductRow key={product.name} product={product} />

// product 안에 있는 요소들을 분해해서 넘겨주었을 때
// -> 만약 추후에 stocked 라는 요소를 필요로 한다면 어떻게 될까? prop을 또다시 분해해서 념겨줘야 함
<ProductRow 
key={product.name} 
name={product.name} 
price={product.price} 
category={product.category} />
```

</br>

### 찾아보기*****!!

- 강의 40분. -> 아샬님이 '이럴 땐 interface'를 사용한다고 한게 무슨 의미인지. 그 '이럴 땐'이 어떤 때인건지??
- 강의 54분. 여기서도 Category를 단순히 category로 두는 것이 아니라 해당하는 product[]를 소유하는 Category로 가공할 때 type이 아니라 interface를 사용. 궁극적으로는 interface를 사용하는 것들은 나중에 마치 자바의 'Class' 처럼 사용할 수 있는것인가? 궁금하다. 지금까지의 느낌으로는 type은 말 그대로 type을 정의하는 데 사용되는 것 같고, interface는 타입의 느낌보다는 마치 추후에 class처럼 활용하려는 느낌.
- 강의 58분부터 나오는 `select` function에 대한 내용(ex. 제네릭, 레코드 등). 잘 모르는 내용이기 때문에 타입스크립트 책이나 공식 문서 통해서 키워드 정리해두기  

```typescript
export default function select<T>(
  items: Record<string, T>[],
  field: string,
  value: T,
): Record<string, T>[] {
  return items.filter((item) => item[field] === value);
}
```

```typescript
export default function select<T1 extends object, T2>(
    items: T1[],
    field: string,
    value: T2,
): T1[] {
    return items.filter((item) => Reflect.get(item, field) === value);
}
```

- `useRef` 별도 정리. 하단 코드 참고

```typescript
import { useRef } from 'react';

type CheckBoxFieldProps = {
    label: string;
}

export default function CheckBoxField({ label } : CheckBoxFieldProps) {
  const id = useRef(`checkbox-${label}`.replace(/ /g, '-').toLowerCase());

  return (
    <div>
      <input type="checkbox" id={id.current} />
      <label htmlFor={id.current}>
        {label}
      </label>
    </div>
  );
}
```
---

</br>

## 키워드 정리

### REST API와 GraphQL

- REST API란 무엇인가
- GraphQL은 왜 등장했는가?
- REST API vs GraphQL

</br>

### JSON

</br>

### DSL(Domain-Specific Language)

</br>

### 선언형 프로그래밍

</br>

### 명령형 프로그래밍

</br>

### SRP(단일 책임 원칙)

</br>

### Atomic Design

</br>

### React Component와 props
