# 1. Express

## 강의 요약 및 노트

### express에 대하여

- 굉장히 유명한 nodeJS의 서버 웹 프레임워크
- nodeJS로 무언가를 만든다면 기본적으로 express를 우선 고려할 만큼 많이 사용됨

### ts-node와 nodemon

- ts-node를 사용해서 컴파일할 때 코드를 수정하면 자동으로 반영되지 않음. -> **궁금한 점. 그럼 parcel은 왜 자동으로, 실시간으로 반영이 되었을까? 편하긴 servor는 어땠었지?
- 코드 뿐만 아니라 숨김파일이 아닌 파일들(ex. package.json)에서 발생하는 변동사항도 monitoring하기 때문에 서버가 자동으로 restart 됨
- 새로고침을 하면 변동사항을 반영해 줌
- nodemon의 경우 ts-node에 의존하고 있기 때문에 ts-node가 설치되지 않은 상태에서는 정상 실행이 되지 않음
- 편하게 사용하기 위해 package.json의 scripts 를 수정

```json
"start": "nodemon app.ts",
```

- express는 백엔드를 mocking해서 테스트를 진행해야 할 때, 백엔드 개발자와 소통할 때 활용할 수 있음

</br>

### REST API

- 대개는 '필딩 제약 조건' 4가지를 모두 만족하지 않고, Resource와 HTTP verb만 도입하는 수준으로 사용한다고 함
- Resource에 대해서 CRUD만 함.

---

## 키워드 정리

### Express란

- (from 공식문서) 웹 및 모바일 애플리케이션을 위한 일련의 강력한 긴으을 제공하는 간결하고 유연한 Node.js 웹 애플리케이션 프레임워크
- HTTP 유틸리티 메서드 및 미들웨어를 통해 쉽고 빠르게 API 작성 가능
- [from MDN](https://developer.mozilla.org/ko/docs/Learn/Server-side/Express_Nodejs/Introduction). Express가 제공하는 매커니즘
    - HTTP 통신 요청(GET, POST, DELETE 등)에 대한 핸들러를 만든다.
    - 템플릿에 데이터를 넣어 응답(response)을 만들기 위해 view의 렌더링 엔진과 결합(integrate)한다.
    - 접속을 위한 포트나 응답 렌더링을 위한 템플릿 위치같은 공통 웹 어플리케이션 세팅을 한다.
    - 핸들링 파이프라인(request handling pipeline) 중 필요한 곳에 추가적인 미들웨어 처리 요청을 추가한다.

** 궁금한 점. 미들웨어 패키지가 뭐지? 미들웨어란 뜻이 뭘 의미하는건지 잘 모르겠는데 찾아보기.

- 미들웨어는 정적 파일 제공, 오류 처리, HTTP 응답 압축까지 광범위하게 사용됨
- 추후에 읽고 정리해보기. [Node.js Middleware란?](https://lakelouise.tistory.com/211)

</br>

### URL 구조

- [URL이란? from MDN](https://developer.mozilla.org/ko/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL) Uniform Resource Locators(URLs)
- 웹에 게시된 어떤 자원을 찾기 위해 브라우저에 의해 사용되는 메커니즘. URL은 Uniform Resource Locator, 즉 인터넷에서의 자원 위치를 나타낸다. 웹에서 정해진 유일한 자원의 주소이며 이론적으로는 유일한 URL이 유일한 자원을 가리킨다. 자원이라 함은 HTML, 페이지, CSS 문서, 이미지 등이 될 수 있다.
- 예. http://www.

</br>

### REST API

</br>

### HTTP Method(CRUD)
