#	Webpack

## Webpack을 사용하는 이유 및 배경

1. 새로운 형태의 Web Task Mangager
	* 기존 Web Task Manager (Gulp, Grunt)의 기능 + 모듈 의존성 관리
	* 예) minification을 Webpack default cli 로 실행 가능

2. 자바스크립트 코드 기반 모듈 관리
	* 기존 모듈 로더들과 차이점 : 모듈 간의 관계를 청크(chunk) 단위로 나눠 필요할 때 로딩
	* 현대의 웹에서 JS 역할이 커짐에 따라, Client Side에 들어가는 코드량이 많아지고 복잡해짐
	* Common JS, AMD, ES6 Modules 등이 등장
	* 가독성 및 다수 모듈 미 병행 처리등의 약점을 보완하기 위해 Webpack이 등장

**자바스크립트 모듈화 문제**

``` js
<script src="module1.js"></script>
<script src="module2.js"></script>
<script src="module3.js"></script>
```

* 위 모듈 로딩 방식의 문제점
	1. 전역 변수 충돌
		``` js
		//module1.js
		var module = function () {};

		//module2.js
		var module = function () { return 1 };
		```

		위 코드는 module2.js의 module이 module1.js의 module을 덮어 씌어 문제가 발생한다.

	1. 스크립트 로딩 순서
	1. 복잡도에 따른 관리상의 문제

## webpack 철학

1. 모든 것이 모듈이다.
	* 모든 웹 자원(js, css, html)이 모듈 형태로 로딩 가능(js 파일로 들어간다.)
	``` js
	require('base.css');
	require('main.js');
	```

1. 초기에 불필요한 것들을 모두 로딩하지 않고, 필요할때 필요한 것만 로딩하여 사용

## Webpack 시작하기

node, npm 설치
npm install -g webpack
npm init -y // package.json 디폴트 생성

`webpack app/index.js dist/bundle.js`
cli로 다른 기능까지 넣으면 너무 복잡해져
`webpack.config.js` 생성한다.
`webpack`

## webpack entry

* webapack으로 묶은 모든 라이브러르들을 로딩할 시작점 설정
* a,b,c 라이브러러를 모두 번들링한 bundle.js를 로딩한다.
* 1개 또는 2개 이상의 엔트리 포인트를 설정할 수 있다.

``` js
entry: './pulbic/src/index.js', // string

entry: ['./pulbic/src/index.js'], // array

entry: {
	index: './pulbic/src/index.js' // object
}
```
