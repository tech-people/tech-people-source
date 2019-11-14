---
title: 비콘(Beacon)에 대한 이해
date: 2019-11-13 14:56:55
thumbnail :
tags: [beacon]
category : [IT Tech, 1. IoT]
---

#### 1. 비콘(Beacon)이란 무엇인가
- 전통적인 의미에서의 비콘은 어떤 신호를 알리기 위해 주기적으로 신호를 전송하는 기기를 모두 의미한다. 
  역사적으로 보면 비콘이 가장 널리 사용된 부분은 적의 침입을 알리는 일종의 군사 연락 수단이었다. 언덕이나 높은 곳에 설치한 불빛을 의미한다. 육지에서는 적의 습격을 미리 감지해 방어 할 수 있도록 알려주는 기능을 했고 바다에서 보면 등대의 역할로 선박의 안전 운행을 유도하는 역할로 기능을 했다. 
  따라서 옛날의 등대나 봉화 같은 것도 전통적인 의미에서는 모두 비콘에 포함된다고 할 수 있다. 이러한 비콘의 개념은 현대에 이르러 IT기술과 만나 보다 확장되고, 일상 깊숙이 들어왔다. 비콘을 활용하면 사물과 상황인식, 콘텐츠 푸시, 실내위치 측위, 자동 체크인, 지오펜스 등 다양한 응용 서비스 제공이 가능 하다. 비콘은 본질적으로 위치를 알려주는 기준점 역할을 하며, 정보 전달을 위해서는 통신기술(음파(Ultrasonic) , LED, WiFi, 블루투스) 활용이 필요하다



##### 2. 비콘의 종류
- 신호를 전송하는 방법에 따라 사운드기반의 저주파 비콘, LED 비콘, WiFi 비콘, 블루투스 비콘 등으로 나눌 수 있다.

##### 2.1. iBeacon
- 아이비콘(iBeacon)은 비콘의 전통적인 개념만 따왔을 뿐 기술적으로는 기존의 비콘과는 많이 다르다. 아이비콘의 기본 개념은 위치서비스(Location Services)를 iOS로 확장한 것이다.
  애플이 아이비콘에 사용한 핵심 기술은 블루투스 4.0 표준에 포함된 사양 중 하나인 블루투스 로우 에너지(BLE, Bluetooth Low Energy)다. iBeacon은 BLE 4.0의 애드버터이징 패킷(Advertising Packet)전송 표준을 활용, 이를 iOS기기에 적용한 것이다.
  아이비콘 장치의 비콘 신호 영역 안에 iOS 기기를 소지한 사용자가 들어오면 해당 어플리케이션에 신호(Beacon)를 보내게 된다. 예를 들어 특정 상점 근처를 지나갈 때 상점에 설치된 비콘이 할인 쿠폰을 보내주거나 박물관에서 특정 전시물 앞에 가면 관련된 내용을 iOS 기기로 보내주는 등 이러한 기술을 이용할 수 있는 영역이 매우 다양하다.
  
##### 2.2. no-iBeacon  
- iBeacon 규격을 사용하지 안는 Beacon 모델도 있다.
1. AltBeacon : Radius Networks 사에서 오픈 한 규격으로 애플의 iBeacon 규격을 보다 유연성 있게 재 구성한 비콘 이다. 
2. UriBeacon : Google의 Physical Web 프로젝트와 연계되어 특정 URI(Universal Resouce Identifier)가 포함된 Advertising Data를 브라우저를 통해 바로 정보를 보여주는 방식이다. 
3. Placedge : 삼성이 자사의 스마트폰인 갤럭시폰에 관련 기능을 지원하면서 해당 서비스 App이 설치되어 있지 않아도 이를 바로 연결해 주도록 지원하고 쿠폰, 콘텐츠 등을 전달하는 플랫폼 또한 별도로 개발하지 않아도 가능하도록 서버 플랫폼을 함께 제공하고 있다.  
4. 에디스톤(Eddystone) : 2015년 구글에서 발표했으며 오픈 소스 프로젝트이다. 차이점은 에디스톤의 전송패킷은 Eddyston-UID, Eddyston-URL, Eddyston-TML(Telemetry)세가지 타입이 있다. 기존 규격을 지키면서도 유연한 구조로 iOS와 Android 모두에서 잘 동작하는 것이 목표라고 한다. 



##### 3. 비콘의 장단점
##### 3.1. 장점
- Bluetooth 기반의 무선 통신이지만 페어링 하는 과정이 없습니다. 사(용자의 수동적인 작업이 필요 없다.)
- 저전력 기술이다
- BLE(Bluetooth Low Energy) 기술을 활용하여 구현하여 iOS는 물론 BLE를 장착한 모든 기기(안드로이드 포함)에서 Beacon을 이용 할 수 있다.
- 근거리 무선 통신인 NFC가 10m 이내의 근거리에서만 작동하는 반면, Beacon은 약5m에서 50m(최대70m) 정도의 먼 거리에서도 사용이 가능하다. 안전적 거리는 20~30m이다
- 장비에 접촉 없이 Beacon이 설치된 곳을 지나가기만 해도 데이터 전송이 가능하다.
  
