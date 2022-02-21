# `Babel`과 `Webpack`을 이용한 `ES6+`/ES.NEXT 개발 환경 구축

- 크롬, 사파리, 파이어폭스, 엣지 같은 에버 그린 브라우저(`evergreen browser`)의 `ES6` 지원율은 약 98%로 거의 대부분 `ES6` 사양 지원.
- `IE 11`의 `ES6` 지원율은 약 11%며, 매년 새롭게 도입되는 `ES6` 이상의 버전(`ES6+`)과 제안 단계에 있는 `ES` 제안 사양(`ES.NEXT`)은 브라우저에 따라 지원율이 제각각.
- 따라서 최신 `ECMAScript` 사양을 사용하여 프로젝트를 진행하기 위해선 `IE`를 포함한 구형 브라우저에서 문제 없이 동작시키기 위한 개발 환경을 구축하는 것이 필요.
- 대부분의 프로젝트가 모듈을 사용하므로 모듈 로더도 필요.
  - `ES6` 모듈(`ESM`)은 대부분의 모던 브라우저에서 사용할 수 있음.
  - 모듈 로더를 사용하는 이유
    - `IE`를 포함한 구형 브라우저는 `ESM` 미지원.
    - `ESM`를 사용하더라도 트랜스파일링이나 번들링 필요.
    - `ESM`이 아직 지원하지 않는 기능(`bare import` 등)이 있고 몇 가지 이슈 존재.

## `Babel`

- `Babel`은 최신 사양의 소스코드를 `IE` 같은 구형 브라우저에서도 동작하는 `ES5` 사양의 소스코드로 변환(트랜스파일링)할 수 있음.

### `Babel` 설치

- `Babel` 설치 명령어

  ```shell
  npm install --save-dev @babel/core @babel/cli
  ```

- `Babel`, `Webpack`, 플러그인의 버전은 빈번하게 업그레이드되므로, 버전을 확인하며 설치.

### `Babel` 프리셋 설치와 `babel.config.json` 파일 설정

- `Babel`을 사용하기 위해선 `@babel/preset-env`를 설치해야 함.
  - 함께 사용되어야 하는 `Babel` 플러그인을 모아둔 것.
- `Babel`이 제공하는 공식 프리셋(`official preset`)
  - `@babel/preset-env`
  - `@babel/preset-flow`
  - `@babel/preset-react`
  - `@babel/preset-typescript`
- `@babel/preset-env`는 필요한 플러그인들을 프로젝트 지원 환경에 맞춰 동적으로 결정.
- 프로젝트 지원 환경은 `Browserlist` 형식으로 `.browserlistrc` 파일에 상세히 설정 가능. (생략 시 기본값)

  ```shell
  npm install --save-dev @babel/preset-env
  ```

- 설치 후 `babel.config.json` 파일 생성.

  ```json
  {
    "presets": ["@babel/preset-env"]
  }
  ```

### 트랜스파일링

- `Babel CLI` 트랜스파일링 명령어

  ```shell
  babel /src/js -w -d dist/js
  ```

  - `src/js` 폴더(타깃 폴더)에 있는 모든 `JavaScript` 파일을 트랜스파일링.
  - 결과물을 `dist/js` 폴더에 저장.
  - `-w`(`--watch`) 옵션: 타깃 폴더에 있는 모든 `JavaScript` 파일들의 변경을 감지하여 자동으로 트랜스파일.
  - `-d`(`--out-dir`) 옵션: 트랜스파일링 결과물이 저장될 폴더를 지정. 지정된 폴더가 존재하지 않으면 자동으로 생성.

### `Babel` 플러그인 설치

- 설치가 필요한 `Babel` 플러그인은 `Babel` 홈페이지에서 검색 가능.
- `@babel/plugin-proposal-class-properties`: `public/private` 클래스 필드를 지원하는 플러그인.
- `babel.config.json` 파일 수정.

  ```json
  {
    "presets": ["@babel/preset-env"],
    "plugins": ["@babel/plugin-proposal-class-properties"]
  }
  ```

### 브라우저에서 모듈 로딩 테스트

- 위의 예제는 `Node.js` 환경에서 동작한 것이고, `Babel`이 모듈을 트랜스파일링한 것도 `Node.js`가 기본 지원하는 `CommonJS` 방식의 모듈 로딩 시스템에 따른 것.
- 브라우저는 `CommonJS` 방식의 `require` 함수를 지원하지 않으므로 브라우저에서 실행하면 에러 발생.
- `Babel`을 설정하여 `ES6` 모듈(`ESM`)을 사용하도록 할 수 있음.

---

## `Webpack`

- `Webpack`은 의존 관계에 있는 `JavaScript`, `CSS`, 이미지 등의 리소스들을 하나(또는 여러 개)의 파일로 번들링하는 모듈 번들러.
- `Webpack`을 사용하면 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요 없음.
- 여러 개의 `JavaScript`팡리을 하나로 번들링하므로 `HTML` 파일에서 `script` 태그로 여러 개의 `JavaScript` 파일을 로드해야 하는 번거로움도 사라짐.

### `Webpack` 설치

- `Webpack` 설치 명령어

  ```shell
  npm install --save-dev webpack webpack-cli
  ```

### `babel-loader` 설치

- `Webpack`이 모듈을 번들링할 때 `Babel`을 사용하여 `ES6+`/`ES.NEXT` 사양의 소스코드를 `ES5` 사양의 소스코드로 트랜스파일링하도록 `babel-loader` 설치.

  ```shell
  npm install --save-dev babel-loader
  ```

### `webpack.config.js` 설정 파일 작성

- `Webpack`이 실행될 때 참조하는 설정 파일.

  ```js
  const path = require("path");

  module.exports = {
    entry: "./src/js.main.js",
    output: {
      path: path.resolve(__dirname, "dist/js"),
      filename: "bundle.js",
    },
    module: {
      rules: [
        {
          test: /\.js$/,
          include: [path.resolve(__dirname, "src/js")],
          exclude: /node_modules/,
          use: {
            loader: "babel-loader",
            options: {
              presets: ["@babel/preset-env"],
              plugins: ["@babel/plugin-proposal-class-properties"],
            },
          },
        },
      ],
    },
    devtool: "source-map",
    mode: "development",
  };
  ```

- 트랜스파일링은 `Babel`이 수행하고 번들링은 `Webpack`이 수행.

### `babel-polyfill` 설치

- `Babel`을 사용하여 `ES6+`/`ES.NEXT` 사양의 소스코드를 `ES5` 사양의 소스코드로 트랜스파일링해도 브라우저가 지원하지 않는 코드가 남아 있을 수 있음.
- `Promise`, `Object.assign`, `Array.from` 등과 같이 `ES5` 사양으로 대체할 수 없는 기능은 트랜스파일링 되지 않음.
- `IE` 같은 구형 브라우저에서도 위와 같은 객체나 메서드를 사용하기 위해서 `@babel/polyfill` 설치.

  ```shell
  npm install @babel/polyfill
  ```

  - `@babel/polyfill`은 개발 환경에서만 사용하는 것이 아니라 실제 운영 환경에서도 사용해야 하므로 개발용 의존성으로 설치하지 않음.

- `ES6`의 `import`를 사용하는 경우 진입점의 선두에서 폴리필을 로드

  ```js
  import "@babel/polyfill";
  import { pi, power, Foo } from "./lib";
  ```

- `Webpack`을 사용하는 경우 `webpack.config.js` 파일의 `entry` 배열에 폴리필을 추가.

  ```js
  module.exports = {
    entry: ["@babel/polyfill", "./src/js/main.js"],
  };
  ```
