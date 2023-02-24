# 5. usehooks-ts

## 강의 요약 및 노트

---

## 키워드 정리

### usehooks-ts

1. useBoolean

2. useEffectOnce

3. useFetch

4. useInterval

5. useEventListener

6. useLocalStorage

7. useDarkMode

</br>

### swr

- 데이터 가져오기를 위한 react hooks라고 함. swr는 먼저 캐시(스테일)로부터 데이터를 반환한 후, fetch 요청(재검증)을 하고, 최종적으로 최신화된 데이터를 가져오는 전략([출처. 공식 홈페이지 메인](https://swr.vercel.app/ko))
- swr 사용 시 컴포넌트는 지속적이며 자동으로 데이터 업데이트 스트림을 받게 됨. UI는 빠르고 반응적  

- 설치 명령어는 아래 참조

```bash
npm i swr
```

-- 기본 사용 형태

```javascript
import useSWR from 'swr'

function Profile() {
  const { data, error, isLoading } = useSWR('/api/user', fetcher)

  if (error) {
    return (
        <div>failed to load</div>
    )
  }; 

  if (isLoading) {
    return (
        <div>loading...</div>
    )
  };

  return (
    <div>hello {data.name}!</div>
  )
}
```

- useSWR의 파라미터는 key 문자열, fetcher 함수
    - key는 데이터의 고유한 식별자로 일반적으로 API URL을 나타내며 fetcher로 전달됨
    - fetcher는 데이터를 반환하는 어떠한 비동기 함수도 될 수 있음. 네이티브 fetcher 또는 axios와 같은 도구 사용 가능([fetch, axios, graphql 사용 예시 참고-공식문서](https://swr.vercel.app/docs/data-fetching))
- useSWR은 data, error, isLoading, isValidating(fetcher 함수의 상태에 의존)을 리턴함
- useSWR의 특징 중 하나는 한 번 fetch한 원격 상태의 데이터를 내부적으로 캐시하고 다른 컴포넌트에서 동일한 상태를 사용하고자 할 경우 이전에 캐시했던 상태를 그대로 리턴해주므로 서로 다른 컴포넌트가 동일한 상태를 공유할 수 있음 -> 또한 캐시한 상태에서 변동사항이 없다면 다시 fetch하지 않기 떄문에 중복되는 fetch는 줄어들지 않을까!?. 결국 SWR은 원격상태와 로컬 상태를 하나로 통합하는 역할이며 그게 가능한 이유는 SWR이 내부적으로 적절한 타이밍에 지속적으로 데이터를 폴링하기 떄문. (사실 실시간 까지는 아니지만 충분히 용납 가능한 수준으로 최신 데이터로 갱신. SWR은 브라우저 창이 focus를 얻을 때, 또는 네트워크가 offline에서 online으로 바뀔 때, 브라우저 탭을 전환할 때 자동으로 데이터를 fetch). polling 주기를 직접 설정하는 것도 가능 
- mutate 함수가 호출되면 상태를 즉시 다시 fetch하고 데이터를 갱신하는데, 만약 fetch 없이 로컬의 캐시되어 있는 상태만 갱신하고자 한다면 `mutate(user, false)`, 즉 첫 번째 인자로 갱신할 데이터, 두 번째 인자로 데이터 fetch 여부를 받을 수 있음. 이 mutate()는 프라미스.

- 참고하기
    - [SWR 공식 홈페이지](https://swr.vercel.app/ko)
    - [카카오엔터 FE 기술블로그: React에서 서버 데이터를 최신으로 관리하기(React Query, SWR)](https://fe-developers.kakaoent.com/2022/220224-data-fetching-libs/)
    - [Redux 를 넘어 SWR 로(2)](https://min9nim.vercel.app/2020-10-05-swr-intro2/)

</br>

### react-query

- 리액트 라이브러리로 데이터 fetching, 캐싱, 동기화, 서버 사이드 데이터 업데이트 등을 쉽게 만들어 줌

- install 방법

```bash
npm i @tanstack/react-query

npm i @tanstack/react-query-devtools
```

- 기본 사용 예시

```typescript
import {
  QueryClient,
  QueryClientProvider,
  useQuery,
} from '@tanstack/react-query'

const queryClient = new QueryClient()

export default function App() {
  return (
    <QueryClientProvider client={queryClient}>
      <Example />
    </QueryClientProvider>
  )
}

function Example() {
  const { isLoading, error, data } = useQuery({
    queryKey: ['repoData'],
    queryFn: () =>
      fetch('https://api.github.com/repos/tannerlinsley/react-query').then(
        (res) => res.json(),
      ),
  })

  if (isLoading) return 'Loading...'

  if (error) return 'An error has occurred: ' + error.message

  return (
    <div>
      <h1>{data.name}</h1>
      <p>{data.description}</p>
      <strong>👀 {data.subscribers_count}</strong>{' '}
      <strong>✨ {data.stargazers_count}</strong>{' '}
      <strong>🍴 {data.forks_count}</strong>
    </div>
  )
}
```

- 참고하기
    - [공식 홈페이지](https://tanstack.com/query/latest)
    - [카카오 테크블로그. My 구독의 React Query 전환기](https://tech.kakao.com/2022/06/13/react-query/)
    - [위 내용 관련 발표자료](https://speakerdeck.com/kakao/nune-boiji-anhneun-gaeseon-mygudogyi-reduxeseo-react-query-jeonhwan-gyeongheom-gongyu)
    - [카카오페이 테크블로그. 카카오페이 프론트엔드 개발자들이 React Query를 선택한 이유](https://tech.kakaopay.com/post/react-query-1/)
    - [트렌비 기술블로그. 찜으로 찜해보는 react-query](https://tech.trenbe.com/2022/08/08/%EC%B0%9C%EC%9C%BC%EB%A1%9C%EC%B0%9C%ED%95%B4%EB%B3%B4%EB%8A%94react-query.html)
    - [우아한 형제들 기술블로그. Store에서 비동기 통신 분리하기(feat. React Query](https://techblog.woowahan.com/6339/)
    - [리액트 쿼리 관련 블로그 글 모음](https://tkdodo.eu/blog/practical-react-query)
