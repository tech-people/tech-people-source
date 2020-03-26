---
title: Flutter-1부 [Flutter란 무엇인가?]
date: 2020-03-15 14:56:55
thumbnail : /images/flutter/thumbnail.jpg
tags: [flutter]
category : [IT Tech, 5. Flutter]
---

> 작성자 : 솔루션 모바일팀

# 목차
### 1.Flutter
- Flutter란?
- 장점 및 단점
- Framework 구조

### 2.React Native와 비교

### 3.Dart언어
- Dart가 만들어진 이유
- Dart의 특성
- Dart의 실행방식

### 4.References

# Flutter
### 1. Flutter란?
- iOS의 스토리보드나 Android의 xml에서 UI를 만들고, 불러오는 방식으로 코딩을 했었습니다. 플러터의 가장 큰 특징은 UI까지 모두 코드로 작성한다는 점입니다.
구글에서 새롭게 선보이는 프레임워크이며, 개발자가 iOS와 Android 모두를 위한 고품질 기본 인터페이스를 제작하는데 도움을 주는 크로스 플랫폼 프레임워크입니다.

![flutter02_1](/images/flutter/flutter02_1.PNG)

- 2019년 기준으로 Flutter는 React-Natice보다 선호하며, 75.4%로 높은 순위를 차지했습니다.

### 2.장점 및 단점
##### 1)장점
- 뛰어난 성능
 직접 컴파일 되어서 직접 Render하기 때문에 훨씬 빠르다.
- 신속한 개발
 상태를 기록하는 핫리로드(Stateful Hot Reload), 새로운 반응형 프레임워크, 다양한 위젯 세트 및 통합 도구와 같은 기능을 제공한다.
- 쉬운 크로스 프랫폼
 Android와 iOS 기반의 앱을 하나의 코드 베이스로 개발할 수 있다. Android의 material UI와 iOS의 cupertino UI를 플랫폼 제한 없이 상대 OS에서도 사용할 수 있다.
- 풍부하고 유연한 디자인
 작성 가능한 위젯 세트와 풍부한 애니메이션 라이브러리 및 확장 가능한 계층형 아키텍처를 제공한다.
- 고품질 환경
 이식성 있는 GPU 가속 렌더러 및 고성능의 네이티브 ARM 코드 런타임, 플랫폼 상호 운용성 기능을 통해 기기 및 플랫폼 전반에 걸쳐 고품질 환경을 지원한다.
 
##### 2)단점
- Dart라는 언어 사용하여 관련 오픈소스가 많지 않다.
- UI가 획일적으로 보일 수 있다.

### 3.Framework 구조

![flutter02_3](/images/flutter/flutter02_3.png)

- Flutter는 Dart로 구성되어 있는 코드들을 Flutter Engine을 통해서 Platform(Android, iOS 등)과 통신하게 되어있고, 이 과정이 JIT컴파일이던 AOT컴파일이던 상관 없이 동작합니다.
 또한 Dart의 중요한 개념 중 snapshot이라는 개념이 있는데, Dart 프로그램이 최초로 실행될 때 느리게 실행되는 것을 방지하기 위해서 파일을 미리 패키징하여 불러오는 방식이 있습니다. 이를 통해 최초 로딩 속도가 빨라지는 장점이 있습니다.
- widget
 Image, Icon, Text 그리고 Row, Column, Padding도 모두 위젯이다. 프로젝트를 만들어보면, main.dart에서 runApp함수에 위젯을 전달하는 것부터 앱을 시작하는 것을 확인 가능하다.
- Rendering
 위젯으로 만들어진 Layer Tree를 Skia라는 그래픽 라이브러리를 이용하여 화면을 만들어낸다.
 
 ※ JIT(Just In Time)  
:프로그램을 실제 실행하는 시점에 기계어로 번역하는 컴파일 기법이다.
 
 ※ AOT(Ahead Of Time)  
