---
title: viewWillTransition에서 화면 회전 후의 CGRect 얻기
tags:
  - iOS
  - Swift
categories:
  - Programming
  - iOS
date: 2018-07-30 20:53:09
updated:
comments:
---
viewWillTransition에서 화면 회전시 `view.bounds`로 CGRect를 얻어오게 되면 화면이 회전되기 이전의 CGRect가 리턴돼요. 이 사이즈를 다른 view에 적용시키게 되면 항상 view가 짤려보일수 밖에 없어요. 아마 `viewDidTransitio`(추측)같은게 있을지도 모르지만, 일단 해결 방법은 아래와 같아요.

```Swift
    override func viewWillTransition(to size: CGSize, with coordinator: UIViewControllerTransitionCoordinator) {
        super.viewWillTransition(to: size, with: coordinator)
        let rect = CGRect(origin: CGPoint(x: 0, y: 0), size: size)
        videoPreviewLayer?.frame = rect
    }
```
