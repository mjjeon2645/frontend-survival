# 1. 개발 환경

## 강의 요약 및 노트

- 메가테라 1기 강의를 들을 때 Node.js를 설치한 후 별다른 후작업을 진행하지 않았었다. 오랜만에 강의를 들으며 버전을 확인해보니 과거에 사용하던 v16.16.0이 default로 설정되어있는 것을 보았다. 시간이 아주 오래 지난 것도 아닌데 그 사이에 버전이 바뀌고 또다른 latest가 있는 걸 보니 반성이 되었다. 빠르게 변하고 있다는 걸 코스 시작 5분만에 새삼 느낀다.

```bash
// Node.js lts 버전 설치
fnm intall --lts

// 설치된 리스트 확인하기
fnm list

// 지정한 버전 default로 설정하기
fnm default $(fnm current)

// 현재 기준 LTS 최신버전 확인하기
node -v
```

</br>

- `.gitignore` 파일 설정 잊지말기. google에서 'github gitignore node'라고 검색만 해도 예시 확인 가능. 설정 후에 ignore되어야 하는 내용들(ex. node_modules, .parcel-cache, dist 등)이 포함되어 있는지 확인할 것

</br>

- 타입스크립트는 Dev 환경에서 활용하는 것으로 아래와 같은 명령어로

```bash
npm i -D typescript

npx tsc --init
```

> 마치 우리가 eslint 설치하고 실행할 때 `npx init --eslint`로 실행했던 것과 동일함.  
이번에 새로 알게 된 내용 -> npx 명령어 말고 node_modules로 시작하는 명령어  
예전에 이거랑 비슷한 상황이 있었는데 '왜 저렇게 하는게 가능하지?'라고 의문을 가졌었음. 이번 강의에서 짤막하게 짚고 넘어갔는데 좀 더 이해가 가는 너낌.  
바이너리 파일로 해당하는 루트에 가보면 실제 실행 파일이 존재함(tsc는 타입스크립트 컴파일러)  
`./node_modules/.bin/tsc`

- 생성된 json 파일에서 몇가지 설정 변경
    - "jsx": "react-jsx"

- 이후 eslint 설치 및 jest 사용을 위한 설정. extends에 `'plugin:react/jsx-runtime` 추가해주기.  
**중요.** 잊지 말고 .eslintignore 파일 작성하기. => 현재 익숙하지 않은 부분이기에 위에서부터 이어진 process 전체를 반복해서 학습할 필요가 있음  
해당 파일에는 아래와 같은 3종 세트가 빠지지 않도록 넣어주기~! 개발하면서 계속 추가될 것.

```bash
/node_modules/
/dist/
/.parcel-cache/
```

> 궁금. 이제까지 계속 swc를 쓰기는 썼는데 도대체 뭐지...  
오. 이거 예전에 정리해 둔 내용이 있어서 우선 복붙.

- Rust라는 언어로 짜여진 컴파일러로 현재는 자바스크립트/타입스크립트 트랜스파일링을 주로 담당
- 속도와 성능 개선에 초점. 기존 바벨 등의 트랜스파일러를 압도적으로 뛰어넘는 성능을 보여줌
- 컴파일러의 기능만 제공하는 것은 아님. 웹팩과 같은 자바스크립트 번들러의 기능을 제공할 spack도 개발중
- SWC가 빠른 이유 ⇒ Rust라는 프로그래밍 언어가 이벤트 루프 기반의 싱글 스레드 언어인 자바스크립트와는 다르게 ‘병렬 처리'를 고려해서 설계된 언어이기 때문. 의존성이 없는 파일들을 동시에 변환할 수 있음.

> 그럼 코딩도장 셋업도 그렇고 프로젝트 실행에서 jest를 설치할 때 `@swc/core @swc/jest` 명령을 사용하는게 자바/타입스크립트 트랜스파일링을 하기 위해서인가.  
트랜스파일링을 하지 않으면 무슨 문제가 생겼더랬지...? => 내 기억엔 function 앞에 export (default) 붙이면서 문제가 생겼던 것 같은데 다음번 코딩도장 세팅할 때 해당 내용 빼고 실행해서 어떤 문제가 발생하는지 찾아보자.

