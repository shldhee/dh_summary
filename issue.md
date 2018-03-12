## 1. bxslider - 로딩시 이미지 정렬, 틀어짐, 따닥 등

* 해당 부모 `style="visibility:hidden; opacity:0;"` 추가

```js
<div class="bxsliderWrap" style="visibility:hidden; opacity:0;">
<<<<<<< HEAD
  <ul class="bxslider">//...</ul>
=======
  <ul class="bxslider">
    //...
  </ul>
>>>>>>> 2319a7523014d9092e541a7110c3ed80c40a5508
</div>
```

* `onSliderLoad` 속성 추가

```js
<<<<<<< HEAD
$('.bxslider').bxSlider({
=======
$(".bxslider").bxSlider({
>>>>>>> 2319a7523014d9092e541a7110c3ed80c40a5508
  auto: true,
  autoControls: true,
  captions: true,
  pause: 7000,
  onSliderLoad: function() {
<<<<<<< HEAD
    $('.bxsliderWrap')
      .css('visibility', 'visible')
      .animate({ opacity: 1 });
  },
=======
    $(".bxsliderWrap").css("visibility", "visible").animate({opacity:1});
  }
>>>>>>> 2319a7523014d9092e541a7110c3ed80c40a5508
});
```

**초기 스타일 줘서 안보이게 하고 bxslider 로드되면 그때 나타나게 한다.**
<<<<<<< HEAD

## 2. bxslider - 텍스트 위로 흐르게

```js
$('.hot-slider2').bxSlider({
  mode: 'vertical', // 방향 조절 수직
  minSlides: 4, // 최소 보여지는 슬라이드 갯수
  moveSlides: 1, // 슬라이드시 움직이는 개수
  // maxSlides: 5,
  auto: true,
});
```

* vTicker, 뉴스티커 비슷한 라이브러리

## 3. imagemap

* [https://www.image-map.net/](https://www.image-map.net/)
* [http://maschek.hu/imagemap/imgmap/](http://maschek.hu/imagemap/imgmap/)

## 4. scrollbar styling

* [https://codepen.io/devstreak/pen/dMYgeO](https://codepen.io/devstreak/pen/dMYgeO)

## 5. bxslider 참고 사이트

* [http://gpresss.tistory.com/31](http://gpresss.tistory.com/31)


## 6. rem, em

* [https://webdesign.tutsplus.com/ko/tutorials/comprehensive-guide-when-to-use-em-vs-rem--cms-23984](https://webdesign.tutsplus.com/ko/tutorials/comprehensive-guide-when-to-use-em-vs-rem--cms-23984)
* rem : 변환된 픽셀 크기는 페이지 최상위(root) 요소, 그러니까 html 요소의 폰트 크기가 기준이 됩니다. 이 최상위 요소의 폰트 크기에다 rem 단위로 지정한 숫자를 곱한 값이 바로 마지막 변환된 값입니다.
* em : em 단위를 쓰면 변환되는 픽셀값은 스타일을 **지정한 요소의 폰트 크기**를 곱한 값이 됩니다.

``` css
html {
  font-size: 16px;
  padding: 10rem; // 160px;
  padding: 10em; // 160px;
}
```

=======
>>>>>>> 2319a7523014d9092e541a7110c3ed80c40a5508
