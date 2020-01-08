# 디자인 패턴

디자인 패턴은 주로 객체지향 프로그래밍 언어로 소프트웨어 개발할 때에, 특정 상황에서 <u>자주 나타나는 문제</u>를 해결하기 위해 수많은 개발자가 쌓아온 **솔루션**입니다.
> 어떠한 **문제**가 발생하는 **상황**이 있는데, 이런 **방법**으로 <u>해결한다.</u>

디자인 패턴은 결국 어떠한 문제를 해결하기 위해 존재하는 것입니다.  
디자인 패턴에서는 문제 해결의 목적을 기준으로 3가지 형태로 분리하고 있습니다.
그 종류로는 '객체를 어떻게 생성할 것인가?'와 관련된 <u>*생성 패턴*</u> 과 '생성된 객체들을 어떻게 조합/합성할 것인가?'와 관련된 <u>_구조 패턴_</u> , '객체들이 어떻게 상호작용하고 어떻게 책임을 가져가는가?'와 관련된 <u>*행동 패턴*</u>이 있습니다.

먼저 객체를 어떻게 생성하느냐와 관련된 '생성 패턴'부터 살펴보겠습니다.

# 생성 패턴(Creational Pattern)

* 생성패턴은 <u>인스턴스를 만드는 절차</u>를 **추상화**하는 패턴입니다. **시스템으로부터** 객체의 생성/합성 방법을 **분리**해내기 위함입니다.
생성패턴은 시스템이 _어떤 구체 클래스를 사용하는지_, 또한 *인스턴스들이 어떻게 만들어지고 어떻게 합성되는지*에 대한 정보를 완전히 <u>가려줍니다.</u>  
`'무엇이 생성되나요?'` → 알 수 없음  
`'어떻게 생성되나요?'` → 알 수 없음  
`'언제 생성되나요?'` → 알 수 없음  
`'누가 이걸 생성하나요?'` → 알 수 없음

생성 패턴의 종류는 다음과 같습니다.

## 종류
* 싱글턴 패턴 (Singleton Pattern)
* 프로토타입 패턴 (Prototype Pattern)
* 팩토리 메소드 패턴 (Factory Method Pattern)
* 빌더 패턴 (Builder Pattern)
* 추상 팩토리 패턴 (Abstract Factory Pattern)

# 싱글턴 패턴 (Singleton Pattern)

## 구조
![싱글턴 패턴](/images/design-pattern/singleton-pattern.png)

## 의도
> **오직 한 개의 인스턴스**만을 갖도록 하며, 이에 대한 전역적인 접근을 허용합니다.

일반적으로는 특정 클래스의 <u>인스턴스가 반드시 하나</u>여야 하나 여러 곳에서 사용하는 경우에 *싱글턴 패턴*을 사용합니다.
또한, 생성된 인스턴스를 여러 곳에서 **공유**하여 사용해도 무리가 없다면 메모리 낭비를 방지하기 위해 *싱글턴 패턴*을 적용하기도 합니다.

예제로 보겠습니다.

## 예제
카페의 와이파이를 사용하여 네트워크에 연결한다고 경우를 생각해봅시다. 카페에서는 와이파이가 필요한 사용자마다 와이파이를 새로 만들어주지는 않습니다.
ID와 패스워드를 알려주어 기존에 있던 네트워크를 <u>공유해서 사용하도록</u> 하죠.
위 상황을 생각하면서 와이파이 클래스를 만들어보도록 합시다.

고객으로부터 와이파이 정보 요청이 오면,  
ㄴ 있으면 있는 와이파이 정보 주고  
ㄴ 없으면 생성하여 준다.

* 와이파이 클래스
  ```
  public class Wifi {
      private static Wifi wifi;
  
      private Wifi() {
      }
  
      public static Wifi requestWifi() {
          if (wifi == null) {
              wifi = new Wifi();
          }
          return wifi;
      }
  }
  ```
* 실행과 결과
  ```
  Wifi wifi1 = Wifi.requestWifi();    // 와이파이 요청 1
  Wifi wifi2 = Wifi.requestWifi();    // 와이파이 요청 2
  Wifi wifi3 = Wifi.requestWifi();    // 와이파이 요청 3
  System.out.println(wifi1);
  System.out.println(wifi2);
  System.out.println(wifi3);
  ```
  creational_pattern.singleton.Wifi@13b6d03  
  creational_pattern.singleton.Wifi@13b6d03  
  creational_pattern.singleton.Wifi@13b6d03

3번의 와이파이 요청 결과 모두 **같은** 와이파이 정보임을 알 수 있습니다. 100번을 해도 요청한 정보가 모두 같을 것입니다.

이처럼 *싱글턴 패턴* 을 사용하면 단 하나의 인스턴스만 생성됩니다.
*싱글턴 패턴* 을 사용하면 고정된 메모리 영역을 받기 때문에 단 하나의 객체만 생성되어 메모리가 낭비를 방지할 수 있습니다.
하지만 위와 같은 'java'의 _싱글턴 패턴_ 방식에는 큰 문제점들이 존재합니다. (java라고 강조한 이유는 타 언어에서는 싱글턴 방식을 위의 방식으로 구현하지 않을 수도 있기 때문입니다.)

## 문제점

1. **상속**할 수 없다.  
위 방식에서는 객체가 어디서든지 <u>원하는 대로 생성되는 걸 방지하기 위해</u> 생성자를 private으로 선언합니다. 바로 이것이 문제가 되는 핵심입니다.
java에서는 생성자를 private으로 선언하면 상속을 할 수 없습니다. 이는 곧 객체지향 프로그램의 핵심인 **상속**과 **다형성**을 해치는 개념입니다.
2. 강제로 <u>전역 상태</u>  
애초에 <u>공유의 목적</u>으로 생성된 클래스이기 때문에 객체를 요청하는 메소드를 <u>public으로 강제</u>할 수밖에 없습니다. 바로 이것이 두 번째 문제의 원인입니다.
특정 메소드가 정보의 은닉 범위, 공개 수준 등등에 <u>전혀 상관없이</u> public으로 선언할 것을 강제했기 때문에 객체지향 프로그램의 또 다른 핵심인 '**정보 은닉**'을 해칩니다.
3. 객체가 하나인 것을 보장할 수 없다.  
사실 *싱글턴 패턴* 의 핵심은 싱글턴인 것을 보장할 수 있어야 한다는 것입니다.
하지만 java의 고전적 *싱글턴 패턴* 은 객체가 하나인 것을 보장할 수 없습니다.  
</u>멀티쓰레드</u>를 예로 들겠습니다. 해당 인스턴스는 공유돼서 사용되기 때문에 여러 개의 쓰레드가 동시에 접근하여 메소드를 호출할 수 있습니다.
문제는 2개 이상의 쓰레드가 동시에 객체 생성을 하게 되면 2개 이상의 객체가 생성된다는 것입니다. 즉, 싱글턴으로 사용하기 위해 *싱글턴 패턴* 을 적용했으나, <u>싱글턴이 아닌 게 되는 것</u>입니다.

## 결론
*싱글턴 패턴* 은 굉장히 많이 활용되는 패턴이나, 앞서 말한 객체지향 프로그래밍의 기본 사상들을 많이 침해하기 때문에 굉장히 비판을 많이 받는 디자인 패턴입니다.
따라서 *싱글턴 패턴* 은 사용 시 매우 조심해서 사용해야 합니다. 그것이 아니라면 위의 고전적인 *싱글턴 패턴* 이 아닌 개선된 방식으로 객체의 싱글턴 방식을 구현하여 사용해야 합니다. 

# 프로토타입 패턴 (Prototype Pattern)

## 별명
> 견본 또는 원형

## 구조
![프로토타입 패턴](/images/design-pattern/prototype-pattern.png)

## 의도
> 프로토타입이 될 인스턴스를 생성하여 앞으로 생성할 객체의 종류를 명시하고, 그 인스턴스로부터 새로운 인스턴스를 **복제**합니다

인스턴스들이 서로 다른 상태 값 또는 서로 다른 조합으로 지속적으로나 주기적으로 필요할 때, 나중의 인스턴스 생성을 위해 복제의 견본이 되어줄 **원형 인스턴스**를 준비해둡니다.
그 후, 새로운 인스턴스의 생성 요청이 오거나 필요할 때마다 미리 만들어둔 견본을 <u>복제하여 사용합니다.</u>

예제를 통해 자세히 보도록 하겠습니다.

## 예제
게임을 예로 들어보겠습니다.
우리는 어떤 게임을 만들고자 합니다. 우리는 특정 위치에서 지속적으로 몬스터들을 출현시킬 것입니다.
이 몬스터들은 각자 정해진 체력이 있고, 이 체력이 모두 다하면 죽습니다. 또한, 구역 별로 초기 체력의 양이 다릅니다.  
이 몬스터들을 어떻게 구현해야 할까요?

물론 위치 정보와 체력 정보를 저장해놓고, 시간이 되면 저장해놓은 위치 정보와 체력정보를 보고 그때그때 몬스터들을 새로 생성해도 됩니다.
하지만 우리는 *프로토타입 패턴* 을 적용하여 구현해볼 수 있습니다.
몬스터들의 유형별로 견본을 준비해놓고, 필요할 때마다 그 견본으로부터 복제해가며 사용하는 것입니다.

