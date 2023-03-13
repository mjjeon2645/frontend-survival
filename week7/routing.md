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

** 잘 모르겠는 것 2. reflect는 뭐임..

```ts
const Page = Reflect.get(pages, path) || HomePage;
```

---

## 키워드 정리

### HTML DOM API

#### Location

#### pathname
