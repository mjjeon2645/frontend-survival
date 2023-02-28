# 1. Express

## 강의 요약 및 노트

### express에 대하여

- 굉장히 유명한 nodeJS의 서버 웹 프레임워크
- nodeJS로 무언가를 만든다면 기본적으로 express를 우선 고려할 만큼 많이 사용됨

### ts-node와 nodemon

- 개발시 ts-node를 사용하면 좋은 점 중 하나는 node 실행 시 ts 코드들을 별도로 노드로 컴파일을 한버 더 해줭야 하는데, 이 과정이 필요하지 않으므로 편리
- ts-node를 사용해서 컴파일할 때 코드를 수정하면 자동으로 반영되지 않음. -> **궁금한 점. 그럼 parcel은 왜 자동으로, 실시간으로 반영이 되었을까? 편하긴 servor는 어땠었지?
- 코드 뿐만 아니라 숨김파일이 아닌 파일들(ex. package.json)에서 발생하는 변동사항도 monitoring하기 때문에 서버가 자동으로 restart 됨
- 새로고침을 하면 변동사항을 반영해 줌
- nodemon의 경우 ts-node에 의존하고 있기 때문에 ts-node가 설치되지 않은 상태에서는 정상 실행이 되지 않음
- 편하게 사용하기 위해 package.json의 scripts 를 수정

```json
"start": "nodemon app.ts",
```

- express는 백엔드를 mocking해서 테스트를 진행해야 할 때, 백엔드 개발자와 소통할 때 활용할 수 있음
  
- 강의에서 예시로 나온 코드에 대한 간단한 해석

