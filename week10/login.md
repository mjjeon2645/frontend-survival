# 1. 로그인

## 강의 요약 및 노트

- 로그인은 사용자를 unique하게 특정하는 id(또는 이메일 등), 그리고 그 unique한 사용자가 자기 자신이 맞는지를 증빙하기 위한 pw를 백엔드에 전송
- 모든 행위 때마다 다시 id, pw를 받을 수 없기 때문에 이를 대체할 것을 들고있으면서 매 요청마다 함께 보내는데 이를 access token이라고 할 수 있음

- html 속성에 undefined가 넘어가면 아예 속성을 만들어주지 않는다.

- eslint 속성에 추가되는 내용 뭔지 잘 모르겠음 -> 살펴보기.

- ReferenceError: Request is not defined -> 해당 메시지가 테스트에서 보일 때 node의 fetch에 대한 polyfill이 필요. 인스톨 후 `setupTest.ts` 파일에 `import 'whatwg-fetch'` 추가

```bash
npm i -D whatwg-fetch
```

- 매 호출시마다 header를 보낼 때, instance를 만들어 보내면 편하다! 예전에는 매번 header에 accessToken을 보내는 방식으로 했었는데... axios의 instance를 최대한 활용하자.

<hr />

## 키워드 정리
