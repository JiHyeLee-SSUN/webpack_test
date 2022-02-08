# Babel-loader

---
Babel-loader는 바벨과 웹팩을 사용하여 자바스크립트 파일을 트랜스파일링 할 수 있다.

## install
> *webpack 4.x | babel-loader 8.x | babel 7.x*
```js
npm install -D babel-loader @babel/core @babel/preset-env webpack
```

## package

- `babel-loader` : 웹팩에서 babel을 loader로 사용하기 위한 loader
- `core` : 트랜스파일링 시 이용되는 코어 모듈
- `preset-env` : 최신 JavaScript를 사용할 수 있는 스마트 사전 설정입니다
- `preset-react` : babel의 react관련 트랜스파일링을 위한 모든 플러그인을 모은 프리셋

## usage
```js
module: {
  rules: [
    {
      test: /\.m?js$/,
      exclude: /(node_modules|bower_components)/,
      use: {
        loader: 'babel-loader',
        options: {
          presets: ['@babel/preset-env']
        }
      }
    }
  ]
}
```
---
## options
### cacheDirectory
- Default `false`.
- 설정하면 지정된 디렉터리는 로더의 결과를 캐시하는 데 사용됩니다. 향후 웹 팩 빌드는 잠재적으로 비용이 많이 드는 바벨 재컴파일 프로세스를 실행할 필요가 없도록 캐시에서 읽기를 시도한다. 
- 옵션`cacheDirectory: true`에서 값이 `true`로 설정된 경우 로더는 `node_modules/.cache/babel-loader`의 기본 캐시 디렉토리를 사용하거나 루트 디렉토리에서 `node_modules` 폴더를 찾을 수 없는 경우 기본 OS 임시 파일 디렉토리로 fallback 한다.

### cacheCompression
- Default `true`.
- 설정하면 각 바벨 변환 출력이 `Gzip`으로 압축됩니다. 
- 캐시 압축을 해제하려면 `false`로 설정하십시오. 수천 개의 파일을 변환하는 경우 프로젝트에 도움이 될 수 있습니다.

## Trouble Shooting
### babel-loader is slow!
- 가능한 적은 수의 파일을 변환해야 합니다. /\.m?js$/과(와) 일치하므로 `node_modules` 폴더 또는 기타 원하지 않는 소스를 변환하고 있을 수 있습니다.
- node_modules를 제외하려면 위에 설명된 대로 loaders 구성의 제외 옵션을 참조하십시오.`cacheDirectory` 옵션을 사용하여 babel-loader 속도를 최대 2배까지 높일 수도 있습니다. 이렇게 하면 파일 시스템에 변환이 캐시됩니다.

### Babel is injecting helpers into each file and bloating my code
- Babel은 `_extend`와 같은 일반적인 함수에 매우 작은 도우미를 사용한다. 기본적으로 이 파일은 필요한 모든 파일에 추가됩니다.
- 대신 중복을 방지하기 위해 별도의 모듈로 Babel 런타임을 요구할 수 있습니다. 
- 다음 구성은 Babel에서 파일별 런타임 자동 주입을 비활성화하며, 대신 `@babel/plugin-transform runtime`이 필요하고 모든 도우미 참조가 이를 사용하도록 합니다.
