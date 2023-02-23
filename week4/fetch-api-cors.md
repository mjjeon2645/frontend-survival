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

- promise는 ES6에서 도입된 것으로 비동기 처리에 사용되는 '객체'이다.
- 비동기 처리란?
    - 특정 코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 수행하는 자바스크립트의 특성
- promise는 3가지 상태를 가진다.
    - 대기(pending): 이행하지도, 거부하지도 않은 초기 상태
    - 이행(fulfilled): 연산이 성공적으로 완료됨
    - 거부(rejected): 연산 실패
- promise 객체는 아래의 문법으로 만들 수 있다.

```javascript
let promise = new Promise(function(resolve, reject) {
    // executor(실행자, 실행 함수)
});
```

- executor는 new Promise가 만들어 질 때 자동으로 실행되는데, 결과를 최종적으로 만들어내는 제작 코드를 포함한다.
- executor의 인수인 resolve, reject는 자바스크립트에서 자체 제공하는 콜백이며, resolve와 reject를 고려하지 말고 executor 안의 코드만 작성하면 됨. 다만 executor에서는 결과를 언제 얻던 상관 없이 상황에 따라 인수로 넘겨 준 콜백 중 하나를 반드시 호출해야 함
    - resolve(value): 연산 성공일 경우 그 결과를 나타내는 value와 함께 호출
    - reject(error) 에러 발생 시 에러 객체를 나타내는 error와 함께 호출
- executor는 자동으로 실행되며 여기서 원하는 일이 처리됨. 처리가 끝날 경우 처리 성공 여부에 따라 resolve나 reject를 호출함
- `promise` 객체는 아래의 내부 프로퍼티를 가짐
    - state: 위에서 말한 3가지 상태(pending, fulfilled, rejected). 처음엔 pending이었다가 resolve가 호출되면 fulfilled, reject가 호출되면 rejected로 변함
    - result: 처음엔 undefined였다가 resolve(value)가 호출되면 value, reject(error)가 호출되면 error로 변함
- promise 객체의 state, result는 내부 프로퍼티이므로 개발자가 직접 접근할 수 없으나 `.then`, `.catch`, `.finally` 메서드 사용시 접근 가능함
- `.then`은 프라미스에서 가장 중요, 기본이 됨. `then`의 첫 번째 인수는 프라미스가 이행됐을 때 실행되는 함수이고 여기서 실행 결과를 받으며, 두 번째 인수는 프라미스 거부됐을 때 실행되는 함수이며 여기서 에러를 받는다.
- 만약 처리 성공 경우만 다루고 싶다면 `then`에 인수 하나만 전달하면 된다.

```javascript
promise.then(
    function (result) {
        // 결과(result)를 다룸
    }, 

    function (error) {
        // 에러(error)를 다룸
    }
);
```

- 참고내용
    - [모던자바스크립트 프라미스](https://ko.javascript.info/promise-basics)
    - [MDN 프라미스](https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Global_Objects/Promise)
    - [캡틴판교 프라미스 쉽게 이해하기](https://joshua1988.github.io/web-development/javascript/promise-for-beginners/)
    - [PoiemaWeb 프라미스](https://poiemaweb.com/es6-promise)

</br>

### ReadableStream

- `Streams API`를 사용하면 자바스크립트가 네트워크를 통해 수신된 데이터 스트림에 프로그래밍 방식으로 액세스하고 개발자가 원하는 대로 처리할 수 있음(from [Runebook.dev](https://runebook.dev/ko/docs/dom/streams_api))
- 이러한 Streams API의 ReadableStream 인터페이스는 바이트 데이터를 읽을 수 있는 스트림을 제공하는데, Fetch API는 Response 객체의 body 속성을 통해 ReadableStream의 구체적인 인스턴스를 제공함
- `ReadableStream.getReader()`를 통해 Reader 객체를 얻어 데이터를 읽을 수도 있고, `ReadaeblStream().cancel()`로 Stream을 취소하는 것도 가능하다.
- 그 밖에 `WritableStream`을 사용하면 Stream에 데이터를 쓰는 것도 가능하다.
- ReadableStream 객체인 `response.body`를 사용하면 응답 본문을 청크 단위로 읽을 수 있다.
    - 청크란? 스트림에 쓰거나 스트림에서 읽은 단일 데이터 조각. 모든 유형이 될 수 있으며 서로 다른 유형의 청크가 스트림에 포함될 수 있다. 특정 스트림의 가장 최소, 즉 원자 단위의 데이터가 아닌 경우가 많으므로 유의.([출처. streams spec](https://streams.spec.whatwg.org/#chunk))

- 참고자료
    - [MDN Streams API](https://developer.mozilla.org/ko/docs/Web/API/Streams_API)
    - [MDN ReadableStream](https://developer.mozilla.org/ko/docs/Web/API/ReadableStream)
    - [모던자바스크립트 fetch](https://ko.javascript.info/fetch)


</br>

### Unicode

- wiki. 전 세계의 모든 문자를 컴퓨터에서 일관되게 표현하고 다룰 수 있도록 설계된 산업 표준. 숫자와 글자, 키와 값이 1:1로 매핑된 형태의 코드

</br>

### CORS란

- Cross Origin Resource Sharing. 교차 출처 리소스 공유
- 두 개의 출처가 서로 갖다고 판단하는 로직은 간단하며, 두 URL의 구성요소 중 scheme, host, port 3가지가 모두 동이라혐ㄴ 됨
- `https://www.naver.com/`
    - scheme: https
    - host: www.naver.com
    - port: null(비공개)
- 현재 자신이 속한 출처(origin)을 기준으로 다른 출처에 API를 요청하게 되면 브라우저에서 이 요청으로 넘어오는 경과가 안전한지를 판단함. 이 떄 중요한 것은 **브라우저**가 판단의 주체라는 점이다. 비교 로직이 서버가 아닌 브라우저에 구현되어 있음
- 따라서 브라우저를 통하지 않고 서버 간 통신 시에는 정책이 적용되지 않음
- 또한 CORS 정책을 위반하는 리소스 요청때문에 브라우저에서 에러가 발생했다고 하더라도 network의 응답을 살펴보면 정상 로그만 남음. 서버는 응답을 정상적으로 하나, 그 응답을 브라우저가 받아 CORS 위반인지 아닌지를 판단, 결정하기 떄문임
- CORS는 다른 Origin에 대한 요청을 허용하는 정책임. 같은 Origin에서 http 통신을 하는 경우 자동으로 coocie가 request header에 들어가나 교차출처로 요청하는 상황에서는 그렇지 않음
- 동작 원리
    - 클라이언트가 다른 출처의 리소스를 요청할 때는 요청 헤더에 origin이라는 필드에 요청을 보내는 출처를 함께 담아 보냄
    - 서버가 응답할 때 헤더에 `Access-Control-Allow-Origin`이라는 값에 '허용된 출처'를 내려줌
    - 브라우저는 응답을 받은 뒤 오리진과 서버 응답에 있는 `Access-Control-Allow-Origin`을 비교한 뒤 응답이 유효한지 아닌지를 결정

- 참고. [내 노션 정리](https://ionized-yew-dea.notion.site/CORS-SOP-CSP-975af130acc0403c9ce57b71604aebf1)
