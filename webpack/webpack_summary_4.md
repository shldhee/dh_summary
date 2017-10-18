# 웹팩

## 미들웨어

클라이언트 와 서버 사이를 구성

서버 요청 클라이언트 요청 시

중간에 웹팩이 관여하여 최적화 및 번들링 등

* webpack-dev-server: webpack 자체에서 제공하는 개발 서버이고 빠른 리로딩 기능 제공

* wepback-dev-middleware: 서버가 이미 구성된 경우에는 webpack을 미들웨어로 구성하여 서버와 연결

Webpack Dev Server 페이지 자동고침을 제공하는 webpack 개발용 node.js 서버

설치 및 실행 npm install --save-dev webpack-dev-server

실행 webpack-dev-server --open

또는 package.json 추가

“scripts”: { “start”: “webpack-dev-server” }

npm start 시 실행됨

https://webpack.js.org/configuration/dev-server/

## Webpack Dev Server Option

* publicPath : Webpack으로 번들한 파일들이 위치하는 곳 default값은 /
* 항상 ‘/‘를 앞뒤로 붙여야 한다.
* publicPath : “/assets”/
* contentBase : 서버가 로딩할 static 파일 경로를 지정. default 값은 working directory
* // 절대 경로를 사용할것
* contentBase: path.join(__dirname, “public”)
* contentBase: false static 폴더? 서버로 자원 로딩시 지정할 경로 의미 번들링 되기전 또는 이후에 결과물들을 한번에 로딩할수 있는 경로
* compress: gzip 압축 방식을 이용하여 웹자원의 사이즈를 줄인다.
* compress: true
* Gzip : 클라이언트, 서버간의 압축 방식 클라이언트에서 Gzip 풀수 있다고 하면 서버에서 Gzip으로 압축하여 클라이언트에 전달 클라이언트는 Gzip을 풀어 원사이즈로 만들어 활용

웹 성능 최적화 [https://ko.wikipedia.org/wiki/Gzip](https://ko.wikipedia.org/wiki/Gzip)

실습

example3하고 다른점 webpack-dev-server 사용시 dist 폴더가 만들어지지 않는다. filepath로 접근할 수 없다. in-memory 에 있다. 메모리에서 바로 브라우저로 간다.

배포시
webpack 쳐서 dist폴더 생성 후 배포

인메모리 [https://webpack.github.io/docs/webpack-dev-server.html#content-base](https://webpack.github.io/docs/webpack-dev-server.html#content-base)

### [부록] Path vs Public Path

ouput : Path dev-server : Public Path

webpack dev server의 path, publicPath를 구분하기 위해 파악
output의 path : 번들링할때 정적으로 나오는 파일이 나오는곳 위치
dev의 public path : 로더, 기타기능 url 이미지 cdn 그런것들 일일이 수동 힘드니깐 웹팩자체로 퍼블릭패스 설정해놓으니 주소가 박혀져서 나온다.
output.path: 번들링한 결과가 위치할 번들링 파일의 절대 경로 output.publicPath: 브라우저가 참고할 번들링 결과의 파일의 URL 주소를 지정. (CDN을 사용하는 경우 CDN 호스트 지정)

```` js
// publicPath 예제 #1
output: { path: “/home/proj/public/assets”, putblicPath: “/assets/“ }

// publicPath 예제#2
output: { path: “/home/proj/cdn/assets/[hash]”, publicPath: “http://cdn.example.com/assets/[hash]/“ }

사례

//Development: Both Server and the image are on localhost 개발 환경
.image { background-image: url(‘./test.png’); }

//Production: Server is on Heroku but the image is on a CDN 배포
//P.image { background-image: url(‘https://someCDN/test.png'); }
//
```

Webpack Dev Middleware 기존에 구성한 서버가 있을때 웹팩 사용 node.js 서버

기존에 구성한 서버에 webpack에서 컴파일한 파일을 전달하는 middleware wrapper
webpack 에 설정한 파일을 변경시, 파일에 직접 변경 내역을 저장하지 않고 메모리 공간을 활용한다.(dev-server 실행시)
따라서, 변경된 파일 내역을 파일 디렉토리 구조안에서는 확인이 불가능하다.
