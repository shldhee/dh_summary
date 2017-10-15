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

## plugins