##### 3.1. 단점
- Beacon 자체의 보안성 문제인 스푸핑(Spoofing)과 클로닝(Cloning)에 취약 하다.
- 스푸핑 : 네트워크에서 MAC 주소, IP주소, 포트 등 네트워크 통신과 관련된 정보들을 속여서 통신 흐름을 왜곡시키는 공격.
- 클로닝 : 원본 시스템의 복제본을 하나 이상을 생성하는 것으로, 기존에 체크인 되었던 대사물의 정보를 복제하여 그 대상물이 없어도 있는 것처럼 속여서 정보를 빼내는 방법.
- 비콘 확산으로 많은 채널이 점유되면 비콘 간의 전파 간섭과 충돌로 인해 신호 수신율 저하와 서비스 자체가 불가능해질 우려가 있다.
- 비콘이 부착된 사물 또는 소유하는 사용자의 개인정보가 노출 될 수 있다



##### 4. Bluetooth Low Energy(BLE)
- BLE는 종종 Bluetooth Smart 로도 불리며 classic Bluetooth의 경량화 버전을 목표로 블루투스 4.0의 일부로 발표되었습니다. Classic Bluetooth와 겹치는 부분이 존재하지만 BLE는 완전히 다른 표준으로 블루투스 표준화 그룹인 Bluetooth SIG에 의해서 개발되기 전까지 Nokia의 사내 프로젝트(Wibree)로 시작하였습니다.
  BLE를 지원하는 디바이스들은 기본적으로 Advertise(Broadcast) 과 Connection 이라는 방법으로 외부와 통신한다.

##### 4.1. Advertise Mode(Broadcast Mode)
- 특정 디바이스를 지정하지 않고 주변의 모든 디바이스에게 Signal을 보낸다. 다시 말해, 주변에 디바이스가 있건 없건, 다른 디바이스가 Signal을 듣는 상태이건 아니건, 자신의 Signal을 일방적으로 보내는 것이라고 생각하면 된다. 이 때, Advertising type의 Signal을 일정 주기로 보내게 된다.
![출처 : Nerdery (BLE communication)](/images/screenshot_1.png)

- Advertise 관점에서, 디바이스의 역할은 다음과 같이 구분된다.
  Advertiser(Broadcaster) : Non-Connectable Advertising Packet을 주기적으로 보내는 디바이스. (Beacon)
  Observer : Advertiser가 Advertise를 Non-Connectable Advertising Packet을 듣기 위해 주기적으로 Scanning하는 디바이스. (스마트폰 및 스캐너)
  Advertise 방식은 한 번에 한 개 이상의 디바이스와 통신할 수 있는 유일한 방법이다. 주로 디바이스가 자신의 존재를 알리거나 적은 양(31Bytes 이하)의 User 데이터를 보낼 때도 사용된다. 한 번에 보내야 하는 데이터 크기가 작다면, 굳이 오버헤드가 큰 Connection 과정을 거쳐서 데이터를 보내기 보다는, Advertise를 이용하는 것이 더 효율적이기 때문이다. 게다가 전송할 수 있는 데이터 크기 제한을 보완하기 위해 Scan Request, Scan Response을 이용해서 추가적인 데이터를 주고 받을 수 있다. Advertise 방식은 말 그대로 Signal을 일방적으로 뿌리는 것이기 때문에, 보안에 취약하다.

##### 4.2. Connection Mode
- 양방향으로 데이터를 주고받거나, Advertising Packet으로만 전달하기에는 많은 양의 데이터를 주고 받아야 하는 경우에는, Connection Mode로 통신을 한다. Advertise처럼 ‘일대다’ 방식이 아닌, ‘일대일’ 방식으로 디바이스 간에 데이터 교환이 일어난다. 디바이스간에 Channel hopping 규칙을 정해놓고 통신하기 때문에 Advertise보다 안전하다.
  Connection 관점에서 디바이스들의 역할은 다음과 같이 구분된다.
  
 - Central (Master) : Central 디바이스는 다른 디바이스와 Connection을 맺기 위해, Connectable Advertising Signal을 주기적으로 스캔 하다가, 적절한 디바이스에 연결을 요청한다. 연결이 되고 나면, Central 디바이스는 timing을 설정하고 주기적인 데이터 교환을 주도한다. 여기서 timing이란, 두 디바이스가 매번 같은 Channel에서 데이터를 주고 받기 위해 정하는 hopping 규칙이라고 생각하면 된다.
 
 - Peripheral (Slave) : Peripheral 디바이스는 다른 디바이스와 Connection을 맺기 위해, Connectable Advertising Signal을 주기적으로 보낸다. 이를 수신한 Central 디바이스가 Connection Request를 보내면, 이를 수락하여 Connection을 맺는다. Connection을 맺고 나면 Central 디바이스가 지정한 timing에 맞추어 Channel을 같이 hopping을 하면서 주기적으로 데이터를 교환한다.
 
 

