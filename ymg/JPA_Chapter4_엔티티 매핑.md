<aside>
💡 개요

기본키 생성 전략 ::  131 참조 - 세희씨

JPA를 사용하는 데 가장 중요한 것 - 엔티티와 테이블을 정확히 매핑하는 것

**매핑 어노테이션**

- 객체와 테이블 매핑 : @Entity, @Table
- 기본 키 매핑 : @Id
- 필드와 컬럼 매핑 : @Column
- 연관관계 매핑 : @ManyToOne, @JoinColumn
</aside>

# 4.1 @Entity

> JPA를 사용해서 테이블과 매핑할 클래스는 필수로 붙여야 한다. → `@NoArgsConstructure`
>
>
> @Entity가 붙은 클래스는 JPA가 관리하는 것
>

## 적용시 주의사항

- 기본 생성자 필수
- `final`클래스, `enum`, `interface`, `inner`클래스에는 사용할 수 없다.
- 저장할 필드에 `final`사용하면 안됨

# 4.2 @Table

> 엔티티와 매핑할 테이블 지정
>
>
> 생략기 매핑한 엔티티 이름을 테이블 이름으로 사용
>

# 4.3 다양한 매핑 사용

```java
@Getter
@Setter
@Entity
@NoArgsConstructor
@DynamicUpdate
@Table(name="MEMBER")
public class Member {
    //
    @Id
    @Column(name = "ID")
    private String id;

    @Column(name = "NAME")
    private String username;

    private Integer age;

    //== 추가 ==
    @Enumerated(EnumType.STRING)
    private RoleType roleType;

    @Temporal(TemporalType.TIMESTAMP)
    private Date createdDate;

    @Temporal(TemporalType.TIMESTAMP)
    private Date lastModifiedDate;

    @Lob
    private String description;
}
```

### @Enumerated

> enum을 컬럼으로 매핑하기 위해 사용하는 어노테이션
>

### @Temporal

> 자바의 날짜 타입을 매핑하기 위해 사용하는 어노테이션
>

### @Lob

> CLOB, BLOB 타입을 매핑하기 위한 어노테이션
>

# 4.4 데이터베이스 스키마 자동 적용

## persistence.xml 속성

```java
<property name="hibernate.hbm2ddl.auto" value="create" />
```

> ⇒ create로 인해 애플리케이션 실행 시점에 데이터베이스 테이블을 자동으로 생성한다.
>

application.yml의 경우

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/030c8bcf-7da5-484b-8393-a551004f8415/Untitled.png)

### 속성

| 옵션 | 설명 |
| --- | --- |
| create | DROP + CREATE |
| create-drop | DROP + CREATE + DROP |
| update | 변경사항만 수정 |
| validate | 테이블과 매핑정보를 비교해서 차이가 있으면 경고, 어플리케이션을 실행하지 않음
DDL을 수정하지 않는다. |
| none | 자동생성기능을 사용하지 않음(유효하지 않은 값) |

> 운영 단계에서는 create, create-drop, update를 절대 사용하지 않는다.
>
>
> 스테이징, 운영에서는 validate, none만을 사용할 것
>

---

```java
<property name="hibernate.show_sql" value="true" />
```

> ⇒ 콘솔에 DDL(Data Definition Language)을 출력할 수 있다.
>
>
> 이때 자동 생성되어 출력되는 DDL은 dialect에 따라 달라진다.
>

application.yml의 경우

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c44a64c1-94c0-4b5f-8a09-8ad6296877f3/Untitled.png)

# 4.5 DDL 생성 기능

> @Column의 매핑 정보에 속성 값을 추가하여 컬럼에 제약조건을 추가할 수 있다.
>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cc09df13-b7a0-4428-8ff3-a705dd5964e0/Untitled.png)

![L : Before, R : After](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a9e2d30b-3dac-409b-9757-9a58a15d16cd/Untitled.png)

L : Before, R : After

> @Table에 유니크 제약조건을 추가할 수도 있다.
>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0dc6d339-c01d-4fe7-868c-43fe094727b6/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/990d1802-2ae0-4bba-885c-1aa6bdee1af2/Untitled.png)

<aside>
💡 이런 속성값들은 단지 DDL을 자동 생성할 때만 사용되고 JPA의 실행 로직에는 영향을 주지 않는다.

⇒ 직접 DDL을 만든다면 사용할 이유가 없다.

</aside>

# 4.6 기본 키(Primary Key) 매핑

## JPA가 제공하는 기본 키 생성 전략

- 직접 할당 : 기본 키를 애플리케이션에서 직접 할당
- 자동 생성 : 대리 키 사용 방식
    - IDENTITY : 기본 키 생성을 데이터베이스에 위임
    - SEQUENCE : 데이터 베이스 시퀀스를 사용해서 기본 키 할당
    - TABLE : 키 생성 테이블을 사용

