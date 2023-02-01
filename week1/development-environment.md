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

- `.gitignore` 파일 설정 잊지말기. google에서 'github gitignore node'라고 검색만 해도 예시 확인 가능. 설정 후에 ignore되어야 하는 내용들(ex. node_modules, .parcel-cache, dist 등)이 포함되어 있는지 확인할 것


- 타입스크립트는 Dev 환경에서 활용하는 것으로 아래와 같은 명령어로

```bash
npm i -D typescript

npx tsc --init
```

> 마치 우리가 eslint 설치하고 실행할 때 `npx init --eslint`로 실행했던 것과 동일함
> 이번에 새로 알게 된 내용 -> npx 명령어 말고 node_modules로 시작하는 명령어. 예전에 이거랑 비슷한 상황이 있었는데 '왜 저렇게 하는게 가능하지?'라고 의문을 가졌었음. 이번 강의에서 짤막하게 짚고 넘어갔는데 좀 더 이해가 가는 너낌.  
바이너리 파일로 해당하는 루트에 가보면 실제 실행 파일이 존재함(tsc는 타입스크립트 컴파일러)  
`./node_modules/.bin/tsc`

- 생성된 json 파일에서 몇가지 설정 변경
    - "jsx": "react-jsx"

- 이후 eslint 설치 및 jest 사용을 위한 설정. extends에 `'plugin:react/jsx-runtime` 추가해주기.  
**중요.** 잊지 말고 .eslintignore 파일 작성하기. => 현재 익숙하지 않은 부분이기에 위에서부터 이어진 process 전체를 반복해서 학습할 필요가 있음  
해당 파일에는 아래와 같은 3종 세트가 빠지지 않도록 넣어주기~! 개발하면서 계속 추가될 것.

```
/node_modules/
/dist/
/.parcel-cache/
```

> <U>궁금. 이제까지 계속 swc를 쓰기는 썼는데 도대체 뭐지...</U>

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
    하지만 강의 흐름 상 현재포인트에서는 웹서버를 띄워야 하기 때문에 이 부분을 `"source": "./index.html" 또는 "source": "index.html"`로 수정해야 parcel로 웹서버를 띄울 수 있음

---

## 키워드 정리

- Node.js
- NPM(Node Package Manager)
    - package.json / package-lock.json
    - node_modules
    - npx
- ES Modules vs CommonJS
