---
title: Java refactoring 1부
date: 2019-12-24 14:34:05
thumbnail : /images/refactoring/ExtractMethod_intellij.png
tags: [java]
category : [IT Tech, 4. Java]
---

> 작성자 : 플랫폼 개발실 서버개발팀 유현선

# 리팩토링 입문 1부
리팩토링은 참신한 주장이 아니라 여러 설계자나 프로그래머의 경험을 정리한 것으로 이런 점에서 디자인 패턴과 유사합니다. 
그래서 다른 개발에 관련된 문서들을 찾아보면 리팩토링과 디자인패턴, TDD(Test Driven Development, 테스트 주도 개발)를 같은 내용으로 함께 다루기도 합니다.

리팩토링이라는것 자체가 좀 더 좋은 코드를 작성하기 위한 과정/방법의 하나인 만큼 너무나 당연한 얘기고 이미 코드를 작성하면서 따로 리팩토링이라는 이름을 붙이지 않고서도 이미 그렇게 작성하고 있는 내용들도 꽤 있었습니다. 
저도 <자바로 배우는 리팩토링 입문> 책을 읽으면서 '어? 이미 이렇게 하고있었는데' 싶었던 것도 있고 새롭게 알게된 점도 있었습니다. 
이미 알고있고 또 그렇게 해왔던 내용이어도 초심으로 돌아가 자신의 코드를 점검해보는 마음으로 재밌게 읽어봐주시면 좋겠습니다.  

<자바로 배우는 리팩토링 입문> 책을 읽고 작성하였습니다.

---

### 리팩토링이란
- 외부에서 본 프로그램의 동작은 바꾸지 않고
- 프로그램의 내부 구조를 개선하는 것
- 기능추가나 버그 수정 등은 리팩토링이라고 할 수 없음

#### 1. 리팩토링의 목적
- 버그를 발견하기 쉽게 만듬
- 기능을 추가하기 쉬워짐
- 코드 리뷰를 하기 용이함

#### 2. 리팩토링 시 주의해야 할 점
리팩토링을 진행하면서 실수 할 수 있고 놓치기 쉬운 점을 아래와 같이 정리해봤습니다.
##### (1) 유닛테스트
위에서 말했던 것처럼 리팩토링은 외부 동작이 변하지 않고 내부 구조만을 개선 시키는 것으로, 만약 리팩토링의 결과로 새로운 버그가 생기거나 동작이 기존의 명세와 달라진다면 그건 잘못된 리팩토링입니다.
그래서 정말로 동작이 변하지 않고 제대로 된 리팩토링이 되었는지 확인하기 위해서는 **유닛 테스트**가 중요합니다.

리팩토링 과정에서 유닛테스트를 하는 방법은 간단합니다.
1. 리팩토링 전에 테스트
2. 리팩토링 진행
3. 리팩토링 후에 테스트
 1번과 3번의 결과(외부 동작이)가 같다면 성공적으로 리팩토링을 완료한 것입니다.

##### (2) Step by Step
1. 여러개의 리팩토링을 동시에 진행하지 말 것 : 한번에 하나의 리팩토링만 진행 
2. 되돌리기 쉽게 하기 : 만약 리팩토링을 진행하다가 문제가 생기거나 하는 경우가 반드시 존재할 수 있으므로 이를 반드시 고려
3. 단계마다 확인 : 작업 단계마다 제대로 수정이 되고 있는건지 확인
4. 오래된 걸 새로운 걸로 바꿈  

#### 3. 리팩토링 카탈로그
리팩토링들의 목적과 절차를 카탈로그 형식으로 정리한 것을 리팩토링 카탈로그라고 부릅니다. 
책에서 소개된 <http://www.refactoring.com/> 라는 사이트가 있는데 많은 종류의 리팩토링들이 (영어지만) 수식과 함께 설명되어 있어 이해가 어렵지 않아 심심하실때 한번씩 들어가서 구경해보시는 것을 추천드립니다. 

---

### 매직 넘버를 기호 상수로 치환
코드 내 매직넘버(특정 의미를 갖는 숫자값)-흔히 우리가 말하는 '하드코딩' 된 수치값-을 상징이 되는 이름을 써서 상수로 선언하여 치환시키는 방법입니다. 