⇒ 전략이 이렇게 다양한 이유 : DB벤더마다 지원하는 방식이 다르기 때문

ex) 오라클은 시퀀스 제공 / MySQL은 AUTO_INCREMENT - 기본 키 값 자동으로 채워줌

<aside>
💡 키 생성 전략을 사용하려면 persistence.xml에 아래 속성을 반드시 추가해야 한다.

과거 버전과의 호환성을 위해 기본 값이 false이기 때문이다.

```java
<property name="hibernate.id.new_generator_mappings" value="true" />
```

</aside>

## 4.6.1 기본 키 직접 할당 전략

> @Id로 매핑하는 것
>
>
> 이는 em.persist()로 엔티티를 저장하기 전에 애플리케이션에서 기본 키를 직접 할당하는 방법이다.
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4870405c-68ec-4165-9e2b-fd6fb029f6e8/Untitled.png)
>

## 4.6.2 IDENTITY 전략

> 기본 키 생성을 DB에 위임하는 전략
>
>
> 주로 MySQL, PostgreSQL, SQL Server, DB2에서 사용
>

### 코드

아래처럼 `@GeneratedValue`어노테이션에 `strategy = GenerationType.IDENTITY`를 옵션으로 입력

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d9fd5321-fb22-441d-a4a6-5634580c6d62/Untitled.png)

데이터를 입력할 때 별도로 Id를 지정해주지 않아도

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/366e65da-064e-4e5f-96ec-446cfcf57e16/Untitled.png)

DB에는 Id가 자동으로 입력된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/14b69182-4d85-4140-b641-18ffd90aff6a/Untitled.png)

⇒ `em.persist()`를 호출해서 엔티티를 저장한 직후에 할당된 식별자 값을 출력

여기서 출력된 값 1,2는 **저장 시점에 DB가 생성한 값을 JPA가 조회한 것**이다

<aside>
💡 개발자가 엔티티에 직접 식별자를 할당하면 `@Id`만 있으면 되지만 식별자를 자동으로 생성하고 싶은 경우는 `@GeneratedValue`를 사용하고 식별자 생성 전략을 선택해야 한다.

IDENTITY전략은 데이터를 DB에 INSERT한 후에 기본 키 값을 조회할 수 있따. 따라서 엔티티에 식졀자 값을 할당하려면 JPA는 추가로 DB를 조회해야 한다.

</aside>

<aside>
❗ 엔티티가 영속 상태가 되려면 식별자가 반드시 필요하다.

그런데 IDENTITY 식별자 생성 전략은 엔티티를 DB에 저장해야 식별자를 구할 수 있으므로 em.persist()를 호출하는 즉시 INSERT SQL이 전달된다. **⇒  트랜잭션을 지원하는 쓰기 지연이 동작하지 않는다.**

</aside>

## 4.6.3 SEQUENCE 전략

> 시퀀스 : 유일한 값을 순서대로 생성하는 특별한 DB 오브젝트
>
>
> 오라클, PostgreSQL, DB2, H2 에서 사용
>

### 코드

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/acc55dd6-2886-4b20-84a9-935fb03a87f3/Untitled.png)

> `@SequenceGenerator`로 시퀀스 생성기 등록
>
>
> `sequenceName` : JPA는 이 속성의 이름으로 시퀀스 생성기를 실제 DB의 시퀀스와 매핑한다.
>
> `@GeneratedValue` 의 `strategy`로 전략을 선택하고, `generator` 로 시퀀스 생성기를 선택
>

값을 set 하고 `em.persist()` 호출

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/98925bfb-022d-4425-bee5-6fc780ca2518/Untitled.png)

출력값

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3d44779-b543-4520-b413-b32870c038b7/Untitled.png)

<aside>
💡 여기서 출력 형식은 IDENTITY와 같지만 내부 동작 방식은 다르다

SEQUENCE는 em.persist()를 호출할 때 먼저 DB 시퀀스를 사용해서 식별자를 조회한다.

그리고 조회한 식별자를 엔티티에 할당 한 후에 영속성 컨텍스트에 저장

이후 트랜잭션 커밋 - 플러시가 일어나면 엔티티를 DB에 저장

</aside>

| IDENTITY | SEQUENCE |
| --- | --- |
| 엔티티를 DB에 저장한 후 식별자 조회 - 엔티티의 식별자에 할당 | DB 시퀀스를 사용해서 식별자 조회 - 조회한 식별자를 엔티티에 할당한 후 엔티티를 영속성 컨텍스트에 저장 - 이후 플러시가 일어나면 엔티티를 DB에 저장 |

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/133d5ebc-b0d1-4023-99f2-d4006f490920/Untitled.png)

