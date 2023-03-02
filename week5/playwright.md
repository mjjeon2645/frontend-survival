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

</br>

### Headless Chrome

</br>

### Puppeteer

</br>

### Playwright

</br>

### CodeceptJS
