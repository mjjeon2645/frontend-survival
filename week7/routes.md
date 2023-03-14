# 2. Routes

## 강의 요약 및 노트

- react-router-dom 설치하기

```bash
npm i react-router-dom
```

- App은 `BrowserRouter`로 감싸주어야 하고, 테스트 환경에서는 `MemoryRouter`로 감싸주어야 함. MemoryRouter로 감쌀 때 `initialEntries`를 배열로 잡아줄 수 있음

---

## 키워드 정리

### 라우터란?

- 라우터, 혹은 라우팅 기능을 갖는 공유기. 컴퓨터 네트워크 간에 데이터 패킷을 전송하는 네트워크 장치이다.
- 간단히 말해 서로 다른 네트워크 간에 중계 역할을 해 주는 장치
- 패킷의 위치를 추출하여, 그 위치에 대한 최적의 경로를 지정하고 이 경로르 따라 데이터 패킷을 다음 장치로 전달한다.

- React Router 관점에서 라우팅을 생각해보면 '출발지에서 목적지까지의 경로를 결정하는 기능'이다. 사용자가 요청한 URL에 따라 해당 URL에 맞는 페이지(컴포넌트)를 보여주는 것. <a> 태그를 사용해도 페이지 이동이 가능하나 화면을 모두 새로고침 한다음 페이지를 이동한다는 단점. 다만 리액트 라우터는 새로운 페이지를 로드하지 않고 하나의 페이지 안에서 필요한 데이터만 가져오는 형태이므로 불필요한 렌더링을 막을 수 있음

- 참고
    - [위키피디아 라우터](https://ko.wikipedia.org/wiki/%EB%9D%BC%EC%9A%B0%ED%84%B0)
    - [poiemaweb js SPA](https://poiemaweb.com/js-spa)

</br>

### React Router

- React Router는 '클라이언트 측 라우팅'을 활성화하는 라이브러리
- 전통적인 웹사이트는, 브라우저가 웹 서버에 문서를 요청하면 이를 다운로드 받고 평가하여 서버에서 전송된 HTML을 렌더링한다. 사용자가 링크를 클릭하면 새 페이지에 대한 프로세스를 처음부터 다시 시작한다. 하지만 클라이언트 사이드의 라우팅을 사용하면 앱에서 서버에 다른 문서를 요청하지 않고도 링크 클릭으로 URL을 업데이트할 수 있다. 대신, 앱은 즉시 새 UI를 렌더링하고, 'fetch'로 데이터를 요청해 새 정보로 페이지를 업데이트 할 수 있다. 이는 브라우저가 완전히 새로운 문서를 요청하거나, 다음 페이지를 위해 CSS, JS 자원들을 다시 평가할 필요가 없어 사용자 경험을 증대시키며 애니메이션 같은 역동적인 사용자 경험을 제공할 수 있다.
- 클라이언트 사이드 라우팅은 'Router'를 생성함으로 가능하고, 페이지 링킹과 제출하기는 'Link'와 '<Form>'으로 실현시킬 수 있다.

- 참고
    - [리액트 라우터 공식 페이지 feature overview](https://reactrouter.com/en/main/start/overview)

#### Browser Router

- <BrowserRouter>는 clean URL을 사용한(?) 브라우저의 주소바의 현재 위치를 저장하고, 브라우저에 내장된 히스토리 스택을 이용해 navigate 한다.
- 타입은 아래와 같음

```typescript
declare function BrowserRouter(
  props: BrowserRouterProps
): React.ReactElement;

interface BrowserRouterProps {
  basename?: string;
  children?: React.ReactNode;
  window?: Window;
}
```

- 사용 형태는 아래와 같음

```javascript
import * as React from "react";
import { createRoot } from "react-dom/client";
import { BrowserRouter } from "react-router-dom";

const root = createRoot(document.getElementById("root"));

root.render(
  <BrowserRouter>
    {/* The rest of your app goes here */}
  </BrowserRouter>
);
```

#### Route

- 라우트는 라우터 생성 함수에 전달되는 객체로, URL 세그먼트를 컴포넌트, 데이터 로딩, 데이터 변형과 결합시킨다. 
경로 중첩을 통해 복잡한 앱 레이어와 데이터 종속승을 단순하고 선언적으로 만들 수 있다.
- 타입은 아래와 같다.

```typescript
interface RouteObject {
  path?: string;
  index?: boolean;
  children?: React.ReactNode;
  caseSensitive?: boolean;
  id?: string;
  loader?: LoaderFunction;
  action?: ActionFunction;
  element?: React.ReactNode | null;
  Component?: React.ComponentType | null;
  errorElement?: React.ReactNode | null;
  ErrorBoundary?: React.ComponentType | null;
  handle?: RouteObject["handle"];
  shouldRevalidate?: ShouldRevalidateFunction;
  lazy?: LazyRouteFunction<RouteObject>;
}
```

- 참고
    - [react router browser router](https://reactrouter.com/en/main/router-components/browser-router)

#### Memory Router

- `<MemoryRouter>`는 내부적으로 '배열'에 위치를 저장한다. 테스트와 같이 히스토리 스택을 완벽하게 제어해야 하는 시나리오에 이상적이다.
    - `<MemoryRouter initialEntries>`의 기본값은 ["/"]이다. (루트 '/' URL에 있는 단일 항목)
- 용례

```javascript
import * as React from "react";
import { create } from "react-test-renderer";
import {
  MemoryRouter,
  Routes,
  Route,
} from "react-router-dom";

describe("My app", () => {
  it("renders correctly", () => {
    let renderer = create(
      <MemoryRouter initialEntries={["/users/mjackson"]}>
        <Routes>
          <Route path="users" element={<Users />}>
            <Route path=":id" element={<UserProfile />} />
          </Route>
        </Routes>
      </MemoryRouter>
    );

    expect(renderer.toJSON()).toMatchSnapshot();
  });
});
```

- 타입

```javascript
declare function MemoryRouter(
  props: MemoryRouterProps
): React.ReactElement;

interface MemoryRouterProps {
  basename?: string;
  children?: React.ReactNode;
  initialEntries?: InitialEntry[];
  initialIndex?: number;
}
```

- 참고
    - [react router memory router](https://reactrouter.com/en/main/router-components/memory-router)