```javascript
// 1. express 함수를 가져온다.
import express from 'express';

const port = 3000;

// 2. express 함수를 실행하여 app이라는 인스턴스를 만든다.
const app = express();

// 3. 인스턴스에서 내가 하고싶은 것을 실제로 적용한다.
app.get('/', (request, response), => {
    response.send('Hello, world!');
});

app.listen(port, () => {
    console.log(`Server running at http://locahost:${port}`);
});
```

</br>

### REST API에 대하여

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
> http://www.example.com:80/path/to/myfile.html?key1=value1&key2=value2#SomewhereInTheDocument

#### 1. 프로토콜(Scheme)

- `http`는 프로토콜, 즉 규약
- URL의 첫 파트는 브라우저가 어떤 규약을 사용해야 하는지를 나타냄
- 보통 http, 또는 https 프로토콜을 사용하나 `mailto:`, 또는 `ftp:` 프로토콜도 있음

#### 2. 도메인 이름(Domain Name)

- `www.example.com` 은 도메인 이름
- 기억하기 쉬운 주소이고 특정 서버의 IP 주소를 가리킴(웹 서버의 위치를 지정)
- 직접 IP address를 사용하는 것도 가능하나 편리하지 않기 때문에 웹에서 주로 사용되지는 않음

#### 3. 포트(Port)

- `:80`에 해당
- 웹 서버에서 자원에 접근하기 위해 사용하는 '관문(gate)'을 가리킴
- 만약 웹 서버가 리소스에 접근하기 위해 표즌 http 포트(http는 80, https는 443)를 사용한다면 포트 번호는 생략하나(optional) 그렇지 않다면 필수

#### 4. 경로(Path)

- `/path/to/myfile.html`. 슬래시가 들어가는 것에 유의
- 웹 서버에서 자원에 대한 경로
- 초기 웹에서는 물리적 파일 위치를 나타냈으나 현대에는 실제 물리적 경로를 나타내는 것이 아닌 웹 서버에서 추상화하여 보여줌
- URL은 ASCII 문자 세트에 포함된 문자만 허용하며, 이 문자 세트에서도 허용되지 않는 문자일 경우 '%'로 시작하는 16진수 문자 코드를 넣게 됨. 또한 공백 문자는 '+' 또는 공백을 나타내는 '%20'으로 치환해야 함

#### 5. 쿼리 스트링(Query String), 추가 파라미터(Parameter)

- `key1=value1&key2=value2`는 웹 서버에 제공하는 추가 파라미터로 '&' 기호로 구분된 키/값으로 짝을 이룬 일련의 쿼리 리스트들로 이루어져 있음
- 웹 서버는 자원을 반환하기 전 추가 작업을 위해 이런 파라미터들을 사용할 수 있음

#### 6. 앵커(Anchor)

- 앵커는 일종의 자원 안에서의 'bookmark'임. 즉, bookmarked 지점에 위치된 내용을 보여주기 위해 브라우저에게 방향을 알려주는 '닻'의 역할
- 예로, HTML 문서에서 브라우저는 anchor가 정의한 곳의 점을 스크롤 할 것.  비디오나 오디오 문서라면 브라우저는 anchor가 나타내는 '시간'을 찾으려 할 것
- 중요한 것은 '#' 뒤에 오는 부분은 무가치하며, 'fragment identifier(부분 식별자)'로 알려지므로 요청이 서버에 절대 보내지지 않음

- 참고문서
    - [MDN What is a URL?](https://developer.mozilla.org/ko/docs/Learn/Common_questions/Web_mechanics/What_is_a_URL)
    - [Google 단순한 URL 구조 유지하기](https://developers.google.com/search/docs/crawling-indexing/url-structure?hl=ko)
    - [CODNS 공식 사이트](http://www.codns.com/b/B05-195)
    - [더 나은 웹](https://www.betterweb.or.kr/blog/url%EC%9D%B4%EB%9E%80/)
    - [아마존 공식블로그. 웹 브라우저에 URL을 입력하면 어떤 일이 생기나요?](https://aws.amazon.com/ko/blogs/korea/what-happens-when-you-type-a-url-into-your-browser/)

</br>

### REST API

- [REST API 정리본 업데이트 from week3](../week3/react-component.md/#rest-api-graphql)

</br>

### HTTP Method(CRUD)

- CRUD는 Create, Read, Update, Delete의 약어로 저장된 데이터에서 작업할 수 있는 방법들을 의미함. CRUD는 일반적으로 데이터베이스 또는 데이터스토어에서 수행되는 작업을 의미하지만 데이터가 실제로 삭제되지 않지만 상태값을 통해 '삭제된 것'으로 표시되는 'soft delete'와 같은 애플리케이션 상위 수준 기능에도 적용할 수 있음  

- HTTP Method는 한마디로 '서버에 어떤 임무를 부여하고 싶은지', '서버에 대해 무엇을 하고 싶은지'를 나타내는 것. '클라이언트가 서버에 요청의 목적 및 종류를 알리는 수단'
- HTTP 요청 시에 메시지 시작 줄에 표기됨

#### 1. [GET](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods)

- 특정한 리소스를 가져오도록 요청함(CRUD -> Read)
- GET을 통한 요청은 URL 주소 끝에 파라미터(query string)로 포함되어 전송됨
- 특징
    - [캐시 가능](https://developer.mozilla.org/ko/docs/Glossary/Cacheable)
    - 브라우저 히스토리에 남음
    - 북마크 될 수 있음
    - 길이 제한이 있음(브라우저마다 제한이 다름)
    - 보안 측면에서 중요한 정보를 다루어서는 안됨(파라미터에 모두 노출되기 때문)
    - 데이터 요청시에만 사용됨(수정, 삭제 등의 기능이 없으므로 반드시 데이터 요청, READ)
    - 성공 응답에 본문이 존재함
    - [멱등임](https://developer.mozilla.org/ko/docs/Glossary/Idempotent)

#### 2. [POST](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/POST)

- 서버로 데이터(엔티티)를 전송함. 요청 시 본문의 유형은 `Content-Type` 헤더로 나타냄
- 데이터 전송을 통해 리소스를 새로 생성하거나 업데이트 할 수 있음([CRUD](https://developer.mozilla.org/en-US/docs/Glossary/CRUD) -> Create)
- 특징
    - 캐시되지 않음
    - 브라우저 히스토리에 남지 않음
    - 요청이 북마크되지 않음
    - 데이터 길이에 제한 없이 요청 가능
    - 성공 응답에 본문이 존재함
    - 멱등이 아님

> [GET, POST 차이점](https://noahlogs.tistory.com/35)
> - 사용 목적: 서버 리소스에서 데이터 요청시에는 GET. 서버의 리소스 새로 생성, 또는 업데이트 시 POST 사용
> - 요청 body 유무: GET은 없음(URL 파라미터에 요청하는 데이터를 query string 형태로 담아서 보냄). POST는 있음
> 멱등성(idempotent). -> 동일한 요청을 한 번 보내는 것과 여러 번 연속으로 보내는 것이 같은 효과를 지니고, 서버의 상태도 동일하게 남을 때, 해당 HTTP 메서드가 멱등성을 가졌다고 말함

#### 3. [PUT](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/PUT)

- 요청 페이로드를 사용해 새로운 리소스를 생성하거나, 또는 대상 리소스를 나타내는 데이터를 대체함
- overwrite 하며 전체가 다 바뀜. 즉 리소스 전체 수정이 되는 요청임
- 특징
    - 요청에 본문이 존재함
    - 성공 응답에 본문이 없음
    - 안전하지 않음
    - 멱등임
    - 캐시가 가능하지 않음

#### 4. [PATCH](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/PATCH)

- 리소스의 부분, 일부 수정을 할 때 사용됨
- `PUT` 메서드가 문서 전체의 완전한 교체를 허용하는 반면 PATCH 메서드는 멱등x. 또한 다른 리소스에게 부수효과(side-effects)를 일으킬 가능성이 있음

#### 5. [DELETE](https://developer.mozilla.org/ko/docs/Web/HTTP/Methods/DELETE)

- 지정한 리소스 삭제
특징
    - 요청에 본문 존재할 수도, 아닐 수도
    - 성공 응답에 본문 존재 할 수도, 아닐 수도
    - 안전하지 않음
    - 멱등임
    - 캐시는 가능하지 않음
