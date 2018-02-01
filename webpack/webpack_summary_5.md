# webpackDevMiddleware 실습

``` js
npm install webpack --save
npm install express webpack-dev-middleware --save-dev
```


``` js
// server.js

var express = require('express');
var app = express();
var path = require('path');

app.use(express.static(__dirname));

// view engine setup
// __dirname : root folder
app.set('views', path.join(__dirname));
app.set('view engine', 'ejs');
app.engine('html', require('ejs').renderFile);

app.get('/', function (req, res) {
  res.send('index');
});

app.listen(3000);
console.log("Server running on port 3000");
```

node 서버 express, ejs 이용

Run server.js
`node server.js`

오류날때
Can't find module 'ejs'
npm install ejs --save

``` js
// webpackConfig.js
var path = require('path');
var webpack = require('webpack');

module.exports = {
  entry: './app/index.js',
  output: {
    filename: 'bundle.js',
    path: path.resolve(__dirname, 'dist'),
    publicPath: 'http://localhost:3000/dist' // node server 인메모리 위치
  },
};
```

add server.js

``` js
// webpack 불러오기
var webpackDevMiddleware = require("webpack-dev-middleware");
var webpack = require("webpack");
var webpackConfig = require("./webpack.config");
var compiler = webpack(webpackConfig);

// webpack 미들웨어 사용
app.use(webpackDevMiddleware(compiler, {
  publicPath: webpackConfig.output.publicPath,
  stats: {colors: true} // log color 강조
}));
```

## webpack1.x와 wepback3.x 차이점

* webpack1은 개념적인 설명
* webpack2 API, 플러그인 등활용

## webpack 주요 명령어

* `webpack` : 웹팩 빌드 기본 명령어 (주로 개발용)
* `webpack -p` : minification 기능이 들어간 빌드(주로 배포용) 프로덕션 압축 로딩 빠른 시간 확보
* `webpack -watch(w)` : 개발에서 빌드할 파일의 변화를 감지 와치 부록 참고
* `webpack -d` : sourcemap 포함하여 빌드 소스맵 부록
* `webpack --display-error-details` : error 발생시 디버깅 정보를 상세히 출력 웹팩에서 에러 발생시 상세한 디버깅 정보
* `webpack --optimize-minimize --define process.env.NODE_ENV="'production'"`배포시 추가 webpack.config.js 자동화 작업


## webpack watch 옵션

webpack 설정에 해당하는 파일이 변경이 일어나면 자동으로 번들링을 다시 진행
`webpack --progress --watch`

progress는 과정을 나타낸다.

참고 : `npm install --save-dev serve` 한 후 아래처럼 `package.json`에 명령어 설정 가능
``` js
"scripts" :{
  "start" : "serve"
}
```

## sourcemap 활용

웹팩에서 개발자 도구 연동

* 브라우저에서 webpack 으로 컴파일된 파일을 디버깅 하기란 어려움 오류 역 추적이 어렵다. 에러난 부분이 어느파일인지 모른다. 그럴때 필요한게 소스맵
* 따라서, 아래와 같이 source-map 설정을 추가하여 원래의 파일구조에서 디버깅이 가능 번들링 된 파일들에 대해서 각각의 파일들을 추적할 수 있는 옵션

``` js
module.exports = {
  ...
  devtool: '#inline-source-map'
}
```

[https://webpack.js.org/configuration/devtool/](https://webpack.js.org/configuration/devtool)

## gulp 연동

* 일반적으로 웹개발 개발 사이클을 자동화해주는 도구 성능을 위한 압축, 페이지 리프레쉬 등

* Gulp와 Webpack 모두 node.js 기반이기 떄문에 통합해서 사용하기 쉽다.
* Gulp로직에 webpack을 포함시킨다.

``` js
var gulp = require('gulp');
var webpack = require('webpack-stream');
var webpackConfig = require('./webpack.config.js');

gulp.task('default', function() {
  return gulp.src('src/entry.js')
    .pipe(webpack(webpackConfig))
    .pipe(gulp.dest('dist/'));
});
```

[https://gulpjs.com/](https://gulpjs.com/)

## Hot Module Replacement

* React기준 사용법 작성 HMR 로그에 찍힘 앱 로직 변경시 화면이 깜빡이지 않고 뒷단에서 필요한 모듈들만 변경

* 웹 앱에서 사용하는 JS 모듈들을 갱신할 때, 화면의 새로고침 없이 뒷 단에서 변경 및 삭제 기능을 지원
* 공식 가이드에는 React 를 기준으로 사용법이 작성되어 있으므로 참고

[https://webpack.js.org/concepts/hot-module-replacement/](https://webpack.js.org/concepts/hot-module-replacement/)

## 참고 문서

* Webpack2 Doc https://webpack.js.org/
* Webpack1 Doc http://webpack.github.io/docs/
* webpack-howto https://github.com/petehunt/webpack-howto
* webpack-howto2 https://gist.github.com/xjamundx/b1c800e9282e16a6a18e
* webpack beginners guide, Site Point * https://www.sitepoint.com/beginners-guide-to-webpack-2-and-module-bundling/?utm_s* ource=frontendfocus&utm_medium=email
* requireJS-to-webpackConfig https://www.npmjs.com/package/requirejs-to-webpack-cli
* migration from requirejs to webpack * https://medium.com/@ArtyomTrityak/migration-from-require-js-to-webpack-2-a733a436* 6ab5
* webpack-shimming https://webpack.js.org/guides/shimming/
* Critical-Dependencies * http://webpack.github.io/docs/context.html#critical-dependencies
* Gulp Webpack plugin https://www.npmjs.com/package/gulp-webpack
* Webpack Dev Server StackOverFlow * https://stackoverflow.com/questions/42712054/content-not-from-webpack-is-served-f* rom-foo
* Webpack Dev Middleware in Express * http://madole.github.io/blog/2015/08/26/setting-up-webpack-dev-middleware-in-your* -express-application/
* Webpack Confusing Part * https://medium.com/@rajaraodv/webpack-the-confusing-parts-58712f8fcad9
* Regular Expression, MDN * https://developer.mozilla.org/ko/docs/Web/JavaScript/Guide/%EC%A0%95%EA%B7%9C%EC%* 8B%9D
* Regular Expression Test http://regexr.com/
* Webpack First Principle, Video https://www.youtube.com/watch?v=WQue1AN93YU*
