---
title: Flutter-1부 [Flutter란 무엇인가?]
date: 2020-02-27 14:56:55
thumbnail : /images/flutter/thumbnail.jpg
tags: [flutter]
category : [IT Tech, 5. Flutter]
---

> 작성자 : 모바일 개발팀

# 목차
### 1.Flutter
- Flutter란?
- 장점 및 단점
- Framework 구조
- BLoC Pattern
- 위젯 종류
### 2.React Native와 비교
### 3.Dart언어
- Dart가 만들어진 이유
- Dart의 특성
- Dart의 실행방식
- Stateful Widget Lifecycle
### 4.References

# Flutter
### 1. Flutter란?
- iOS의 스토리보드나 Android의 xml에서 UI를 만들고, 불러오는 방식으로 코딩을 했었습니다. 플러터의 가장 큰 특징은 UI까지 모두 코드로 작성한다는 점입니다.
구글에서 새롭게 선보이는 프레임워크이며, 개발자가 iOS와 Android 모두를 위한 고품질 기본 인터페이스를 제작하는데 도움을 주는 크로스 플랫폼 프레임워크입니다.

![flutter02_1](/images/flutter/flutter02_1.PNG)

- 2019년 기준으로 Flutter는 React-Natice보다 선호하며, 75.4%로 높은 순위를 차지했습니다.

### 2.장점 및 단점
#### 1)장점
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
#### 2)단점
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
 
### 4.BLoC Pattern
- BloC Pattern이란?

 BLoC(Bussiness Logic Component) Pattern 은 Flutter 의 상태 관리를 제어하기 위해서 Google 개발자에 의해서 디자인 되었습니다. Flutter 에서는 상태에 따라서 렌더링이 일어나기 때문에, 상태 관리가 매우 중요합니다. BLoC는 UI 와 Bussiness Logic 을 분리하여, 각각 코드의 의존성을 낮추게 해줍니다. Flutter 을 위해 설계 되었지만, 디자인 패턴이기 때문에, 어떠한 프레임워크 나 언어에서도 사용이 가능합니다.
 
- BLoC의 특징

 ① UI 에서는 여러 BLoC 이 존재할 수 있다.  
 ② UI 에서는 화면에 집중하고, BLoC 에서는 Logic 에 집중한다.  
 ③ UI 에서는 BLoC 의 내부 구현에 대해서 몰라도 된다.  
 ④ BLoC 은 여러 UI 에서 구독 할 수 있다. 때문에 재사용이 용의하다.  
 ⑤ BLoC 만을 분리해서 테스트가 가능하다.
 
- BLoC의 형태

 BLoC 에서 각 UI 객체 들은 BLoC 객체를 구독하고 있다.
 BLoC 객체의 상태가 변경되면, BLoC 의 상태를 구독중인 UI 객체 들은 그 즉시 해당 상태로 UI 를 변경한다.
 BLoC 객체는 UI 객체로 부터 이벤트를 전달받으면, BLoC 객체는 필요한 Provider 나 Repository 로 부터 데이터를 전달받아, Bussiness Logic 을 처리한다.
 Bussiness Logic 을 처리한후, BLoC 객체를 구독중인 UI 객체 들에게 상태를 전달한다.
 
  ```
  class BLoC {
      provider: Provider = new Provider();
      stream: Subject = new Subject();
      async sink() {
          const data = await Provider.getCounterModel();
          const result = await this.BussinessLogic(data);
          this.stream.next(result);
      }
      private async BussinessLogic() {
          // ...
      }
  }
  ```
  
 UI 객체는 구독중이던 BLoC 객체의 상태가 변경되면 상태를 전달받는데, 이때 얻은 상태를 이용하여 화면을 재구성한다.
 
  ```
  class UI {
      bloc = new BLoC();
      constructor() {
          this.bloc.stream.subscribe((data) => {
              this.render(data);
          });
      }
      render(data) {
          return (
              <div onClick={this.bloc.sink}>
                  {data}
              </div>
          )
      }
  }
  ```
  
- Flutter와 BLoC

 Flutter 에서는 Stream 을 이용해서 BLoC 을 구현한다.
 StreamController 으로 Observable 객체를 생성한다.
 StreamController 의 Sink 를 이용해서 값을 전달한다.
 StreamController 의 Steam 를 이용해서 상태를 구독한다.
 이때 RxDart 를 이용하여 StreamController 을 쉽게 만들 수 있다.
 
  ```
  class Bloc {
    final _repository = Repository();
    final _subject = PublishSubject<String>();
    Observable<String> get stream => _subject.stream;
    action() async {
      String result= await _repository.getData();
      _subject.sink.add(result);
    }
    dispose() {
      _subject.close();
    }
  }
  ```
  
 UI 에서는 StatefulWidget 을 쓰지 않고, 그리고 setState 를 쓰지 않고도 bloc 의 상태 변경에 따라 UI 를 변경할 수 있다.
 
  ```
  class UI extends StatelessWidget {
    UI() {
      bloc.action();
    }
    @override
    Widget build(BuildContext context) {
      return Scaffold(
        // ...
        body: StreamBuilder(
          stream: bloc.stream,
          builder: (context, AsyncSnapshot<String> snapshot) {
            return Text(snapshot.data);
          },
        ),
      );
    }
  }
  ```
 
