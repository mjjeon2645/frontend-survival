# 5. Parcel & ESLint

## 강의 요약 및 노트

- SWC => 우리나라 분이 만드신 타입스크립트 컴파일러라고 함(아하..)
- 교재에 누락된 것 같아서 링크 아래와 같이 다시 정리. vite는 첨 들어보는데 확인해봐야겠다.
> https://github.com/ahastudio/til/tree/main/parcel
https://github.com/ahastudio/til/tree/main/vite

- 새롭게 알게된 점. `parcel-reporter-static-files-copy` 패키지라는 것?? static 폴더의 파일을 정적 파일로 serving한다는게 뭔얘긴지 잘 모르겠는데 알아보기. 근데 이제까지 이걸 설치하지 않았었어도 별다른 문제가 없었던 것 같은데 왜 설치하는걸까? 왜?? 뭘 위해서인지??? 비교해보자.

```bash
npm i -D parcel-reporter-static-files-copy
```

- `touch .parcelrc` 명령어로 해당 파일 생성 후 아래 내용 붙여주기
- 아래 내용 붙인 후 parcel 실행하면 static 폴더가 없다는 내용. 해당 폴더 안에 있는 파일들을 정적 파일로 serving하는 것인데 별도 설정 바꾸지 말고 `mkdir static` 명령어로 폴더 생성

```JSON
{
  "extends": ["@parcel/config-default"],
  "reporters":  ["...", "parcel-reporter-static-files-copy"]
}
```

- 💡 별도로 찾아보기. 정적 프로그램 분석 - 동적 프로그램 분석. 아주 단순하게는 정적 프로그램 분석이란 실제 실행 없이 컴퓨터 s/w 분석하는 것. 소스코드로 분석하는 것. 동적 프로그램 분석은 실행하고 끄고 하면서 메모리 새는것(?) 같은 것들을 잡는 것

- 타입스크립트에서 타입체크를 위한 컴파일, 그리고 린트를 같이 사용하는 명령어

```bash
npm run lint && npm run check
```

---

## 키워드 정리

- Bundler(번들러)
    - Parcel
- Lint(린트)
    - ESLint
    