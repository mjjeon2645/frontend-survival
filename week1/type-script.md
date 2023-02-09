# 2. TypeScript

## 강의 요약 및 노트

- 간단하게 REPL 사용하기. ts-node 실행. -> 해당 내용은 node_modules 안에 없음에도 불구하고 실행됨. 반드시 이 폴더 안에 bin 파일이 있어야만 실행 가능한 것은 아니고, 온라인 상에서 다운받아 설치하는 과정을 거쳐서도 실행이 가능

```bash
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

```bash
let category: 'food';

category = 'food';
```

---

## 키워드 정리

### REPL

- Read - Eval - Print Loop
- 사용자가 특정 코드를 입력하면 그 코드를 평가하고 실행 결과를 출력하는 것을 반복하는 환경. -> simple!
- Node.js에서도 REPL 환경을 지원하며 터미널에서 node를 입력해 엔터를 치면 바로 REPL 환경으로 진입 가능

</br>

### TypeScript

1. Interface vs Type

- 아샬님은 강의에서 타입을 더 선호하신다는 기억이 나는데. 우선 두개 다 익숙해지는 것을 먼저 목표로.
- [공식문서 핸드북](https://d2.naver.com/helloworld/12864)에서 이야기하는 Type Aliases와 Interfaces의 차이점.  
-> 대부분의 interface의 특징들을 type으로도 사용 가능하다. 하지만 가장 중요한 차이점은 type은 **cannot be re-opened to add new properties** 인 반면, interface는 언제나 확장 가능함
- 아마도 immutable 속성 때문에 아샬님이 type을 더 선호하시는게 아닐까? 하는 나만의 생각...
- 또한 interface는 object 형태로만 선언이 가능하나, type은 object 뿐만 아니라 union type처럼도 name이 가능(예. `type ID = number | string;`)
- type과 interface의 특징은 예시를 통해 금방 이해할 수 있다.  
여기서 조금 헷갈렸던 건, 왜 type을 확장할 때 union이 아닌 intersection을 사용하는가? 였다. 당연히 교집합보다 합집합(union)이 넓은 의미이기 때문에 확장의 의미를 지니는 것 아닌가? 라고 생각했는데 직접 `ts-node`로 확인해보면서 이해할 수 있었다.  
만약 아래 예시에서 intersection이 아닌 union을 사용하면 Bear type은 name과 honey 프로퍼티를 모두 갖는 것이 아니라 name을 갖거나, honey를 갖거나, 또는 name honey 모두를 갖거나가 되기 때문에 우리가 궁극적으로 만들고자 했던 '확장'의 개념은 아니게 되는 것이다.  

확장 예시

```javascript
// interface
interface Animal {
  name: string
}

interface Bear extends Animal {
  honey: boolean
}

const bear = getBear() 
bear.name
bear.honey


// type
type Animal = {
  name: string
}

type Bear = Animal & { 
  honey: boolean 
}

const bear = getBear();
bear.name;
bear.honey;
```

새로운 filed를 추가하려고 시도하면?

```javascript
// interface
interface Window {
  title: string
}

interface Window {
  ts: TypeScriptAPI
}

const src = 'const a = "Hello World"';
window.ts.transpileModule(src, {});


// type
type Window = {
  title: string
}

type Window = {
  ts: TypeScriptAPI
}

 // Error: Duplicate identifier 'Window'.
```

</br>

2. 타입 추론

- 동적 타이핑 언어인 자바스크립트에서 전형적으로 볼 수 있음. `const number = 'a';`라고 할 때, number라는 identifier가 숫자라는 의미임에도 불구하고 믄자열 a가 할당되었기 때문에 자바스크립트는 number를 string으로 추론함
- 자바스크립트에서 숫자 1과 문자 1을 더했을 때 예상치 못했던 답이 나오는 이유가 타입 추론과 관련있음
- 타입스크립트 역시 자바스크립트의 슈퍼셋이기 때문에 타입을 명시하지 않더라도 추론하여 작동

</br>

3. Union Type vs Intersection Type

- Union은 합집합
- Union은 `|`로 구분하며 OR 연산자와 비슷한 역할.

- Intersection은 교집합
- Intersection은 `&`로 구분하며 AND 연산자와 비슷.

- [참고. 카카오 엔터 FE기술블로그](https://fe-developers.kakaoent.com/2022/221124-typescript-tip/)

</br>

4. Optional Parameter

- 함수 선언 시에 반드시(필수로) 주어지지 않아도 되는 파라미터가 있을 경우 `parameterName?: type` 과 같이 '?'를 사용하여 옵셔널로 지정 가능.
- 위 경우에는 파라미터에 인자를 전달하지 않아도 가능
