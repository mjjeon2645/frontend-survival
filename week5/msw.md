# 3. MSW

## 강의 요약 및 노트

### MSW

- 원래는 서비스 워커라는, 브라우저에서 사용할 수 있는 것이 있다. 오프라인 상황에서(인터넷 연결이 안된 상황에서) 특정 주소에 무언가를 요청할 때 그것을 가로채서(프록시) 어떤 결과값을 주는 것.
- 코드 레벨이 아닌 네트워크 레벨에서 가짜를 구현. 네트워크 레벨에서 프록시를 이용해서 가짜로 해주는 것
- 노드, 브라우저 둘 다 지원함. 우리는 jest를 이용해 node 환경에서 확인하려 함

- `waitFor()` 될 때까지 기다린다 보다는 될 때까지 확인한다 라고 표현하는 것이 더 나을 것 같음. waitFor 명세서에 들어가보면 promise로 반환하는 것을 확인할 수 있음. 그래서 `async`와 `await`를 붙여야 하는 것임

- (나도 겪었던 문제 같은데 다시한번 제대로 확인해보기.) 최신 node에 `fetch` 문법이 들어왔음. 근데 내가 사용하고 있는 것에서는 fetch가 없음. fetch가 브라우저에서는 되는데 node에서는 되지 않음. -> 깃허브에서 만든 fetch polyfill이 있음. fetch가 `window`에 있는건데 node에는 window가 없기 때문에 발생하는 문제(아... 그래서 브라우저에서만 된다는게 그런 의미구나...). 물론 최신 node에서 fetch를 사용할 수 있지만 LTS 버전에 fetch가 아직 들어오지 않은 것이라면 나는 LTS 버전을 사용하고 있기 때문에 적용이 안되고 있을 것임.  
이를 처리하기 위해 아래와 같이 설치 가능

```bash
npm i -D whatwg-fetch
```

모든 테스트에 저 내용을 다 적용해줄 수 없으니 `setupTests.ts`에 한번 import 해주어 프록시 적용받는 것에 다 적용될 수 있도록 함

```js
import 'whatwg-fetch';

import server from './mocks/server';

beforAll(() => server.listen({ onUnhandledRequest: 'error' }));

afterAll(() => server.close());

afterEach(() => server.restHandlers());
```

---

## 키워드 정리

### Service worker

#### MDN 출처: 정의와 특징

- 웹 응용 프로그램, 브라우저, (사용 가능한 경우) 네트워크 사이의 **프록시 서버 역할**을 한다.
- 출처(origin)와 경로에 대해 등록e된 event-driven 워커이며 JavaScript 파일 형태이다.
- 연관된 웹 페이지/사이트를 통제해 탐색, 리소스 요청을 가로채 수정하고, 리소스를 매우 세분화된 방식으로 캐싱한다. 이를 통해 웹 앱이 어떤 상황에서 어떻게 동작해야 하는지를 완벽히 바꿀 수 있다. (대표적 상황으로 네트워크를 사용하지 못할 때임)  
**** 잘 모르겠다. 찾아볼것 -> 왜 캐싱인가? 왜 service worker가 캐싱하는지? 이제까지의 경험으로는 이게 왜 캐싱역할을 하는건지 이해가 안됨
- 워커 컨텍스트에서 실행되기 때문에 DOM 접근이 불가하다. 
- 서비스 워커는 비동기적으로 설계됐고, 이로 인해 동기적 XHR이나 웹 저장소 등의 API를 서비스 워커 내에서 사용 불가
- 보안상의 이유로 HTTPS에서만 동작함. 중간자 공격에 의한 악성 코드 삽입에 취약하기 때문
  
- 개발 의도: 효과적인 오프라인 경험을 생성, 네트워크 요청을 가로채서 네트워크 사용 가능 여부에 따라 적절한 조치를 취하고 서버의 asset을 업데이트 하기 위함. 또한 푸시 알림과 백그라운드 동기화 API에 대한 액세스도 허용

#### 서비스 워커의 위치

- 카카오 엔터 내용 보고 정리해보기

#### 서비스 워커의 생명주기

- 카카오 엔터 내용 보고 정리해보기

- 참고
    - [MDN Service Worker API](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
    - [카카오엔터 FE 블로그 - 서비스 워커에 대해 알아보고 Mock Response 만들기](https://fe-developers.kakaoent.com/2022/221208-service-worker/)

</br>

### MSW(Mock Service Worker)

- 브라우저와 node에서 사용할 수 있는 API mocking library. 서버향의 네트워크 요청을 가로채어 모의 응답(Mocked response)을 보내주는 역할을 함. MSW 라이브러리를 통하면 Mock 서ㅓㅂ

</br>

### polyfill(폴리필)

- 