### 5.위젯 종류
 widget은 widgets library의 standard widget과 Material library의 widget 입니다.
 widgets library는 어느 app이든 사용가능하지만 Material library는 Material app에서만 사용 가능합니다.
 
 1)Standard widgets
 - Container: padding, margins, borders, background color, 기타 style을 적용 가능
 - GridView: widget 을 스크롤 가능한 grid 모양으로 배치
 - ListView: widget 을 스크롤 가능한 list 로 배치
 - Stack: widget을 순차적으로 쌓아가면서 표시
 
 2)Material widgets
 - Card: 관련된 정보를 box 안에 표시
 - ListTile: 최대 3줄의 text를 표시하고, trailing icon 삽입
 
 3)Container
 
 ![flutter02_4](/images/flutter/flutter02_4.png)
 
 - padding, margin, border 사용 가능
 - background color 또는 image 변경 가능
 - 하나의 child widget을 갖는다. 다만, child widget으로 Row, Column 등 다수의 자식을 갖는 widget이 올 수 있다.

 4)GridView
 
 ![flutter02_5](/images/flutter/flutter02_5.png)
 
 - grid 형태로 widget 배치
 - scrolling지원
 - 직접 custom grid를 만들던지, 아래 제공되는 두가지 속성을 사용가능
 - GridView.count 지정된 개수 만큼의 column 을 제공
 - GridView.extent 타일의 최대 width를 지정
 - MediaQuery.of(context).orientation 를 사용하여 device의 landscape 나 portrait 을 인지하여 적절히 반응하도록 할 수 있다.
 
 5)ListView
 
 ![flutter02_6](/images/flutter/flutter02_6.png)
 
 - Column형 widget으로 list를 만든다.
 - 수평 또는 수직의 layout을 만들 수 있다.
 - 자동으로 스크롤링을 만든다.
 - Column widget 보다 확장성은 적다.
 
 6)Stack
 
 ![flutter02_7](/images/flutter/flutter02_7.png)
 
  Stack을 이용해 image 상단에 그라디언트를 넣습니다. 이 그라디언트를 통해 toolbar 의 icon이 iamge와 구분됩니다.
 
 - 다른 widget 위에 widget을 배치
 - 첫번째 widget이 base widget 이다. 그 위로 다른 widget 이 배치된다.
 - Stack 은 scroll 되지 않는다.
 - box를 넘는 자식 widget을 자를 수 있다.
 
 7)Stack
 
 ![flutter02_8](/images/flutter/flutter02_8.png)
 
 - Material card 이다.
 - 관련된 정보를 표현하는데 사용
 - 자식을 하나만 갖지만, 멀티플 자식을 갖는 위젯을 자식으로 가질 수 있다.
 - 둥근 테두리와 그림자를 보여준다.
 - 스크롤이 되지 않는다.
 - Material library 의 widget 이다.
 
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

### 4.Stateful Widget Lifecycle
 StatefulWidget을 만들 때 State라는 오브젝트를 만듭니다. 이 오브젝트는 위젯이 동작하는 동안 mutable state를 뜻합니다.
- state 정의

 ①위젯에 의해서 사용되어지는 데이터는 변할 수 있다.
 
 ②위젯이 빌드될 때 데이터들을 동기적으로 읽을 수 없다. 모든 state들은 build 함수가 호출될 때까지 설정되어야 한다.
 
- Lifecycle

 ① createState()  
:Framework가 StatefulWidget을 만들경우 createState()가 즉시 호출된다.
 
 ② mounted is true  
:모든 위젯은 this.mounted : bool 속성을 가지고 있다. 즉 buildContext가 할당될 때 this.mounted가 true로 리턴된다.
 
 ③ initState()  
:widget이 만들어지고 생성자 후에 처음 메소드 실행할때 이 함수가 실행된다. super.initState() 를 필수적으로 호출해야 한다.

 ④ didChangeDependencies()  
:위젯이 최초 생성될때 initState 다음에 바로 호출 된다. 또한 위젯이 의존하는 데이터의 객체가 호출될때마다 호출된다. 예를 들면 업데이트되는 위젯을 상속한 경우 공식문서 또한 상속한 위젯이 업데이트 될때 네트워크 호출(또는 다른 비용이 큰 액션)(역자주: API호출)이 필요한 경우 유용하다고 함.
 
 ⑤ build()  
:필수적으로 오버라이딩해서 구현해야되는 함수이다. 위젯을 리턴한다.
 
 ⑥ didUpdateWidget(Widget oldWidget)  
:didUpdateWidget()는 부모 위젯이 변경되어 이 위젯을 재 구성해야 하는 경우(다은 데이터를 제공 해야하기 때문) 이것은 플러터가 오래동안 유지 되는 state를 다시 사용하기 때문이다. 이 경우 initState()에서 처럼 읿부 데이터를 다시 초기화 해야 한다. 
 
 ⑦ setState()  
:setState() 메서드는 플러터 프레임워크 자체적, 또는 개발자로 부터 자주 호출된다. '데이터가 변경되었음’을 프레임워크에 알리는데 사용되며 build context의 위젯을 다시 빌드하게 한다. setState()는 비동기적이 않은 callback을 사용한다.(역자주: callback으로 비동기를 사용할 수 없다는 말임).
 
 ⑧ deactivate()  
:이 메서드는 거의 사용되지 않는다. deactivate()는 tree에서 State가 제거 될때 호출 된다. 그러나 현재 프레임 변경이 완료되기 전에 다시 삽입 될 수 있다. 이 메서드는 State객체가 tree의 한 지점에서 다른 지점으로 이동 할 수 있기 때문에 기본적으로 존재한다.
 
 ⑨ dispose()  
:dispost()는 State객체가 영구히 제거 된다.
 
 ⑩ mounted is false  
:이 상태에서 state 객체는 결코 다시 mount되지 않으며, setState()가 호출되면 에러가 발생한다.

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