### @SequenceGenerator

> 특이사항
>
>
> `allocationSize`는 시퀀스 한 번 호출에 증가하는 수를 지정하는 변수인데, **기본값이 50으로 설정되어 있다.**
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/44a0a3c8-4edb-4dd2-b48a-ef614c72d084/Untitled.png)
>
> 5번 persist할 동안 `allocationSize`를 뺄 경우
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b91e2871-2adb-4863-bb0b-0fd3eff520c4/Untitled.png)
>
> 5번 persist할 동안 `allocationSize`를 2로 줄 경우 더 많은 SEQ호출이 필요하다
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2f2cd7d8-cb86-4c7e-b4b3-3328ed92b6c9/Untitled.png)
>

<aside>
❗ 저장 되는 DB에서 테스트 해볼 것!!

</aside>

<aside>
💡 JPA는 시퀀스에 접근하는 횟수를 줄이기 위해 `allocationSize`를 사용한다.

기본 값(50)이라면 시퀀스를 한 번에 50 증가 시킨 다음 1~50까지는 **메모리에서 식별자를 할당**한다. 그리고 51이 되면 시퀀스 값을 100으로 증가시킨 다음 51~100까지 메모리에서 식별자를 할당한다.

⇒ 이렇게 하면 시퀀스 값을 선점하므로 여러 JVM이 동시에 동작해도 기본 키 값이 충돌하지 않지만, DB에 **직접 접근해서 데이터를 등록할 때 시퀀스 값이 한 번에 많이 증가**한다는 것을 염두해야 한다.

INSERT 성능이 중요치 않으면 `allocationSize`값을 1로 줄 것

</aside>

## 4.6.4 TABLE 전략

> 키 생성 전용 테이블을 하나 만들고 여기에 이름과 값으로 사용할 컬럼을 만들어 DB 시퀀스를 흉내내는 전략 → 테이블을 사용하므로 모든 DB에 적용 가능
>

### 코드

`pkColumnValue`로 시퀀스 컬럼을 추가한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a1d3f29b-dd03-4992-a2b9-53a45b69e32f/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/032d8d74-4827-4395-b692-a5c806585f44/Untitled.png)

내부 동작 방식은 SEQUENCE전략과 동일하다 → 식별자 조회 - 엔티티 할당 - DB저장

하지만 시퀀스 자체를 테이블에 저장하는 방식이기 때문에 시퀀스 테이블 안에서 SELECT - UPDATE를 반복해서 시퀀스를 업데이트 한 후 할당한다 ⇒ 효율적인지는 잘 모르겠음

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e3d6fca4-3750-4450-ac1f-8a5ded55377c/Untitled.png)

이 후 사용자 입력값 개수 -1 만큼 SELECT UPDATE를 반복한다.

## 4.6.5 AUTO전략

> 선택한 DB방언에 따라 위 세가지 전략 중 하나를 자동으로 선택한다.
>
>
> ⇒ DB를 중간에 변경해도 코드를 수정할 필요가 없다.
>
> 단, SEQUENCE나 TABLE 전략이 선택도면 시퀀스나 키 생성요 테이블을 미리 만들어두어야 한다. - 스키마 자동 생성 기능을 사용하면 Hibernate가 기본값을 사용해서 자동으로 만들어 줄 것
>

# 4.7 필드와 컬럼 매핑: 레퍼런스

| 매필 어노테이션 | 설명 |
| --- | --- |
| @Column | 컬럼 매핑 |
| @Enumerated | 자바의 enum 타입을 매핑한다 |
| @Temporal | 날짜 타입을 매핑한다 |
| @Lob | BLOB. CLOB 타입을 매핑한다. |
| @Transient | 특정 필드를 데이터베이스에 매핑하지 않는다. |
| @Access | JPA가 엔티티에 접근하는 방식을 지정한다. |

## 4.7.1 @Column

책145p 속성은 나중에 사용할 때마다 정리할 것

특이한 경우만 정리한다.

### unique

> 한 컬럼에 유니크 제약조건
>
>
> 만약에 두 컬럼 이상을 사용해서 제약조건을 사용하려면 클레스 레벨에서 `@Table.uniqueConstraint`를 사용할 것
>

### columnDefinition

> 데이터베이스 컬럼 정보를 직접 줄 수 있다.
>

예제