SWC 추가 정리. (Speedy Web Compiler)

- 참고. [카카오엔터 FE 기술블로그](https://fe-developers.kakaoent.com/2022/220217-learn-babel-terser-swc/)
- SWC는 JS 프로젝트의 컴파일과 번들링에 모두 사용될 수 있는 Rust 언어로 제작된 빌드 툴
- 왜 이제까지 swc를 사용해왔는지 공식문서의 글을 통해 확인할 수 있었음
-> To make your Jest tests run faster, you can swap out the default JavaScript-based runner (ts-jest) for a drop-in Rust replacement(opens in a new tab) using SWC.

- 실제로 swc-jest 관련 의존성을 설치하지 않고 테스트를 실행하니 아래와 같은 에러 문구 확인

```bash
SyntaxError: Cannot use import statement outside a module

Jest encountered an unexpected token

Jest failed to parse a file. This happens e.g. when your code or its dependencies use non-standard JavaScript syntax, or when Jest is not configured to support such syntax.

Out of the box Jest supports Babel, which will be used to transform your files into valid JS based on your Babel configuration.

By default "node_modules" folder is ignored by transformers.
```

</br>

- 어쨌든. `jest.config.js` 파일 생성하여 설정하기. `setupFilesAfterEnv`의 내용 중 `'./jest.setup'` 내용은 삭제

- parcel까지 설치 후 package.json 파일의 scripts 내용을 아래를 참고하여 수정

```bash
"scripts": {
    "start": "parcel --port 8080",
    "build": "parcel build",
    "check": "tsc --noEmit",
    "lint": "eslint --fix --ext .js,.jsx,.ts,.tsx .",
    "test": "jest",
    "coverage": "jest --coverage --coverage-reporters html",
    "watch:test": "jest --watchAll"
  },
```

- **새로 알게된 내용**
    - Node의 경우 진입점을 main으로 잡아줌. 이에 따라 package.json의 설정을 보면 `"main": "index.js"`를 확인할 수 있음.  
    하지만 강의 흐름 상 현재 포인트에서는 웹서버를 띄워야 하기 때문에 이 부분을 `"source": "./index.html" 또는 "source": "index.html"`로 수정해야 parcel로 웹서버를 띄울 수 있음

---

## 키워드 정리

### Node.js

- Node.js란? -> 한 마디로 자바스크립트 런타임 환경.
- 자바스크립트로 브라우저 밖에서 서버를 구축하는 등 코드를 실행할 수 있게 해 주는 런타임 환경
- 안정성을 보장하고 싶을 경우 LTS(Long Term Support) 버전을 사용하는 것이 적절함

</br>

### NPM(Node Package Manager)

- npm -> 노드 패키지(라이브러리, 프로그램 조각 또는 도구)를 관리해 줌(manager)

1-1. package.json

- package.json은  
(1) 프로젝트에 관한 정보(버전, author, 프로젝트 이름 등)가 포함되어 있고,  
(2) npm으로 설치한 라이브러리, 모듈 등의 의존성 정보를 포함하고 있다.  
따라서 프로젝트가 의존하는 패키지들을 관리해주는 파일이라고 할 수 있음
- 생성은 `npm init -y` 명령어로 생성 가능
- 파일 내 정보는 기본적으로 key-value로 저장됨(JSON 객체)
- 참고. node_modules에는 package.json에서 확인할 수 있는 모듈들이 의존하고 있는 모듈 전부를 포함 -> 그래서 용량도  크고... 커밋 시 올릴 필요도 없고. -> package.json에 설치한 패키지들 정보 모두가 있기 때문에 커밋이 필요 없기도 함
- 참고. [npm docs의 package.json](https://docs.npmjs.com/cli/v6/configuring-npm/package-json)

</br>

1-2. package-lock.json

- 협업 시에 다른 co-workers들과 버전을 맞추기 위해 필요하다는 이야기를 언뜻 들은 기억이..
- 참고. [npm docs의 package-lock.json](https://docs.npmjs.com/cli/v6/configuring-npm/package-lock-json)
- package-lock.json은 자신이 생성되는 시점의 의존성 트리(node_modules)에 대한 모든 정보를 가지고 있는 파일이라고 함.  
=> 그렇담 node_modules는? package.json에 등록된 모듈과 그 모듈들이 의존하고 있는 모듈 전부를 포함.  
=> package-lock.json은 이 모든 모듈들의 정보를 '기록'한 파일

2. node_modules

- package.json에 있는 모듈 뿐 아니라 이 모듈들이 의존하고 있는 모듈 전부를 포함. 실제 라이브러리가 설치되는 디렉토리
- [참고 velog](https://velog.io/@we_in/package.json)
- [토스테크-패키지 관리 시스템을 npm->Yarn Berry로 바꾼 과정](https://toss.tech/article/node-modules-and-yarn-berry)

3. npx

> npx는 경험상 npm으로 install한 라이브러리나 패키지(?), 모듈(?)의 실행 명령어같다는 느낌을 받았음  
예를 들어 eslint를 설치하고(`npm i -D eslint`) 난 뒤에 eslint 설정 파일을 생성하기 위해서는 `npx eslint --init`이라는 명령어를 사용하게 됨. 마치 exe 파일을 실행하는 느낌이랄까..

- 간단하게 정리하면 npm은 Package Manager 즉 관리이고, npx는 Package Runner 즉 실행. npm 버전 5.2.0 이상부터 사용할 수 있는 명령어라고 하며 또한 전역에 설치하는 것이 아니라 원하는 프로젝트에만 적용되도록 실행함
- [참고 blog](https://studium-anywhere.tistory.com/21)
- [참고 공식문서](https://docs.npmjs.com/cli/v7/commands/npx). npm Docs가 따로 있구나.. CLI 커맨드 안내가 다 되어있는데 추후에는 여기서 찾아보면 좋겠다.

</br>

### ES Modules vs CommonJS

-> 대략 찾아봤는데 생각보다 훨씬 더 복잡하고 방대한 것 같음.
-> 간략히 ES Modules, CommonJS의 차이를 나열

- CommonJS의 기본 문법

```javascript
// add.js
module.exports.add = (x, y) => x + y;


// main.js
const { add } = require('./add');

add(1, 2);
```

- ECMAScript Modules

```javascript
// add.js
export function add(x, y) {
  return x + y
}


// main.js
import { add } from './add.js';

add(1, 2);
```

- CJS는 `require` / `module.exports`를 사용하고, ESM은 `import` / `export` 문을 사용
- CJS module loader는 동기적으로 작동, ESM module loader는 비동기적으로 작동
    - ESM은 Top-level Await를 지원하므로 비동기적으로 동작함([Top-level await에 대한 카카오엔터 정리글](https://fe-developers.kakaoent.com/2022/220728-es2022/))
- EMS에서 CJS를 `import` 할 수 있지만 CJS에서 ESM을 `require` 할 수는 없음. CJS가 Top-level Await을 지원하지 않기 때문
- 결론적으로 두 Module System은 서로 호환되기 어려움

- [참고 1. 토스테크 블로그](https://toss.tech/article/commonjs-esm-exports-field)
- [참고 2. ES modules: 만화로 보는 심층 탐구](https://ui.toast.com/weekly-pick/ko_20180402)
- [참고 3. TOAST UI: You don't know JS module](https://ui.toast.com/weekly-pick/ko_20190418)
- [참고 4. 모던자바스크립트 튜토리얼: 모듈이란](https://ko.javascript.info/modules-intro)
- [참고 5. 모던자바스크립트 딥다이브: module](https://poiemaweb.com/es6-module)
- [참고 6. 네이버 d2-CommonJS와 AMD](https://d2.naver.com/helloworld/12864)
- [참고링크](https://www.knowledgehut.com/blog/web-development/commonjs-vs-es-modules)
- [참고 블로그](https://velog.io/@doondoony/JavaScript-Module-System#-es6-modulesesm)