java에서는 객체의 복제를 위해 Object 클래스에 이미 **clone**이라는 메소드가 존재합니다.
Cloneable 인터페이스를 상속받고 clone이라는 메소드를 오버라이딩하면 인스턴스의 <u>복제가 가능</u>합니다.

## 적용
* 위치정보 클래스 : `Location`
  ```
  public class Location implements Cloneable {
  
      private int x;
      private int y;
  
      public Location(int x, int y) {
          this.x = x;
          this.y = y;
      }
  
      // getters
      public int getX() { return x; }
      public int getY() { return y; }
  
      // 위치정보 복제
      @Override
      public Location clone() throws CloneNotSupportedException {
          return (Location) super.clone();
      }
  }

  ```
* 몬스터 클래스 : `Monster`
  ```
  public class Monster implements Cloneable {
  
      private Location location;
      private int health;
  
      public Monster(Location location, int health) {
          this.location = location;
          this.health = health;
      }
  
      // getters
      public Location getLocation() { return location; }
      public int getHealth() { return health; }
  
      // 몬스터 복제
      @Override
      public Monster clone() throws CloneNotSupportedException {
          Monster clonedMonster = (Monster) super.clone();
          clonedMonster.location = location.clone();
          return clonedMonster;
      }
  
  }

  ```
* 실행과 결과
  ```
  Monster monsterA = new Monster(new Location(5, 5), 100);    // 프로토타입 A
  Monster monsterB = new Monster(new Location(0, 0), 10);     // 프로토타입 B
  Monster monsterC = new Monster(new Location(-3, 3), 50);    // 프로토타입 C

  while (true) {
      System.out.println("몬스터들 출현!");

      Monster cloneA = monsterA.clone();      // 프로토타입 A 복제
      Monster cloneB = monsterB.clone();      // 프로토타입 B 복제
      Monster cloneC = monsterC.clone();      // 프로토타입 C 복제

      // 출력과 10초 기다림
      System.out.println(String.format("몬스터 생성 [\t체력 : %d,\t위치 : (%d, %d)\t]", cloneA.getHealth(), cloneA.getLocation().getX(), cloneA.getLocation().getY()));
      System.out.println(String.format("몬스터 생성 [\t체력 : %d,\t위치 : (%d, %d)\t]", cloneB.getHealth(), cloneB.getLocation().getX(), cloneB.getLocation().getY()));
      System.out.println(String.format("몬스터 생성 [\t체력 : %d,\t위치 : (%d, %d)\t]", cloneC.getHealth(), cloneC.getLocation().getX(), cloneC.getLocation().getY()));
      System.out.println("------------------------------------------------");
      Thread.sleep(10000);
  }
  ```
  <p>
  몬스터들 출현!  <br>
  몬스터 생성 [	체력 : 100,	위치 : (5, 5)	]  <br>
  몬스터 생성 [	체력 : 10,	위치 : (0, 0)	]  <br>
  몬스터 생성 [	체력 : 50,	위치 : (-3, 3)	]  <br>
  ------------------------------------------------  <br>
  몬스터들 출현!  <br>
  몬스터 생성 [	체력 : 100,	위치 : (5, 5)	]  <br>
  몬스터 생성 [	체력 : 10,	위치 : (0, 0)	]  <br>
  몬스터 생성 [	체력 : 50,	위치 : (-3, 3)	]  <br>
  ------------------------------------------------  <br>
  몬스터들 출현!  <br>
  몬스터 생성 [	체력 : 100,	위치 : (5, 5)	]  <br>
  몬스터 생성 [	체력 : 10,	위치 : (0, 0)	]  <br>
  몬스터 생성 [	체력 : 50,	위치 : (-3, 3)	]  <br>
  ------------------------------------------------  <br>
  </p>

진짜 프로토타입으로 몬스터가 생성된 것인지, 그냥 기존에 만들어두었던 프로토타입을 보여주는 것인지 알 길이 없으니 **hashCode**를 찍도록 코드를 수정하여 다시 출력해봅시다.

* 실행과 결과
  ```
  Monster monsterA = new Monster(new Location(5, 5), 100);    // 프로토타입 A
  Monster monsterB = new Monster(new Location(0, 0), 10);     // 프로토타입 B
  Monster monsterC = new Monster(new Location(-3, 3), 50);    // 프로토타입 C

  while (true) {
      System.out.println("몬스터들 출현!");

      Monster cloneA = monsterA.clone();      // 프로토타입 A 복제
      Monster cloneB = monsterB.clone();      // 프로토타입 B 복제
      Monster cloneC = monsterC.clone();      // 프로토타입 C 복제

      // 출력과 10초 기다림
      System.out.println(String.format("몬스터 생성 [\t체력 : %d,\t위치 : (%d, %d)\t]", cloneA.getHealth(), cloneA.getLocation().getX(), cloneA.getLocation().getY()));
      System.out.println(String.format("몬스터 생성 [\t체력 : %d,\t위치 : (%d, %d)\t]", cloneB.getHealth(), cloneB.getLocation().getX(), cloneB.getLocation().getY()));
      System.out.println(String.format("몬스터 생성 [\t체력 : %d,\t위치 : (%d, %d)\t]", cloneC.getHealth(), cloneC.getLocation().getX(), cloneC.getLocation().getY()));
      System.out.println("------------------------------------------------");
      Thread.sleep(10000);
  }
  ```
  <p>
  몬스터들 출현!  <br>
  Monster@f5f2bb7  <br>
  Monster@73035e27  <br>
  Monster@64c64813  <br>
  ------------------------------------------------  <br>
  몬스터들 출현!  <br>
  Monster@3ecf72fd  <br>
  Monster@483bf400  <br>
  Monster@21a06946  <br>
  ------------------------------------------------  <br>
  몬스터들 출현!  <br>
  Monster@77f03bb1  <br>
  Monster@326de728  <br>
  Monster@25618e91  <br>
  ------------------------------------------------  <br>
  </p>

해쉬코드를 통해 초기에 만들어두었던 프로토타입을 기반으로 새로운 몬스터가 생성되는 것임을 확인할 수 있습니다.

여러분들은 제가 작성한 Monster 클래스의 clone 메소드에 의문을 품을 수도 있습니다.
  ```
  Monster clonedMonster = (Monster) super.clone();
  clonedMonster.location = location.clone();
  return clonedMonster;
  ```
집중할 포인트는 바로 두 번째 줄입니다.  

'몬스터를 복제하고 난 후, 복제된 몬스터의 위치정보를 왜 굳이 또 복제하여 복제된 몬스터에 세팅하나요?'

이것이 바로 clone 메소드를 사용할 경우, <u>유의해야 할 사항</u>입니다.

## 주의사항
프로토타입 자체를 '**깊은 복사**'하지만 프로토타입 <u>내의 또 다른 객체</u>가 있을 때, 그 객체들은 **얕은 복사**가 수행됩니다.    
프로토타입에 단순한 기본 자료형만 있을 때, 프로토타입을 복제하였을 때 만들어진 인스턴스들끼리 공유하는 정보가 없습니다.
하지만 프로토타입 내부에 참조형 자료형이 있을 때, 프로토타입을 복제하였을 때 그 인스턴스들끼리 내부에 있는 <u>참조형 자료형들을 서로 공유</u>합니다.
내부에 있는 참조형 자료형들까지 깊은 복사가 되지 않기 때문입니다.  
만약, 소프트웨어의 기능 특성상, 내부에 있는 참조형 자료형들까지 깊은 복사가 필요하다면 위의 코드처럼 깊은 복사를 다루는 코드가 필요합니다.

## 정리
이처럼 *프로토타입 패턴* 은 특정 객체가 지속적으로나 주기적으로 새로이 필요할 때 **견본**을 만들어놓고 이를 <u>복제하는 형식</u>으로 객체를 제공하기 위해 존재하는 패턴입니다.  
복제를 위해 견본이 반드시 필요하여 뒤에 나올 다른 생성 패턴처럼 서브클래싱은 필요하진 않아도 **초기화 동작**은 반드시 필요한 패턴입니다. 
clone과 같이 <u>복사를 수행할 메소드를 반드시 구현해줘야 한다</u>는 단점도 존재하지만 매번 필요한 상태 조합을 수동적으로 초기화하지 않는다는 점에서 장점도 존재합니다.  

# 팩토리 메소드 패턴 (Factory Method Pattern)

## 별명
> 가상 생성자

## 구조
![팩토리 메소드 패턴](/images/design-pattern/factory-method-pattern.png)

## 의도
> 객체를 생성하기 위해 인터페이스를 정의하지만 어떤 클래스의 인스턴스를 생성할지는 **서브 클래스가 결정**합니다.

로직을 구현할 때에 특정 부분에서 어떤 인터페이스(또는 추상 클래스)를 구현한 클래스의 인스턴스 필요하다는 것은 정의되었으나,
구체적으로 <u>어떤 클래스의 인스턴스가 쓰일지</u> 예측이 불가할 때가 있습니다.

예제를 통해 자세히 보도록 하겠습니다.

