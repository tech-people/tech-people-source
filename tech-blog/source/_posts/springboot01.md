---
title: 스프링부트 시작하기
date: 2020-01-20 20:00:00
thumbnail : /images/springboot/springboot-thumbnail.png
tags: [springBoot]
category : [IT Tech, 5. SpringBoot]
---

> 작성자 : 플랫폼 개발실 서버개발팀 김은혜

## 스프링부트
스프링 웹 어플리케이션을 만들기 위해서 서버 설치, config 설정 등의 여러 기본 설정들이 필요합니다. 또한, 템플릿 엔진의 버전과 Spring 버전의 호환성, 의존성을 확인하는 과정이 꼭 필요합니다.
스프링부트는 이러한 과정을 거치지 않고도 빠른 개발이 가능케 합니다. 내장된 웹 서버(Tomcat, Jetty)와 의존성을 손쉽게 관리할 수 있는 스타터 POM(Project Object Model), 설정의 표준화와 자동화 등을 지원하여 보다 쉽게 웹 애플리케이션 개발할 수 있도록 도와줍니다. 이를 바탕으로 스프링부트는 '스프링부트 애플리케이션의 대부분은 스프링 설정을 필요로 하지 않는다'라고 설명하고 있습니다.

이번 글은 스프링부트 프로젝트의 기본을 설정하면서 새롭게 알게된 정보들로 구성하여 작성했습니다.

## 스프링부트의 버전
스프링부트 1.0버전을 2014년 4월 1일 공개한 이후로 1.5.X버전은 2019년 8월 1일부로 지원을 중단했습니다. 현재는 2.2.X버전이 공개되어 있으며, 아래 예제는 최신 버전 2.2.X버전을 사용했습니다.
스프링부트 1.5.X 까지는 JDK 6과 7을 지원했지만 *2.2.X는 Java 8 이상이 필수*이며, Java 13까지 호환됩니다.
Build Tool로 Maven은 3.3+버전, Gradle은 5.X와 6.X을 지원합니다. Gradle은 4.10 또한 지원되지만 deprecated 되었습니다.
2.2.X버전에서 Tomcat은 9.0, Jetty는 9.4, Undertow는 2.0을 지원하고 있습니다.

## 개발환경
예제로 보일 프로젝트의 개발환경은 아래와 같습니다.
- OS : Window 10
- IntelliJ IDEA Ultimate
- SpringBoot 2.2.X
- JDK 8
- Gradle 6.0.1
- MySql 5.6.41

### 1. 프로젝트 생성하기
IntelliJ에서는 Spring Initiallizr로 스프링부트 프로젝트의 생성을 시작합니다.

![프로젝트 생성1](/images/springboot/springboot1.png)

Type은 Gradle Project로 생성했습니다. Packaging은 빠르고 간편하게 생성할 수 있는 Jar로 선택했습니다.
War는 기존과 같이 외부의 톰캣으로 배포하는 구조로 만들어지며, Jar로 로컬개발 및 테스트를 하다 나중에 배포할 때 War로 변경해서 배포할 수도 있습니다.

![프로젝트 생성2](/images/springboot/springboot2.png)

* eveloper Tools 의 Lombok
* Web의 Spring Web
* Template Engines의 Apache Freemarker
를 선택했습니다.

![프로젝트 생성3](/images/springboot/springboot3.png)

프로젝트의 이름과 위치를 지정한 후 Finish로 프로젝트를 생성합니다.

![프로젝트 생성4](/images/springboot/springboot4.png)

### 2. 프로젝트 구성

프로젝트가 생성되면 다음 파일들이 생성됩니다.
* 메인은 SpringWebApplication
* 프로퍼티파일은 application.properties
* 빌드는 build.gradle

![프로젝트 구성](/images/springboot/springboot5.png)

톰캣 관련한 설정들은 스프링부트가 구동될 때 내부 모듈에 의해서 자동설정 됩니다.

#### 1) SpringBootRestApplication.java

스프링부트의 메인 class 입니다.
```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;

@SpringBootApplication
public class DemoApplication {
    public static void main(String[] args) {
        SpringApplication.run(DemoApplication.class, args);
    }
}
```

