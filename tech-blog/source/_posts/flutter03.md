---
title: Flutter-3부 [Flutter 이해하기]
date: 2020-03-17 14:56:55
thumbnail : /images/flutter/thumbnail.jpg
tags: [flutter]
category : [IT Tech, 5. Flutter]
---

> 작성자 : 모바일 개발팀

# 목차
### 1.Stateful Widget Lifecycle

### 2.위젯 종류

### 3.BLoC Pattern

### 4.References

# 1.Stateful Widget Lifecycle
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

# 2.위젯 종류
 widget은 widgets library의 standard widget과 Material library의 widget 입니다.
 widgets library는 어느 app이든 사용가능하지만 Material library는 Material app에서만 사용 가능합니다.
 
##### 1)Standard widgets
 - Container: padding, margins, borders, background color, 기타 style을 적용 가능
 - GridView: widget 을 스크롤 가능한 grid 모양으로 배치
 - ListView: widget 을 스크롤 가능한 list 로 배치
 - Stack: widget을 순차적으로 쌓아가면서 표시  

##### 2)Material widgets
 - Card: 관련된 정보를 box 안에 표시
 - ListTile: 최대 3줄의 text를 표시하고, trailing icon 삽입  
 
##### 3)Container
 
 ![flutter02_4](/images/flutter/flutter02_4.png)
 
 - padding, margin, border 사용 가능
 - background color 또는 image 변경 가능
 - 하나의 child widget을 갖는다. 다만, child widget으로 Row, Column 등 다수의 자식을 갖는 widget이 올 수 있다.  

##### 4)GridView
 
 ![flutter02_5](/images/flutter/flutter02_5.png)
 
 - grid 형태로 widget 배치
 - scrolling지원
 - 직접 custom grid를 만들던지, 아래 제공되는 두가지 속성을 사용가능
 - GridView.count 지정된 개수 만큼의 column 을 제공
 - GridView.extent 타일의 최대 width를 지정
 - MediaQuery.of(context).orientation 를 사용하여 device의 landscape 나 portrait 을 인지하여 적절히 반응하도록 할 수 있다.  
 
##### 5)ListView
 
 ![flutter02_6](/images/flutter/flutter02_6.png)
 
 - Column형 widget으로 list를 만든다.
 - 수평 또는 수직의 layout을 만들 수 있다.
 - 자동으로 스크롤링을 만든다.
 - Column widget 보다 확장성은 적다.  
 
##### 6)Stack
 
 ![flutter02_7](/images/flutter/flutter02_7.png)
 
  Stack을 이용해 image 상단에 그라디언트를 넣습니다. 이 그라디언트를 통해 toolbar 의 icon이 iamge와 구분됩니다.
 
 - 다른 widget 위에 widget을 배치
 - 첫번째 widget이 base widget 이다. 그 위로 다른 widget 이 배치된다.
 - Stack 은 scroll 되지 않는다.
 - box를 넘는 자식 widget을 자를 수 있다.  
 
##### 7)Stack
 
 ![flutter02_8](/images/flutter/flutter02_8.png)
 
 - Material card 이다.
 - 관련된 정보를 표현하는데 사용
 - 자식을 하나만 갖지만, 멀티플 자식을 갖는 위젯을 자식으로 가질 수 있다.
 - 둥근 테두리와 그림자를 보여준다.
 - 스크롤이 되지 않는다.
 - Material library 의 widget 이다.

# 3.BLoC Pattern
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



