---
layout: single
title:  "Github blog - 폰트 추가, 폰트 적용, 포스트 제목 밑줄 제거"
date:   2021-09-01 20:31:00 +0530
categories: "Github"
tags : [Github, "Minimal mistakes"]
toc: true
toc_sticky: true
toc_label: "Contents"
---

<br><br>

# 폰트 변경
---

1. 폰트 추가

  - CSS

      `assets/main.scss` 에서 원하는 폰트 코드 추가

      ```scss
      @import url('https://cdn.jsdelivr.net/font-iropke-batang/1.2/font-iropke-batang.css');
      @font-face {
          font-family: 'RIDIBatang';
          src: url('https://cdn.jsdelivr.net/gh/projectnoonnu/noonfonts_twelve@1.0/RIDIBatang.woff') format('woff');
          font-weight: normal;
          font-style: normal;
      }
      ```

  - HTML

      `_layouts/default.html` 에서 원하는 폰트 코드 추가

      ```xml
      <!--폰트 : "Nanum Gothic Coding", "Coming Soon"-->
      <link rel="preload" as="style" href="https://fonts.googleapis.com/css2?family=Coming+Soon&family=Nanum+Gothic+Coding&display=swap">
      <link rel="stylesheet" href="https://fonts.googleapis.com/css2?family=Coming+Soon&family=Nanum+Gothic+Coding&display=swap">
      ```


2. 폰트 적용

    `_sass/_variables.scss` 

    ```scss
    /* system typefaces */
    $serif: Georgia, Times, serif !default;
    $sans-serif: "RIDIBatang","Iropke Batang", -apple-system, BlinkMacSystemFont, "Roboto", "Segoe UI",
      "Helvetica Neue", "Lucida Grande", Arial, sans-serif !default;
    $monospace: Monaco, Consolas, "Lucida Console", monospace !default;
    $youp: "Cafe24Oneprettynight" !default;
    ```

    사용하고자 하는 폰트를 가장 앞에 적어준다.

 <br><br><br><br>   

# 폰트 크기 변경
---

`_sass/_reset.scss` 에서 폰트 사이즈 조정. 작성된 코드는 창의 크기에 따라 변하는 폰트의 크기이다.

```scss
tml {
  /* apply a natural box layout model to all elements */
  box-sizing: border-box;
  background-color: $background-color;
  font-size: 14px;

  @include breakpoint($medium) {
    font-size: 16px;
  }

  @include breakpoint($large) {
    font-size: 16px;
  }

  @include breakpoint($x-large) {
    font-size: 16px;
  }

  -webkit-text-size-adjust: 100%;
  -ms-text-size-adjust: 100%;
}
```

<br><br><br><br>

# 포스트 제목 밑줄 제거
---

`_sass/_base.scss` 에서 &:focus 와 같은 레벨에 text-decoration: none; 코드를 추가해준다.

```scss
/* links */

a {
  text-decoration: none;
  &:focus {
    @extend %tab-focus;
  }

  &:visited {
    color: $link-color-visited;
  }

  &:hover {
    color: $link-color-hover;
    outline: 0;
  }
}
```