#### 1. 리팩토링 카탈로그
![매직 넘버를 기호 상수로 치환 - 리팩토링 카탈로그](/images/refactoring/MagicNumber_catalog.PNG)
<br>
[참고:Refactoring catalog](https://www.refactoring.com/catalog/replaceMagicLiteral.html)

#### 1. 예제
아래는 실제 개발했던 코드를 예제에 맞게 간략화하여 수정, 가공한 내용입니다.<br> 
##### (1) 리팩토링 전
```java
private void doSituationOccurrenceSend(Patient patient) {
    SituationOccurrenceDetail sod = new SituationOccurrenceDetail();
    sod.setSittnClCode("SITUATION_CODE_09");
    sod.setReferenceNum(patient.getPatntNum());
    sod.setReferenceType("patient");
    sod.setReferenceValue(patient.getPatntName());
    sod.setTrgterNum(patient.getPatntNum());
    sod.setTrgterType("patient");
    sod.setTrgterName(patient.getPatntName());

    occurenceController.situationOccurrence(sod);
}
```

새로운 환자 정보가 등록되었을때 정보를 가공하여 '상황발생이력 등록` occurenceController.situationOccurrence(sod);`'을 처리하는 method 입니다.
여기서 `sod.setReferenceType("patient");` `sod.setTrgterType("patient");` 이 부분을 보면 환자(patient)라는 구분값이 일반 string 값으로 하드코딩 되어있습니다. 
`sod.setSittnClCode("SITUATION_CODE_09");` 이 부분 또한 특정한 코드값을 의미하는 string 값으로 따로 보면 그 의미를 알기 힘듭니다.

##### (2) 리팩토링 후
```java

private static final String PATIENT_REGIST = "SITUATION_CODE_09";
private static final String PATIENT = "patient";

private void doSituationOccurrenceSend(Patient patient) {
    SituationOccurrenceDetail sod = new SituationOccurrenceDetail();
    sod.setSituationCode(PATIENT_REGIST);
    sod.setReferenceNum(patient.getPatntNum());
    sod.setReferenceType(PATIENT);
    sod.setReferenceValue(patient.getPatntName());
    sod.setTrgterNum(patient.getPatntNum());
    sod.setTrgterType(PATIENT);
    sod.setTrgterName(patient.getPatntName());

    occurenceController.situationOccurrence(sod);
}
```

#### 2. 분류 코드를 클래스로 치환하기 : enum class 사용
단순히 계산값이나 URL 같은 하드코딩 값은 상수로 선언하여 사용하면 되지만 분류코드(type code)는 enum 등을 활용하여 class로 치환하여 사용하면 좋습니다.
예를 들어 내부 로직에 위해 계산된 결과에 따라 상태값을 지정해주거나 할 때 이 상태값을 enum 클래스로 생성하여 관리하면 어떤 상태값들이 있는지 확인하고 수정, 추가하기가 쉽습니다. 

(1) 예제
아래는 센서 상태값을 처리하는 enum 클래스를 만들어 관리를 쉽게 만든 내용입니다.
```java
public enum SensorStatusType implements EnumValue<String> {

    NONE(""),
    OUTOFRANGE("OUTOFRANGE"),
    CRITICAL("CRITICAL"),
    WARNING("WARNING"),
    NORMAL("NORMAL");

    SensorStatusType(String value) {
        this.value = value;
    }

    private String value;

    @Override
    public String getValue() {
        return value;
    }
}
```

---

### 메소드 추출
하나의 method 는 그 이름에 맞는 하나의 기능만 처리하는 것이 좋습니다. 
<br>만약 한 메소드 안에 이러저런 세세한 처리가 많다면 그런 처리를 묶어서 나누고 독립된 메소드로 추출하는 것이 메소드 추출 리팩토링입니다.
저 같은 경우는 어떤 기능을 개발 할 때 
1. 처리해야 할 내용을 순서대로 작성 
2. 처리 내용에 따라 보통 Test 코드로 하나의 method 에 기능을 전부 작성
3. 테스트로 그 기능이 정상적으로 작동하는지 확인
4. 정상적으로 작동하는 코드를 기능별로 method 로 분리
<br>

하는 과정을 거치는데 여기서 4번이 이번에 서술할 메소드 추출 리팩토링에 속한다고 볼 수 있을것 같습니다.


#### 1. 리팩토링 카탈로그
![메소드 추출 - 리팩토링 카탈로그](/images/refactoring/ExtractMethod_catalog.PNG)
<br>
[참고:Refactoring catalog](https://refactoring.com/catalog/extractFunction.html)


#### 2. 예제
IntelliJ IDEA는 리팩토링에 강력한 여러가지 기능들을 제공해주고 있습니다. 여기서 그 중 하나인 메소드 추출 기능을 예제와 함께 설명하겠습니다. 

(1) 리팩토링 전<br>
 아래 코드는 하루동안의 입실/퇴실 목록을 조회하는 JSON 으로 각각 가져오는 Query 는 다르지만 넘겨주는 parameter 값이 중복되고 있습니다. 
```java
@RequestMapping(value = "/dashboard/inList.json", method = RequestMethod.POST)
@ResponseBody
public Object getTodayFloorInTargetList() {
    FloorLogSearchParam logSearchParam = new FloorLogSearchParam();
    TimeZone loginTimeZone = TimeZone.getTimeZone(Security.getLoginDetail().getTimeZone());
    logSearchParam.setComNum(Security.getLoginDetail().getCompanyNumber());    
    logSearchParam.setStartDate((int) DateUtil.str2timestamp(DateUtil.getDate("yyyy-MM-dd 00:00:00"), "yyyy-MM-dd HH:mm:ss", loginTimeZone));
    logSearchParam.setEndDate((int) DateUtil.str2timestamp(DateUtil.getDate("yyyy-MM-dd 23:59:59"), "yyyy-MM-dd HH:mm:ss", loginTimeZone));

    List<?> res = presenceFloorService.getTodayFloorInList(logSearchParam);
    return res;
}

@RequestMapping(value = "/dashboard/outList.json", method = RequestMethod.POST)
@ResponseBody
public Object getTodayFloorOutTargetList() {
    FloorLogSearchParam logSearchParam = new FloorLogSearchParam();
    TimeZone loginTimeZone = TimeZone.getTimeZone(Security.getLoginDetail().getTimeZone());
    logSearchParam.setComNum(Security.getLoginDetail().getCompanyNumber());
    logSearchParam.setStartDate((int) DateUtil.str2timestamp(DateUtil.getDate("yyyy-MM-dd 00:00:00"), "yyyy-MM-dd HH:mm:ss", loginTimeZone));
    logSearchParam.setEndDate((int) DateUtil.str2timestamp(DateUtil.getDate("yyyy-MM-dd 23:59:59"), "yyyy-MM-dd HH:mm:ss", loginTimeZone));

    List<?> res = presenceFloorService.getTodayFloorOutList(logSearchParam);
    return res;
}
```

(2) 리팩토링 후<br>
중복된 검색Parameter 설정을 method 로 분리
```java
@RequestMapping(value = "/dashboard/inList.json", method = RequestMethod.POST)
@ResponseBody
public Object getTodayFloorInTargetList() {
    FloorLogSearchParam logSearchParam = setSearchParam();
    List<?> res = presenceFloorService.getTodayFloorInList(logSearchParam);
    return res;
}

@RequestMapping(value = "/dashboard/outList.json", method = RequestMethod.POST)
@ResponseBody
public Object getTodayFloorOutTargetList() {
    FloorLogSearchParam logSearchParam = setSearchParam();
    List<?> res = presenceFloorService.getTodayFloorOutList(logSearchParam);
    return res;
}

private FloorLogSearchParam setSearchParam() {
    FloorLogSearchParam logSearchParam = new FloorLogSearchParam();
    TimeZone loginTimeZone = TimeZone.getTimeZone(Security.getLoginDetail().getTimeZone());
    logSearchParam.setStartDate((int) DateUtil.str2timestamp(DateUtil.getDate("yyyy-MM-dd 00:00:00"), "yyyy-MM-dd HH:mm:ss", loginTimeZone));
    logSearchParam.setEndDate((int) DateUtil.str2timestamp(DateUtil.getDate("yyyy-MM-dd 23:59:59"), "yyyy-MM-dd HH:mm:ss", loginTimeZone));
    logSearchParam.setComNum(Security.getLoginDetail().getCompanyNumber());
    return logSearchParam;
}
```

#### 3. IntelliJ의 Refactor > Extract Method 기능
IntelliJ IDEA는 리팩토링에 강력한 여러가지 기능들을 제공해주고 있습니다. 
여기서 그 중 하나인 메소드 추출 기능을 소개해드리겠습니다.
참고로 IntelliJ IDEA 는 프로젝트 안에서 소스코드의 내용이 중복되면 노란색 밑줄로 경고표시를 해줘서 동일한 코드가 중복으로 작성되는 일을 방지할 수 있습니다.(설정에서 변경 가능)

1. 추출할 메소드를 블럭
2. 오른쪽 마우스 클릭 > Refactor > Extract Method (단축키 : Ctrl + Alt + M)<br>
![IntelliJ의 Extract method 기능](/images/refactoring/ExtractMethod_intellij.png)
3. 자동으로 선택된 코드가 method 로 분리됨
4. 선택했던 영역 외에도 중복으로 코드가 작성된 부분이 있다면 자동으로 생성된 메소드로 대체

---

##### 출처
  - 책 : 자바로 배우는 리팩토링 입문
