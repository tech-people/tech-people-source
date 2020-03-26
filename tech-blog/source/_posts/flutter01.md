---
title: Flutter-2부 [개발환경구축]
date: 2020-03-16 14:56:55
thumbnail : /images/flutter/thumbnail.jpg
tags: [flutter]
category : [IT Tech, 5. Flutter]
---
> 작성자 : 솔루션 모바일팀
# Flutter 개발 환경 구축
- Flutter는 Android Studio, IntelliJ, VS Code 등에서 Plugin을 제공합니다.
- 여기서는 Android Studio 를 기준으로 다음과 같이 개발 환경을 구축합니다.
1. Android Studio 설치 / Flutter SDK 다운로드
2. Flutter Plugin 설치
3. 프로젝트 생성
4. flutter doctor 확인
5. 프로젝트 실행
### 1. Android Studio 설치 / Flutter SDK 다운로드
- 아래 링크에서 안드로이드 스튜디오를 설치 해줍니다.
[https://developer.android.com/studio?hl=ko](https://developer.android.com/studio?hl=ko)
- 아래 링크에서 Flutter SDK를 다운로드 받습니다. 안드로이드 스튜디오에 Flutter SDK 경로를 입력해 줘야 하기 때문에 적당한 경로에 이동 시킵시다.
[https://flutter-ko.dev/docs/get-started/install](https://flutter-ko.dev/docs/get-started/install)
### 2. Flutter Plugin 설치
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
### 3. flutter doctor 확인
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
### 소스 코드 작성
- 기본적으로 생성해 주는 프로젝트는 FloatingActionButton 을 누르면 숫자를 카운트 하여 보여주는 앱 입니다. 그런데 늘어나기만 합니다.. 초기화 해주는 버튼을 한번 넣어 봅시다.
- 아래 main.dart의 소스 코드 중 body 부분의 소스코드를 보자면 Column이 있습니다. Column은 하위 위젯을 세로로 배치 할 수 있는 위젯입니다. 이 Column에 버튼을 추가하여 counter 변수를 초기화 해 주면 될 것 같습니다.
```
import 'package:flutter/material.dart';
void main() => runApp(MyApp());
class MyApp extends StatelessWidget {
 @override
 Widget build(BuildContext context) {
   return MaterialApp(
     title: 'Flutter Demo',
     theme: ThemeData(
       primarySwatch: Colors.blue,
     ),
     home: MyHomePage(title: 'Flutter Demo Home Page'),
   );
 }
}
class MyHomePage extends StatefulWidget {
 MyHomePage({Key key, this.title}) : super(key: key);
 final String title;
 @override
 _MyHomePageState createState() => _MyHomePageState();
}
class _MyHomePageState extends State<MyHomePage> {
 int _counter = 0;
 void _incrementCounter() {
   setState(() {
     _counter++;
   });
 }
 @override
 Widget build(BuildContext context) {
   return Scaffold(
     appBar: AppBar(
       title: Text(widget.title),
     ),
     body: Center(
       child: Column(
         mainAxisAlignment: MainAxisAlignment.center,
         children: <Widget>[
           Text(
             'You have pushed the button this many times:',
           ),
           Text(
             '$_counter',
             style: Theme.of(context).textTheme.display1,
           ),
         ],
       ),
     ),
     floatingActionButton: FloatingActionButton(
       onPressed: _incrementCounter,
       tooltip: 'Increment',
       child: Icon(Icons.add),
     ),
   );
 }
}
```
- Column에 MaterialButton을 추가해주고 텍스트, 컬러 등을 적용해 주었습니다. 그리고 onPressed를 통해 버튼이 눌렸을 경우 counter 변수를 초기화하고 초기화 한 상태를 다시 적용해주기 위해 setState를 사용하였습니다.
```
@override
 Widget build(BuildContext context) {
   return Scaffold(
     appBar: AppBar(
       title: Text(widget.title),
     ),
     body: Center(
       child: Column(
         mainAxisAlignment: MainAxisAlignment.center,
         children: <Widget>[
           Text(
             'You have pushed the button this many times:',
           ),
           Text(
             '$_counter',
             style: Theme.of(context).textTheme.display1,
           ),
           MaterialButton(
             color: Colors.blue,
             child: Text(
               '초기화',
               style: TextStyle(
                 color: Colors.white
               )),
             onPressed: () {
               setState(() {
                 _counter = 0;
               });
             },
           )
         ],
       ),
     ),
     floatingActionButton: FloatingActionButton(
       onPressed: _incrementCounter,
       tooltip: 'Increment',
       child: Icon(Icons.add),
     ),
   );
```
- 이제 실행해 보면, 초기화 버튼이 생기고 카운트 초기화가 잘 작동하는지 확인 하실 수 있습니다.
![run02](/images/flutter/run02.png)
