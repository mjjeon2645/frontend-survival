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

- Jest
- Describe-Context-It 패턴
- React Testing Library
