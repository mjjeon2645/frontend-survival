# 1. Routing

## 강의 요약 및 노트

- `window.location`과 `location`을 같게 사용할 수 있음
- 우리가 사용하려는 것은 `location`의 `pathname`
- 우리가 anchor로 사용하는 부분들은 보통 url에서 한글로 보이는데, 이를 복사해서 붙여넣기 해보면 16진수로 변환되어 있는 것을 확인할 수 있다. 우선 이 anchor를 추출하는 방법은 `location.hash`로 가능하고 16진수로 인코딩 된 결과를 미리 확인해보고 싶다면 `encodeURI('anchor 단어')`로 확인 가능하다. 해당 부분들은 개발자도구 콘솔에서 실험해볼 수 있음
- 당연히 반대로도 가능함. 예를들어 `decodeURI(location.hash)`라고 한다면 원래의 anchor를 확인 가능
- `location.host`와 `location.hostname`의 차이는 로컬 환경에서 띄웠을 때 명확하게 드러난다.
    - `location.host`는 localhost:8080과 같이 포트 넘버까지 나오는 반면
    - `location.hostname`은 localhost까지 출력되므로 포트 넘버는 나오지 않고 말 그대로 호스트네임만 확인 가능

** 잘 모르겠는 것 1. record가 뭐임..?

```ts
const pages: Record<string, React.FC> = {
  '/': HomePage,
  '/about': AboutPage,
};
```

-> [from 공식문서](https://developer.mozilla.org/ko/docs/Web/API/Window/location)
- 타입의 프로퍼티 키의 집합으로 타입을 생성. 이 유틸리티는 타입의 프로퍼티를 다른 타입에 매핑시키는 데 사용될 수 있음

```ts
Record<Keys, Type>
```

```ts
interface PageInfo {
  title: string;
}
 
type Page = "home" | "about" | "contact";
 
const nav: Record<Page, PageInfo> = {
  about: { title: "about" },
  contact: { title: "contact" },
  home: { title: "home" },
};
 
nav.about;
const nav: Record<Page, PageInfo>
```

** 잘 모르겠는 것 2. reflect는 뭐임..

```ts
const Page = Reflect.get(pages, path) || HomePage;
```

-> [from MDN Reflect](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Reflect)
-> [from MDN Reflect.get()](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Reflect/get)

- Reflect는 중간에서 가로챌 수 있는 JS 작업에 대한 메서들르 제공하는 내장 객체로서 메서드의 종류는 프록시 처리기와 동일함. Reflect는 함수 객체가 아니므로 생성자로 사용할 수 없다.
- `Reflect.get()` 정적 메서드는 객체의 속성을 가져오는 함수로 `target[propertyKey]`와 비슷

---

## 키워드 정리

### HTML DOM API

- HTML DOM API는 HTML의 각 요소의 기능을 정의하는 인터페이스, 해당 요소가 의존하는 모든 지원 유형 및 인터페이스로 구성된다.
- HTML DOM API에 포함된 기능 영역은 아래와 같다.
  - DOM을 통한 HTML 요소 액세스 및 제어
  - form data 액세스 및 조작
  - 2D 이미지의 컨텐츠와 HTML <canvas>의 컨텍스트와 상호작용(예. 이미지 위에 그리기)
  - HTML 미디어 요소(<audio>, <video>)에 연결된 미디어 관리
  - 웹페이지에서의 컨텐츠 드래그 앤 드롭
  - 웹 컴포넌트, 웹 스토리지, 웹 워커, 웹소켓, 서버 전송 이벤트와 같은 기타 API에 대한 지원, 연결 인터페이스

- 참고
  - [MDN the HTML DOM API](https://developer.mozilla.org/en-US/docs/Web/API/HTML_DOM_API)

#### Location

- Location 인터페이스는 객체가 연결된 장소(URL)를 표현한다.
- Location 인터페이스에 변경을 가하면 연결된 객체에도 반영되는데 Document, Window 인터페이스가 이런 Location을 갖고 있으며 각각 `Document.location`, `Window.location`으로 접근 가능함
- 속성에 대한 예제

```js
// location: https://developer.mozilla.org:8080/en-US/search?q=URL#search-results-close-container
const loc = document.location;
console.log(loc.href); // https://developer.mozilla.org:8080/en-US/search?q=URL#search-results-close-container
console.log(loc.protocol); // https:
console.log(loc.host); // developer.mozilla.org:8080
console.log(loc.hostname); // developer.mozilla.org
console.log(loc.port); // 8080
console.log(loc.pathname); // /en-US/search
console.log(loc.search); // ?q=URL
console.log(loc.hash); // #search-results-close-container
console.log(loc.origin); // https://developer.mozilla.org:8080

location.assign("http://another.site"); // load another page
```

- 메서드
  - `Location.assign()`: 주어진 URL의 리소스를 불러옴
  - `Location.reload()`: 현재 URL의 리소스를 다시 불러옴. 선택적으로 매개변수에 true를 제공해 브라우저 캐시를 무시하고 서버에서 새로 불러올 수 있음
  - `Location.replace()`: 현재 리소스를 제공된 URL에 있는 리소스로 바꿈. replace()를 사용한 후에는 현재 페이지가 세션 히스토리에 저장되지 않으므로 뒤로가기 버튼을 사용할 수 없다는 점이 assign() 메서드와의 차이점
  - `Location.toString()`: 전체 URL이 포함된 문자열 반환. 값을 수정하는 데 사용할 수는 없으나 Location.href와 동읠

#### pathname

- Location 인터페이서의 pathname 속성은 위치의 URL 경로가 포함된 문자열로, 경로가 없는 경우 비어있다. 하지만 그렇지 않다면 경로명에는 쿼리 문자열, fragment를 포함하지 않고 '/' 뒤에 URL의 path 가 포함

```js
// Let's say an <a id="myAnchor" href="/en-US/docs/Location.pathname"> element is in the document
const anchor = document.getElementById("myAnchor");
const result = anchor.pathname; // Returns:'/en-US/docs/Location.pathname'
```

- 참고
  - [MDN Location](https://developer.mozilla.org/ko/docs/Web/API/Location)
  - [MDN Window.location](https://developer.mozilla.org/ko/docs/Web/API/Window/location)