## 예제
우리는 놀이동산을 만들고 일정 시간이 지나면 놀이동산을 폐쇄하는 프로그램을 만들 것입니다.
그런데 그 놀이동산은 비스킷으로 만들어질 수도, 젤리로 만들어질 수도 있습니다.
* [FMP-1.1] 놀이동산 클래스 : `AmusementPark` `JellyAmusementPark` `BiscuitAmusementPark`
  ```
  public class AmusementPark {
      public void open() {
          System.out.println(toString() + "이(가) 생겼습니다.");
      }
      public void close() {
          System.out.println(toString() + "이(가) 폐쇄되었습니다.");
      }
  }
  ```
  ```
  public class JellyAmusementPark extends AmusementPark {
      @Override
      public String toString() {
          return "젤리로 된 놀이동산";
      }
  }
  ```
  ```
  public class BiscuitAmusementPark extends AmusementPark {
      @Override
      public String toString() {
          return "비스킷으로 된 놀이동산";
      }
  }
  ```
* [FMP-1.2] 놀이동산 운영 클래스 : `AmusementParkOperator`
  ```
  public class AmusementParkOperator {
      // 놀이동산을 만들고 5초가 지나면 폐쇄한다.
      public void operate() throws InterruptedException {
          AmusementPark amusementPark = new JellyAmusementPark();
          amusementPark.open();
          Thread.sleep(5000);
          amusementPark.close();
      }
  }
  ```
* 실행과 결과
  ```
  AmusementParkOperator operator = new AmusementParkOperator();
  operator.operate();
  ```
  젤리로 된 놀이동산이(가) 생겼습니다.  
  젤리로 된 놀이동산이(가) 폐쇄되었습니다.

우리는 지금 방금 젤리로 된 놀이동산을 운영시켰습니다.  
자 그렇다면 이제 비스킷으로 된 놀이동산을 운영시켜봅시다. 그러기 위해서 우리는 위의 `AmusementParkOperator`의 `new JellyAmusementPark()` 부분을 `new BiscuitAmusementPark()`로 바꿔줘야 합니다.
이래서는 재료가 바뀔 때마다 `AmusementParkOperator` 코드를 바꿔주어야 할지도 모르겠습니다.

## 적용
우리는 이제 *팩토리 메소드 패턴* 을 적용하여 놀이동산을 생성하는 부분을 아예 <u>별도의 메소드로 분리</u>하고 난 후 상속을 통해 그때그때 **서브클래스**가 자신이 운영할 놀이동산의 <u>종류를 결정</u>하도록 바꿔줄 것입니다.

* [FMP-2.1] 놀이동산 운영 클래스 : `AmusementParkOperator`
  ```
  public abstract class AmusementParkOperator {
      public void operate() throws InterruptedException {
          AmusementPark amusementPark = makeAmusementPark();
          amusementPark.open();
          Thread.sleep(5000);
          amusementPark.close();
      }
      public abstract AmusementPark makeAmusementPark();
  }
  ```
  ```
  public class BiscuitAmusementParkOperator extends AmusementParkOperator {
      @Override
      public AmusementPark makeAmusementPark() {
          return new BiscuitAmusementPark();
      }
  }
  ```
  ```
  public class JellyAmusementParkOperator extends AmusementParkOperator {
      @Override
      public AmusementPark makeAmusementPark() {
          return new JellyAmusementPark();
      }
  }
  ```
* 실행과 결과
  ```
  AmusementParkOperator operator = new JellyAmusementParkOperator();
  operator.operate();
  ```
  젤리로 된 놀이동산이(가) 생겼습니다.  
  젤리로 된 놀이동산이(가) 폐쇄되었습니다.
  ```
  AmusementParkOperator operator = new BiscuitAmusementParkOperator();
  operator.operate();
  ```
  비스킷으로 된 놀이동산이(가) 생겼습니다.  
  비스킷으로 된 놀이동산이(가) 폐쇄되었습니다.

자, 이제 우리는 재료가 바뀔 때마다 `AmusementParkOperator` 코드를 변경해주지 않아도 됩니다.
실행부에서 선택하는 `AmusementParkOperator` 종류에 따라 코드의 변경 없이도 놀이동산의 재료를 바꿔줄 수 있게 되었습니다.
혹시라도 사탕으로 된 놀이동산이 필요하다 하더라도 `AmusementParkOperator`를 <u>변경할 필요 없이</u> **상속**하여 구현해주면 되는 것입니다.

이처럼 *팩토리 메소드 패턴* 은 구체 클래스들이 **병렬구조**를 이루어 그때그때 교체하여 사용하면 되기 때문에 프로그램에 유연성을 제공해줍니다.
소프트웨어가 우리들의 코드에 <u>종속되지 않도록</u> 해주는 것입니다.

*팩토리 메소드 패턴* 의 기본 개념의 이게 끝입니다. 하지만 여러분들은 아마 마음에 들지 않는 부분이 있을수도 있을 겁니다.

## 개선
'아니, 그럼 놀이동산 객체가 필요할 때마다 저 `AmusementParkOperator` 클래스를 상속해줘야 한다는 거야?'  
그렇습니다. 현 구조에서는 그럴 수밖에 없습니다.
놀이동산 객체가 필요할 때마다 운영하는 기능을 가진 저 `AmusementParkOperator`를 **서브클래싱**해줘야 한다는 것이죠. 왜일까요?
#### 책임의 분리
`AmusementParkOperator`의 <u>핵심 기능</u>은 무엇일까요? 바로 '놀이동산을 <u>어떻게</u> 운영시킬 것이다'입니다.
물론 operate 메소드가 있으나(예제에는 operate밖에 없지만 사실 더 많은 기능이 들어갈 수 있을 겁니다.) 한 가지 <u>책임이 더</u> 있습니다. 그것은 바로 '<u>어떤</u> 놀이동산을 만들어낸다'입니다.
'어떤 놀이동산을 운영한다'와 '놀이동산을 어떻게 운영한다'의 **책임**이 한 클래스 안에 존재하면 각자의 책임에 서로 다른 <u>변경사항</u>이 생기더라도 영향을 주고 받을 수밖에 없습니다.  
이 문제를 해결하기 위해 우리는 **책임을 분리**해주는 것이 좋을 것 같습니다.

* 현재
  * `AmusementParkOperator` : 어떤 놀이동산을 만들고, 그 놀이동산을 어떻게 운영한다
* To be
  * `AmusementParkFactory` : 어떤 놀이동산을 만들고,  
  * `AmusementParkOperator` : 받은 놀이동산을 어떻게 운영한다.  
  
* [FMP-3.1] 놀이동산 공장 : `AmusementParkFactory`
  ```
  public interface AmusementParkFactory {
      AmusementPark makeAmusementPark();
  }
  ```
  ```
  public class BiscuitAmusementParkFactory implements AmusementParkFactory {
      @Override
      public AmusementPark makeAmusementPark() {
          return new BiscuitAmusementPark();
      }
  }
  ```
  ```
  public class JellyAmusementParkFactory implements AmusementParkFactory {
      @Override
      public AmusementPark makeAmusementPark() {
          return new JellyAmusementPark();
      }
  }
  ```
* [FMP-3.2] 놀이동산 운영 클래스 : `AmusementParkOperator`
  ```
  public class AmusementParkOperator {
      private AmusementPark amusementPark;
  
      public AmusementParkOperator(AmusementPark amusementPark) {
          this.amusementPark = amusementPark;
      }
  
      public void operate() throws InterruptedException {
          amusementPark.open();
          Thread.sleep(5000);
          amusementPark.close();
      }
  }
  ```
* 실행과 결과
  ```
  AmusementPark amusementPark = new JellyAmusementParkFactory().makeAmusementPark();
  AmusementParkOperator operator = new AmusementParkOperator(amusementPark);
  operator.operate();
  ```
  젤리로 된 놀이동산이(가) 생겼습니다.  
  젤리로 된 놀이동산이(가) 폐쇄되었습니다.
  ```
  AmusementPark amusementPark = new BiscuitAmusementParkFactory().makeAmusementPark();
  AmusementParkOperator operator = new AmusementParkOperator(amusementPark);
  operator.operate();
  ```
  비스킷으로 된 놀이동산이(가) 생겼습니다.  
  비스킷으로 된 놀이동산이(가) 폐쇄되었습니다.
  
이렇게 놀이동산을 만드는 **전용 공장**이 생기니 새로운 종류의 놀이동산이 생기더라도 놀이동산을 운영하는 클래스가 변경되지는 않을 것입니다.
또한 놀이동산의 운영방법이 바뀌더라도 놀이동산을 만들어내는 클래스가 변경되지는 않을 것입니다.

## 다른 방식
눈치가 빠르시다면 눈치채셨을 수도 있습니다.

*'넌 #적용 파트에서 다른 방식을 취할 수도 있었는데 얼렁뚱땅 패턴을 적용했어'*
  
과연 어떤 다른 방식이 있었을까요? 바로 '**매개변수로 분기처리**' 방법입니다.

* [FMP-4.1] 놀이동산 종류 : `AmusementParkType`
  ```
  public enum AmusementParkType {
      JELLY,
      BISCUIT;
  }
  ```
* [FMP-4.2] 놀이동산 운영 클래스 : `AmusementParkOperator`
  ```
  public class AmusementParkOperator {
  
      public void operate(AmusementParkType type) throws InterruptedException {
          AmusementPark amusementPark = makeAmusementPark(type);
          amusementPark.open();
          Thread.sleep(5000);
          amusementPark.close();
      }
  
      private AmusementPark makeAmusementPark(AmusementParkType type) {
          switch (type) {
              case JELLY:
                  return new JellyAmusementPark();
              case BISCUIT:
                  return new BiscuitAmusementPark();
          }
          return null;
      }
  
  }
  ```
