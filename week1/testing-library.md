# 4. Testing Library

## 강의 요약 및 노트

- describe-context(사실은 describe) - it
- React Testing Library에서 .getByText와 같이 get의 경우에는 찾으려는 내용이 없으면 에러를 냄.  하지만 query는 에러를 내지 않으므로 아래와 같이 변환해서 사용해볼 수 있음

```javascript
expect(screen.queryByText(/Hi/)).toBeFalsy();

// 또는 더 좋은 방법은
expect(screen.queryByText(/Hi/)).not.toBeInTheDocument();

// 언뜻 보면 같아보이지만 위에랑은 좀 다름. 이건 아예 존재하지 않는 것이 아니라 CSS로 가려놓은 것을 찾을 떄.
expect(screen.queryByText(/Hi/)).not.toBeVisible();
```

---

## 키워드 정리

### Jest

- Jest란? 한 마디로 자바스크립트 테스트 프레임워크
- 페이스북에서 만듦!
- 홈페이지 상에서는 babel, TS, node, React, Angular, Vue 등으로 만든 프로젝트에서 사용할 수 있다고 함

</br>

### Describe-Context-It 패턴

- 테스트를 계층 구조로 만들어서 테스트의 실행 흐름을 추후에 쉽게 확인할 수 있다. 마치 애플리케이션의 설명서처럼.
- `jest --verbose` 명령어로 계층구조의 테스트 문맥을 확인할 수 있음
- describe: 설명할 테스트의 대상(예. 함수명)을 명시함
- context: 테스트할 대상의 상황을 설명(when, 또는 with로 시작하기를 추천)
- it: 행동을 설명

</br>

### React Testing Library

- airbnb의 Enzyme의 대안으로 자리잡았다고 함.
- `@testing-library` family of packages helps you **test UI components in a user-centric way.** [인트로덕션 발췌](https://testing-library.com/docs/) 
- (나도 혼동되는 부분이었는데) React Testing Library는 Jest와 함께 React 내에서 테스트를 진행하므로 상호 보완관계라고 볼 수 있으며 엄밀하게는 RTL이 Jest를 포함하는 구조
- [참고링크. 테코블 블로그 글](https://tecoble.techcourse.co.kr/post/2021-10-22-react-testing-library/)
