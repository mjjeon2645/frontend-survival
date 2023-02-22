# 2. Fetch API & CORS

## 강의 요약 및 노트

- fetch api는 'fetch'라는 함수를 쓴다 정도로 알고 있기
- fetch api로 받아오는 값은 promise라서 then, 또는 await 문법을 같이 활용하여 받아주어야 함
- 그렇게 받아온 response의 body를 살펴보면 Readable stream이라는 것을 알 수 있다. Readable stream에서 `.getReader()`를 하면 chunk를 얻을 수 있고 이렇게 얻은 chunk에서 `.read()`를 하면 value, done이 키로 있는 객체가 반환된다. 
- done이 false일 경우 아직 읽어들일 데이터가 남아있다는 뜻임. done이 true가 되면 반복을 종료시킴으로써 데이터 읽어들이는 것을 멈출 수 있다.
- value는 Byte array인데 우리가 읽을 수 있는 string으로 바꾸어야 알아볼 수 있음. 그 전까지는 Byte 숫자로 구성됨. 이를 변환하기 위해 텍스트 디코더, 텍스트 인코더 등이 필요.

```javascript
// 디코딩 방법
new TextDecoder().decode(chunk.value)

JSON.parse(new TextDecoder().decode(chunk.value))
```

- 하지만 고맙게도 JSON을 기본으로 지원하기 때문에 복잡하지 않게 원하는 형태로 데이터를 받을 수 있다.

```javascript
const response = await fetch('http://localhost:3000/products');

const data = await response.json();
```

### SOP, CORS

- 중요한 것은 '웹브라우저'의 보안정책. 따라서 웹 브라우저가 해당 정책을 사용하지 않겠다고 하면 문제가 없음

---

## 키워드 정리

### Fetch API란

- fetch를 공부하기 전에 알아두어야 할 **AJAX(Asynchronous JavaScript And XML)** 비동기적 Javascript와 XML
- AJAX는 XMLHttpRequest(XHR)와 Javascript와 DOM을 이용하여 서버에서 추가 정보를 비동기적으로 가져올 수 있게 해 주는 포괄적인 기술을 나타내는 용어이다.
- 만들어진지 오래되었고 jQuery와 보다 쓰기 쉬운 표준 API가 등장했는데 그게 바로 사용이 쉽고 Promise 값을 반환하는 `fetch API`이다. 즉 fetch API는 AJAX를 구현하는 여러 기술 중 최신 기술의 하나이다.
- fetch 명세는 jQuery.ajax()와 크게 두 가지 면에서 다르다(from MDN)
    - fetch()가 반환하는 프라미스 객체는 404, 500과 같은 HTTP 오류 상태를 수신하더라도 거부되지 않으며, 프라미스는 서버에서 헤더를 포함한 응답을 받는 순간 정상적으로 이행함. 다만, 응답 상태가 200~299를 벗어날 경우 ok 속성이 false로 설정됨. 프라미스가 거부되는 경우는 네트워크 연결이 실패하는 경우를 포함, 아예 요청을 완료하지 못한 경우로 한정됨
    - credentials 옵션을 제공하지 않은 경우 fetch()는 교차 출처 쿠키를 전송하지 않음(credential 정책의 기본 값이 same-origin)

- fetch() 기본 문법은 아래와 같음

```javascript
let promise = fetch(url, [options]);
```

- url은 접근하고자 하는 URL이며 options는 선택 매개변수로 method나 header 등을 지정할 수 있다. 또한 options에 아무것도 넘기지 않으면 요청이 GET 메서드로 진행되므로 url로부터 콘텐츠가 다운로드 횐다.

- 일반적인 fetch 요청은 두 개의 await 호출로 구성된다.

```javascript
let response = await fetch(url, options); // 응답 헤더와 함께 이행됨
let result = await response.json(); // json 본문을 읽음
```

- await 없이도 요청을 보낼 수 있는데 프라미스 객체이기 때문에 then을 활용하면 된다.

```javascript
fetch(url, options)
  .then(response => response.json())
  .then(result => /* 결과 처리 */)
```

- 응답 본문을 얻기 위해 사용할 수 있는 메서드들
    - `response.text()`: 응답을 텍스트 형태로 반환
    - `response.json()`: 응답을 파싱해 JSON 객체로 변경
    - `response.formData()`: 응답을 FormData 객체 형태로 반환
    - `response.blob()`: 응답을 Blob(타입이 있는 바이너리 데이터) 형태로 반환
    - '`response.arrayBuffer()`: 응답을 ArrayBuffer(바이너리 데이터를 로우 레벨로 표현한 것) 형태로 반환

- fetch 옵션들
    - `method`: HTTP 메서드
    - `headers`: 요청 헤드가 담긴 객체(제약사항이 있음)
    - `body`: 보내려는 데이터(요청 본문)로 string, FormData, BufferSource, Blob, UrlSearchParams 객체 형태

- 참고문서
    - [모던자바스크립트 네트워크 요청-fetch](https://ko.javascript.info/fetch)
    - [MDN Fetch API](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API)
    - [MDN Fetch 사용하기](https://developer.mozilla.org/ko/docs/Web/API/Fetch_API/Using_Fetch)
    - [블로그 글. fetch API](https://pstudio411.tistory.com/entry/fetch-API)
    - [블로그 글. Ajax란? + XMLHttpRequest, fetch API, axios](https://pul8219.github.io/javascript/js-ajax/#axios)

</br>

### Promise

</br>

### ReadableStream

</br>

### Unicode

</br>

### CORS란