* 실행과 결과
  ```
  AmusementParkOperator operator = new AmusementParkOperator();
  operator.operate(AmusementParkType.JELLY);
  ```
  젤리로 된 놀이동산이(가) 생겼습니다.  
  젤리로 된 놀이동산이(가) 폐쇄되었습니다.
  ```
  AmusementParkOperator operator = new AmusementParkOperator();
  operator.operate(AmusementParkType.BISCUIT);
  ```
  비스킷으로 된 놀이동산이(가) 생겼습니다.  
  비스킷으로 된 놀이동산이(가) 폐쇄되었습니다.

이 방식으로도 여전히 책임에 대한 문제점이 보이니 개선하도록 하겠습니다.

* [FMP-5.1] 놀이동산 공장 : `AmusementParkFactory`
  ```
  public class AmusementParkFactory {
      AmusementPark makeAmusementPark(AmusementParkType type) {
          switch (type) {
              case JELLY:
                  return new JellyAmusementPark();
              case BISCUIT:
                  return new BiscuitAmusementPark();
          }
          return null;
      }
  }
  ```
* [FMP-5.2] 놀이동산 운영 클래스 : `AmusementParkOperator`  
  [FMP-3.2] 동일
* 실행과 결과
  ```
  AmusementPark amusementPark = new AmusementParkFactory().makeAmusementPark(AmusementParkType.JELLY);
  AmusementParkOperator operator = new AmusementParkOperator(amusementPark);
  operator.operate();
  ```
  젤리로 된 놀이동산이(가) 생겼습니다.  
  젤리로 된 놀이동산이(가) 폐쇄되었습니다.
  ```
  AmusementPark amusementPark = new AmusementParkFactory().makeAmusementPark(AmusementParkType.BISCUIT);
  AmusementParkOperator operator = new AmusementParkOperator(amusementPark);
  operator.operate();
  ```
  비스킷으로 된 놀이동산이(가) 생겼습니다.  
  비스킷으로 된 놀이동산이(가) 폐쇄되었습니다.
  
이처럼 매개변수를 넘겨 분기에 따라 처리해주는 방식도 존재합니다.  
하지만 이 방법은 새로운 유형의 데이터가 추가될 때마다 기존 메소드를 계속 변경시켜줘야 한다는 단점이 있습니다.
또한 구체 클래스가 명시되어 있어서 유연성을 제공해주기 힘들다는 단점도 존재합니다.
하지만 위 방법과 비교하면 비교적 구조가 간단하다는 장점이 있습니다.

각 방법의 장단점을 파악하고 그때그때 프로젝트나 소프트웨어의 환경에 맞춰 적합한 방법을 선택하는 것이 바람직한 방법일 것입니다.

## 정리
객체를 생성하는 클래스 또는 인터페이스가 있지만, 정확히 어떤 구체 클래스의 인스턴스가 생성되는지 모를 때(또는 유연성을 제공하고 싶을 때) 서브클래스에게 결정권을 넘겨준다.

# 빌더 패턴 (Builder Pattern)

_빌더 패턴_ 을 정의하고 있는 2가지 방식이 있습니다.
한 가지는 서적 'gof의 디자인 패턴'에 나오는 *빌더 패턴* 과 '이펙티브 자바'에 나오는 아이템 2번입니다.
_빌더 패턴_ 의 전통적인 의미와 구조는 'gof의 디자인 패턴'에 나오는 *빌더 패턴* 이나,
지금의 _빌더 패턴_ 은 '이펙티브 자바'에 나오는 의미와 구조가 더 많이 알려져 있으며 그만큼 더 많이 사용되는 듯 보입니다.
  
우리는 이 두 가지 방식 모두 살펴볼 것입니다. 먼저 'gof의 디자인 패턴'에 나오는 _빌더 패턴_ 먼저 보도록 하겠습니다.

## 1. gof의 빌더 패턴

## 구조
![프로토타입 패턴](/images/design-pattern/builder-pattern.png)

## 의도
> 복잡한 객체를 생성하는 방법과 표현하는 방법을 정의하는 클래스를 별도로 **분리**하여,
서로 다른 표현이라도 이를 생성할 수 있는 <u>동일한 절차를 제공</u>한다.

여러 객체들이 <u>조립되어 생성</u>되는 복잡한 객체의 경우, '내부 객체들을 어떻게 생성하는가'와 '내부 객체들을 어떻게 조립하는가'를 분리시킵니다.
조립될 각 객체들의 **구체적인 클래스**나 객체들의 **조립 방법**이 서로 <u>다르더라도</u> 내부 객체들이 어떻게 생성되는지 제공해줘야 하고,
생성된 각 객체들로 최종적인 객체가 만들어지는 과정에 동일한 절차를 제공해야 하는 경우 *빌더 패턴* 을 사용합니다. 

예제를 통해 파악하도록 하겠습니다. 

## 예제
우리는 어떤 방을 생성하려고 합니다. 원룸인 집을 생각하셔도 좋습니다.  
방은 바닥과 벽, 문, 창문으로 이루어지는데 그 <u>조립의 방법</u>이 각기 다를 수 있습니다.
바닥을 짓고 사방에 벽을 세우고 동쪽에 문을 서쪽에 창문을 달 수도 있고, 바닥을 짓고 사방에 벽을 세우고 남쪽에 문을, 남쪽을 제외한 모든 곳에 창문을 달 수도 있습니다. 
허나, 어떤 조립 방법이든, '방'은 바닥, 벽, 문, 창문의 객체들이 생성되고 난 후 최종적으로 생성됩니다.

* 방향 유형 : `Direction`
  ```
  public enum Direction {
      NORTH("남"), SOUTH("북"), EAST("동"), WEST("서");
      private String value;
      
      Direction(String value)   { this.value = value; }
      public String getValue()  { return value; }
  }

  ```
* 바닥/문/벽/창문 클래스 : `Floor` `Door` `Wall` `Window`
  ```
  public class Floor {
      @Override
      public String toString() { return "바닥"; }
  }
  ```
  ```
  public class Door {
      @Override
      public String toString() { return "문"; }
  }
  ```
  ```
  public class Wall {
      @Override
      public String toString() { return "벽"; }
  }
  ```
  ```
  public class Window {
      @Override
      public String toString() { return "창문"; }
  }
  ```
* 방 클래스 : `Room`
  ```
  public class Room {
  
      private Floor floor;
      private Map<Direction, Wall> walls;
      private Map<Direction, Door> doors;
      private Map<Direction, Window> windows;
  
      // 바닥과 벽들, 문들, 창문들로 방을 생성
      public Room(Floor floor, Map<Direction, Wall> walls, Map<Direction, Door> doors, Map<Direction, Window> windows) {
          this.floor = floor;
          this.walls = walls;
          this.doors = doors;
          this.windows = windows;
      }
  
      // 출력을 위함
      @Override
      public String toString() {
        StringBuffer buffer = new StringBuffer(floor.toString()).append("\n");
        for(Direction direction : walls.keySet()) {
            buffer.append(direction.getValue()).append("쪽의 ").append(walls.get(direction).toString()).append("\n");
        }
        for(Direction direction : doors.keySet()) {
            buffer.append(direction.getValue()).append("쪽의 ").append(doors.get(direction).toString()).append("\n");
        }
        for(Direction direction : windows.keySet()) {
            buffer.append(direction.getValue()).append("쪽의 ").append(windows.get(direction).toString()).append("\n");
        }
        return buffer.toString();
      }
  }
  ```

우리 소프트웨어에서는 두 가지 구조의 방을 만들어낸다고 가정합시다.
* 유형 A의 구조 : 바닥이 있고, 남쪽을 제외한 모든 방향에 벽이 세워져있고, 북쪽에 창문이 나 있음
* 유형 B의 구조 : 바닥이 있고, 사방에 벽이 세워져 있고, 남쪽에 문이, 사방에 창문이 나 있음

유형 A와 유형 B를 만드는 클래스를 작성해봅시다.

* 방 생성 클래스 : `RoomCreateor`
  ```
  public class RoomCreator {
  
      public Room createTypeARoom() {
          // 바닥 생성
          Floor floor = new Floor();
          // 남쪽을 제외한 모든 방향에 벽 생성
          Map<Direction, Wall> walls = new HashMap<>();
          walls.put(Direction.EAST, new Wall());
          walls.put(Direction.WEST, new Wall());
          walls.put(Direction.SOUTH, new Wall());
          // 북쪽에 창문 생성
          Map<Direction, Window> windows = new HashMap<>();
          windows.put(Direction.SOUTH, new Window());
  
          // 방 생성
          return new Room(floor, walls, new HashMap<>(), windows);
      }
  
      public Room createTypeBRoom() {
          // 바닥 생성
          Floor floor = new Floor();
          // 사방에 벽 생성
          Map<Direction, Wall> walls = new HashMap<>();
          walls.put(Direction.EAST, new Wall());
          walls.put(Direction.WEST, new Wall());
          walls.put(Direction.NORTH, new Wall());
          walls.put(Direction.SOUTH, new Wall());
          // 남쪽에 문 생성
          Map<Direction, Door> doors = new HashMap<>();
          doors.put(Direction.NORTH, new Door());
          // 사방에 창문 생성
          Map<Direction, Window> windows = new HashMap<>();
          windows.put(Direction.EAST, new Window());
          windows.put(Direction.WEST, new Window());
          windows.put(Direction.NORTH, new Window());
          windows.put(Direction.SOUTH, new Window());
          
          // 방 생성
          return new Room(floor, walls, doors, windows);
      }
  }
  ```
