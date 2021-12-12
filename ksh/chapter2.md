
### 2장 JPA 시작

2장에서는 일단 개발을 위한 세팅작업이 대다수이다. 
책에서는 maven, xml 등을 사용하는데 나는 gradle, yaml 등을 사용하려고 한다. 그래서 이번 장에서는 책 내용 정리보다는 실제로 코드를 작성하고 실습해본 과정을 정리하려고 한다.

2장의 핵심 목차는 아래와 같다.

- 필요한 프로그램 설치 및 세팅 과정
- 객체 매핑 시작
- persistence.xml 설정
- 애플리케이션 개발

---

### 필요한 프로그램 설치 및 세팅 과정

#### H2 데이터베이스 설치

다운로드 링크 : https://www.h2database.com/html/download.html

설치 후에 `~사용자/H2/bin` 으로 들어가서 `h2.bat` 을 실행하면 h2 데이터베이스 콘솔에 접속할 수 있다.
또는 http://localhost:8082/ 로 접속하면 된다. 

---

### 프로젝트 세팅
- build.gradle
```
plugins {
    id 'org.springframework.boot' version '2.5.3'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
}

group = 'jpastudy'
version = '1.0-SNAPSHOT'
sourceCompatibility = '1.8'

configurations {
    compileOnly {
        extendsFrom annotationProcessor
    }
}

repositories {
    mavenCentral()
}

dependencies {
    implementation 'org.springframework.boot:spring-boot-starter-data-jpa'
    compileOnly 'org.projectlombok:lombok'
    annotationProcessor 'org.projectlombok:lombok'
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
    runtimeOnly 'com.h2database:h2'
}

test {
    useJUnitPlatform()
}

```

- application.yml

```

spring:
  application:
    name: jpastudy
  datasource:
    url: jdbc:h2:tcp://localhost/~/test
    username: sa
    password:
    driver-class-name: org.h2.Driver
  jpa:
    hibernate:
      ddl-auto: create-drop
    properties:
      hibernate:
        show_sql: false
        format_sql: true


logging:
  level:
    jdbc.sqlonly: debug
```

---

### entity

- `Member` 객체를 만들어보겠다.

아래 코드에서 사용된 어노테이션들을 간단히 정리해보자면,
- `@Entity` : 이 클래스를 테이블과 매핑한다고 JPA에 알려준다.
- `@Table` : 엔티티 클래스에 매핑할 테이블 정보를 알려준다.
            이 어노테이션을 생략하면 클래스 이름을 테이블 이름으로 매핑한다. (정확히는 엔티티 이름을 사용.)
- `@Id` : 엔티티 클래스의 필드를 테이블의 기본키(`primary key`)에 매핑한다.
- `@Column` : 필드를 컬럼에 매핑한다. 매핑정보가 없는 필드의 경우 필드명을 사용해서 컬럼명으로 매핑한다. 그런데 **대소문자를 구분하는 DB를 사용할 경우 `@Column` 어노테이션을 사용해서 명시적으로 매핑해야 한다.**
  

#### 기본키 매핑 (4.6장 참고)
JPA는 크게 직접할당과 자동생성으로 기본키를 매핑한다. 위에서 본 어노테이션 중에서 `@Id`를 사용하면 직접 할당으로 매핑되는 것이다. `@GeneratedValue`는 기본키를 **자동생성**해주는 어노테이션이다.
-  `@GeneratedValue` : 자동생성. **대리키** 사용 방식.
   자동생성 전략이 아래와 같이 다양한 이유는 데이터베이스 벤더마다 지원하는 방식이 다르기 때문이다. 예를 들어서 MySQL은 시퀀스를 제공하지 않는다. 대신에 `AUTO_INCREMENT` 기능을 제공한다.
    - **strategy**
        - `IDENTITY` : 기본키 생성을 DB에 위임한다. MySQL의 경우 `AUTO_INCREMENT`를 사용하여 기본키를 생성한다. 이 `IDENTITY`전략은 데이터를 데이터베이스에 INSERT한 후에 기본키 값을 조회할 수 있다. 
        - `SEQUENCE` : DB의 특별한 시퀀스 오브젝트 사용하여 기본키를 생성한다. 오라클, PostgreSQL, H2 데이터베이스에서 사용할 수 있다. MYSQL은 시퀀스를 지원하지 않는다. 주의할 점은 `@SequenceGenerator`에서 `allocationSize`의 **default값이 50**이라는 점이다. 이는 성능최적화 때문에 그렇다는데 나중에 다시 해당 챕터를 공부할 때 자세히 살펴보도록 하겠다.
        - `TABLE` : 데이터베이스에 키 생성 전용 테이블을 하나 만들고 이를 사용하여 기본키를 생성한다. 그러므로 모든 DB에 적용이 가능하다.
        - `AUTO`(default) : JPA구현체가 자동으로 생성 전략을 결정한다. 데이터베이스의 종류는 다양하고 기본키를 만드는 방법도 다양하기 때문에 선택한 DB에 따라 위 3가지 전략 중 자동으로 하나를 선택하여 적용한다. 
    
아래 코드에서 첨에 그냥 `strategy`를 `AUTO`로 잡으니까 에러가 났다. 그 이유는 h2 DB를 사용시 기본전략으로 `SEQUENCE`를 사용하기 때문이다. 그러므로 id를 `String`값으로 매핑하려고 하여 에러가 발생했던 것이다. id를 `int`로 바꿔도 되고, `String`으로 저장할 수 있는 방식으로 바꿔도 된다. 나의 경우는 `uuid`로 생성하게 하여 `String`으로 저장하였다.


```java
package example.entity;

import lombok.AllArgsConstructor;
import lombok.Getter;
import lombok.NoArgsConstructor;
import lombok.Setter;
import org.hibernate.annotations.GenericGenerator;

import javax.persistence.*;

@Getter
@Setter
@NoArgsConstructor
@Entity
@Table(name = "MEMBER")
public class Member {
    //
    @Id
    @GeneratedValue(generator = "USER_GENERATOR")
    @GenericGenerator(name = "USER_GENERATOR", strategy = "uuid")
    private String id;
    @Column(name = "NAME")
    private String username;
    private Integer age;

    public Member(String username, Integer age) {
        this.username = username;
        this.age = age;
    }
}

```

- Member 생성 테스트

```java
package example;

import example.entity.Member;
import example.logic.MemberLogic;
import org.junit.jupiter.api.Test;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.test.context.SpringBootTest;
import org.springframework.test.context.ContextConfiguration;

import static org.junit.jupiter.api.Assertions.assertEquals;


@SpringBootTest(classes = JpaStudyApplicationTest.class)
class MemberLogicTest {
    //
    @Autowired
    private MemberLogic memberLogic;
    
    @Test
    void save() {
        //
        Member member = new Member("홍길동", 30);
        String id = memberLogic.save(member);

        Member found = memberLogic.find(id);

        assertEquals(member.getUsername(), found.getUsername());
    }
}

```

---

### 참고자료
- https://songii00.github.io/2020/03/25/2020-03-25-@GeneratedValue(strategy%20=%20GenerationType.IDENTITY)%EA%B8%B0%EB%B3%B8%ED%82%A4%20%EC%98%81%EC%86%8D%EC%84%B1%20%EA%B4%80%EB%A6%AC/
- https://codingexplore.tistory.com/m/69
- https://hyeonic.tistory.com/m/196