##### 5. iBeacon 스펙
- Advertiser’s Data (Max 31 byte) 영역은 iBeacon prefix, UUID, Major, Minor, TX Power 로 나뉩니다

![출처:사물 인터넷 네트워크와 서비스 구축 강좌 iBeacon spec](/images/screenshot_2.png)



- iBeacon prefix(9 byte) : 비콘의 설정이나 특성 값이 기록되는 부분입니다. iBeacon 헤더 정보라 생각하시면 됩니다. 우리가 손대지 않고 정해진 값을 사용해도 됩니다.
- UUID (16 byte) : 사실 iBeacon에서 가장 중요한 데이터는 UUID, Major, Minor 값입니다. 이 값을 추출한 뒤, 서버에 보내서 내가 어느 위치에 있는지, 이 비콘이 어떤 역할을 하는지, 그래서 사용자에게 어떤 정보를 보여주는 것이 좋은지 판단하게 됩니다. iBeacon 관련 공식 문서를 보면 주변에서 스캔한 UUID, Major, Minor 로 사용자의 위치를 판단하는 예시들을 언급하는데, iBeacon 장치가 위치기반 서비스의 일부임을 표시하기 위해 미리 정해둔 UUID 를 사용합니다. UUID는 꼭 표준에 지정된 값을 쓸 필요는 없으며, 서비스 개발자가 임의로 지정해서 사용해도 됩니다.
- Major (2 byte), Minor (2 byte) : UUID 와 함께 사용자의 위치(Major, Minor = 지역, 세부 장소)를 판별하는데 주로 사용됩니다. 하지만 사용방법이 고정된 것은 아니며, 개발자가 이 값들을 임의의 목적으로 사용할 수도 있습니다. 예를 들어, 온도와 습도 데이터를 Major, Minor 데이터로 보낼 수도 있습니다. 물론 이 경우 비콘을 스캔 하는 장치도 해당 데이터를 온도, 습도로 인식하도록 만들어야 합니다.
- TX Power(1 byte) : 비콘 장치가 신호를 송출할 때의 power 레벨을 여기에 적어 보내줍니다. 비콘 신호를 수신할 때 신호 세기를 알 수 있기 때문에 TX power 보다 얼마나 감소했는지를 계산하고, 대강의 거리를 짐작할 수 있습니다. 하지만 이렇게 계산된 거리는 대략적인 추정치입니다. 비콘 신호는 주변 상황이나 움직임, 장애물에 의한 변동이 심할 수 있습니다.


##### 6. iBeacon 서비스 종류
- 페블(pebBLE)형 : 매장 및 상점들을 겨냥한 소형 타입,  체크인 기능만 가능
- 마블(MarBLE)형 : 병원이나 공항에서 실내 네비게이션을 가능케 하는 타입, 체크인과 내비게이션 기능이 모두 탑재
- 님블(nimBLE)형 : 전시 솔루션이나 박물관에서 적용할 수 있는 타입, 체크인과 내비게이션 기능이 모두 탑재
- 트래블(treBLE)형 : 실외용 위치 솔루션을 위한 타입, 체크인과 내비게이션 기능이 모두 탑재



##### 7. iBeacon  활용방법 
- 비콘 신호를 활용하여 기획 할 수 있는 서비스는 다양 합니다. 실제 야구장의 좌석 안내 서비스, 미술관의 작품 해설 서비스, 쿠폰이나 매장 이벤트 알림 서비스, 결제 서비스, 미아 방지 서비스, 작업자 안전관리 서비스, 출입보안, 자산관리, 창고관리, 차량관제 등에 비콘이 사용되고 있습니다. 복잡한 연결 설정이나 코드 없이도 주변 비콘 장치들을 짧을 시간 안에 스캔하고 정보를 얻을 수 있는 비콘의 특징을 연구하여 획기적인 서비스로 구현해 보시기 바랍니다.


> 작성자 : 서버개발팀 과장 김대영

##### 출처 
-http://www.ibeacon.com/what-is-ibeacon-a-guide-to-beacons
-http://www.hardcopyworld.com/ngine/aduino/index.php/archives/3202
-http://www.isquery.com/wiki/lib/exe/fetch.php?media=beacon_.pdf
