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

</br>

### MSW(Mock Service Worker)

</br>

### polyfill(폴리필)