```java
public class Board {
    //
    @Id
    @GeneratedValue(strategy = GenerationType.TABLE,
            generator = "BOARD_SEQ_GENERATOR")
    private Long id;
    private String name;
    @Column(columnDefinition = "varchar(100) default 'EMPTY'")
    private String data;
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cced62f9-71b9-4fe8-b1f0-1efed7b37798/Untitled.png)

## 4.7.2 @Enumerated

`@Enumerated(EnumType.*STRING*)` : enum 이름을 DB에 저장 → 권장

- 저장된 enum의 순서가 바뀌거나 enum이 추가되어도 안전
- 단, DB에서 저장되는 데이터 크기가 ORDINAL에 비해 크다

`@Enumerated(EnumType.*ORDINAL*)` : enum 순서를 DB에 저장 → 기본값

- DB에 저장되는 데이터 크기가 작다
- 단, 이미 저장된 enum의 순서를 변경할 수 없다

<aside>
❗ ORDINAL사용시 주의점

1번(0), 2번(1) 사이에 enum이 하나 추가 되면 1번(0), 3번(1), 2번(2) 이 되는데,

이때까지 2번으로 저장된 값은 DB에서 2로 변경되는 것이 아니라 1로 그대로 남았다

</aside>

## 4.7.3 @Temporal

> 날짜 타입 매핑시 사용
>

| 옵션 TemporalType | 기능 |
| --- | --- |
| DATE | 날짜, 데이터베이스 date타입과 매핑 |
| TIME | 시간, 데이터베이스 time타입과 매핑 |
| TIMESTAMP | 날짜와 시간, 데이터베이스 timestamp타입 |

<aside>
💡 생략할 경우

데이터베이스 방언에 따라서 DDL이 자동 생성된다.

datetime : MySQL

timestamp : H2, 오라클, PostgreSQL

</aside>

### 사용예

```java
@Getter
@Setter
@Entity
@SequenceGenerator(
        name = "BOARD_SEQ_GENERATOR",
        sequenceName = "BOARD_SEQ", // 매핑할 DB 시퀀스 이름
        initialValue = 1, allocationSize = 2
)
public class Board {
    //
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE,
            generator = "BOARD_SEQ_GENERATOR")
    private Long id;
    private String name;

    @Temporal(TemporalType.DATE)
    private Date date;

    @Temporal(TemporalType.TIMESTAMP)
    private Date time;
}
```

출력

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/16f7c409-752d-48c7-92f9-12ade9cb516e/Untitled.png)

## 4.7.4 @Lob

> 필드타입이 문자면 CLOB, 그 외에는 BLOB으로 매핑한다.
>

## 4.7.5 @Transient

> 이 필드는 매핑하지 않는다
>
>
> ⇒ DB에 저장하지도 않고 조회하지도 않는다. - 객체에 임시로 어떤 값을 보관하고 싶을 때 사용한다.
>

### 사용예

> @Transient를 필드에 붙여주고
>

```java
@Getter
@Setter
@Entity
@SequenceGenerator(
        name = "BOARD_SEQ_GENERATOR",
        sequenceName = "BOARD_SEQ", // 매핑할 DB 시퀀스 이름
        initialValue = 1, allocationSize = 2
)
public class Board {
    //
    @Id
    @GeneratedValue(strategy = GenerationType.SEQUENCE,
            generator = "BOARD_SEQ_GENERATOR")
    private Long id;
    private String name;

    @Temporal(TemporalType.DATE)
    private Date date;

    @Temporal(TemporalType.TIMESTAMP)
    private Date time;

    @Transient
    private String transientVal;
}
```

값까지 입력해주었지만

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7698acee-1651-4ff1-b867-f9621413ade9/Untitled.png)

테이블 생성시에는 해당 필드가 제외된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60302191-2779-479f-a896-757eca39973a/Untitled.png)

## @Access

> JPA가 엔티티 데이터에 접근하는 방식을 지정
>

### 필드 접근

> AccessType.FIELD로 지정한다.
>
>
> 필드에 직접 접근
>
> 필드 접근권한이 private여도 접근 할 수 있다.
>
> Access를 설정하지 않으면 @Id의 위치를 기준으로 접근방식이 설정된다. ⇒ 일반적인 경우
>

### 프로퍼티 접근

> Id가 프로퍼티에 있을때는 생략할 수 있다.
>

```java
private String id;

@Id
public String getId(){
	return id;
}
```

### 필드 + 프로퍼티 접근 방식

> 함께 사용하는 경우
>
>
> 책 예제에서 빼 먹은 것 → setter를 꼭 넣어주어야 한다
>

엔티티

```java
@Entity
@NoArgsConstructor
@Table(name="MEMBER")
public class Member {
    //
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long id;

    @Transient
    private String firstName;

    @Transient
    private String lastName;

    @Access(AccessType.PROPERTY)
    public String getFullName(){
        //
        return firstName + lastName;
    }

    public void setFullName(String fullName) {
        //
    }
```

결과

⇒ 자동으로 fullName이 컬럼으로 생성된다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04f7fa94-4968-4ff6-a3d7-199242935cd3/Untitled.png)