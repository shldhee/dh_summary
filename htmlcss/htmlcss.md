# html, css 공부하기

## 1. `form` 사용시 `<a><button></button></a>`

* 작동 되나 유효성 검사를 하면 오류가 발생한다.
* `form` 태그 밖에서는 작동 된다.
* `form` 태그 안에서는 작동이 안된다.

``` html
<a href="http://shldhee.github.io"><button>click</button></a>
<form>
  <fieldset>
    <legend>
    </legend>
    <a href="http://shldhee.github.io"><button>click!!</button></a>
  </fieldset>
</form>
```

참고 : [https://stackoverflow.com/questions/6393827/can-i-nest-a-button-element-inside-an-a-using-html5](https://stackoverflow.com/questions/6393827/can-i-nest-a-button-element-inside-an-a-using-html5)

## 2. `meta`태그를 이용한 새로고침(리다이렉션)

* 사이트 새로고침
* 다른 사이트 주소로 리다이렉션

``` html
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">

  <meta http-equiv="refresh" content="0; url=http://shldhee.github.io"> </head>

  <title>Document</title>
</head>
```

`<meta http-equiv="refresh" content="0; url=http://shldhee.github.io"> </head>`

* `content` 에서 `0;`은 0초마다 refresh
* `url`은 리다이렉션할 url 주소 (""쓰지 않는다.)


**`meta`태그에 refresh 사용하지 마십시오. 검색 순위가 낮아집니다.**

출처 : [https://msdn.microsoft.com/ko-kr/library/ff724055(v=expression.40).aspx](https://msdn.microsoft.com/ko-kr/library/ff724055(v=expression.40).aspx
)

## 