스프링부트의 Auto Configuration 기능을 사용하기 위해선 @EnableAutoConfiguration 또는 @SpringBootApplication 중 하나를 사용해야 합니다.
@SpringBootApplication은 @EnableAutoConfiguration @ComponentScan @Configuration 를 포함하고 있습니다. 

* @EnableAutoConfiguration : 스프링부트의 Auto Configuration 을 사용할 수 있습니다. 의존성 라이브러리를 기반으로 사용 가능성이 높은 bean을 추측해 자동으로 등록합니다.
* @ComponentScan : 사용할 application의 패키지를 bean으로 찾아서 등록합니다. (스프링부트 문서가 추천하는 방법입니다.)
* @Configuration : bean을 추가 등록하거나 configuration을 추가 import 할 때 사용합니다.

데이터베이스 설정없이 구동하고 싶다면, 
@SpringBootApplication 을 
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class}) 로 대체하면 오류없이 구동이 가능합니다.
구동 후 http://localhost:8080 을 입력하면 에러 페이지가 나오는데 이는 설정된 페이지가 없을 뿐, 정상적으로 실행은 가능합니다.

간단하게 localhost 에서 페이지를 보고 싶다면 정적인 웹리소스를 관리하는 src/main/resources/static 안에 아래와 같은 index.html 을 하나 생성하고 구동하면 그 다음 아래와 같은 이미지를 볼 수 있습니다. 

static 폴더는 처음 프로젝트를 만들 때 Web 라이브러리를 선택했다면 자동으로 생성됩니다.

```html
<!DOCTYPEhtml>
    <htmllang="en">
    <head>
        <metacharset="UTF-8">
        <title>Title</title>
    </head>
    <body>
        Hello,SpringBoot!!
    </body>
</html>
```

![Hello, SpringBoot!](/images/springboot/springboot6.png)

#### 2) application.properties를 application.yml로

처음에는 빈 파일로 application.properties 파일이 생성됩니다.
생성된 파일에 먼저 freemarker의 경로를 설정했습니다.

```
spring.freemarker.template-loader-path=classpath:/templates/
spring.freemarker.suffix=.ftl
```

프로퍼티파일인 application.properties 파일은 아래와 같이 application.yml 파일로 변경해 사용할 수도 있습니다.
아래와 같이 가독성이 좋은 yml로 변경해서 설정을 변경해보았습니다.
/static/index.html 은 삭제해야만 localhost에서 /templates/index.ftl 이 실행이 됩니다.

```
spring:
  freemarker:
    template-loader-path: classpath:/templates/
    suffix: .ftl
```

yml 파일은 사람이 보기 편하며, profile은 하이픈(---)으로 나누어 환경에 따라 설정값을 다르게 가져갈 수 있는 장점이 있습니다. 
주의할 점은 Yaml 언어는 공백 하나에도 민감합니다. 하위 계층으로 내려갈 때 탭이 아닌 스페이스바를 사용하고, 콜론(:)이나 하이픈(-) 이후에도 공백 한 칸이 필요합니다.

#### 3) build.gradle

프로젝트 생성이 끝나면 빌드도구 Gradle의 설정파일이 아래와 같이 자동 설정됩니다. 

```
plugins {
    id 'org.springframework.boot' version '2.2.2.RELEASE'
    id 'io.spring.dependency-management' version '1.0.8.RELEASE'
    id 'java'
}
   
group = 'com.springboot'
version = '0.0.1-SNAPSHOT'
sourceCompatibility = '1.8'
   
configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}
 
dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-freemarker'
    implementation 'org.springframework.boot:spring-boot-starter-web'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation('org.springframework.boot:spring-boot-starter-test') {
        exclude group: 'org.junit.vintage', module: 'junit-vintage-engine'
    }
}
    
test {
    useJUnitPlatform()
}
```

* Repositories는 외부 jar파일들을 추가하기 위해 존재해야 하고, Repositories 정의는 default 값이 없으므로 꼭 정의가 필요합니다.
* mavenCentral() 저장소 또는 jcenter() 저장소를 사용할 수 있습니다. Url을 통해 원격으로 사용할 수도 있고, 로컬 저장소를 참조하여 사용할 수도 있습니다.
* dependencies는 필요한 라이브러리를 추가할 수 있으며, 버전도 같이 명시해 줄 수 있습니다.