:미리 번역한 파일을 실행하는 기법이다.
 
# React Native와 비교

|                |Flutter   |React Native|
|:---------------|:---------|:-----------|
|언어|Dart|Javascript|
|외부 패키지|구글이 선두에 있는 만큼 빠르게 업데이트가 진행되고 있긴 하지만 아직은 패키지 버전들이 낮다.|라이브러리 호환성이 상당히 좋고, 패키지를 많이 보유하고 있다.|
|코드 재사용성|오버라이딩이 허용되어 코드 재사용이 가능하다.|코드의 재사용을 허용하지만 몇 가지 요소로 제한되어 있고, 재활용을 위한 스타일화의 시간이 오래 걸린다.|
|UI|자체적인 위젯으로 동작한다.|Native Component를 기반으로 한다.|
|개발속도|개발속도는 React Native와 비교하면 느리다.|개발속도는 Flutter와 비교하면 빠르다.|

# Dart언어
### 1.Dart가 만들어진 이유?
- 구글에서 만든 프로그래밍 언어로 플러터를 작성하는데 사용됩니다. 다트는 구글이 server-side 및 front-end 코드를 작성하는데 javascript 보다 더 나은 언어를 원했기에 만들어졌습니다. 
- 구글은 아래와 같은 목표를 가지고 언어를 제작하게 되었습니다.
  - 유연한 구조의 언어
  - 개발자들에게 친근하고 자연스러운 언어
  - 웹으로 접근하는 장비들을 모두 지원 가능한 언어
  - 많이 사용되는 모던 브라우저들을 지원 가능한 언어
  
### 2.Dart의 특성
- OOP(Object Oriented Programming)언어
- Interface를 가진 단일 상속되는 Class를 가진 언어
- isolates 기능을 통해 single-thread 언어임에도 불구하고 동시에 여러 프로세스를 동작 가능

### 3.Dart의 실행방식

![flutter02_2](/images/flutter/flutter02_2.png)

- Dart VM이 있는 브라우저(Chrome/Chromium)에서는 Dart Source가 바로 실행이 됩니다. 지원하지 않는 브라우저를 위해서는 DartC를 통해 Javascript 파일을 생성하여 실행 가능합니다.

# References
- https://brunch.co.kr/@myner/5
- https://medium.com/flutter-korea/%EC%99%9C-flutter%EB%8A%94-dart%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%98%EB%8A%94%EA%B0%80-e838b9415f57
- https://kimch3617.tistory.com/entry/Flutter%EB%9E%80-Flutter%EC%9D%98-%ED%8A%B9%EC%A7%95
- https://medium.com/withj-kr/react-native-vs-flutter-%ED%81%AC%EB%A1%9C%EC%8A%A4%ED%94%8C%EB%9E%AB%ED%8F%BC-%EC%95%B1-%EA%B0%9C%EB%B0%9C-%EC%96%B4%EB%96%A4-%ED%94%84%EB%A0%88%EC%9E%84%EC%9B%8C%ED%81%AC%EB%A5%BC-%EC%82%AC%EC%9A%A9%ED%95%B4%EC%95%BC-%ED%95%98%EB%82%98-701ecbc19a24
- https://skuld2000.tistory.com/69
- https://medium.com/@pks2974/flutter-%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC%ED%95%98%EA%B8%B0-9532e16aff57
- https://javaexpert.tistory.com/974
- https://jaceshim.github.io/2019/01/28/flutter-study-stateful-widget-lifecycle/
- https://insights.stackoverflow.com/survey/2019#methodology
- https://medium.com/@pks2974/bloc-%EC%9D%B4%ED%95%B4-%ED%95%98%EA%B8%B0-%EB%B0%8F-%EA%B0%84%EB%8B%A8-%EC%A0%95%EB%A6%AC-%ED%95%98%EA%B8%B0-7dc705e4c640
- http://murmurblog.com/layouts-in-flutter/



