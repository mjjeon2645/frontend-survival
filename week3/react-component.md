# 1. React Component

## 강의 요약 및 노트

- __step 1. Break the UI into a component hierarchy__
    - UI를 컴포넌트 계층구조로 쪼개기. 조금 더 거칠게 이야기하자면 컴포넌트 트리 같은 구조로 쪼개는 것

- __step 2. Build a static version in React__
    - 리액트로 정적인 버전을 만들기
  
### 컴포넌트 계층 구조

- React의 강력한 특징을 생각해보았을 때, '복잡한 UI'를 만들기 위해서 '간단한 component'를 조립해야 한다는 점을 상기해야 함 => 컴포넌트는 '자신의 상태를 캡슐화 하고 있는' 상태여야 함
- 위 말을 반대로 뒤엎어보면, component를 복잡하게 만들고 있는 순간부터 '아, 내가 react의 장점을 제대로 살려서 만들고 있는건가?' 라는 의심을 해봐야 한다는 것

### 컴포넌트를 나누는 몇 가지 기준 -> 컴포넌트 하이어아키를 만들기 위해 활용할 수 있는 기준들

- SRP(Single Responsibility Principle)
    - 단일 책임 원칙(객체지향 프로그래밍의 SOLID 중 하나)
- CSS -> 이미 알고 있는 기준을 재활용(예. class명으로 할 수 있는 무언가를 하기)
- Design's Layer
- Information Architecture(JSON Schema의 영향)

</br>

- TS 사용할 때는 props를 넘겨줄 때 그 안에 어떤 요소들이, 어떤 타입으로 들어가는지 알기 때문에 굳이 예전에 JS 사용할 때처럼 분해해서 다 넘겨주는 방식이 아니어도 된다. 두 방법 모두 맞다, 틀리다의 관점이 아닌 어떤 방법이 더 '강력한가'에 대해 고민해보면 좋을 것
- props가 분해되어 넘겨졌을 때는 물론 하나 하나, 개별적인 요소를 고칠 때 편할 수 있음