#### _참고_
_`"Plugins 설정"`_
Plugins 설정은 buildscripts와 plugins 두 가지 방식으로 선언이 가능합니다.
아래와 같은 buildscripts 방식은 고전적인 방식이라고 설명되어 있습니다.

```
buildscript {
    def springBootVer = "2.0.6.RELEASE"
    repositories {
        mavenCentral()
    }
    dependencies {
        classpath "org.springframework.boot:spring-boot-gradle-plugin:$springBootVer"
    }
}
apply plugin : 
```
Gradle 공식 문서에 따르면 Plugins DLS은 플러그인 의존성을 선언하는데 간결하며 편리한 방법을 제공하며, 코어 및 커뮤니티 플러그인에 모두 쉽게 접근할 수 있다고 합니다.
gradle 4.6부터 적용되었고 다른 버전의 플러그인을 각각 지정하거나 전역으로 적용할지에 대한 여부 등을 선택할 수도 있습니다.
```
plugins {
            id 'org.springframework.boot' version '2.2.2.RELEASE'
            id 'io.spring.dependency-management' version '1.0.8.RELEASE'
            id 'java'
        }
```

_`"빌드도구 Maven와 Gradle의 차이"`_
빌드도구 Maven, Gradle은 라이브러리 의존성을 관리, 애플리케이션을 배포가능한 상태로 포장(Packaging or Archiving, 패키징 또는 아카이빙)하는 과정을 담당합니다.
- Maven은 XML을 사용하지만, Gradle은 Groovy 문법을 사용합니다.
   - XML을 사용함으로써 설정 내용이 길어지고 가독성이 복잡하지만, Gradle은 Tab으로 구분하여 스크립트 길이와 짧고 가독성이 좋습니다.
- Gradle은 상속구조를 이용한 멀티 모듈 구현이 쉽습니다.
- Maven보다 Gradle의 빌드 속도가 최대 100배 빠르다고 합니다.
   - Gradle Daemon은 메모리에 오래 살아있으며, 변경에 영향을 받는 것들만 재컴파일을 실행합니다.
   - Gradle은 캐시를 사용하기 때문에 테스트 반복 시 차이가 더 커집니다.

### 3. MySql 연동 + MyBatis 설정

MyBatis와 연결하기 위해서 build.gradle 에서 dependency를 추가해야 합니다.
*implementation 'org.springframework.boot:spring-boot-starter-web'* 하단에
*implementation 'org.mybatis.spring.boot:mybatis-spring-boot-starter:2.1.1'* 를 추가합니다.
compileOnly 하단에 *runtimeOnly 'mysql:mysql-connector-java'* 를 추가합니다.

추가 후 Gradle Toolbar를 열고 새로고침 아이콘에 마우스오버 하면 Reimport All Gradle Projects가 보입니다.
실행완료 후 External Libraries에 mysql-connector-java와 mybatis 라이브러리가 정상적으로 import 됐는지 확인하면 됩니다.

![Reimport Gradle, Libraries 확인](/images/springboot/springboot7.png)

application.yml 은 아래와 같이 datasource를 추가합니다. com.mysql.jdbc.Driver는 deprecated 되었기 때문에 com.mysql.cj.jdbc.Driver를 적용해야 합니다.
스프링부트 2.0부터 HikariCP가 기본으로 변경되었다고 하며, 저는 hikari를 덧붙여 이름을 정의해 MySql을 연동했습니다.

```
spring:
  freemarker:
    template-loader-path: classpath:/templates/
    suffix: .ftl
  datasource:
    hikari:
      driver-class-name: com.mysql.cj.jdbc.Driver
      jdbc-url: jdbc:mysql://localhost/test?serverTimezone=UTC
      username: scott
      password: scott
```

