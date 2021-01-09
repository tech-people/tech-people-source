---
title: InfluxDB-01 [살펴보기]
date: 2021-10-09 14:56:55
thumbnail : /images/influxDB.png
tags: [InfluxDB]
category : [IT Tech, InfluxDB]
---

> 작성자 : 플랫폼 개발실 R&D팀 유주빈

InfluxDB는 시시각각 수집되는 데이터를 시간 순서로 저장을 하고 조회할 수 있는 시계열 데이터 베이스 입니다. 이는 Go 언어로 작성이 되었고, 2013년 9월에 출시가 되었습니다. 또한 SQL 계열 언어를 제공하기에 기존에 Oracle, Mysql을 사용하는 사용자들에게 더욱 친숙하게 기능을 사용할 수 있습니다.

---

### 1. 컨셉

#### 1.1 데이터 구성 요소
InfluxDB는 개인적으로 사용해보면서 RDB와 유사한 느낌을 많이 받습니다. 그러나 쓰이는 용어 및 개념에서 차이가 존재하기 때문에 데이터의 구성요소에 대해 살펴보겠습니다.

- Database
`
다수의 Measurement 를 포함하고 있는 개념이며, RDB의 Database와 동일한 개념으로 이해할 수 있습니다.
`
- Measurement
`
Measurement는 Tag 및 Field, Timestamp 관련된 정보들을 포함하고 있는 구조입니다. RDB에서 테이블로 이해를 할 수 있습니다.
`
    - ex) Measurement 예시
    |Tag Key|Field Key|Time|
    |:----:|:---:|:---:|
    |Tag Value A|Field Value A|1610176701000000000|
    |Tag Value B|Field Value A|1610176702000000000|
    |Tag Value A|Field Value B|1610176703000000000|

- Timestamp
`
InfluxDB에 저장되는 모든 데이터들은 timestamp를 저장하고 있는 time이라는 칼럼을 갖고 있으며 나노(10의 -9승) 초까지의 정밀도로 저장이 됩니다. 기본적은 Tag Key 입니다.
`
- Field
    - Field Key
    `
    Field Key는 문자열이며 Field에 대한 이름을 표현합니다. RDB에서 인덱스가 걸려 있지 않은 칼럼으로 이해할 수 있습니다.
    `
    - Field Value
    `
    Field에 해당하는 값을 의미하며 String, Float, Integer, 또는 Boolean 형으로 데이터를 저장할 수 있습니다.
    `
    - Field Set
    `
    Field Key, Field Value 쌍과 Timestamp 정보를 포함한 Set을 의미합니다.
    `
- Tag
    - Tag Key
    `
    Tag Key는 문자열이며 Tag에 대한 이름을 표현합니다. RDB에서 인덱스가 걸려 있거나 PK를 의미하는 칼럼으로 이해할 수 있습니다.
    `
    - Tag Value
    `
    Tag에 해당하는 값을 의미하며 String 형으로 데이터를 저장할 수 있습니다.
    `
    - Tag Set
    `
    Tag Key, Tag Value 쌍으로 이루어진 Set을 의미합니다.
    `
- Point
`
Point는 Series Key와 Field Value 및 Timestamp를 포함하고 있습니다.
`
- Series
    - Series Key
    `
    Series Key는 Point에서 공유되는 Measurement와 Tag Set, Field Key를 포함하고 있습니다.
    `
- Bucket
`
모든 InfluxDB 데이터는 Bucket에 저장이 됩니다. Bucket은 Database와 각 Point들의 보관 시간에 대한 설정 정보인 Retention Policy(보관 정책) 개념이 합쳐진 개념입니다.
`
- Organization
`
Organization은 InfluxDB를 사용하는 사용자 그룹을 위한 작업공간입니다. 모든 Bucket과 사용자는 Organization에 속하게 됩니다.
`

#### 1.2 설계 원칙
InfluxDB의 데이터 요소들은 효율적으로 압축된 데이터로 저장하기 위해 TSM(time-structured merge tree), TSI(time series index) 파일로 저장됩니다.

- 데이터의 시간 정렬
`
time을 기준으로 오름차순으로 정렬됩니다.
`
- 엄격한 업데이트 및 삭제 권한
`
InfluxDB는 쿼리 및 쓰기 성능일 높이기 위해 업데이트 및 삭제 권한을 엄격하게 제한합니다.
`
- 우선적인 읽기, 쓰기 쿼리 처리
`
InfluxDB는 데이터에 일관성보다 읽기 및 쓰기에 대한 우선순위를 더 높게 생각합니다. 즉, InfluxDB에 많은 쓰기가 발생하는 중에 읽기 쿼리를 할 경우 최신 데이터가 포함되지 않을 수 있습니다.
`
- 중복 데이터
`
데이터에 대한 충돌을 단순히 해결하고 쓰기 성능을 높이기 위해 InfluxDB는 중복된 Tag Key들을 갖고 있는 정보에 대해서는 가장 최근의 Field Value로 업데이트 합니다.
`
- 기타
`
현재 사용해본 경험으로는 데이터를 삭제할 경우 조건으로 Field Key는 조건으로 줄 수 없으며 Tag Key만 가능하다는 것을 확인하였습니다.
`

### 2. 엔진
InfluxDB는 HTTP POST 프로토콜과 같은 쓰기 API를 요청이 수신 된 시점부터 데이터를 서버의 물리적인 디스크에 기록합니다. 이때, 내구성을 위해 일괄적인 Point들이 압축되고 WAL 에 저장됩니다. 또한 Point는 즉각적으로 쿼리할 수 있도록 캐시화 됩니다. 이렇게 캐시 정보는 주기적으로 TSM 파일로 기록됩니다.

- WAL(Write Ahead Log)
`
WAL 은 InfluxDB 스토리지 엔진이 다시 시작될 때 데이터를 유지할 수 있게 해줍니다. 이러한 WAL은 예기치 않은 오류 발생시 데이터의 내구성을 보장해줍니다. 스토리지 엔진이 쓰기 요청을 받았을 경우 아래와 같은 순서로 작동됩니다.
`
    - 쓰기 요청은 WAL 파일 마지막에 추가됩니다.
    - 데이터가 디스크에 저장됩니다.
    - 캐시 메모리를 업데이트 합니다.
    - 정상적으로 디스크에 데이터가 저장이 되었다면 성공을 결과로 반환합니다.
- Cache
`
Cache는 현재 WAL 저장된 데이터 Point들에 대한 복사본입니다. 이러한 Cache는 아래와 같은 특징을 같습니다.
`
    - Point들의 Measurement, Tag set, Unique field로 구성되어 있습니다.
    - 압축이 되지 않은 데이터 입니다.
    - 스토리지 엔진이 다시 시작될 때, WAL에서 업데이트 정보를 갖고 갑니다. 이러한 캐시는 런타임시 조회되고 TSM 파일에 저장된 데이터와 병합됩니다.
- Time-Structured Merge Tree (TSM)
`
데이터를 효율적으로 압축하고 저장하기 위해 스토리지 엔진은 Series Key별로 필드 값을 그룹화 한 다음 해당 필드 값을 시간별로 정렬합니다.
` 
- Time Series Index (TSI)
`
데이터 카디널리티 (시리즈 수)가 증가함에 따라 쿼리는 더 많은 Series Key를 읽고 느려집니다. TSI는 데이터 카디널리티가 증가해도 쿼리가 빠르게 유지되도록합니다
`

# References
- https://docs.influxdata.com/influxdb/v2.0/reference/key-concepts
- https://docs.influxdata.com/influxdb/v2.0/reference/internals