* 실행과 결과  
  ```
  RoomCreator roomCreator = new RoomCreator();
  
  Room typeA = roomCreator.createTypeARoom();
  System.out.println(typeA);

  ```
  <p>
  바닥<br/>
  서쪽의 벽<br/>
  동쪽의 벽<br/>
  북쪽의 벽<br/>  
  북쪽의 창문<br/>
  <br/>
  </p>
  
  ```
  RoomCreator roomCreator = new RoomCreator();
  
  Room typeB = roomCreator.createTypeBRoom();
  System.out.println(typeB);
  ```
  <p>
  바닥<br/>  
  남쪽의 벽<br/>  
  서쪽의 벽<br/>  
  동쪽의 벽<br/>  
  북쪽의 벽<br/>  
  남쪽의 문<br/>
  남쪽의 창문<br/>  
  서쪽의 창문<br/>
  동쪽의 창문<br/>
  북쪽의 창문<br/>
  </p>

위의 예제에서는 방이라는 객체를 생성해주는 `RoomCreator`가 방을 이루는 여러 구성 요소들을 <u>어떻게 합성하는지</u>와 각 구성 요소들이 <u>어떤 타입으로 이루어져 있는지</u>를 모두 알고 있으며
그 정보들을 바탕으로 객체들을 생성합니다.
그래서 우리는 각 구성요소들의 생성 방법이나 타입이 달라질 때마다 구성요소의 <u>합성 방법이 같을지라도</u> `RoomCreator`를 계속 변경해주거나 추가해줘야 합니다.
예를 들자면, 구조는 그대로이나 단순한 벽이 아닌 '철제로 만든 벽'과 같이 구성요소의 타입을 변화시켰을 때,
또는 바닥을 생성하게 되면 사방에 자동으로 벽을 생성되는 방의 유형이 새로 생겼을 때, 우리는 `RoomCreator`를 변경해주거나 추가해줘야 합니다.

우리는 이제 *빌더 패턴* 을 적용하여 구성요소를 '어떻게 합성할 것인가'과 어떤 구성 요소를 '어떻게 만들어낼 것인가'를 분리하여 방이라는 객체를 만들어내는 작업에 유연성을 제공해줄 것입니다.
 
## 적용
* 구성물 빌드 인터페이스 : `RoomBuilder`
  ```
  public interface RoomBuilder {
      void buildFloor();                       // 바닥 생성
      void buildWall(Direction direction);     // 벽 생성
      void buildDoor(Direction direction);     // 문 생성
      void buildWindow(Direction direction);   // 창문 생성
      Room build();                            // 최종적으로 '방' 빌드
  }
  ```
* 구성물 빌드 클래스 : `BasicRoomBuilder`
  ```
  public class BasicRoomBuilder implements RoomBuilder {
  
      private Floor floor;
      private Map<Direction, Wall> walls = new HashMap<>();
      private Map<Direction, Door> doors = new HashMap<>();
      private Map<Direction, Window> windows = new HashMap<>();
  
      @Override
      public void buildFloor() {  floor = new Floor();  }
  
      @Override
      public void buildWall(Direction direction) {
          walls.put(direction, new Wall());
      }
  
      @Override
      public void buildDoor(Direction direction) {
          doors.put(direction, new Door());
      }
  
      @Override
      public void buildWindow(Direction direction) {
          windows.put(direction, new Window());
      }
  
      @Override
      public Room build() { return new Room(floor, walls, doors, windows);  }
  }

  ```
* 방 구성 클래스 : `RoomDirector`
  ```
  public class RoomDirector {
  
      private RoomBuilder builder;
      public RoomDirector(RoomBuilder builder) { this.builder = builder; }
  
      public Room createTypeARoom() {
          // 바닥 생성
          builder.buildFloor();
          // 사방에 벽 생성
          builder.buildWall(Direction.EAST);
          builder.buildWall(Direction.WEST);
          builder.buildWall(Direction.NORTH);
          builder.buildWall(Direction.SOUTH);
          // 남쪽에 문 생성
          builder.buildDoor(Direction.NORTH);
          // 북쪽에 창문 생성
          builder.buildWindow(Direction.SOUTH);
  
          return builder.build();
      }
  
      public Room createTypeBRoom() {
          // 바닥 생성
          builder.buildFloor();
          // 사방에 벽 생성
          builder.buildWall(Direction.EAST);
          builder.buildWall(Direction.WEST);
          builder.buildWall(Direction.NORTH);
          builder.buildWall(Direction.SOUTH);
          // 남쪽에 문 생성
          builder.buildDoor(Direction.NORTH);
          // 사방에 창문 생성
          builder.buildWindow(Direction.EAST);
          builder.buildWindow(Direction.WEST);
          builder.buildWindow(Direction.NORTH);
          builder.buildWindow(Direction.SOUTH);
  
          return builder.build();
      }
  
  }
  ```
* 실행과 결과
  ```
  RoomCreator roomCreator = new RoomCreator();
  
  Room typeA = roomCreator.createTypeARoom();
  System.out.println(typeA);

  Room typeB = roomCreator.createTypeBRoom();
  System.out.println(typeB);
  ```
  <p>
  바닥<br/>
  서쪽의 벽<br/>
  동쪽의 벽<br/>
  북쪽의 벽<br/>  
  북쪽의 창문<br/>
  <br/>
  바닥<br/>  
  남쪽의 벽<br/>  
  서쪽의 벽<br/>  
  동쪽의 벽<br/>  
  북쪽의 벽<br/>  
  남쪽의 문<br/>
  남쪽의 창문<br/>  
  서쪽의 창문<br/>
  동쪽의 창문<br/>
  북쪽의 창문<br/>
  </p>

우리는 *빌더 패턴* 을 적용하여 방의 구성 요소들을 어떻게 합성하는지(`Director`)와 구성 요소들이 어떤 타입으로 생성되는지를(`Builder`)를 분리하였습니다.  
이제는 새로운 구성요소 타입이 나오더라도 또는 구성 요소에 구체적인 생성 방법이 생기더라도 우리가 정의한 빌더 인터페이스를 <u>상속하여 새로이 구현</u>하여 사용하면 됩니다.
구성 요소를 생성하는 작업들을 **추상화**시켰고, 직접 방을 구성하는 클래스가 이 추상화된 <u>인터페이스를 사용하는 구조</u>로 바꾸었기 때문에.
그러니까 최종 산출물을 내기까지(방이라는 객체를 생성하기까지) 외부로부터 어떤 요소들로 제품이 조합되는지를 모두 가렸기 때문에 가능한 일입니다.

## 정리
GoF의 *빌더 패턴* 은 '객체를 생성하는 방법'과 '객체를 합성/조합하는 방법'을 분리해 복잡한 객체를 생성하는 과정에 유연성을 제공한다.

## 2. 이펙티브 자바의 빌더 패턴

이펙티브 자바의 *빌더 패턴* 은 GoF의 *빌더 패턴* 의 사용 이유와 그 초점이 서로 다릅니다.

## 의도
> 생성자에 매개변수가 많을 때 빌더 패턴을 사용하여 코드를 깨끗이 한다.

생성자에 매개변수가 많고 또 그 매개변수가 모두 필수 정보가 아닐 때, 우리는 생성자를 만드는 데 어려움을 겪습니다. 
예를 들어 봅시다. 우리 시스템에서 만들 '방'이라는 객체는 바닥이 필수로 존재해야 하지만 벽이나 문, 창문은 선택적 사항이라고 해봅시다.
바닥은 필수 사항이기 때문에 생성자에 매개변수를 추가해줄 수 있지만, 벽과 문, 창문은 반드시 생성하도록 강제해줄 수 없습니다.
  
그렇다면 우리는 이런 방법을 생각해볼 수 있습니다

'선택 파라미터의 경우의 수대로 **생성자**를 준비해놓자!'

  ```
  // 생성자 1 : 바닥
  Room(Floor floor) {..생략..}  
  // 생성자 2. : 바닥 + 벽
  Room(Floor floor, Map<Direction, Wall> walls) {..생략..}
  // 생성자 3. : 바닥 + 문
  Room(Floor floor, Map<Direction, Door> doors) {..생략..}
  // 생성자 4. : 바닥 + 창문
  Room(Floor floor, Map<Direction, Window> windows) {..생략..}
  // 생성자 5. : 바닥 + 벽 + 문
  Room(Floor floor, Map<Direction, Wall> walls, Map<Direction, Door> doors) {..생략..}
  // 생성자 6. : 바닥 + 벽 + 창문
  Room(Floor floor, Map<Direction, Wall> walls, Map<Direction, Window> windows) {..생략..}
  // 생성자 7. : 바닥 + 문 + 창문
  Room(Floor floor, Map<Direction, Door> doors, Map<Direction, Window> windows) {..생략..}
  // 생성자 8. : 바닥 + 벽 + 문 + 창문
  Room(Floor floor, Map<Direction, Wall> walls, Map<Direction, Door> doors, Map<Direction, Window> windows) {..생략..}
  ```

