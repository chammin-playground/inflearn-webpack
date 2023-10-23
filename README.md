# ⚠️ 프론트엔드 개발 환경의 이해 실습

["프론트엔드 개발 환경의 이해"](https://github.com/jeonghwan-kim/lecture-frontend-dev-env) 강의 자료입니다.

<br>

## 실습 1. 웹팩 엔트리/아웃풋 실습

프로젝트 세팅(package.json 생성)

```bash
npm init -y
npm i webpack webpack-cli
```

<br>

package.json 설정

```json
{
  ...
  "scripts": {
    "build": "webpack"
  },
  ...
}
```

<br>

webpack.config.js 설정

```javascript
const path = require("path");

module.exports = {
  mode: "development",
  entry: {
    main: "./src/app.js",
  },
  output: {
    filename: "[name].js",
    path: path.resolve("./dist"),
  },
};
```

- `path` : Node.js의 내장 모듈 중 하나로, 파일 및 디렉토리 경로를 조작하는 유틸리티 기능을 제공한다. OS에 따라 다른 경로 구분자 문제나 상대 경로, 절대 경로 등의 변환과 관련된 작업을 간편하게 처리할 수 있다.
  - `path.join([...paths])` : 주어진 경로 조각들을 함께 조합하고, 결과 경로를 정규화하여 반환한다.
    ```javascript
    path.join("/foo", "bar", "baz/asdf");
    // '/foo/bar/baz/asdf'
    ```
  - `path.resolve([...paths])` : 상대 경로를 절대 경로로 변환한다.
    ```javascript
    path.resolve("foo/bar", "/tmp/file/", "..", "a/../subfile");
    // On Unix: Returns '/tmp/foo/bar/subfile'
    ```

<br>

## 실습 2. 로더 실습

### 2.1 CSS 파일을 엔트리포인트(app.js)에서 로딩

index.html에서 CSS파일을 로드했던 것을 엔트리포인트(app.js)에서 로딩하기. 웹팩에서 로드할 수 있도록 로더를 설정해야 한다.

```html
<!-- 기존 <link> 삭제하기 -->
<head>
  <link rel="stylesheet" href="src/main.css" />
</head>
```

<br>

CSS 파일을 JavaScript에서 모듈처럼 가져오려면, css-loader가 필요하며 해당 내용이 .html에 주입되려면 style-loader가 필요하다.

```bash
npm i style-loader css-loader
```

<br>

webpack.config.js에서 로더 설정

```javascript
const path = require("path");

module.exports = {
  // ...
  module: {
    rules: [
      {
        test: /\.css$/,
        // 실행 순서는 오른쪽에서 왼쪽으로 (css-loader => style-loader)
        use: ["style-loader", "css-loader"],
      },
    ],
  },
};
```

<br>

엔트리 포인트(app.js)에서 CSS파일을 로드하도록 수정

```javascript
// app.js

import MainController from "./controllers/MainController.js";
import "./main.css";

document.addEventListener("DOMContentLoaded", () => {
  new MainController();
});
```

<br>

### 2.2 파일을 로딩할수 있도록 웹팩 로더 설정을 추가

로더 설치 진행

```bash
npm i file-loader
```

<br>

webpack.config.js에서 로더 설정. webpack5부터는 file-loader, url-loader 대신에 [asset modules](https://webpack.kr/guides/asset-modules/)를 사용한다. url-loader에서 설정한 용량을 넘기면 file-loader 설정이 없어도 자동으로 file-loader를 실행한다.

```javascript
const path = require("path");

module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: ["style-loader", "css-loader"],
      },
      {
        test: /\.(jpg|png)$/,
        type: "asset/resource",
        generator: {
          filename: "[name][ext][query]",
        },
      },
    ],
  },
};
```

<br>
