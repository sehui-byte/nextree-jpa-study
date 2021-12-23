

## Chapter4. 엔티티 매핑

이번 챕터는 책을 읽어보니까 너무 꼼꼼하게 적혀있어서 모두 다 정리하기는 힘들 것 같고, 일단 스스로 중요도를 판별하여 핵심만 정리하려고 한다.



### @Entity

- JPA를 사용해서 테이블과 매핑할 클래스는 `@Entity` 어노테이션을 필수로 붙여야 한다.
- name : JPA에서 사용할 엔티티 이름을 지정한다. 보통 기본값인 클래스명을 사용하는데 만약 다른 패키지에 이름이 같은 엔티티 클래스가 있다면, 이름을 지정해서 충돌하지 않도록 해야 한다. => `@Entity(name = "Member")

- **기본 생성자는 필수이다. JPA가 엔티티 객체를 생성할 때 기본 생성자를 사용하기 때문**이다.

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



여러가지 매핑 어노테이션들이 있는데 추후 더 자세히 살펴보도록 하겠다.

ex) `@Emurated`,  `@Temporal` , `@Lob` , `@Transient` , `@Access` ....



---



### 데이터베이스 스키마 자동생성

- **JPA는 DB스키마를 자동생성하는 기능을 지원**한다. => `ddl-auto` 속성 사용.

  - 운영서버에서는 절대 `ddl-auto`를 사용해서는 안된다!!! 
  - ddl-auto 속성의 옵션은 5개 가 있다.
    - **create** : 기존꺼 삭제하고 새로 생성
    - **create-drop** : create + 종료시 삭제
    - **update** : 변경사항만 수정
    - **validate** : DB테이블과 엔티티 매핑간의 차이가 있으면 경고 남기고 어플리케이션을 실행X
    - **none** : 자동생성기능을 사용X

- `hibernate.properties.show_sql` 을 true로 설정하면 콘솔에서 실행되는 DDL문을 확인할 수 있다.

  ```yaml
  
  spring:
    application:
      name: jpastudy
    datasource:
      url: jdbc:h2:tcp://localhost/~/testdb
      username: sa
      password: password
      driver-class-name: org.h2.Driver
    jpa:
      hibernate:
        ddl-auto: update
      properties:
        hibernate:
          dialect: org.hibernate.dialect.H2Dialect
          show_sql: false #true
          format_sql: true
  ```

  

---



### 기본키 매핑

기본키매핑에 관해서는 지난번에 chapter2에서 이미 다뤘기 때문에 아래 지난 내용을 인용한 후에 추가적인 부분만 다뤄보려고 한다.

> #### 기본키 매핑 (4.6장 참고)
>
> JPA는 크게 직접할당과 자동생성으로 기본키를 매핑한다. 위에서 본 어노테이션 중에서 `@Id`를 사용하면 직접 할당으로 매핑되는 것이다. `@GeneratedValue`는 기본키를 **자동생성**해주는 어노테이션이다.
>
> - `@GeneratedValue` : 자동생성. **대리키** 사용 방식.
>   자동생성 전략이 아래와 같이 다양한 이유는 데이터베이스 벤더마다 지원하는 방식이 다르기 때문이다. 예를 들어서 MySQL은 시퀀스를 제공하지 않는다. 대신에 `AUTO_INCREMENT` 기능을 제공한다.
>
>    - **strategy**
>      - `IDENTITY` : 기본키 생성을 DB에 위임한다. MySQL의 경우 `AUTO_INCREMENT`를 사용하여 기본키를 생성한다. 이 `IDENTITY`전략은 데이터를 데이터베이스에 INSERT한 후에 기본키 값을 조회할 수 있다. 
>      - `SEQUENCE` : DB의 특별한 시퀀스 오브젝트 사용하여 기본키를 생성한다. 오라클, PostgreSQL, H2 데이터베이스에서 사용할 수 있다. MYSQL은 시퀀스를 지원하지 않는다. 주의할 점은 `@SequenceGenerator`에서 `allocationSize`의 **default값이 50**이라는 점이다. 이는 성능최적화 때문에 그렇다는데 나중에 다시 해당 챕터를 공부할 때 자세히 살펴보도록 하겠다. *(CHAPTER4 확인)*
>      - `TABLE` : 데이터베이스에 키 생성 전용 테이블을 하나 만들고 이를 사용하여 기본키를 생성한다. 그러므로 모든 DB에 적용이 가능하다.
>      - `AUTO`(default) : JPA구현체가 자동으로 생성 전략을 결정한다. 데이터베이스의 종류는 다양하고 기본키를 만드는 방법도 다양하기 때문에 선택한 DB에 따라 위 3가지 전략 중 자동으로 하나를 선택하여 적용한다. 
>
>   ```java
>      @Id
>      @GeneratedValue(generator = "USER_GENERATOR")
>      @GenericGenerator(name = "USER_GENERATOR", strategy = "uuid")
>      private String id;
>   ```
>
>   



#### `Identity` 전략

- identity전략은 기본키 생성을 DB에 위임하기 때문에, 엔티티에 식별자값을 부여하기 위해서 **DB에 엔티티를 insert한 후에 다시 조회해서 식별자값을 할당**해줘야 한다. <u>hibernate는 JDBC3에 추가된 `Statement.getGeneratedKeys()`를 사용하여 데이터를 저장하면서 동시에 생성된 기본키를 가져와서 DB와 한번만 통신할 수 있도록</u> 한다.

- 때문에 "**쓰기지연 전략(transactional write-behind)"는 동작하지 않는다**. 즉, `transaction.commit()` 호출시가 아니라 `entityManager.persist()`를 호출했을 때 바로 DB에 insert쿼리를 날린다.

- 참고자료 : https://songii00.github.io/2020/03/25/2020-03-25-@GeneratedValue(strategy%20=%20GenerationType.IDENTITY)%EA%B8%B0%EB%B3%B8%ED%82%A4%20%EC%98%81%EC%86%8D%EC%84%B1%20%EA%B4%80%EB%A6%AC/



#### Sequence 전략과 `allocationSize`

- 참고자료 : https://dololak.tistory.com/479

- `SEQUENCE` 전략에서 <u>`allocationSize`의 default값을 50으로 설정</u>해뒀는데 이는 **최적화**를 위해서이다. `IDENTITY`전략은 일단 DB에 인서트를 한 후에 DB에서 기본키를 조회해와서 엔티티에 식별자값을 부여하는 식이었다. (DB insert 후에 다시 DB조회) 반면 `SEQUENCE`전략은 DB에서 먼저 시퀀스를 조회한 담에 조회한 시퀀스를 기본키로 하여 DB에 값을 저장한다. 즉 INSERT전에 DB의 시퀀스를 조회해온다.

- JPA는 시퀀스에 접근하는 횟수를 줄이기 위해 `@SequenceGenerator.allocationSize`를 사용한다. 

  - > JPA에서 가상으로 관리할 시퀀스 할당 범위로 성능 최적화를 위해 값을 수정할 수 있습니다. 
    >
    > 기본값은 50이며, 1로 설정하는 경우 매번 insert시마다 DB의 시퀀스를 호출합니다.

  > allocationSize 값은 DB에 매번 시퀀스를 호출하지 않기 위해서 최적화를 위해 존재하는 속성입니다. 하이버네이트의 경우 기본값은 50인데, 값을 바꾸지 않는다면 최초에 DB에 시퀀스를 호출한 이후 50까지는 **메모리에서 현재 시퀀스 값을 저장한 이후 가상으로 증가시키며 관리하고 51이 되는 시점에 DB의 시퀀스를 한번 더 호출한 이후 그 값으로부터 50만큼인 100까지 가상으로 시퀀스 식별자를 관리하게 됩니다.**
  >
  > 주의할점이 있다면 DB의 시퀀스 증가값이 1인 경우입니다. DB의 시퀀스 증가값이 1인 상태는 반드시 allocationSize 또한 1로 맞춰 주어야 합니다. 이유는 allocationSize 가 다음과 같은 알고리즘으로 동작하기 때문입니다.
  >
  > 1. 최초 `persist()` 실행시에 설정에 따른 **DB 시퀀스를 두 번 호출하여 첫번째 시퀀스값을 가상으로 관리할 시작값, 두번째 시퀀스 값을 가상으로 관리할 범위의 끝(MAX)값으로 지정**합니다.
  >
  > 2. **이후에는 `persist()`를 실행해도 db에 시퀀스를 호출하지 않고 메모리에서 가상으로 관리하며 할당**합니다. persist() 실행시마다 메모리에서 관리하는 가상의 값을 1씩 증가시키며 엔티티에 할당합니다.
  >
  > 3. 어느 시점에 다다르면 엔티티에 식별자를 할당할 값이 관리할 범위의 끝(MAX)이 되고, 이후 다시 한번 persist()를 실행하는 시점에 DB에 시퀀스를 호출합니다.
  >
  > 4. 다시 호출한 시퀀스값을 가상으로 관리할 끝(MAX)값으로 바꾸고 시작값 또한 변경하는데 끝(MAX)값 - (allocationSize - 1) 공식을 사용하 시작값을 정합니다.

위와 같은 방식으로 동작하기 때문에 `allocationSize`를 1로 설정한다면, 매번 DB시퀀스에 접근하게 되는 것이다. 

다만 `allocationSize`가 50이면 DB에 저장시에 시퀀스 값은 50씩 증가된다. 

```sql
CREATE SEQUENCE [Sequence name] INCREMENT BY 50;            
```



#### AUTO 전략

- `GenerationType.AUTO`는 선택한 DB Dialect에 따라 IDENTITY, SEQUECE, TABLE 전략 중 하나를 자동으로 선택한다.
- 장점은 **데이터베이스를 변경해도 코드를 수정할 필요가 없다**는 것이다. 특히 키 생성 전략이 아직 확정되지 않은 개발 초기단계나 프로토타입 개발시 편리하게 사용할 수 있다.





