---
title: 애드센스가 메뉴를 가리는 현상 수정하기
tags:
  - null
categories:
  - null
date: 2018-08-15 15:34:13
updated:
comments:
---

블로그 메뉴 위에 마우스를 올려봤는데 하위 메뉴들이 광고에 가리는 현상이 있었어요. `z-index`를 설정해주면 간단하게 해결할 수 있어요.

필자가 쓰고 있는 hexo hueman 테마의 경우 `/블로그/themes/hueman/source/css/_partialnav.styl` 파일에서 `.main-nav-list-child` 아래에 `z-index: 1`를 넣어주면 해결할 수 있어요. `1` 말고도 다른 값을 넣어도 되기는 하는데, 굳이 크게 할 필요가 없을 것 같아서 저렇게 설정했네요.