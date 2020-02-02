---
title: Jedis Pool 최적화
date: 2020-02-02 14:34:05
thumbnail : /images/jedispool.png
tags: [java]
category : [IT Tech, 4. Java]
---

> 작성자 : 플랫폼 개발실 실장 김완철(David Kim)

[https://partners-intl.aliyun.com/help/doc-detail/98726.htm](https://partners-intl.aliyun.com/help/doc-detail/98726.htm) 에 있는 JedisPool optimization 영문 내용을 번역하였습니다. 

JedisPool 파라미터를 설정하면 Redis의 성능을 향상시키는 데 도움이 된다. 이 주제에서는 JedisPool을 사용하고 리소스 풀 파라미터를 구성하는 방법에 대해 설명한다. 또한 JedisPool을 최적화하기 위한 권장 파라미터 구성을 제공한다.

### JedisPool 사용법

---

2.9.0 버전을 사용하며, 메이븐 디펜던시는 다음과 같다.

```
<dependency> 
    <groupId>redis.clients</groupId> 
    <artifactId>jedis</artifactId> 
    <version>2.9.0</version>
    <scope>compile</scope> 
</dependency> 
```

Jedis는 Apache Commons-pool2를 사용하여 리소스 풀을 관리한다. JedisPool을 정의 할 때 GenericObjectPoolConfig 파라미터 (리소스  풀)에 주의해야 한다. 다음 예제는 이 파라미터를 사용하는 방법을 설명한다.

```
GenericObjectPoolConfig jedisPoolConfig = new GenericObjectPoolConfig();
jedisPoolConfig.setMaxTotal(...);
jedisPoolConfig.setMaxIdle(...);
jedisPoolConfig.setMinIdle(...);
jedisPoolConfig.setMaxWaitMillis(...) ;
...
```

JedisPool의 초기화는 다음과 같다. (번역 주, 일반적으로 Spring Framework의 경우 RedisTemplate 를 이용하여 구성한다)

```
// redisHost indicates the instance IP address. redisPort indicates the instance port. redisPassword indicates the password of the instance. The timeout parameter indicates both the connection timeout and the read/write timeout.
JedisPool jedisPool = new JedisPool(jedisPoolConfig, redisHost, redisPort, timeout, redisPassword);
//Run the command as follows:
Jedis jedis = null;
try {
    jedis = jedisPool.getResource();
    // Specific commands
    jedis.executeCommand()
} catch (Exception e) {
    logger.error(e.getMessage(), e);
} finally {
    //In JedisPool mode, the Jedis resource will be returned to the resource pool.
    if (jedis ! = null) 
        jedis.close();
}
```

### 파라미터 설명

---

Jedis 커넥션은 커넥션 풀에서 JedisPool이 관리하는 리소스이다. JedisPool은 스레드로부터 안전한 커넥션 풀이며 모든 리소스를 관리 가능한 범위 내에서 유지할 수 있다. GenericObjectPoolConfig 파라미터를 올바르게 구성하면 Redis 성능을 향상시키고 리소스 소비를 줄일 수 있다. 다음 두 표는 중요한 파리미터를 설명하고 파라미터 구성에 대한 권장 사항을 제공한다.

표 1. 리소스 설정 및 리소스 사용과 관련된 파라미터

|<center>파라미터</center>|<center>설명</center>|<center>기본값</center>|<center>추천</center>|
|:----:|-----|:------:|------|
|maxTotal|풀에서 할당 할 수 있는 최대 연결 수|<center>8</center>|아래의 주요 파라미터 구성에 대한 권장 사항에서 확인|
|maxIdle|추가 커넥션을 해제하지 않고 풀에서 유휴 상태로 유지할 수 있는 최대 연결 수.|<center>8</center>|아래의 주요 파라미터 구성에 대한 권장 사항에서 확인|
|minIdle|추가 커넥션을 만들지 않고 풀에서 유휴 상태로 유지할 수있는 최소 연결 수.|<center>0</center>|아래의 주요 파라미터 구성에 대한 권장 사항에서 확인|
|blockWhenExhausted|리소스 풀이 소진 될 때 호출자가 대기해야 하는지 여부. true 인 경우 maxWaitMillis 값이 적용된다.|<center>true</center>|기본값을 사용하는 것을 추천한다.|
|maxWaitMillis|사용 가능한 커넥션이 없을 때 호출자가 대기해야 하는 최대 시간 (밀리초)|<center>-1 (timeout 없음)</center>| 기본값은 권장하지 않는다.|
|testOnBorrow|풀에서 빌리기 전에 ping 명령을 사용하여 연결의 유효성을 검사할지 여부. 만약 커넥션이 유효하지 않은 것으로 판명되면 풀에서 제거된다.|<center>false</center>|비즈니스 트래픽이 많으면 ping 테스트 소비를 줄이기 위해 false로 설정하는 것이 좋다.|
|testOnReturn|풀로 리턴되기 전에 ping 명령을 사용하여 연결의 유효성을 검증할지 여부. 만약 커넥션이 유효하지 않은 것으로 판명되면 풀에서 제거된다.|<center>false</center>|비즈니스 트래픽이 많으면 ping 테스트 소비를 줄이기 위해 false로 설정하는 것이 좋다.|
|jmxEnabled|JMX 모니터링을 사용할지 여부.|<center>true</center>|JMX 모니터링을 사용하도록 권장한다. 사용하기 위해서는 애플리케이션에서도 활성화해야 한다|

idle (유휴) Jedis 객체 감지는 다음 네 가지 파라미터로 구성됩니다. testWhileIdle은 이 기능의 스위치이다.

표 2. idle 리소스 감지와 관련된 파라미터

|<center>파라미터</center>|<center>설명</center>|<center>기본값</center>|<center>추천</center>|
|:----:|---|:--------:|---|
|testWhileIdle|idle(유휴) 리소스 감지를 사용할지 여부.|<center>false</center>|<center>true</center>|
|timeBetweenEvictionRunsMillis|idle 객체 제거 스레드 실행 사이에서 휴면 시간 (밀리 초)|<center>-1 (실행되지 않음)</center>|이 파라미터를 설정하고 자체 사이클을 선택하는 것이 좋다. 또는 JedisPoolConfig 에서 구성할 수 있다.|
|minEvictableIdleTimeMillis|idle 객체 제거 스레드가 객체를 제거하기 전에 풀에서 idle 상태로 있을 수 있는 최소 시간|<center>180000 (30분)</center>| 기본값은 대부분의 경우에 적합하다. 비즈니스 시나리오에 따라 JeidsPoolConfig 에서 구성할 수 있다.|
|numTestsPerEvictionRun|idle 객체 제거 스레드가 실행될 때마다 검사할 객체 수. (있는 경우에만)|<center>3</center>|애플리케이션 커넥션 수에 따라 변경 될 수 있다. 이 값을 -1로 설정하면 모든 커넥션에서 idle 유효성 검사가 수행된다.|

사용자 편의를 위해 Jedis는 idle 커넥션의 유효성을 검사 할 때 GenericObjectPoolConfig와 일부 구성을 공유하는 JedisPoolConfig를 제공한다.


```
public class JedisPoolConfig extends GenericObjectPoolConfig {
  public JedisPoolConfig() {
    // defaults to make your life with connection pool easier :)
    setTestWhileIdle(true);
    setMinEvictableIdleTimeMillis(60000);
    setTimeBetweenEvictionRunsMillis(30000);
    setNumTestsPerEvictionRun(-1);
    }
}
```

> org.apache.commons.pool2.impl.BaseObjectPoolConfig에서 모든 기본값을 볼 수 있다.

### 주요 파라미터 구성에 대한 권장 사항

---

**maxTotal** : 최대 커넥션 수

maxTotal 을 올바르게 설정하려면 다음 요소를 고려해야 한다.

-   비즈니스에 필요한 Redis 동시 커넥션.
-   클라이언트가 명령을 실행하는 데 걸리는 시간
-   Redis 리소스의 한계. 예를 들어 maxTotal에 노드 (애플리케이션) 수를 곱한 결과는 Redis에서 허용되는 최대 커넥션 수보다 작아야 한다.
-   커넥션 생성 및 해제 비용. 각 요청에 대해 생성 및 해제된 커넥션 수가 많으면 생성 및 해제 프로세스시 막대한 비용이 소요된다.

리소스를 얻기 위해 명령을 실행하는 시간은 리소스를 빌리는 시간, 반환하는 시간, JedisPool이 명령을 실행하는 시간 및 네트워크 연결 시간으로 구성된다. 리소스를 얻기 위해 명령을 실행하는 데 걸리는 평균 시간이 약 1ms 인 경우 연결의 QPS(Queries Per Second-초당 쿼리스) 는 약 1,000이며 비즈니스에서 예상하는 QPS는 50,000 인 경우 이론적으로 필요한 풀 크기는 50 (50,000 / 1,000 = 50) 이다. 그러나 이것은 이론적인 수치일 뿐이다. 일부 리소스를 확보하기 위해 **maxTotal** 파라미터의 값은 이론적인 값보다 클 수 있다. 그러나 이 값이 너무 크면 연결에 너무 많은 클라이언트 및 서버 리소스가 사용된다. 반면, QPS가 높은 Redis와 같은 서버의 경우 명령 실행시 Blocking 이 있으면 큰 리소스 풀조차도 이 문제를 해결하는 데 도움이 되지 않는다.

**maxIdle and minIdle**

**maxIdle** 은 비즈니스에 필요한 실제 최대 커넥션 수이며, **maxTotal**은 idle 커넥션 수를 잉여로 포함한다. 로드량이 많은 시스템에서 **maxIdle** 을 너무 낮게 설정하면 요청을 처리하기 위해 new Jedis (추가 연결)가 생성된다. 따라서 **minIdle**은 풀에 보관해야 하는 최소 커넥션수를 지정하도록 구성된다.

**maxTotal** = **maxIdle** 인 경우 커넥션 풀이 최상의 성능에 도달한다. 이렇게 하면 커넥션 풀의 확장시에도 성능은 영향을 받지 않는다. 그러나 동시 커넥션 수가 작거나 **maxTotal** 파라미터가 너무 높게 설정되면 커넥션 리소스가 낭비된다.

실제 총 QPS 및 Redis를 호출하는 클라이언트 수를 기반으로 각 노드에서 사용하는 커넥션 풀 크기를 평가할 수 있다.

**최상의 파라미터 값을 찾으려면 모니터링 도구를 사용하자**

모니터링 도구를 사용하여 최상의 파라미터 값을 얻는 것이 보다 안정적인 방법이다. JMX 모니터링 또는 기타 모니터링 도구를 사용하여 최상의 파라미터 값를 찾을수 있다.

### FAQ

---

**부족한 리소스**

다음 두 경우에는 리소스 풀에서 리소스를 얻을 수 없다.

-   Timeout

```
redis.clients.jedis.exceptions.JedisConnectionException: Could not get a resource from the pool
…
Caused by: Java. util. nosuchelementexception timeout waiting for idle object
at org.apache.commons.pool2.impl.GenericObjectPool.borrowObject(GenericObjectPool.java:449)
```

-   **blockWhenExhausted**를 false로 설정하면, maxWaitMillis 로 지정된 시간이 사용되지 않으며, idle connection 이 사용 가능할 때까지 호출이 block 된다.

```
redis.clients.jedis.exceptions.JedisConnectionException: Could not get a resource from the pool
…
Caused by: java.util.NoSuchElementException: Pool exhausted
at org.apache.commons.pool2.impl.GenericObjectPool.borrowObject(GenericObjectPool.java:464)
```

풀 크기가 제한되어 있기 때문에 이 예외가 반드시 발생하는 것은 아닙니다. 자세한 정보는 **주요 파라미터 구성에 대한 권장 사항** 참조해라. 이 문제를 해결하려면 네트워크, 리소스 풀의 파라미터 구성, 리소스 풀 모니터링 (JMX 모니터링), 코드 (예 : jedis.close ()가 실행되지 않는 이유일 수 있음), 느린 쿼리 및 DNS 를 확인하는 것이 좋다.

**Warm up JedisPool**

작은 timeout 설정과 같은 몇 가지 이유로 프로젝트 초기 구동시 시간이 초과 될 수 있다. JedisPool은 최대 리소스 수와 최소 idle 리소스 수를 정의 할 때 커넥션 풀에 Jedis 커넥션을 만들지 않는다. 풀에 idle 커넥션이 없으면 new Jedis 커넥션이 생성된다. 이 커넥션은 사용 된 후에 풀로 반환된다. 그러나 새 커넥션을 만들고 매번 커넥션을 해제하려면 시간이 많이 걸린다. 따라서 JedisPool을 정의한 후 최소 idle 커넥션 수로 JedisPool을 예열(Warm up) 하는 것이 좋다. 예는 다음과 같다.

```
List<Jedis> minIdleJedisList = new ArrayList<Jedis>(jedisPoolConfig.getMinIdle()); 

for (int i = 0; i < jedisPoolConfig.getMinIdle(); i++) {
    Jedis jedis = null;
    try {
        jedis = pool.getResource();
        minIdleJedisList.add(jedis);
        jedis.ping();
    } catch (Exception e) {
        logger.error(e.getMessage(), e);
    } finally {
    }
}

for (int i = 0; i < jedisPoolConfig.getMinIdle(); i++) {
    Jedis jedis = null;
    try {
        jedis = minIdleJedisList.get(i);
        jedis.close();
    } catch (Exception e) {
        logger.error(e.getMessage(), e);
    } finally {
    
    }
}
```

원문 : [https://partners-intl.aliyun.com/help/doc-detail/98726.htm](https://partners-intl.aliyun.com/help/doc-detail/98726.htm)

참고 : [https://d2.naver.com/helloworld/5102792](https://d2.naver.com/helloworld/5102792)