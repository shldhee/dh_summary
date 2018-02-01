# webpack

## entry

```js

// 기본
entry: './app/index.js',

// 배열
entry: ['./app/index.js'],

// 객체
entry: {
  index : './app/index.js'
},
```

## entry & output

``` js
var path = require('path');

module.exports = {
  entry: {
      ele1: './app/index.js',
      ele2: './app/test.js',
  },
  output: {
    filename: '[name].js',
    path: path.resolve(__dirname, 'dist')
  }
};
```

생성
* dist/ele1.js
* dist/ele2.js

## output

``` js
output : {
  filename: '[name].js', // 파일명으로
  filename: '[hash].js', // 빌드시 해쉬명
  filename: '[chunkhash].js', // 청크별로
}
```

## path.resolve

* nodejs 내장 API
* 해당 API가 동작하는 OS의 파일 구분자를 이용하여 파일 위치를 조합한다.(인자 구분으로 '/'으로 설정)
* path.join()

``` js
path.join('/foo', 'bar', 'baz/asdf');
// returns: '/foo/bar/baz/asdf'
```

## webpack-loader

* 웹팩은 js 파일만 처리가 가능하도록 되어있다.
* loader를 통해 다른 파일들을 js로 변환하여 로딩

``` js
module.exports = {
  entry :{

  },
  output :{

  },
  module: {
    rules: [
      { test: /\.css$/, use: ['style-loader', 'css-loader']}
    ]
  };
}
```

* loader 에서 모듈 로딩 순서는 배열의 요소 오른쪽에서 왼쪽으로 진행된다.
* 순서대로 1. jquery, 2. underscore("_" 이 형태로 로드한다.) 로딩

``` js
  test: /backbone/,
  use: [
    'expose-loader?Backbone',
    'imports-loader?_=underscore, jquery'
  ]
}
```

* 위 설정 파일은 webpack으로 번들링한 결과물

``` js
var _ = __webpack_require__(0);
var jquery = __webpack_require__(1);
```

## babel-loader

* tree shaking : 쓰지 않는 모듈들은 호출하지 않는다.
* presets 부분은 `.babelrc` 에 쓰기도 한다.

``` js
module: {
  rules: [{
    test: /\.js$/,
    use: [{
      loader: 'babel-loader',
      options: {
        presets: [
          ['es2015', 'react', {modules: false}]
        ]
      }
    }]
  }]
}
```

``` js
{
  "presets": ["react", "es2015"]
}
```

## css code splitting

### style-loader, css-loader

``` js
use: ['style-loader', 'css-loader']
// 1. css-loader 로드 css->js파일안으로
   2. html에다 style를 입힌다.
```

* `base.css` 내용이 html 상단쪽에 삽입된다.

``` html
<style type="text/css">p {
  color: blue;
}
</style>
```

### plugin

* 오류 날땐 `package.json` 에 local로 webpack 설치(특수기능들 사용하기 위해)

``` js
module.exports = {
  entry: './app/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist')
  },
  module: {
    rules: [{
      test: /\.css$/,
      // use: ['style-loader', 'css-loader']
      use: ExtractTextPlugin.extract({
        fallback: "style-loader",
        use: "css-loader"
      })
    }]
  },
  plugins: [
    new ExtractTextPlugin('styles.css')
  ]
}
```

* 위 플로그인 사용시
* styles.css 파일이 생성됨
* index.html에 추가

``` js
<link rel="stylesheet" href="./dist/styles.css">
```

## plugin

* 플러그인은 파일별 커스텀 기능을 사용하기 위해서 사용한다.

굳이 구분하자면
로더 웹팩 빌딩할때 번들링하면서 작업 처리하면서 중간 개입
플러그인 번들링 끝나고 마지막 결과값을 낼때 관여한다.

``` js
module.exports = {
  entry: {},
  output: {},
  module: {},
  plugins: [
    new webpack.optimize.UglifyJsPlugin() // 용량 줄이는 라이브러리
  ]
}
```

### plugin 종류

#### ProvidePlugins

* 모든 모듈에서 사용할 수 있도록 모듈을 변수로 변환한다.
* import, require, script 파일로 로드할 필요없이 번들링되고 난뒤 전역변수로 선언한다.

``` js
new webpack.ProvidePlugin({
  $: "jquery"
})
```

#### DefinePlugin

* Webpack 번들링을 시작하는 시점에 사용 가능한 상수들을 정의한다.
* 일반적으로 계발계 & 테스트계에 따라 다른 설정을 적용할 때 유용하다

``` js
new webpack.DefinePlugin({
  PRODUCTION: JSON.stringify(true),
  VERSION: JSON.stringify("5fa3b9"),
  BROWSER_SUPPORTS_HTML5: true,
  TWO: "1+1",
  "typeof window": JSON.stringify("object")
})
```

#### ManifestPlugin

* 번들링시 생성되는 코드 (라이브러리)에 대한 정보를 json 파일로 저장하여 관리
* 라이브러르가 많을 시에 유용

``` js
  new ManifestPlugin({
    fileName: 'manifest.json',
    basePath: './dist'
  })
```

## Libraries Code Splitting

``` js
var webpack = require('webpack');
var path = require('path');

module.exports = {
  entry: {
    main: './app/index.js', // main 내용
    vendor: [ // 제 3 라이브러리 써드파티 라이브러리 모음
      'moment',
      'lodash'
    ]
  },
  output: {
    filename: '[name].js', // main.js, vendor.js 생성
    path: path.resolve(__dirname, 'dist')
  }
}
```

``` js
var moment = require('moment');
var _ = require('lodash');
var ele = document.querySelectorAll('p');

document.addEventListener("DOMContentLoaded", function(event) {
  ele[0].innerText = moment().format();
  ele[1].innerText = _.drop([1, 2, 3], 0);
});
```

* `dist/main.js`,`dist/vender.js` 생성
* 하지만 main에 lodash, moment 내용이 있으면 즉 main.js와 vendor.js와 같아서 중복작업임
* 이걸 해결하기 위해 `Plugin` 사용

``` js
plugins: [
  new webpack.optimize.CommonsChunkPlugin({ // 공통적으로 사용하는 라이브러리는 공통(커먼)으로 사용하겠다 하고 따로 빼기
    //name: 'vendor' // Specify the common bundle's name. (entry name)
    names: ['vendor', 'manifest'] // Extract the webpack bootstrap logic into manifest.js
    // menifest 는 초기화 코드를 따로 빼 분리해서 생성된다.(웹팩 초기화 로직)
  }),
]
```

* `dist/vendor.js`에는 라이브러리(moment, lodash), `dist/main.js`에는 index.js 본문 내용만 용량이 팍 줄어들었다.


### Mainifest Plugin

``` js
var ManifestPlugin = require('webpack-manifest-plugin');

new ManifestPlugin({
  fileName: 'manifest.json',
  basePath: './dist/'
})
```

*manifest.json*
``` json
{
  "./dist/main.js": "./dist/main.js",
  "./dist/manifest.js": "./dist/manifest.js",
  "./dist/vendor.js": "./dist/vendor.js"
}
```
