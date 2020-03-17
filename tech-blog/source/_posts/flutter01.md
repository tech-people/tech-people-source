---
title: Flutter-2부 [개발환경 구축하기]
date: 2020-02-25 14:56:55
thumbnail : /images/flutter/thumbnail.jpg
tags: [flutter]
category : [IT Tech, 5. Flutter]
---

> 작성자 : 모바일 개발팀

# Flutter 개요

- 구글에서 개발한 크로스 플랫폼 모바일 앱 개발 프레임워크 입니다.
- Android와 iOS를 지원하며, 이후 구글의 차기 OS 퓨시아의 메인 개발 환경이 된다고 합니다.
- Dart 언어를 사용합니다.

# Flutter 의 특징

### 1. 네이티브 수준의 성능
- 플랫폼별 차이를 통합하여 Android와 iOS 모두에서 네이티브 수준의 성능을 제공합니다.

### 2. UI 렌더링
- 기존 UI를 버리고 자체적으로 UI를 렌더링 하기 때문에 Android와 iOS 모두 동일한 UI를 생성합니다.

### 3. Stateful Hot Reload
- Stateful Hot Reload 기능을 통해 수정사항을 빠르게 적용해 볼 수 있습니다.

# Flutter 개발환경 설정
- 안드로이드 스튜디오 기준으로 시작하겠습니다.

### 1. Android Studio 설치 / Flutter SDK 다운로드
- 아래 링크에서 안드로이드 스튜디오를 설치 해줍니다.


[https://developer.android.com/studio?hl=ko](https://developer.android.com/studio?hl=ko)

- 아래 링크에서 Flutter SDK를 다운로드 받습니다. 안드로이드 스튜디오에 Flutter SDK 경로를 입력해 줘야 하기 때문에 적당한 경로에 이동 시킵시다. 

[https://flutter-ko.dev/docs/get-started/install](https://flutter-ko.dev/docs/get-started/install)

### 2. Plugin 설치
- 안드로이드 스튜디오 실행 화면의 Configure > Plugins 에서 Flutter를 검색하여 설치 해줍니다.

![plugin01](/images/flutter/plugin01.png)

![plugin02](/images/flutter/plugin02.png)

- 설치 완료 시 Flutter 프로젝트를 생성 할 수 있는 메뉴가 추가 됩니다.

![plugin03](/images/flutter/plugin03.png)

### 3. 프로젝트 생성
- New Flutter Project 메뉴를 통해 프로젝트명, Flutter SDK 경로, 프로젝트 저장 경로 등을 설정하여 프로젝트를 생성합니다.

![project01](/images/flutter/project01.png)

![project02](/images/flutter/project02.png)

![project03](/images/flutter/project03.png)

### 3. Flutter doctor
- 터미널에서 flutter doctor 명령어를 통해 Flutter 개발 준비 상태를 확인 하실 수 있습니다.
```
flutter doctor
Doctor summary (to see all details, run flutter doctor -v):
[✓] Flutter (Channel stable, v1.12.13+hotfix.8, on Mac OS X 10.15.3 19D76, locale ko-KR)
[✓] Android toolchain - develop for Android devices (Android SDK version 29.0.2)
[✓] Xcode - develop for iOS and macOS (Xcode 11.3.1)
[✓] Android Studio (version 3.6)
[✓] Connected device (1 available)
• No issues found!
```

### 4. 프로젝트 실행
- flutter doctor 까지 완료 했다면 이제 프로젝트를 실행 해볼 수 있습니다.

![run01](/images/flutter/run01.png)






