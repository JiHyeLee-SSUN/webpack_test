# Loader

---

Webpack은 `javascript`와 `JSON`파일만 이해한다. <br>
로더를 사용하면 webpack이 다른 유형의 파일을 처리하거나, 유효한 모듈로 변환하여 애플리케이션에서 사용하거나 디펜던시 그래프에 추가한다.

로더는 webpack 설정에 <b>두가지</b> 속성을 가집니다.

1. 변환이 필요한 파일을 식별하는 `test` 속성
2. 변환을 수행하는데 사용되는 로더를 가리키는 `use` 속성

```js
const path = require('path')
module.exports = {
  output: {
    filename: 'app.bundle.js'
  },
  module: {
    rules: [{
      test: /\.txt$/, use: 'raw-loader'
    }]
  }
}
```

위의 속성들은 webpack의 컴파일러에 다음과 같이 말합니다.
> webpack 컴파일러, `require() / import` 문 내에 `.txt` 파일로 확인되는 경로를 발견 시 <br>
> 번들에 추가하기전에 `raw-loader`를 사용해 변환해라

## Using Loaders

애플리케이션에서 로더를 사용하는 방법은 두가지가 있다.

- 설정(<strong>recommand</strong>) : webpack.config.js 파일에 지정한다
- 인라인 : 각 `import` 문에서 명확하게 지정한다

## configuration

`module.rules`를 사용하면 webpack 설정 내에 여러개의 로더를 지정할 수 있다.

로더는 오른쪽에서 왼쪽(또는 아래에서 위로) 평가/실행된다. 아래의 예제에서는 `sass-loader`로 실행이 시작되고,
`css-loader`로 이어지며 마지막으로 `style-loader`로 끝난다.

```js
module.exports = {
  module: {
    rules: [
      {
        test: /\.css$/,
        use: [
          { loader: 'style-loader' },
          {
            loader: 'css-loader',
            options: {
              modules: true
            }
          },
          { loader: 'sass-loader' }
        ]
      }
    ]
  }
}
```