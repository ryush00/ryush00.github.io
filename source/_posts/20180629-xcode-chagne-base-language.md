---
title: Xcode에서 기본 언어 한국어로 바꾸기
tags:
  - Xcode
categories:
  - Programming
  - Swift
date: 2018-06-29 23:46:00
updated:
comments:
---
맥은 한국어지만 Xcode는 영어로 뜨고, Xcode에서 만들어진 앱들은 기본 언어가 한국어가 아닌 영어가 되어버려요. 작동은 되지만 나중에 I18n을 고려하게 되면 약간 설정이 꼬이는 문제가 발생해요.

### 해결 방법
해겳 방법은 간단..하지 않아요. 약간 복잡해요. (StackOverflow를 참고했어요)

1. Xcode project에서 한국어를 추가하기.
2. ```Info.plist```에서 ```Localization native development region```을 ```Korean```으로 바꾸기.
3. Xcode를 닫고, 다른 에디터를 이용해서 ```프로젝트이름.xcodeproj/project.pbxproj``` 파일을 열고 ```developmentRegion```를 검색하여 ```developmentRegion = ko;```로 바꿔주기.
