# 3. Router

## 강의 요약 및 노트

- 현재 상태에서 `App.tsx`를 보았을 때 두 가지 정보를 취할 수 있음
    - 어떤 레이아웃을 가지고 있는가
    - 어떻게 라우팅이 되는가

- App을 살려서 써도 되고 필요 없다고 생각된다면 layout만 두는 방식으로 해도 됨(선택)

---

## 키워드 정리

### ReactRouter - RouterProvider

- 모든 라우터 객체가 이 컴포넌트로 전달되어 앱을 렌더링하고, 나머지 API를 활성시킴
- 용례

```ts
import {
  createBrowserRouter,
  RouterProvider,
} from "react-router-dom";

const router = createBrowserRouter([
  {
    path: "/",
    element: <Root />,
    children: [
      {
        path: "dashboard",
        element: <Dashboard />,
      },
      {
        path: "about",
        element: <About />,
      },
    ],
  },
]);

ReactDOM.createRoot(document.getElementById("root")).render(
  <RouterProvider
    router={router}
    fallbackElement={<BigSpinner />}
  />
);
```

- 참고
    - [react router - router provider](https://reactrouter.com/en/main/routers/router-provider)