다음 DemoApplication.java 내부에 빈들을 정의했습니다.
먼저 @ConfigrationProperties를 사용해 yml에 설정한 내용을 DataSource로 가져온 후, sqlSessionFactory와 sqlSessionTemplate을 구현했습니다.
setMapperLocations한 경로 아래 즉, resources 안에 mapper 폴더를 생성하고 그 내부에 ..Mapper.xml 을 생성하면 됩니다.

```java
@Bean
@ConfigurationProperties(prefix = "spring.datasource.hikari")
public DataSource dataSource(){
    return DataSourceBuilder.create()
            .type(HikariDataSource.class)
            .build();
}

@Bean
public SqlSessionFactory sqlSessionFactory(DataSource dataSource, ApplicationContext applicationContext) throws Exception{
    SqlSessionFactoryBean sqlSessionFactoryBean = new SqlSessionFactoryBean();
    sqlSessionFactoryBean.setDataSource(dataSource);
    sqlSessionFactoryBean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mapper/**.xml"));
    return sqlSessionFactoryBean.getObject();
}

@Bean
public SqlSessionTemplate sqlSessionTemplate(SqlSessionFactory sqlSessionFactory){
    return new SqlSessionTemplate(sqlSessionFactory);
}    
```

MyBatis와 연동을 위해 사용할 VO로 java/com/springboot/web/demo 안에 user 폴더를 생성한 후 User.java를 생성했습니다.
Lombok을 사용하여 Getter, Setter를 만들어주었습니다.

```java
@Getter
@Setter
public class User {
    private int num;
    private String name;
    private String email;
}
```

예시로 사용할 select 쿼리를 포함한 UserMapper.xml을 생성했습니다.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="com.example.demo.user.UserMapper">
    <select id="findOne" parameterType="Integer" resultType="com.example.demo.user.User">
        SELECT
            num, name, email
        FROM
            user
        WHERE
            num = #{value}
    </select>
</mapper>
```

UserMapper.xml과 연결을 위해 java/com/springboot/web/demo/user 안에 UserMapper.java를 생성했습니다.
간단하게 @Mapper를 사용했으며, 연결을 위해 ...Mapper.xml 안의 id와 ...Mapper.java 메서드의 이름을 동일하게 설정해주었습니다.

```java
@Mapper
@Repository
public interface UserMapper {
    public User findOne(int num);
}
```

생성한 Mapper를 사용하기 위해 UserController를 생성했습니다.

```java
@Controller
public class UserController {

    @Autowired
    private UserMapper userMapper;
    
    @RequestMapping(value="/user")
    public String getUserList(Model model){
        model.addAttribute("user", userMapper.findOne(1));
        return "user";
    }
}
```

Mapping 될 뷰로 resources.templates 안에 user.ftl을 추가해주었습니다.

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    Hello, Spring Boot!! <br>
    Hello, FreeMarker!! <br>
    Name : ${user.name} <br>
    Email : ${user.email}
</body>
</html>
```

실행 후 브라우저에서 http://localhost:8080/user 을 띄우면 정상적으로 데이터가 보여지는 것을 확인할 수 있습니다.

![Hello, SpringBoot!!](/images/springboot/springboot8.png)

## 마무리
여기까지 스프링부트의 서버 설정 없이, 간단한 의존성 추가와 간단한 소스 구성으로 간단한 웹 프로젝트를 구축해 볼 수 있었습니다.

## References
* https://medium.com/@yongkyu.jang/%EC%8A%A4%ED%94%84%EB%A7%81%EB%B6%80%ED%8A%B8-springboot-%EA%B0%9C%EB%B0%9C%ED%99%98%EA%B2%BD-%EA%B5%AC%EC%84%B1-1-%EC%B2%AB%EA%B1%B8%EC%9D%8C-2aa01e808f62
* http://honeymon.io/tech/2019/06/17/spring-boot-2-start.html
* https://d2.naver.com/helloworld/5626759
* https://docs.spring.io/spring-boot/docs/2.2.2.RELEASE/reference/htmlsingle/#getting-started-introducing-spring-boot
* http://honeymon.io/tech/2019/06/17/spring-boot-2-start.html
* https://jojoldu.tistory.com/250
* https://jojoldu.tistory.com/296
* https://bkim.tistory.com/13
* https://jeong-pro.tistory.com/159
* https://waspro.tistory.com/504