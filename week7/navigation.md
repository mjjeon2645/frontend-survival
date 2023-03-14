# 4. Navigation

## 강의 요약 및 노트

- Link, NavLink
- Navigate -> 무조건 리다이렉션
- 리다이렉션을 위한 route 테스트가 죽어버리는 이유는 polyfill 문제. 예전에 `fetch`에 대해서도 폴리필 한 것 처럼 `whatwg-fetch`를 임포트해서 해결 가능

- Navigate를 사용해도 되고, useNavigate 훅으로 navigate를 불러오는 방법을 더 많이 쓰긴 함

---

## 키워드 정리

### Web APIs - History

- DOM의 Window 객체는 history 객체를 통해 브라우저의 세션 기록에 접근할 수 있는 방법을 제공한다. history는 사용자를 자신의 방문 기록 앞과 뒤로 보내고, 기록 스택의 콘텐츠도 조작할 수 있는 유용한 메서드와 속성을 가진다.
    - `history.back()`: 뒤로가기
    - `history.forward()`: 앞으로 가기
    - `history.go()`: 세션 기록에서 현재 페이지의 위치를 기준으로 상대적인 거리에 위치한 특정 지점까지 이동(예. 한페이지 뒤로 가기 `history.go(-1)`)
    - `History.pushState()`: 주어진 데이터를 지정한 제목(제공한 경우 URL도)으로 세션 기록 스택에 넣음
    - `History.replaceState()`: 세션 기록 스택의 제일 최근 항목을 주어진 데이터, 지정한 제목 및 URL로 대체
- 용례

```ts
window.onpopstate = function(event) {
  alert(`location: ${document.location}, state: ${JSON.stringify(event.state)}`)
}

history.pushState({page: 1}, "title 1", "?page=1")
history.pushState({page: 2}, "title 2", "?page=2")
history.replaceState({page: 3}, "title 3", "?page=3")
history.back() // alerts "location: http://example.com/example.html?page=1, state: {"page":1}"
history.back() // alerts "location: http://example.com/example.html, state: null"
history.go(2)  // alerts "location: http://example.com/example.html?page=3, state: {"page":3}"
```

- 참고
    - [MDN History API](https://developer.mozilla.org/ko/docs/Web/API/History_API)
    - [MDN History](https://developer.mozilla.org/ko/docs/Web/API/History)

</br>

### React Router - NavLink, Link, Navigate, useNavigate

#### NavLink

- 활성(active) 또는 대기(pending) 여부를 알 수 있는 특별한 종류의 <Link>. 현재 선택된 탭을 표시하려는 경우나 탭 집합과 같은 탐색 메뉴를 만들 때 유용하며 화면 리더와 같은 보조기술에 유용한 컨텍스트를 제공한다.
- 용례

```js
import { NavLink } from "react-router-dom";

<NavLink
  to="/messages"
  className={({ isActive, isPending }) =>
    isPending ? "pending" : isActive ? "active" : ""
  }
>
  Messages
</NavLink>;
```

- 참고. [리액트 라우터 NavLink](https://reactrouter.com/en/main/components/nav-link)

#### Link

- 사용자가 클릭하거나 탭 할 때 다른 페이지로 이동할 수 있게 해 준다. 'react-router-dom'에서 <Link>는 링크하는 리소스를 가리키는 실제 href가 있는 접근 가능한 <a> 엘리먼트를 렌더링한다. <Link>를 마우스 오른쪽 버튼으로 클릭하는 것과 같은 동작이 작동한다.
- 타입

```ts
declare function Link(props: LinkProps): React.ReactElement;

interface LinkProps
  extends Omit<
    React.AnchorHTMLAttributes<HTMLAnchorElement>,
    "href"
  > {
  replace?: boolean;
  state?: any;
  to: To;
  reloadDocument?: boolean;
  preventScrollReset?: boolean;
  relative?: "route" | "path";
}

type To = string | Partial<Path>;
```

- 참고. [리액트 라우터 Link](https://reactrouter.com/en/main/components/link)

#### Navigate

- 렌더링 될 때 현재 위치를 변경한다. `useNavigate`를 둘러싸는 컴포넌트 래퍼임
- 타입 선언

```ts
declare function Navigate(props: NavigateProps): null;

interface NavigateProps {
  to: To;
  replace?: boolean;
  state?: any;
  relative?: RelativeRoutingType;
}
```

- 용례

```js
import * as React from "react";
import { Navigate } from "react-router-dom";

class LoginForm extends React.Component {
  state = { user: null, error: null };

  async handleSubmit(event) {
    event.preventDefault();
    try {
      let user = await login(event.target);
      this.setState({ user });
    } catch (error) {
      this.setState({ error });
    }
  }

  render() {
    let { user, error } = this.state;
    return (
      <div>
        {error && <p>{error.message}</p>}
        {user && (
          <Navigate to="/dashboard" replace={true} />
        )}
        <form
          onSubmit={(event) => this.handleSubmit(event)}
        >
          <input type="text" name="username" />
          <input type="password" name="password" />
        </form>
      </div>
    );
  }
}
```

- useNavigate 훅이 있으면 훅을 사용할 수 없는 React.component 하위 클래스에서 이 기능을 더 쉽게 사용할 수 있음

- 참고. [react router - Navigate](https://reactrouter.com/en/main/components/navigate)

#### useNavigate

- 공식문서에서는 리다이렉션이 데이터에 대한 응답일 때, 이 훅을 사용하는 것 보다는 `redirect` 사용을 권장하고 있음([redirect](https://reactrouter.com/en/main/fetch/redirect))
- useNavigate hook은 navigate 함수를 반환
- 용례

```js
import { useNavigate } from "react-router-dom";

function useLogoutTimer() {
  const userIsInactive = useFakeInactiveUser();
  const navigate = useNavigate();

  useEffect(() => {
    if (userIsInactive) {
      fake.logout();
      navigate("/session-timed-out");
    }
  }, [userIsInactive]);
}
```

- 타입선언

```ts
declare function useNavigate(): NavigateFunction;

interface NavigateFunction {
  (
    to: To,
    options?: {
      replace?: boolean;
      state?: any;
      relative?: RelativeRoutingType;
    }
  ): void;
  (delta: number): void;
}
```