자, 모든 경우의 수대로 생성자가 준비되었습니다. 허나 이 위에 있는 코드를 다 작성하면 문제가 생길 것입니다.
1. 생성자의 <u>Signature가 같다</u>.  
생성자 2&#126;4번, 생성자 5&#126;7번은 서로 Signature가 서로 겹칩니다. 따라서 모두 작성할 수 없으며 하나만 남기고 나머지는 다 제거해줘야 합니다. 그렇다면 무엇을 남겨놔야 할까요?
가장 많이 쓰일 것 같다고 생각되는 걸 남겨놓는다 해도 이 객체를 사용하는 프로그래머가 혼란을 겪지 않을까요?
즉, 생성자의 Signature가 같을 때도 있다면 이처럼 어떤 생성자를 두어야 할지 애매한 상황이 발생합니다.
2. 생성자의 <u>엄청난 경우의 수</u>  
현재는 3개의 선택 필드가 존재합니다. 하지만 선택 필드의 개수가 점점 늘어나면 생성자의 경우의 수는 감당할 수 없을 정도로 늘어납니다.
현재 선택 필드가 3개일 때 이미 생성자의 경우의 수가 8개인데(1번에 해당하지 않는다 하더라도) 선택 필드가 하나씩 늘어날 때마다 그의 배로 늘어나게 될 것입니다.
즉, 선택이 가능한 필드의 개수에 따라 생성자를 계속 추가해줄 수는 없습니다.

그렇다면 우리는 다른 방법을 생각해 낼 것입니다.

'**Setter**를 이용하자!'

필수 필드만 생성자로 두고 선택 필드는 Setter를 이용하게 하는 것입니다.

  ```
    public Room(Floor floor) {
        this.floor = floor;
    }

    public void setWalls(Map<Direction, Wall> walls) {
        this.walls = walls;
    }

    public void setWall(Direction direction, Wall wall) {
        this.walls.put(direction, wall);
    }

    public void setDoors(Map<Direction, Door> doors) {
        this.doors = doors;
    }

    public void setWall(Direction direction, Door door) {
        this.doors.put(direction, door);
    }

    public void setWindows(Map<Direction, Window> windows) {
        this.windows = windows;
    }
    
    public void setWall(Direction direction, Window window) {
        this.windows.put(direction, window);
    }
  ```
* 객체 생성
  ```
  Room room = new Room();
  room.setFloor(new Floor());
  room.setWall(Direction.EAST, new Wall());
  room.setWall(Direction.WEST, new Wall());
  room.setWall(Direction.SOUTH, new Wall());
  room.setWindow(Direction.SOUTH, new Window());
  ```
        
하지만 위 코드 역시 필드가 많아지게 되면 문제가 있습니다.
그것은 바로, 최종적인 <u>온전한 객체를 만들 때까지</u> Setter 메소드를 <u>여러 번 호출</u>해야 하며 그 때까지는 **일관성이 무너진 상태**에 놓인다는것입니다.

'아.. 생성자에 매개변수가 많아지면 Setter도 대응하기 어렵겠구나'

하지만 다행히 한가지 방법이 더 있습니다. 바로 *빌더 패턴* 입니다.
우리는 이제 *빌더 패턴* 을 적용하여 코드의 **가독성**을 높여주면서 <u>객체 생성에 안전성</u>을 높여줄 것입니다.

# 적용
* 방 클래스 : `Room`
  ```
  public class Room {
  
      private Floor floor;
      private Map<Direction, Wall> walls;
      private Map<Direction, Door> doors;
      private Map<Direction, Window> windows;

      // 빌더로 필드 세팅
      public Room(RoomBuilder roomBuilder) {
          this.floor = roomBuilder.getFloor();
          this.walls = roomBuilder.getWalls();
          this.doors = roomBuilder.getDoors();
          this.windows = roomBuilder.getWindows();
      }
    
      // 출력을 위함
      @Override
      public String toString() {
          StringBuffer buffer = new StringBuffer(floor.toString()).append("\n");
          for (Direction direction : walls.keySet()) {
              buffer.append(direction.getValue()).append("쪽의 ").append(walls.get(direction).toString()).append("\n");
          }
          for (Direction direction : doors.keySet()) {
              buffer.append(direction.getValue()).append("쪽의 ").append(doors.get(direction).toString()).append("\n");
          }
          for (Direction direction : windows.keySet()) {
              buffer.append(direction.getValue()).append("쪽의 ").append(windows.get(direction).toString()).append("\n");
          }
          return buffer.toString();
      }
  }
  ```
* 방 빌더 클래스 : `RoomBuilder`
  ```
  public class RoomBuilder {
      private Floor floor;
      private Map<Direction, Wall> walls = new HashMap<>();
      private Map<Direction, Door> doors = new HashMap<>();
      private Map<Direction, Window> windows = new HashMap<>();
  
      public RoomBuilder() {
          this.floor = new Floor();
      }
  
      public RoomBuilder buildWalls(Direction direction) {
          this.walls.put(direction, new Wall());
          return this;
      }
  
      public RoomBuilder buildDoors(Direction direction) {
          this.doors.put(direction, new Door());
          return this;
      }
  
      public RoomBuilder buildWindows(Direction direction) {
          this.windows.put(direction, new Window());
          return this;
      }
  
      public Floor getFloor() {
          return floor;
      }
  
      public Map<Direction, Wall> getWalls() {
          return walls;
      }
  
      public Map<Direction, Door> getDoors() {
          return doors;
      }
  
      public Map<Direction, Window> getWindows() {
          return windows;
      }
  
      public Room build() {
          return new Room(this);
      }
  }
  ```
* 실행과 결과
  ```
  Room room = new RoomBuilder()
          .buildWalls(Direction.EAST)
          .buildWalls(Direction.WEST)
          .buildWalls(Direction.SOUTH)
          .buildWindows(Direction.SOUTH)
          .build();
  ```
  <p>
  바닥<br>
  동쪽의 벽<br>
  북쪽의 벽<br>
  서쪽의 벽<br>
  북쪽의 창문<br>
  </p>
  
  ```
  Room room = new RoomBuilder()
                  .buildWalls(Direction.EAST)
                  .buildWalls(Direction.WEST)
                  .buildWalls(Direction.NORTH)
                  .buildWalls(Direction.SOUTH)
                  .buildDoors(Direction.NORTH)
                  .buildWindows(Direction.EAST)
                  .buildWindows(Direction.WEST)
                  .buildWindows(Direction.NORTH)
                  .buildWindows(Direction.SOUTH)
                  .build();
  ```
  <p>
  바닥<br>
  동쪽의 벽<br>
  북쪽의 벽<br>
  서쪽의 벽<br>
  남쪽의 벽<br>
  동쪽의 창문<br>
  북쪽의 창문<br>
  서쪽의 창문<br>
  남쪽의 창문<br>
  </p>

*빌더 패턴* 은 객체를 사용하는 클라이언트가 필요한 객체를 직접 만드는 것이 아니라 <u>빌더에게 객체를 받게 됩니다</u>.
클라이언트는 필수 매개변수만으로 <u>빌더 객체를 생성</u>하고, 빌더를 통해 다른 선택 필드들을 <u>쌓아올리고</u> 마지막으로 빌더 객체에게 **최종 객체**를 받게 됩니다.
*빌더 패턴* 을 통해 클라이언트는 코드를 작성하기 쉬워지며 개발자가 보기에도 가독성이 좋아집니다. 

특히 *빌더 패턴* 은 계층적으로 설계되어있는 클래스에 적절하게 쓰입니다. 추상 클래스에는 추상 빌더를, 구체 클래스에게는 구체 빌더를 정의하여 계층별로 사용하는 것입니다.

# 정리
생성자나 정적 팩터리가 다뤄야 할 매겨변수가 많다면, *빌더 패턴* 을 사용하는 게 나을 수도 있습니다. 특히 모든 필드가 필수가 아니고 선택적으로 정보가 존재할 수 있는 필드들이 많으면 더 그렇습니다.
*빌더 패턴* 을 사용하면 빌더를 사용함으로써 클라이언트들의 코드가 훨씬 깔끔해지며 가독성이 좋아집니다.

# 추상 팩토리 패턴 (Abstract Factory Pattern)

## 구조
![프로토타입 패턴](/images/design-pattern/abstract-factory-pattern.png)

## 별명
* 키트(Kit)

## 의도
> 상세화된 서브클래스를 정의하지 않고도 서로 관련성이 있거나 독립적인 여러 객체의 <u>군을 생성하기 위한 인터페이스를 제공</u>합니다.

추상 팩토리는 객체가 생성/구성되거나 표현이 되는 방식에 전혀 상관없이 시스템을 독립적으로 만들고자 할 때 유용하게 사용합니다.
특히 여러 개의 관련된 제품들이 하나의 **군**을 이루고, 여러 제품군 중에서 하나를 <u>선택하여</u> 사용할 때(시스템에서 설정해야 할 때) 더욱 유용합니다.
또한, 이미 구성됐다 하더라도 일부 제품을 다른 것으로 대체하고자 할 때도 유연하게 대처할 수 있습니다.

예제로 그 의미를 파악해보도록 하겠습니다.

