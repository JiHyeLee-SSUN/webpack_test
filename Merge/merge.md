# Merge

---
웹팩 환경을 분리해 목적에 따른 빌드를 할수 있도록 관리한다.

## 💻 빌드 목표

### Development

- strong source map
- local sever with live reload & Hot module replacement

### production

- minified bundles
- lighter weight source maps
- optimized assets

## ⚙️ Settings

```js
//install
npm i -D webpack-merge
```

환경에 따른 webpack 설정 파일을 각각 생성한다

- `webpack.common.js`
- `webpack.dev.js`
- `webpack.prod.js`

### Config.js
webpack 환경을 merge로 합치는 config 설정파일
```js
//webpack.config.js
const commonConfig = require('./webpack.common.js')
const { merge } = require('webpack-merge')
const { argv } = require('yargs')

module.exports = () => {
    const envConfig = require(`./webpack.${argv.env}.js`)
    return merge(commonConfig, envConfig)
}
```


### Common.js
entry, output, babel-loader 등 `dev`,`prod`에 공통으로 적용되는 사항을 포함
```js
const path = require('path')
module.exports = {
    resolve: {
        alias: {
            '@': path.resolve(__dirname),
        },
    },
    module: {
        rules: [
            {
                test: /\.js$/,
                exclude: path.join(__dirname, 'node_modules'),
                use: {
                    loader: 'babel-loader',
                    options: {
                        plugins: ['@babel/plugin-transform-runtime'],
                        presets: [
                            [
                                '@babel/preset-env',
                                {
                                    targets: {
                                        browsers: '> 5% in KR',
                                    },
                                },
                            ],
                        ],
                        cacheDirectory: true,
                    },
                },
            },
            {
                test: /\.(jpe?g|png|gif|svg)$/,
                type: 'asset/resource',
            },
        ],
    },
}

```


