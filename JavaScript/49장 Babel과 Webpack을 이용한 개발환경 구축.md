# 49. Babel과 Webpack을 이용한 ES6+/ES.NEXT 개발 환경 구축

## 49.1 Babel

---

```jsx
// ES6, ES7
[1, 2, 3].map((n) => n ** n);

// ES5
[1, 2, 3].map(function (n) {
  return Math.pow(n, n);
});
```

- IE 같은 구형 브라우저에서는 ES6의 화살표 함수와 ES7의 지수 연잔자를 지원하지 않을 수 있으므로 Babel을 사용해서 ES5 사양으로 변환할 수 있음

> **Babel**: ES6+/ES.NEXT로 구현된 최신 사양의 소스코드를 **ES5 사양의 소스코드로 트랜스파일링** 할 수 있음

### Babel 설치

```bash
npm init -y
npm install --save-dev @babel/core @babel/cli
```

### Babel 프리셋 설치와 babel.config.json 설정 파일 작성

- Babel 프리셋: Babel이 제공하는 공식 프리셋

```bash
npm install --save-dev @babel/preset-env
```

```jsx
// package.json
{
  "name": "esnext-project",
  "version": "1.0.0",
  "description": "",
  "main": "index.js",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "keywords": [],
  "author": "",
  "license": "ISC",
  "devDependencies": {
    "@babel/cli": "^7.24.8",
    "@babel/core": "^7.25.2",
    "@babel/preset-env": "^7.25.3"
  }
}

```

```jsx
// 루트폴더/babel.config.json
{
  "presets": ["@babel/preset-env"]
}
```

### 트랜스파일링

- package.json에 scripts 추가

```jsx
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "babel src/js -w -d dist/js"
  },
```

- src/js 폴더에 있는 모든 자바스크립트 파일들을 트랜스파일링한 후, 결과물을 dist/js에 저장
  - -w: 타깃 폴더에 있는 모든 자바스크립트 파일들의 변경을 감지해 자동으로 트랜스파일
  - -d: 결과물이 저장될 폴더를 지정

```jsx
// src/js/lib.js
export const pi = Math.PI;

export function power(x, y) {
  return x ** y;
}

export class Foo {
  #private = 10;
  foo() {
    const { a, b, ...x } = { ...{ a: 1, b: 2 }, c: 3, d: 4 };
    return { a, b, x };
  }
  bar() {
    return this.#private;
  }
}
```

```jsx
// src/js/main.js

import { pi, power, Foo } from "./lib.js";

console.log(pi);
console.log(power(pi, pi));

const f = new Foo();
console.log(f.foo());
console.log(f.bar());
```

```bash
npm run build
```

- 트랜스파일링 실행

### Babel 플러그인 설치

```bash
npm install --save-dev @babel/plugin-proposal-class-properties
```

### 브라우저에서 모듈 로딩 테스트

```html
<!DOCTYPE html>
<html>
  <body>
    <script src="dist/js/lib.js"></script>
    <script src="dist/js/main.js"></script>
  </body>
</html>
```

- Node.js 환경에서는 `dist/main.js` 파일이 정상 동작하지만 브라우저에서는 CommonJS 방식의 require 함수를 지원하지 않으므로 에러가 발생
  → Webpack을 통해 해결

## 49.2 Webpack

---

<aside>
💡 **Webpack**: 의존 관계에 있는 자바스크립트, CSS, 이미지 등의 **리소스들을 하나의 파일로 번들링하는 모듈 번들러**
→ 의존 모듈이 하나의 파일로 번들링되므로 별도의 모듈 로더가 필요 없음, 여러개의 script 태그로 자바스크립트 파일을 로드해야할 필요가 없음

</aside>

### Webpack 설치

```bash
npm install --save-dev webpack webpack-cli
```

### babel-loader 설치

- Webpack이 모듈을 번들링할 때 Babel을 사용하여 ES6+/ES.NEXT 사양의 소스코드를 ES5 사양의 소스코드로 트랜스파일링하도록 babel-loader를 설치

```bash
npm install --save-dev babel-loader
```

- package.json의 scripts 변경

```jsx
"scripts": {
    "test": "echo \"Error: no test specified\" && exit 1",
    "build": "webpack -w"
  },
```

### webpack.config.js 설정 파일 작성

```jsx
// webpack.config.js
const path = require("path");

module.exports = {
  // entry file
  // https://webpack.js.org/configuration/entry-context/#entry
  entry: "./src/js/main.js",
  // 번들링된 js 파일의 이름(filename)과 저장될 경로(path)를 지정
  // https://webpack.js.org/configuration/output/#outputpath
  // https://webpack.js.org/configuration/output/#outputfilename
  output: {
    path: path.resolve(__dirname, "dist"),
    filename: "js/bundle.js",
  },
  // https://webpack.js.org/configuration/module
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
  // https://webpack.js.org/configuration/mode
  mode: "development",
};
```

![image.png](https://prod-files-secure.s3.us-west-2.amazonaws.com/7fcc553f-a6b8-4870-a93c-292d8556f276/120e56be-2534-443f-961f-f5d90ab194b0/image.png)

- index.html 수정

```html
<!DOCTYPE html>
<html>
  <body>
    <script src="dist/js/bundle.js"></script>
  </body>
</html>
```

### babel-polyfill 설치

- ES5 사양의 소스코드로 트랜스파일링해도 브라우저가 지원하지 않는 코드가 남아있을 수 있음 (e.g. Promise, Object.assign, Array.from ..)
- IE 같은 구형 브라우저에서도 위와 같은 객체나 메서드를 사용하기 위해 설치
- 실제 운영 환경에서도 사용해야 하므로 --save-dev 옵션을 지정하지 않음

```bash
npm install @babel/polyfill
```

```jsx
// webpack.config.js
module.exports = {
  entry: ["@babel/polyfill", "./src/js/main.js"],
...
```
