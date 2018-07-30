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
viewWillTransition에서 화면 회전시 `view.bounds`로 CGRect를 얻어오게 되면 화면이 회전되기 이전의 CGRect가 리턴된다. 이 사이즈를 다른 view에 적용시키게 되면 항상 view가 짤려보일수 밖에 없을것이다. 무언가 viewDidTransition 같은거도 있을지도 모르지만 일단 아래 코드처럼 해결할 수도 있다.
 
```Swift
    override func viewWillTransition(to size: CGSize, with coordinator: UIViewControllerTransitionCoordinator) {
        super.viewWillTransition(to: size, with: coordinator)
        let rect = CGRect(origin: CGPoint(x: 0, y: 0), size: size)
        videoPreviewLayer?.frame = rect
    }
```