```typescript
// product를 통째로 넘겨주었을 때
<ProductRow key={product.name} product={product} />

// product 안에 있는 요소들을 분해해서 넘겨주었을 때
// -> 만약 추후에 stocked 라는 요소를 필요로 한다면 어떻게 될까? prop을 또다시 분해해서 넘겨줘야 함
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

### [REST API와 GraphQL](#rest-api-graphql)

1. REST API란 무엇인가

- REST(Representational State Transfer)란 2000년도에 로이 필딩 박사의 논문에서 최초로 소개되었고, 웹 설계의 우수성에 비해 제대로 사용되지 못하는 모습에 안타까워 하며 웹의 장점을 최대한 활용할 수 있는 아키텍처로써 발표되었다. 로이필딩 논문에서는 어떠한 제약조건도 없는 Null Style에 제약조건들을 점진적으로 추가해나가면서 REST를 설명하고 있다.
- [from deview-그런 REST API로 괜찮은가](https://www.youtube.com/watch?v=RP_f5dMoHFc)
    - REST API란? REST 아키텍쳐 스타일을 따르는 API
    - 그럼 REST는? 분산 하이퍼미디어 시스템(예. 웹)을 위한 아키텍쳐 스타일
    - 그럼 아키텍쳐 스타일이란? 제약조건의 집합!. 결국 로이 필딩이 말한 모든 제약조건을 지켜야만 REST 아키텍쳐를 따른다고 말할 수 있음
    - REST를 구성하는 스타일(REST는 그 자체로 아키텍쳐 스타일이면서 동시에 아키텍쳐 스타일의 집합. -> 그래서 하이브리드 아키텍쳐라고도 함)
        - client-server
        - stateless
        - cache
        - uniform interface
        - layered system
        - code-on-demand(optional) -> 서버에서 코드를 클라이언트로 보내서 실행할 수 있어야 한다! -> 자바스크립트를 의미하는 것.
    - HTTP 프로토콜만 잘 지켜도 대체로 위 스타일 6개 중 client-server, stateless, cache, layerd system 정도는 만족시킨다. code-on-demand는 옵셔널. 하지만 가장 잘 지켜지지 못하는 것이 바로 `uniform interface`
    - uniform interface의 4가지 제약조건 중 [2가지](./react-component.md/#uniform-interface)가 지켜지지 못하고 있음
        - self-descriptive message. 즉 메시지는 스스로를 설명해야 한다.
        - HATEOAS. 애플리케이션의 상태는 Hyperlink를 이용해 전이되어야 한다.한가?

    - 그럼 왜 uniform interface가 필요???? => 바로 '독립적인 진화'를 하기 위해서 => 서버와 클라이언트가 각각 독립적으로 진화 => 서버의 기능이 변경되어도 클라이언트를 업데이트 할 필요가 없는!!
    - 그럼 이 REST를 잘 지키는 예시는? 바로 '웹' 웹 페이지를 변경했다고 웹 브라우저를 업데이트 할 필요도, 그 반대의 필요도 없음. HTTP 명세나 HTML 명세가 변경되어도 웹은 잘 동작함.
    - 상호 운용성(interoperability)에 대한 집착
        - 업데이트 하나 마음대로 하지 못함. 왜냐하면 지금 잘 돌아가고 있는 웹 세계를 깨뜨릴 수 없기 떄문에. 로이필딩의 REST도 이 고민에서 나왔음
        - 예. Referer 오타지만 안고침. charset 잘못 지은 이름이지만 안 고침

    - 그럼 결론적으로 REST가 웹의 독립적인 진화에 도움을 주었는가? 강연자는 YES!로 봄
        - HTTP에 지속적으로 영향을 줌
        - Host 헤더 추가
        - 길이 제한을 다루는 방법이 명시(414 URI Too Long 등)
        - URI에서 리소스의 정의가 추상적으로 변경됨: '식별하고자 하는 무언가'
        - 기타 HTTP와 URI에 많은 영향을 줌 -> 실제로 HTTP/1.1 명세 최신판에는 REST에 대한 언급이 들어감
    
    - **로이필딩의 정의**. 하이퍼텍스트를 포함한 self-descriptive한 메시지의 uniform interface를 통해 리소스에 접근하는 API.
    - 어쨌든 로이필딩 님은 제약조건을 제대로 지키지 못한다면 제발 REST 라는 단어좀 쓰지 말라고 하니...  
  
    - REST API를 만들어보자. 일단 왜 API는 REST가 잘 안되는지 일반적인 웹과 비교해보기.  
| | 흔한  웹 페이지 | HTTP API |
| Protocol | HTTP | HTTP |
| 커뮤니케이션 | 사람 - 기계 | **기계-기계** |
| Media Type | HTML | ***JSON***|
  
- 결국 커뮤니케이션의 주체, 미디어타입의 차이로 인해 발생하는 것이라고 추측해볼 수 있겠다. HTML, JSON을 잠시 비교해보면.  
| | HTML | JSON |
| Hyperlink | 됨(a 태그 등) | 정의되어 있지 않음 |
| Self-descriptive | 됨(HTML 명세) | 불완전* |
  
- *불완전하다는 의미는 문법 해석은 가능하나, 의미를 해석하려면(예. 그 안에 들어간 키가 어떤 의미인지, 밸류가 어떤 의미인지는 규정되어 있지 않음) 별도 문서가(API 문서) 필요하다.  
  
- 그렇다면 REST를 따르기 위해 Self-descriptive, HATEOAS를 만족시킬 수 있는 방법은 없을까?
    - custom media type, profile link relation 등으로 해결
    - HATEOAS는 HTTP 헤더나 본문에 linkㄹ르 담아 만족시킬 수 있음
  
- '두 컴퓨터 시스템이 인터넷을 통해 정보를 안전하게 교환하기 위해 사용하는 인터페이스'이다.
    - API란? 다른 소프트웨어 시스템과 통신하기 위해 따라야 하는 규칙을 정의
- REST 특징([참고링크](https://m.blog.naver.com/aservmz/222234406469))
    - Client-Server 구조
        - Client-Server 제약조건의 원칙은 '관심사의 분리(Seperation of concern)'이다. 
        - 데이터 저장에 대한 관심(data storage concerns)과 UI(User Interface)에 대한 관심을 분리시킨다. 이를 통해 다양한 플랫폼에 대한 UI의 이식성(portability. 대대적인 개정이 없이 다른 플랫폼에서 실행하거나 사용할 수 있는지의 여부)을 개선하고 서버측의 구성 요소를 단순화하여 확장성(scalability)을 높일 수 있다.
        - 하지만 웹에서 가장 중요한 것은 이러한 분리를 통해 각 컴포넌트들이(요소) 독립적으로 진화, 발전할 수 있게 해준다는 것이고, 따라서 여러 organizational domains들의 Internet-scale 요구사항을 뒷받침해줄 수 있게 된다는 점이다.
    - Stateless(무상태성)
        - 클라이언트-서버 간 interaction에 제약조건을 추가. 바로 클라이언트-서버 간 통신은 stateless해야 한다는 것이다. (CSS style. client-stateless-server). 
        - 즉, 클라이언트가 서버에 보내는 각 요청은 그 요청을 이해하는데 필요한 모든 정보들을 포함해야 한다. 또한 서버측에 저장된 어떠한 advantage도 이용할 수 없다. 그래서 세션 상태(session state)는 전적으로 클라이언트 측에 가진다.
        - 이 제약조건은 가시성(visibility), 신뢰성(reliability), 확장성(scalability)을 이끌어낸다.
        - 가시성: 모니터링 시스템이 요청의 전체적인 특징을 규정, 파악하기 위해 하나의 요청 데이터(single request datum) 너머의 무언가를 볼 필요가 없기 때문이다. 즉 그 요청 이외의 것들을 고려할 필요가 없음.
        - 신뢰성: 부분적 장애를 복구하기 위한 task를 용이하게 만들기 때문에 신뢰성이 향상된다.
        - 확장성: 요청 간 상태(state)를 저장하지 않기 때문에 리소스 제공(?)이 쉽고, 서버가 요청 간 리소스 사용에 대해 관리하지 않아도 되므로 구현 또한 심플해질 수 있다.
        - 다만 이 stateless 제약조건을 선택함으로 얻는 trade-off 측면이 있는데, 일련의 요청으로 인한 데이터 전송의 반복으로 네트워크 성능을 저하시킬 수 있다. (stateless 이므로 서버에 이전 data에 대한 context가 남아있지 않음) 
    - Cache
        - 네트워크 효율성을 위해 캐시 제약조건을 추가한다. (Client-Cahce-Stateless-Server)
        - Cache 제약조건은 요청에 대한 응답 데이터가 암시적으로나, 또는 명시적으로 cacheable한지 non-cacheable한지 라벨링되어야 한다는 점이다. 만약 cacheable하다면 다음에 동일한 요청이 올 경우 해당 응답 데이터를 추후에 재사용 할 수 있는 권한이 클라이언트 캐시에 부여된다.
        - 이 제약조건의 장점은 클라이언트-서버의 일부 interaction을 부분적으로나 완전히 제거할 수 있어 효율성, 확장성을 높일 수 있고 사용자interaction에 대한 평균 지연시간을 감소시켜 사용자가 느끼는 퍼포먼스를 향상시킬 수 있다.
        - 다만 여기에도 trade-off는 존재하는데 신뢰성이 저하될 수 있다. 캐시 내 데이터가 오래되어(stale) 서버의 데이터와 크게 다를 수 있기 때문
    - 하단부터는 현대의 웹 아키텍처 형태로 확장하기 위해 추가되는 스타일, 즉 제약조건들
    - [Uniform Interface](#uniform-interface)
        - REST 아키텍쳐 스타일이 다른 네트워크 기반의 스타일과 구분되는 핵심적인 특징은 컴포넌트들 간의 __uniform interface__ 이다.
        - Uniform-Client-Cache-Stateless-Server
        - 컴포넌트 인터페이스에 소프트웨어 공학 원칙인 generality(일반성)을 적용하여 전체적인 시스템 아키텍쳐를 단순화하고 interacion의 가시성을 높인다.
        - 제공하는 서비스로부터 구현체가 분리됨으로 각 컴포넌트들은 독립적인 evolvability(진화가능성)를 얻게 된다.
        - trade-off 역시 존재하는데, 효율성이 저하된다. 애플리케이션에 필요한 특정 정보가 전달되는 것이 아닌 표준화된 상태로 변환되어 전달되어야 하기 때문이다. 
        - REST interface는 일반적인 웹에 최적화된 대규모 하이퍼미디어 데이터를 효과적으로 전송하기 위해 디자인되었으며, 다른 형태의 interaction에 최적화된 것은 아니다.
        - uniform interface를 얻기 위해서는 컴포넌트 동작 방식에 대한 다양한 아키텍처 제약조건이 필요하다. REST는 아래 4가지 제약조건에 의해 정의된다.
            - identification of resources -> 리소스가 URI로 식별되면 된다(잘 지켜지는 편)
            - manipulation of resources through representations -> representation 전송을 통하여 리소스를 조작해야 한다는 의미. 리소스를 만들거나, 삭제하거나, 수정할 때 HTTP 메시지에 표현을 담아 전송하여 그 목적을 달성하면 됨(이것 역시 잘 지켜지는 편)
            - self-descriptive messages
            - hypermedia as the engine of application state(HATEOAS)
    - Layered System
        - Internet-scale 요구사항에 대한 동작을 향상시키기 위해 layered system 제약사항을 추가한다.
        - Uniform-Layered-Client-Cahce-Stateless-Server
        - 각 컴포넌트에 직접 접한 계층 너머를 볼 수 없도록 컴포넌트의 동작을 제한시켜 아키텍쳐가 계층적인 레이어를 구성하도록 한다. 이로서 전체적인 시스템의 복잡도를 제한하고 독립성을 촉진시킨다. 
        - 레이어들은 레거시 서비스를 캡슐화 하는데 사용될 수 있고, 기존 클라이언트로부터 새로운 서비스를 보호할 수 있다. 또한 자주 사용되지 않는 기능을 공유 중개 컴포넌트(shared intermediary)로 이동시켜 컴포넌트들을 단순화한다.
        - layered system의 주요 단점은 데이터 처리에 대한 오버헤드 증가와 지연(latency)이다. 이에 따라 사용자가 느끼는 퍼포먼스가 저하된다. 하지만 캐시 제약조건을 지원하는 네트워크 기반 시스템에서서는 중개 컴포넌트에 공유 캐시를 둠으로 상쇄될 수 있다.
        - REST 아키텍처에서, 중개컴포넌트는 메시지 내용을 적극 변환할 수 있다. 왜냐하면 그 메시지가 self-descriptive하고 메시지만으로도 그 의미(semantics)를 중개 컴포넌트가 알 수 있기 때문이다.
    - Code-On-Demand
        - 마지막 추가 제약조건은 code-on-demand 스타일이다. REST는 applet이나 스크립트 형태의 코드를 다운로드, 또는 실행할 수 있게 하여 클라이언트 기능을 확장시키도록 한다. 이는 클라이언트가 사전에 구현해야 할 특징들을 감소함으로서 클라이언트를 단순화할 수 있다. 또한 배포 후에 features(기능? 속성? 특징? 요구사항?)를 다운로드할 수 있게 하여 시스템의 확장성을 높인다.
        - 하지만 가시성을 감소시키므로 이는 REST의 선택적 제약조건이다.
  
2. GraphQL은 왜 등장했는가?

- GraphQL이란
    - (from 공식문서) API를 위한 쿼리 언어. 이미 존재하는 데이터로 쿼리를 수행하기 위한 런타임. API에 GraphQL 쿼리를 보내고 필요한 것만 정확히 얻자
    - [참고. 카카오테크](https://tech.kakao.com/2019/08/01/graphql-basic/)
        - 페이스북에서 만든 쿼리 언어임. SQL(Structed Query Language)와 마찬가지. 하지만 gql과 sql의 언어적 구조 차이나 사용되는 방식의 차이도 매우 큼.
        - sql의 목적은 데이터베이스 시스템에 저장된 데이터를 효율적으로 가져오는 것이라면, __gql은 웹 클라이언트가 데이터를 서버로부터 효율적으로 가져오는 것이 목적__ => 당연히 gql의 문장을 주로 클라이언트 시스템에서 작성하고 호출할 것이다.
  
3. REST API vs GraphQL

- 복잡한 view 하나를 그릴 때 여러번 API를 호출하고, 호출을 통해 받아온 데이터를 조합하여 사용해야 한다. 페이지가 복잡해질수록 호출 횟수가 많아질 수밖에 없으며 데이터 조합의 복잡도 역시 높아질 것이다. REST API의 경우 기존에 서버에서 정의한 데이터 구조만을 요청 한 번에 하나씩 가져올 수 있을 것이다. 반면 쿼리문을 통해서는 view에서 필요한 모든 데이터를 한번에 조합하여 가져올 수 있을 것이다. 이 말인 즉슨 불필요한 데이터 역시 딸려오지 않을 수 있다는 것과 같지 않을까.
- 가장 특징적인 차이로 REST API는 URL과 METHOD를 조합하므로 end-point가 다양하나 gql은 하나의 end-point 존재
- [읽어보기. apollo-blog. GraphQL vs. REST](https://www.apollographql.com/blog/graphql/basics/graphql-vs-rest/)

</br>

### JSON

- JSON(JavaScript Object Notation). 경량의 데이터 교환 형식. 속성-값(attribute-value pairs) 쌍, 배열 자료형(array data types) 또는 기타 모든 시리얼화 가능한 값(serializable value) 또는 '키-값 쌍'으로 이루어진 데이터 오브젝트를 전달하기 위해 인간이 읽을 수 있는 텍스트를 사용하는 개방형 표준 포맷
- 본래 JS 언어로부터 파생되어 JS 구분 형식을 따르나 언어 독립형 데이터 포맷임
- 해당 포맷은 본래 더글라스 크록포드가 규정
- JSON의 공식 인터넷 미디어 타입은 `application/json`임
- 기본 자료형
    - 수(numbers). 정수와 실수(고정 소수점, 부동 소수점)
    - 문자열(항상 큰 따옴표로 묶여야 함)
    - 배열(대괄호로 나타내며 배열의 각 요소는 기본 자료형이나 배열, 또는 객체)
    - 객체(object. 이름-값 쌍의 집합으로 이름은 문자열이기 때문에 반드시 따옴표 사용. 값은 기본 자료형)
    - boolean
    - null
- 장점
    - 텍스트로 이루어져 있으므로 사람과 기계 모두 읽고 쓰기 쉬움
    - 프로그래밍 언어와 플랫폼에 독립적이므로 서로 다른 시스템간 객체 교환하기에 좋다.

</br>

### DSL(Domain-Specific Language)

- 특정 도메인(산업, 분야 등의 특정 영역)에서 발생하는 문제를 해결하는 데 초점을 두는 언어를 의미함. 반대로 general한 목적을 가진 컴퓨터 언어를 지칭하는 GPL(General Purpose Languages). 특
- 예로서 CSS, 정규식, SQL, CSound 등
- Martin Fowler는 이를 외부 DSL과 내부 DSL로 나눈다.
- Internal DSLs
    - 특수한 목적을 위해 제한된 방법으로 호스트 언어를 사용하는 방식. 사용하던 도구를 그대로 이용할 수 있고 처리 결과를 쉽게 예측 가능
- External DSLs
    - 호스트 언어가 아닌 다른 언어에서 생성된 DSL. 대부분 자체 문법을 가지지만 기존 언어의 문법을 쓰는 예도 있음. DSL의 형식을 자유롭게 정할 수 있음
- 장점
    - 대부분 범용 언어보다 복잡하지 않음. 도메인의 복잡성을 낮추어 주기 때문에 단순하며, 이해하기 쉬운 문법을 사용하므로 가독성이 좋음
    - 코드를 간결하게 만들어주기도 함
    - 프로그래머들과의 소통 원활
    - 잘 설계된 DSL을 이용하면 유지보수가 쉬워지며 제품 품질 향상
- 단점
    - 설계의 어려움. 설계가 미흡하면 오히려 이해하기 힘든 코드
    - 적용 분야가 매우 좁음
- [참고링크 1. martin fowler](https://www.martinfowler.com/dsl.html)
- [참고 블로그 2](https://ccambo.blogspot.com/2014/02/dsl-domain-specific-language-1-dsl.html)
- [참고 기사 3](https://www.codingworldnews.com/news/articleView.html?idxno=3428)

</br>

### 선언형 프로그래밍

- (1) 프로그램이 어떤 방법으로 해야 하는지를 나타내기 보다 __'무엇'__ 과 같은지를 설명하는 경우
    - 예. 웹 페이지는 선언형임. 게목, 글꼴, 그림과 같이 '무엇'이 나타나야 하는지를 묘사할 뿐, '어떤 방법으로' 컴퓨터 화면에 페이지를 나타내야 하는지를 묘사하는 것은 아님
    - 명령형 프로그래밍 언어는 프로그래머가 실행할 '알고리즘'을 명시해주고 목표는 명시하지 않는 반면, 선언형 프로그램은 '목표'를 명시하고 알고리즘을 명시하지 않음
- (2) 프로그램이 함수형 프로그래밍 언어, 논리형 프로그래밍 언어, 혹은 제한형 프로그래밍 언어로 쓰인 경우 '선언형'이라고 함
- 선언형 프로그래밍은 DSL의 형태로 자주 사용됨. 

</br>

### 명령형 프로그래밍

- 선언형 프로그래밍과 반대되는 개념으로, 명령형 프로그램은 컴퓨터가 수행할 명령들을 순서대로 써 놓은 것. 

</br>

### SRP(단일 책임 원칙)

- 객체지향 프로그래밍의 5가지 원칙(SOLID. 단일책임 원칙, 개방-폐쇄 원칙, 리스코프 치환 원칙, 인터페이스 분리 원칙, 의존관계 역전 원칙) 중 하나.
- 모든 클래스는 하나의 책임만 가지며 클래스는 그 책임을 완전히 캡슐화 해야함. 어떤 클래스나 모듈은 변경하려는 단 하나의 이유만을 가져야 한다.
- 예를 들어 보고서를 편집하고 출력하는 모듈이 있을 때 이 모듈은 두 가지 이유로 변경될 수 있음
    - (1) 보고서의 내용으로 인한 변경
    - (2) 보고서의 형식으로 인한 변경
- 이 문제의 두 측면이 실제로 분리된 두 책임. 즉 서로가 다른 시기에 다른 이유와 원인으로 변경되어야 하는데 이를 묶는 것은 나쁜 설계이므로 분리된 클래스나 모듈이 되어야 함
- 단일 책임 원칙을 통해 클래스를 더욱 단단하게 만들 수 있으며, 하나의 클래스의 변경에 따라 다른 클래스의 일부가 망가질 위험을 줄일 수 있다.

</br>

### Atomic Design

- 아토믹 디자인은 화학적 관점에서 영감을 얻은 디자인 시스템
    - 디자인 시스템: 어떤 조직이 디지털 인터페이스를 디자인하고 구축하는 방식(from brad frost)
    - 디자인 시스템은 여러 하위 시스템을 포함하는데 UI 컴포넌트와 variants, 타이포그래피 시스템, 컬러 팔레트 시스템, 레이아웃/그리드 시스템 등
- 모든 것은 atom(원자)로 구성되며, atom들이 서로 결합하여 molecule(분자)가 됨. molecule은 더 복잡한 organism(유기체)으로 결합하여 궁극적으로 모든 물질을 생성함
- 아토믹 디자인에서는 이 개념을 차용하여 컴포넌트를 atom, molecule, organism, template, page의 5 레벨로 나눔  
  
1. Atom

- 더 이상 분해될 수 없는 필수 요소로 버튼, 제목, 텍스트 입력필드와 같은 가장 작은 구성 컴포넌트
- 예로 HTML tag와 같은 것들이 될 수 있겠다. label, input, button 등
- 글꼴, 애니메이션, 컬러팔레트, 레이아웃과 같은 추상적인 요소도 포함될 수 있음
- 단일 컴포넌트로 사용하기엔 어려우며 다른 atom과 결합한 molecule, 또는 organism 단위에서 여러 단위와 결합하여 유용하게 사용될 수 있음

2. Molecule

- 여러개의 atoms를 결합하여 고유한 특성을 가지는 단위. 중요한 점은 __한 가지 일__ 을 하는 것
- molecule의 SRP는 재사용성, UI에서의 일관성, 테스트하기 쉬운 조건이라는 이점을 가짐

3. Organism

- molecule의 집합체이며 비교적 복잡하고, 어떠한 인터페이스에서 구분되는 섹션이 될 수 있다는 특징을 가짐
- 서비스에서 표현될 수 있는 명확한 영역, 그리고 특정 컨텍스트를 가지게 됨
- 당연히 재사용성은 상대적으로 낮아질 수 밖에 없음

4. Template

- 실제 컴포넌트를 레이아웃에 배치하고 구조를 잡는 와이어프레임. 실제 컨텐츠가 없는 page 수준의 스켈레톤
- 페이지의 그리드를 정해주는 역할 뿐이며 스타일링이나 컬러가 들어가지는 않음

5. Page

- 유저가 볼 수 있는 실제 컨텐츠를 담고 있으며 template의 인스턴스라고 할 수 있음  
  
- 오해하지 않아야 할 부분은 아토믹 디자인의 각 단계가 linear process가 아님! __멘탈모델__ 임. UI를 응집력 있는 전체와 일부분에 대한 콜렉션으로 생각할 수 있도록 돕는 것.
- [참고. atomic design](https://bradfrost.com/blog/post/atomic-web-design/)
- [참고. 카카오엔터 FE 기술블로그](https://fe-developers.kakaoent.com/2022/220505-how-page-part-use-atomic-design-system/)
- [참고. 테오의 FE 글](https://yozm.wishket.com/magazine/detail/1531/)
- [TOAST UI 글](https://ui.toast.com/weekly-pick/ko_20200213)

</br>

### React Component와 props

- 리액트 컴포넌트는 서로 communicate 하기 위해 `props`를 사용한다. 모든 부모 컴포넌트는 자식 컴포넌트에게 props를 전달함으로써 정보를 전달할 수 있다. Props는 HTML 속성으로 국한되는 것이 아니라 객체, 배열, 심지어 함수와 같이 JS value 그 어떤 것으로도 전달 가능하다.
- props란? 속성을 나타내는 데이터
- 모든 React 컴포넌트는 자신의 props를 다룰 때 반드시 순수 함수처럼 동작해야 함. 즉 컴포넌트 자체 props를 수정해서는 안 됨(props는 읽기 전용)
