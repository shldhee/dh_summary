## 1. bxslider - 로딩시 이미지 정렬, 틀어짐, 따닥 등

* 해당 부모 `style="visibility:hidden; opacity:0;"` 추가

```js
<div class="bxsliderWrap" style="visibility:hidden; opacity:0;">
  <ul class="bxslider">
    //...
  </ul>
</div>
```

* `onSliderLoad` 속성 추가

```js
$(".bxslider").bxSlider({
  auto: true,
  autoControls: true,
  captions: true,
  pause: 7000,
  onSliderLoad: function() {
    $(".bxsliderWrap").css("visibility", "visible").animate({opacity:1});
  }
});
```

**초기 스타일 줘서 안보이게 하고 bxslider 로드되면 그때 나타나게 한다.**
