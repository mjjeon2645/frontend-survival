# 2. TSyringe

## 강의 요약 및 노트

- MS에서 만든 것. 단순하게 사용할 예정. 
- TS에서 데코레이터를 사용하기 위해서는 tsconfig.json에서 `experimentalDecorators`, `emitDecoratorMetadata` 두 개의 주석을 해제

- prop drilling: props를 오로지 하위 컴포넌트로 전달하는 용도로만 쓰이는 컴포넌트들을 거치면서 리액트 컴포넌트 트리의 한 부분에서 다른 부분으로 데이터를 전달하는 과정. prop drilling 자체가 문제인 것이 아니라 prop 전달을 10개, 15개 처럼 많이 거치게 되면 추적이 어려워지고 이에 따른 유지보수도 어려워짐

---

## 키워드 정리

### TSyringe

- TS용 DI(IoC Container) 도구로서 External Store를 관리하는 데 활용할 수 있음

</br>

### 의존성 주입(Dependency Injection)

- 의존성 주입은 프로그램 디자인이 결합도를 느슨하게[6] 되도록하고 의존관계 역전 원칙과 단일 책임 원칙을 따르도록 클라이언트의 생성에 대한 의존성을 클라이언트의 행위로부터 분리하는 것

- 'A가 B를 의존한다'는 표현의 의미: 의존대상 B가 변하면 그것이 A에 영향을 미친다(from 토비의 스프링). 즉 B의 기능이 추가 또는 변경되거나 형식이 바뀌면 그 영향이 A에 미침. 이를 '의존관계'로 보는 것
- DI는 이러한 의존관계를 외부에서 결정하고 주입하는 것. 토비의 스프링에서는 아래 세 가지 조건을 충족하는 작업을 의존관계 주입이라고 함
    - 클래스 모델이나 코드에는 런타임 시점의 의존관계가 드러나지 않음. 그러기 위해서는 인터페이스에만 의존하고 있어야 함
    - 런타임 시점의 의존관계는 컨테이너나 팩토리 같은 제3의 존재가 결정
    - 의존관계는 사용할 오브젝트에 대한 레퍼런스를 외부에서 제공(주입)해줌으로써 만들어짐
- 장점
    - **의존성이 줄어듦**. 의존한다는 것은 의존대상의 변화에 취약하다는 의미. 하지만 DI로 구현하면 주입받는 대상이 변하더라도 그 구현 자체를 수정할 일이 없거나 줄어들게 됨
    - **재사용성이 높은 코드**
    - **테스트하기 좋음**
    - **가독성이 높아짐**

- 참고
    - [위키백과 의존성주입](https://ko.wikipedia.org/wiki/%EC%9D%98%EC%A1%B4%EC%84%B1_%EC%A3%BC%EC%9E%85)
    - [테코블 의존관계 주입 쉽게 이해하기](https://tecoble.techcourse.co.kr/post/2021-04-27-dependency-injection/)

</br>

### reflect-metadata

- Reflect API를 위한 폴리필을 추가할 때 사용
- reflect-metadata를 사용할 수도 있고 공식 문서에서는 core-js, reflection과 같은 useage도 제공
- **데코레이터는 선언적 구문을 통해 클래스가 정의될 때 클래스와 그 멤버를 보강하는 기능을 추가**

</br>

### singleton(싱글톤)

- TSyringe에서의 singleton은 클래스를 전역 컨테이너 내에서 싱글톤으로 등록하는 클래스 데코레이터 팩토리

```js
import {singleton} from "tsyringe";

@singleton()
class Foo {
  constructor() {}
}

// some other file
import "reflect-metadata";
import {container} from "tsyringe";
import {Foo} from "./foo";

const instance = container.resolve(Foo);
```

c.f. 싱글턴 패턴
- 소프트웨어 디자인 패턴에서 싱글턴 패턴을 따르는 클래스는, 생성자가 여러 차례 호출되더라도 실제로 생성되는 객체는 하나이고, 최초 생성 이후에 호출된 생성자는 최초의 생성자가 생성한 객체를 리턴. 이와 같은 디자인 유형을 싱글턴 패턴이라고 함. 주로 공통된 객체를 여러개 생성해서 사용하는 DBCP(DataBase Connection Pool)와 같은 상황에서 많이 사용됨