## 예제
우리는 아까와 동일하게 바닥과 벽, 문, 창문으로 방을 만들어낼 것입니다. 하지만 우리는 '~~로 만든 방'이라는 개념을 도입하여 제품을 라인화시킬 것입니다.
방을 단순하게 만드는 것이 아니라 '나무로 만든 방', '철제로 만든 방' 등과 같이 방의 종류를 지어주는 것입니다.
그렇다면 방의 구성 요소가 되는 바닥, 벽, 문, 창문 역시 모두 '나무로 만든 바닥', '철제로 만든 창문' 등등으로 바뀌어야 합니다.
가장 먼저, 기존에 사용했던 바닥, 벽, 문, 창문 객체를 **추상화** 시켜보겠습니다.

* 바닥 : `Floor` `WoodenFloor` `SteelFloor`
  ```
  public interface Floor { }
  ```
  ```
  public class SteelFloor implements Floor {
      @Override
      public String toString() { return "철제로 된 바닥"; }
  }

  ```
  ```
  public class WoodenFloor implements Floor {
      @Override
      public String toString() { return "나무로 된 바닥"; }
  }

  ```
* 벽 : `Wall` `WoodenWall` `SteelWall`
  ```
  public interface Wall { }
  ```
  ```
  public class SteelWall implements Wall {
      @Override
      public String toString() { return "철제로 된 벽"; }
  }

  ```
  ```
  public class WoodenWall implements Wall {
      @Override
      public String toString() { return "나무로 된 벽"; }
  }

  ```
* 문 : `Door` `WoodenDoor` `SteelDoor`
  ```
  public interface Door { }
  ```
  ```
  public class SteelDoor implements Door {
      @Override
      public String toString() { return "철제로 된 문"; }
  }

  ```
  ```
  public class WoodenDoor implements Door {
      @Override
      public String toString() { return "나무로 된 문"; }
  }

  ```
* 창문 : `Window` `WoodenWindow` `SteelWindow` 
  ```
  public interface Window { }
  ```
  ```
  public class SteelWindow implements Window {
      @Override
      public String toString() { return "철제로 된 창문"; }
  }

  ```
  ```
  public class WoodenWindow implements Window {
      @Override
      public String toString() { return "나무로 된 창문"; }
  }

  ```

자, 그렇다면 이제 '~로 만든 방'을 만들어볼까요? 가장 먼저, '철제로 만든 방'부터 만들어봅시다.  
(방의 구조는 다음과 같다고 해봅시다. '사방에 벽이 있고, 남쪽에 문, 북쪽에 창문이 있다.')

* 방 생성 클래스 : `RoomCreator`
  ```
  public class RoomCreator {
      public Room createRoom() {
          Floor floor = new SteelFloor();
  
          // 사방에 생성
          Map<Direction, Wall> walls = new HashMap<>();
          walls.put(Direction.EAST, new SteelWall());
          walls.put(Direction.WEST, new SteelWall());
          walls.put(Direction.NORTH, new SteelWall());
          walls.put(Direction.SOUTH, new SteelWall());
  
          // 남쪽에 문 생성
          Map<Direction, Door> doors = new HashMap<>();
          doors.put(Direction.NORTH, new SteelDoor());
  
          // 북졲에 창문 생성
          Map<Direction, Window> windows = new HashMap<>();
          windows.put(Direction.SOUTH, new SteelWindow());
  
          return new Room(floor, walls, doors, windows);
      }
  }

  ```
* 실행과 결과
  ```
  RoomCreator roomCreator = new RoomCreator();
  Room room = roomCreator.createRoom();
  System.out.println(room);
  ```
  <p>
  철제로 된 바닥<br/>
  남쪽의 철제로 된 벽<br/>
  동쪽의 철제로 된 벽<br/>
  서쪽의 철제로 된 벽<br/>
  북쪽의 철제로 된 벽<br/>
  남쪽의 철제로 된 문<br/>
  북쪽의 철제로 된 창문<br/>
  </p>

자, 우리는 createRoom 함수에서 모두 다 Steel이 붙어있는 Room, Wall, Door, Window 구체클래스들을 사용했기 때문에 최종적으로 방을 생성했을 때 '철제로 된'이 붙어있는 걸 확인할 수 있습니다.  
그렇다면 '나무로 만든 방'을 만들어봅시다. 어떻게 해야 할까요?

`new Steel~~()`를 모두 `new Wooden~~()`로 바꾼다.  
사실 굉장히 간단합니다. 말 그대로 Steel이라고 쓰인 부분을 모두 Wooden으로 바꾸는 것입니다.
모두 바꿔서 직접 실행을 하게 되면 위의 결과에서 '철제로 된' 부분이 모두 '나무로 된'으로 바뀌어 있을 겁니다.
하지만 여러분들은 이 때문에 `RoomCreator` 함수를 수정하였습니다. 음.. 이러다간 요청한 방의 유형이 바뀔 때마다 `RoomCreator` 함수를 수정해야 할 겁니다.

우리는 위에서 이와 비슷한 문제를 경험하였습니다. 바로 _팩토리 메소드 패턴_ 을 하면서 말입니다.  
위에서 우리는 놀이동산을 운영할 때 젤리 놀이동산에서 비스킷 놀이동산으로 바꿔줄 때 코드가 바뀌는 문제를 경험하여 *팩토리 메소드 패턴* 을 적용해주었습니다.
`AmusementParkOperator`를 바꿔주지 않기 위해 우리는 <u>인스턴스를 생성하여 반환해주는</u> '**팩토리 인터페이스**'를 만들고 서브클래스가 이를 구현하여 정확히 어떤 구체 클래스의 인스턴스를 반환해주는지 결정하였습니다.
동일한 문제가 있으니 먼저 _팩토리 메소드 패턴_ 을 사용하여 이를 해결해주는 게 좋겠습니다.

* 바닥 팩토리 : `FloorFactory` `SteelFloorFactory` `WoodenFloorFactory`
  ```
  public interface FloorFactory {
      Floor makeFloor();
  }
  ```

  ```
  public class SteelFloorFactory implements FloorFactory {
      @Override
      public Floor makeFloor() {  return new SteelFloor();  }
  }
  ```
  ```
  public class WoodenFloorFactory implements FloorFactory {
      @Override
      public Floor makeFloor() {  return new WoodenFloor();  }
  }
  ```
* 벽 팩토리 : `WallFactory` `SteelWallFactory` `WoodenWallFactory`
  ```
  public interface WallFactory {
      Wall makeWall();
  }
  ```

  ```
  public class SteelWallFactory implements WallFactory {
      @Override
      public Wall makeWall() {  return new SteelWall();  }
  }
  ```
  ```
  public class WoodenWallFactory implements WallFactory {
      @Override
      public Wall makeWall() {  return new WoodenWall();  }
  }
  ```
* 문 팩토리 : `DoorFactory` `SteelDoorFactory` `WoodenDoorFactory`
  ```
  public interface DoorFactory {
      Door makeDoor();
  }
  ```

  ```
  public class SteelDoorFactory implements DoorFactory {
      @Override
      public Door makeDoor() {  return new SteelDoor();  }
  }
  ```
  ```
  public class WoodenDoorFactory implements DoorFactory {
      @Override
      public Door makeDoor() {  return new WoodenDoor();  }
  }
  ```
* 창문 팩토리 : `WindowFactory` `SteelWindowFactory` `WoodenWindowFactory`
  ```
  public interface WindowFactory {
      Window makeWindow();
  }
  ```

  ```
  public class SteelWindowFactory implements WindowFactory {
      @Override
      public Window makeWindow() {  return new SteelWindow();  }
  }
  ```
  ```
  public class WoodenWindowFactory implements WindowFactory {
      @Override
      public Window makeWindow() {  return new WoodenWindow();  }
  }
  ```
* 방 생성 클래스 :  RoomCreator
  ```
  public class RoomCreator {
      public Room createRoom(FloorFactory floorFactory, WallFactory wallFactory, DoorFactory doorFactory, WindowFactory windowFactory) {
          // 바닥 생성
          Floor floor = floorFactory.makeFloor();
  
          // 사방에 생성
          Map<Direction, Wall> walls = new HashMap<>();
          walls.put(Direction.EAST, wallFactory.makeWall());
          walls.put(Direction.WEST, wallFactory.makeWall());
          walls.put(Direction.NORTH, wallFactory.makeWall());
          walls.put(Direction.SOUTH, wallFactory.makeWall());
  
          // 남쪽에 문 생성
          Map<Direction, Door> doors = new HashMap<>();
          doors.put(Direction.NORTH, doorFactory.makeDoor());
  
          // 북졲에 창문 생성
          Map<Direction, Window> windows = new HashMap<>();
          windows.put(Direction.SOUTH, windowFactory.makeWindow());
  
          return new Room(floor, walls, doors, windows);
      }
  }
  ```
* 실행과 결과
  ```
  RoomCreator roomCreator = new RoomCreator();
  Room room = roomCreator.createRoom(new SteelFloorFactory(), new SteelWallFactory(), new SteelDoorFactory(), new SteelWindowFactory());
  System.out.println(room);
  ```
  <p>
  철제로 된 바닥<br/>
  남쪽의 철제로 된 벽<br/>
  동쪽의 철제로 된 벽<br/>
  서쪽의 철제로 된 벽<br/>
  북쪽의 철제로 된 벽<br/>
  남쪽의 철제로 된 문<br/>
  북쪽의 철제로 된 창문<br/>
  </p>

