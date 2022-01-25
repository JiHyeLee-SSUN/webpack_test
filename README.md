# Webpack 가이드

---

## 1. 설치

```json
//npm
npm init -y
npm i -D webpack webpack-cli webpack-dev-server

//yarn
yarn init
yarn add webpack webpack-cli webpack-dev-server --dev
```

---

## 2. 기본 구성

### 1. Entry

- 웹팩에서 웹 자원을 변환하기 위해 필요한 최초 진입점 이자 자바스크립트 파일 경로
- 웹팩은 `entry` 를 통해서 모듈을 로딩하고 하나의 파일로 묶는다

```js
module.exports = {
    entry: {
        app: ['./client'],
    },
}
```

### 2. Output

- 웹팩에서 `entry` 로 찾은 모듈을 하나로 묶은 결과물을 반환할 위치

```js
module.exports = {
    output: {
        path: path.join(__dirname, 'dist'),
    },
}
```

### 3. Loader

- 자바스크립트와 JSON외의 다른 자원(HTML, CSS, IMAGE...) 를 빌드할 수 있도록 도와주는 속성

```js
module.exports = {
    // 로더를 지정하기 위한 module 정의
    module: {
        rules: [
            {
                //로더를 적용할 파일 유형 (일반적으로 정규 표현식 사용)
                test: /\.(js|jsx)$/,
                //제외할 파일 정규식
                exclude: /node_modules/,
                //변환을 수행하는데 사용되는 로더를 가리키는 속성
                //로더에서 모듈 로딩 순서는 배열의 요소 오른쪽에서 왼쪽으로 로딩하며 진행
                use: {
                    loader: 'babel-loader',
                    options: {
                        presets: ['@babel/preset-env', '@babel/preset-react'],
                    },
                },
            },
            {
                test: /\.html$/,
                use: [
                    {
                        loader: 'html-loader',
                        options: {
                            //코드 최적화
                            minimize: true,
                        },
                    },
                ],
            },
        ],
    }
}
```

### 4. Plugin

- 웹팩의 기본적인 동작 외 추가적인 기능을 제공하는 속성
- `loader` 는 파일을 해석하고 변환하는 과정에 관여하고, `plugin` 은 결과물의 형태를 바꾸는 역할을 한다.
