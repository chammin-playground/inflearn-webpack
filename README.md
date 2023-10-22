# ⚠️ 프론트엔드 개발 환경의 이해 실습

["프론트엔드 개발 환경의 이해"](https://github.com/jeonghwan-kim/lecture-frontend-dev-env) 강의 자료입니다.

<br>

### 실습 1. 웹팩 엔트리/아웃풋 실습

프로젝트 세팅

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