우리는 기존에 배운 *팩토리 메소드 패턴* 을 적용하여 `RoomCreator`가 직접 객체들을 생성하지 않도록 수정해주었습니다. 적용함으로써 더 이상 제품이 바뀐다 할지라도 `RoomCreator`를 바꿀 일은 없어졌습니다.
하지만 여전히 <u>해결되지 않는 문제</u>가 있습니다. 그것은 바로 구성요소들끼리 <u>**일관성**이 없다</u>는 것입니다.  
물론, 방을 생성하는 걸 요청하는 클라이언트 쪽에서 <u>어떤 팩토리를 쓸지</u> 정해서 넘겨주기 때문에 `RoomCreator` 클래스를 수정하지 않아도 된다는 게 참 다행입니다.  
하지만 애초에 어떤 팩토리를 써줄지 넘겨주는 곳에서 <u>서로 다른 팩토리를 넘겨주게 되면</u> 어떻게 될까요?
벽 팩토리 매개변수만 혼자서 나무로 만든 팩토리로 바꿔서 넘겼다고 해봅시다.

* 실행과 결과
  ```
  RoomCreator roomCreator = new RoomCreator();
  Room room = roomCreator.createRoom(new SteelFloorFactory(), new WoodenWallFactory(), new SteelDoorFactory(), new SteelWindowFactory());
  System.out.println(room);
  ```
  <p>
  철제로 된 바닥<br/>
  남쪽의 나무로 된 벽<br/>
  동쪽의 나무로 된 벽<br/>
  서쪽의 나무로 된 벽<br/>
  북쪽의 나무로 된 벽<br/>
  남쪽의 철제로 된 문<br/>
  북쪽의 철제로 된 창문<br/>
  </p>

두 번째 매개변수를 WoodenWallFactory로 바꾸어서 호출했더니 위와 같은 결과가 표시됩니다. 분명히 '~~로 된 방'을 만든다고 하지 않았었나요? 제품들끼리 일관성이 전혀 없는데 어떻게 해야 할까요?  
해답은 바로 _추상 팩토리 패턴_ 에 있습니다

*추상 팩토리 패턴* 은 제품들의 객체를 생성하는 과정과 책임을 **캡슐화**하고 **추상화**시킵니다.
객체를 생성하는 부분을 특정 클래스가 감싸고 그 과정들을 추상화하여 인터페이스 형태로 제공합니다.
그리하여 실제 방을 생성하는 로직이 담겨있는 `RoomCreator`는 구체적인 클래스가 아니라 <u>인터페이스를 통해서만</u> 인스턴스를 조작하기 때문에 `RoomCreator` 는 방의 종류에 대해서 자유로와집니다.  
적용하여 그 의미를 파악해봅시다.

* 팩토리 클래스 : `RoomFactory` `SteelRoomFactory` `WoodenRoomFactory`
  ```
  public interface RoomFactory {
      Floor makeFloor();
      Door makeDoor();
      Wall makeWall();
      Window makeWindow();
  }
  ```
  ```
  public class SteelRoomFactory implements RoomFactory {
      @Override
      public Floor makeFloor() {  return new SteelFloor();  }
      @Override
      public Door makeDoor() {  return new SteelDoor(); }
      @Override
      public Wall makeWall() {  return new SteelWall(); }
      @Override
      public Window makeWindow() {  return new SteelWindow(); }
  }
  ```
  ```
  public class WoodenRoomFactory implements RoomFactory {
      @Override
      public Floor makeFloor() {  return new WoodenFloor();  }
      @Override
      public Door makeDoor() {  return new WoodenDoor(); }
      @Override
      public Wall makeWall() {  return new WoodenWall(); }
      @Override
      public Window makeWindow() {  return new WoodenWindow(); }
  }
  ```
* 실행과 결과
  ```
  RoomCreator roomCreator = new RoomCreator();
  Room room = roomCreator.createRoom(new SteelRoomFactory());
  System.out.println(room);
  ```

우리는 각각의 제품들을 한 팩토리에서 관리하도록 변경해주었습니다.
따라서 `SteelRoomFactory`를 사용하면 `SteelFloor`, `SteelWall`, `SteelDoor`, `SteelWindow`가 `WoodenRoomFactory`를 사용하면 `WoodenFloor`, `WoodenWall`, `WoodenDoor`, `WoodenWindow`가 생성됩니다.
하나의 상품을 만들기 위해 모든 제품들(Floor, Wall, Door, Window)이 모두 <u>일관성을 갖게 된 것</u>입니다.  

이처럼, *추상 팩토리 패턴* 은 제품 사이의 일관성을 증진시킵니다.
하나의 **군(또는 집합)** 안에 속한 객체들이 서로 <u>함께 동작하도록</u> 되어 있을 때, 그리하여 시스템에서 <u>하나의 군을 선택하도록</u> 되어있을 때,
객체들의 **일관성**을 증진시키기 위하여 주로 *추상 팩토리 패턴* 을 적용합니다.
이는 제품군을 <u>쉽게 대체할 수 있다</u>는 장점이 있습니다.
철제로 된 방에서 나무로 된 방으로 바꾸고 싶으면 내부의 여러 객체들을 일일히 수정할 것이 아니라 `SteelRoomFactory`를 선택했던 것을 `WoodenRoomFactory`로 바꾸어 주면 되는 것입니다.
이처럼 '추상 팩토리'가 앞에서 필요했던 모든 것을 다 생성해주기 때문에 제품군이 한번에 변경될 수 있습니다.

물론 장점만 있는 것은 아닙니다. 추상 팩토리 패턴은 패턴 특성 상 <u>서브클래싱</u>을 해줄 수밖에 없기 때문에 새로운 제품이 추가되어 인터페이스에 메소드를 추가해줘야 하는 경우가 생기면 모든 서브클래스가 이를 반영해줘야 합니다.
예를 들어, 방 구조에 '베란다'라는 개념이 추가되었다면, 인터페이스에 `makeVeranda`라는 메소드가 추가되어야 하고 이를 구현/상속하고 있는 모든 서브클래스를 찾아 이를 반영 및 수정해주어야 합니다. 
하지만 *추상 팩토리 패턴* 은 관련된 객체들이 서로 함께 사용되게 되어있을 때, 그 객체들의 **일관성**을 쉽게 제공해주기 때문에 자주 사용되는 패턴입니다.

## 관련된 패턴
여러분들은 *추상 팩토리 패턴* 을 보면서 이런 **의문**을 가질 수도 있습니다. (사실 제가 가졌던 의문입니다.)
  
'아니, 팩토리 메소드 패턴이랑 다를 게 뭐야? 그냥 팩토리 메소드 패턴이 객체를 여러 개의 객체를 담당하면 추상 팩토리 패턴을 쓰게 되는 거야?'

결론부터 말씀드리면, 여러 디자인 패턴들이 서로 <u>겹치는 부분</u>이 있어서 그렇습니다. 서로 연관될 수밖에 없는 것이죠.
사실상 추상 팩토리 패턴과 팩토리 메소드 패턴의 Area는 분명하게 구분되어 있습니다.
* 팩토리 메소드 패턴 : 부모가 아닌 서브 클래스가 '어떤 객체를 생성한다'는 것을 결정한다.
* 추상 팩토리 패턴 : 여러 객체가 모여 하나의 군을 이룰 때, 그 객체들의 일관성을 제공한다.
하지만 각각의 <u>**목적**을 구현하는 과정</u>에 있어서 서로 **겹칠수도** 또는 서로를 **사용할 수도** 있는 구조가 되기도 하는 것입니다.

위에서는 *추상 팩토리 패턴* 을 적용하기 위해 *팩토리 메소드 패턴* 을 사용하지만 *추상 팩토리 패턴* 은 사실 <u>다른 방식</u>으로도 구현될 수 있습니다.
*프로토타입 패턴* 을 사용해서 *추상 팩토리 패턴* 을 적용할 수 있는 것입니다.(미리 객체군을 만들어놓고 복제하여 사용한다.)

이처럼 하나의 패턴이 적용되는 과정에 <u>다른 패턴을 사용되기도 하고</u> 심지어는 같은 패턴이라 할 지라도 <u>여러 가지 방식</u>이 존재합니다.
패턴의 형태가 한가지로 국한되는 것이 아니란 것입니다.(특히 서로 언어가 다를 때 많이 발생합니다.)

가장 중요한 것은, '**어떤 상황**'에서 '**어떤 문제**'가 있을 때 '**<u>이런 방식으로 해결한다</u>**'입니다.
* 부모에서 어떤 구체클래스를 사용할 지 모를 때 팩토리 메소드 패턴을 적용한다.
* 객체군에 일관성을 제공해주고 싶을 때 추상 팩토리 패턴을 사용한다.
* 등등

----------------------------------------------------------------

## 정리하며..
지금까지 우리는 디자인 패턴의 의미와 생성 패턴의 종류에 관하여 공부해보았습니다.  
다음에는 구조 패턴의 종류에 대해 알아보겠습니다.

## References
* https://gmlwjd9405.github.io/2018/08/08/abstract-factory-pattern.html
* https://gmlwjd9405.github.io/2018/07/06/design-pattern.html
* https://ko.wikipedia.org/wiki/%EB%B9%8C%EB%8D%94_%ED%8C%A8%ED%84%B4
* https://gdtbgl93.tistory.com/19
* http://algamza.blogspot.com/2016/04/prototype-pattern.html
* https://gmlwjd9405.github.io/2018/07/06/singleton-pattern.html
* 서적 : GoF의 디자인 패턴
* 서적 : 이펙티브 자바