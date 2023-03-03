# 4. Playwright

## 강의 요약 및 노트

- 실제로 브라우저에 띄웠다가 하는 것들 중에 정말 유명한 것이 셀레니움
- 그 밖에 화면에 띄우지 않는 headless 방법으로도 E2E 테스트가 가능
- headless chrome을 기반으로 하는 퍼피티어(puppeteer. 구글이 만듦)가 등장
- playwright은 퍼피티어와 api가 비슷하면서도 크롬 뿐만 아니라 모든 브라우저를 지원

- chromium을 사용하지 않고 그냥 chrome을 사용하기 위해선 config 파일에 channel을 추가해야 함

```js
const config: PlaywrightTextConfig = {
    use: {
        channel: 'chrome',
        baseURL: //...생략
    }
};
```

---

## 키워드 정리

### E2E(End to End) Test

- 애플리케이션 흐름이 예상대로 작동하는지를 확인하기 위해 애플리케이션의 흐름을 처음부터 끝까지 테스트하는 것을 의미
    - 유닛테스트, 통합테스트는 모듈의 무결성을 증명할 수 있으나 모듈의 무결성이 애플리케이션 동작의 무결성까지는 증명해줄 수 없음
    - 실제 사용자의 시나리오를 테스트 함으로써 애플리케이션 동작을 테스트하게 되고, 이 테스트를 통과함으로써 애플리케이션의 무결성을 증명
- 목적
    - 사용자 시나리오 시뮬레이션
    - 테스트 대상이 되는 시스템과 해다 ㅇ구성 요소들의 통합, 데이터 무결성을 검증하여 최종 사용자단의 경험에서 테스트하기 위함
- 종류
    - web 환경에서 Selenium, Cypress, TestCafe, CodeceptJS, Playwright 등

- 참고
    - [카카오엔터 FE 블로그 - E2E 테스트 도입 경험기](https://fe-developers.kakaoent.com/2023/230209-e2e/)
    - [Katalon-What is End to End Testing](https://katalon.com/resources-center/blog/end-to-end-e2e-testing)

</br>

### Headless Chrome

- Chrome 59에서 출시되며, 헤드리스 환경에서도 크롬 브라우저를 실행할 수 있는 방법. '크롬 없이 크롬을 실행'. 크롬과 Blink 렌더링 엔진이 제공하는 모든 최신 웹 플랫폼 기능을 커맨드 라인으로 가져옴
- 유용성
    - 헤드리스 브라우저는 눈에 보이는 UI shell이 필요 없는 서버 환경, 자동화된 테스트에 유용
- 과거 PhantomJS와 NightmareJS 등 다른 헤드리스 브라우저도 사용되었었음.
- 테스트 프레임워크도 자동화되어 있으며 대표적으로 Capybara, Jasmin이 있음. 이 두 소프트웨어 모두 사용자 스토리에 대한 시나리오를 시뮬레이션 하고, 행동 중심 개발을 기반으로 웹 애플리케이션 테스트를 자동화 함
- 참고
    - [크롬 디벨로퍼스 블로그](https://developer.chrome.com/blog/headless-chrome/)
    - [디버거 - what is the google chrome headless mode](https://www.debugbar.com/what-is-the-google-chrome-headless-mode/)

</br>

### Puppeteer

- puppeteer는 헤드리스 기능 도입 후 몇 달 후에 출시된 노드 라이브러리. 개발자 도구 프로토콜을 통해 헤드리스 크롬 또는 크롬을 제어할 수 있는 고차원의 API를 제공함
- 브라우저에서 수동으로 할 수 있는 대부분의 작업을 퍼피티어를 통해 수행 가능. 예시
    - 페이지의 스크린샷, PDF 생성
    - SPA를 크롤링하여 미리 렌더링 된 컨텐츠(SSR)를 생성함
    - 양식 제출, UI 테스트, 키보드 입력과 같은 작업을 자동화 함(매크로 기능)
    - 자동화된 최신 테스트 환경을 구축하여 최신버전 크롬에서 테스트
    - 사이트 타임라인 추적을 캡쳐하여 성능문제 진단
    - 크롬 익스텐션에 대한 테스트

- 참고
    - [Puppeteer 홈페이지](https://pptr.dev/)
    - [크롬 디벨로퍼스-Overview of puppeteer](https://developer.chrome.com/docs/puppeteer/overview/)
    - [블로그 글 - Puppeteer란](https://kkangdda.tistory.com/112)

</br>

### Playwright

- E2E 테스트 라이브러리 중 하나로 MS에서 만들었으며 크롬, 파이어폭스, 사파리 등의 브라우저와 윈도우, 리눅스, mac 등의 플랫폼을 모두 지원한다. 또한 타입스크립트, 자바스크립트, 파이썬, .NET, 자바 등의 언어를 지원한다.

- 참고하기
    - [블로그 번역글-Playwright로 E2E 테스트 작성하기](https://ui.toast.com/posts/ko_20210818)
    - [playwright 홈페이지](https://playwright.dev/)

</br>

### CodeceptJS

- codecepjs를 정리하려다가 문득 헷갈리는게 codeceptjs 홈페이지에 들어가보면 using playwright, using puppeteer이 있는데 혼란스럽다. codeceptjs면 codeceptjs지 갑자기 왜 다른 툴인 playwright랑 puppeteer를 같이 쓴다는건지 이해가 안되는데 다시 살펴보기...

- 참고
    - [codeceptjs 홈페이지](https://codecept.io/)
