<aside>
💡 개요

- **프록시와 즉시로딩, 지연로딩**

    프록시를 사용하면 연관된 객체를 처음부터 DB에서 조회하는 것이 아니라, 실제 사용하는 시점에 DB에서 조회할 수 있다

    하지만 자주 함께 사용하는 객체들은 조인을 사용해서 함께 조회하는 것이 효과적이다.

    ⇒ JPA는 즉시 로딩과 지연 로딩이라는 방법으로 둘을 모두 지원한다.

- 영속성 전이와 고아 객체

    JPA는 연관된 객체를 함께 저장하거나 함께 삭제할 수 있는 영속성 전이와 고아 객체 제거라는 기능을 제공한다.

</aside>

# 8.1 프록시

> 엔티티를 조회할 때 연관된 엔티티들이 항상 사용되는 것은 아니다.
>
>
> 예를 들어 회원 엔티티를 조회할 때 연관된 팀 엔티티는 비즈니스 로직에 따라 사용될 때도 있지만 그렇지 않을 때도 있다.
>

*`Member`*

```sql
@Entity
@NoArgsConstructor
@AllArgsConstructor
public class Member {
    //
    @Id
    private String id;
    private String username;

    @ManyToOne
    private Team team;
}
```

*`Team`*

```sql
@Entity
@NoArgsConstructor
@AllArgsConstructor
public class Team {
    //
    @Id
    private String id;
    private String name;
}
```

`저장, 출력 로직`

> 출력할 때 회원과 팀 정보를 동시에 출력
>

```sql
public static void saveUserAndTeam(EntityManager em) {
    //
    Team team1 = new Team("Team1","팀1");
    Member member1 = new Member("Member1", "유저1", team1);
    em.persist(team1);
    em.persist(member1);
}

public static void printUserAndTeam(EntityManager em) {
    //
    Member member = em.find(Member.class, "Member1");
    Team team = member.getTeam();
    System.out.println("회원 이름 :: " + member.getUsername());
    System.out.println("소속팀 :: " + team.getName());
}
```

> 출력 결과
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0c546050-b07e-4b79-b4b1-46b329ef4839/Untitled.png)
>

`팀 정보만 출력하는 로직`

```sql
public static void printUser(EntityManager em) {
    //
    Member member = em.find(Member.class, "Member1");
    System.out.println("회원 이름 :: " + member.getUsername());
}
```

> 회원만 사용 → 회원 엔티티를 조회할 때 팀까지 사용하는 것은 효율적이지 못하다.
>

<aside>
💡 JPA는 위와 같은 문제를 해결하기 위해 엔티티가 실제 사용될 때까지 DB조회를 지연하는 방법을 제공

⇒ 이를 지연 로딩이라고 한다.

하지만 지연 로딩을 사용하기 위해서는 실제 엔티티 객체 대신에 DB조회를 지연할 수 있는 가짜 객체가 필요한데, 이것을 프록시 객체라고 한다.

</aside>

## 8.1.1 프록시 기초

> JPA엔티티 조회 - `EntityManager.find()`
>
>
> 이 메소드는 영속성 컨텍스트에 엔티티가 없으면 DB를 조회한다.
>

```sql
Member member = em.find(Member.class, "Member1");
```

> 이렇게 직접 조회하면 엔티티를 실제 사용하든 사용하지 않든 DB를 조회한다.
>
>
> 엔티티를 실제 사용시점까지 DB조회를 미루고 싶으면 `EntityManager.getReference()`메소드를 사용하면 된다.
>

```sql
Member member = em.getReference(Member.class, "Member1");
```

> 이 메소드를 호출할 때 JPA는 DB를 조회하지 않고 실제 엔티티 객체도 생성하지 않는다.
>
>
> 대신 DB접근을 위임한 프록시 객체를 반환한다.
>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ad907781-a8d3-4229-8e46-c676b4e7c959/Untitled.png)

### 프록시의 특징

> 프록시 클래스는 실제 클래스를 상속받아서 만들어진다. → 실제 클래스와 겉 모양이 같다
>
>
> **⇒ 사용하는 입장에서는 이것이 진짜 객체인지 프록시 객체인지 구분하지 않고 사용하면 된다.**
>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a6ff51de-e736-421e-971c-f8e3a64075f6/Untitled.png)

> 프록시 객체는 실제 객체에 대한 참조(target)을 보관한다.
>
>
> 그리고 **프록시 객체의 메소드를 호출하면 프록시 객체는 실제 객체의 메소드를 호출**한다.
>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4743cb4e-adcc-4a06-8a9c-9668433059ca/Untitled.png)

### 프록시 객체의 초기화

> 프록시 객체는 `member.getName()`처럼 **실제 사용될 때 DB를 조회해서 실제 엔티티 객체를 생성**한다.
>
>
> ⇒ 이를 프록시 객체의 초기화 라고 한다.
>

```sql
//객체의 초기화 예제
public static void proxyReturn(EntityManager em) {
    //
    Member member = em.getReference(Member.class, "Member1");
    System.out.println(member.getUsername());
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e92016e5-9544-415f-9dd1-d65a1e9bc09e/Untitled.png)

> 프록시의 초기화 과정
>
> 1. 프록시 객체에 .getUserName()을 호출해서 실제 데이터를 조회한다.
> 2. 프록시 객체는 실제 엔티티가 생성되어 있지 않으면 영속성 컨텍스트에 실제 엔티티 생성을 요청한다 → 이를 초기화라 한다.
> 3. 영속성 컨텍스트는 DB를 조회해서 실제 엔티티 객체를 생성한다.
> 4. 프록시 객체는 생성된 실제 엔티티 객체의 참조를 Member target 멤버 변수에 저장
> 5. 프록시 객체는 실제 엔티티 객체의 getName()을 호출해서 결과를 반환한다.

### 프록시의 특징

- 처음 사용할 때 한 번만 초기화 된다.
- 프록시 객체를 초기화 한다고 실제 엔티티 객체가 되는 것은 아니다.

    프록시 객체가 초기화되면 이를 통해서 실제 엔티티에 접근할 수 있다.

- 프록시 객체는 원본 엔티티를 상속받은 객체이다 → 타입 체크시에 주의해야 한다.
- 영속성 컨텍스트에 찾는 엔티티가 있으면 DB를 조회할 필요가 없다

    ⇒ `em.getReference()`를 호출해도 프록시가 아닌 실제 엔티티를 반환한다.

- 초기화는 영속성 컨텍스트의 도움을 받아야 가능하다.

    준영속 상태의 프록시를 초기화 하면 `LazyInitalizationException`을 발생시킨다.

    ### 실험

    > `em.detach(member1);`로 준영속화 시킨 후 같은 코드 실행
    >

    ```sql
    public static void saveUserAndTeam(EntityManager em) {
        //
        Team team1 = new Team("Team1","팀1");
        Member member1 = new Member("Member1", "유저1", team1);
        em.persist(team1);
        em.persist(member1);
        em.detach(member1);
    }
    ```

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/09fc0a8f-ad79-4623-80de-faf2cfb83cb7/Untitled.png)

    > 위의 방식으로는 테스트가 되지 않는것 같았다. -그래도 Proxy의 LazyInitalizer위치는 찾을 수 있었다.
    >

### 준영속 상태와 초기화

> `em.close()`로 영속성 컨텍스트를 종료해서 member는 준영속 상태다.
>
>
> `member.getName()`을 호출하면 프록시를 초기화 해야 하는데 영속성 컨텍스트가 없으므로 실제 엔티티를 조회할 수 없다. ⇒ 예외 발생
>

```sql
//테스트 코드
public static void main(String[] args) {

    //엔티티 매니저 팩토리 생성
    EntityManagerFactory emf = Persistence.createEntityManagerFactory("Java_JPA_H2_Test");
    EntityManager em = emf.createEntityManager(); //엔티티 매니저 생성

    EntityTransaction tx = em.getTransaction(); //트랜잭션 기능 획득
    tx.begin(); //트랜잭션 시작
    Member member = em.getReference(Member.class, "Member1");
    tx.commit();//트랜잭션 커밋
    em.close(); //엔티티 매니저 종료

    System.out.println(member.getUsername()); // 호출

    emf.close(); //엔티티 매니저 팩토리 종료
}
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c6d9cea7-e698-4f24-9bf9-42ade9452e50/Untitled.png)

<aside>
💡 JPA 표준 명세는 지연 로딩(프록시)에 대한 내용을 JPA 구현체에 맡겼다.

⇒ 준영속 상태의 엔티티를 초기화 할 때 어떤 일이 발생할 때 표준 명세에는 정의되어 있지 않다.

하이버 네이트를 사용하면 `org.hibernate.LazyInitializationException`를 발생시킨다.

</aside>

## 8.1.2 프록시와 식별자

> 엔티티를 프록시로 조회할 때 식별자(PK) 값을 파라미터로 전달하는데 프록시 객체는 이 값을 보관한다.
>

```sql
Team team = em.getReference(Team.class, "Team1"); //식별자 보관
team.getId(); // 초기화 되지 않음
```

> 프록시 객체는 식별자 값을 가지고 있으므로 getId()를 호출해도 프록시를 초기화 하지 않는다.
>
>
> 단, 엔티티 접근 방식을 필드 :: @Access(AccessType.FIELD))로 설정한 경우 어떤 일을 하는 메소드인지 알지 못하므로 프록시 객체를 초기화 한다.
>

<aside>
💡 프록시의 유용성

프록시는 아래처럼 연관관계를 설정할 때 유용하게 사용할 수 있다.

```sql
Member member = em.find(Member.class, "Member1");
Team team = em.getReference(Team.class, "Team1"); // SQL을 실행하지 않음
member.setTeam(team);
```

연관관계를 설정할 때는 식별자 값만 사용하므로 **프록시를 사용하면 DB 접근 횟수를 줄일 수 있다.**

</aside>

비교 코드

1. find만 사용하는 경우 → select 쿼리가 두 번 실행된다.

    ```sql
    public static void proxyCompare(EntityManager em) {
        //
        Member member = em.find(Member.class, "Member1");
        Team team = em.find(Team.class, "Team1"); // SQL을 실행
        member.setTeam(team);
        System.out.println(team.getName());
    }
    ```

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9c353bdf-362d-4fa4-840a-3cc828430605/Untitled.png)

2. Proxy를 사용하는 경우 → select가 한 번만 실행된다.

    ```sql
    public static void proxyReturn(EntityManager em) {
        //
        Member member = em.find(Member.class, "Member1");
        Team team = em.getReference(Team.class, "Team1"); // SQL을 실행하지 않음
        member.setTeam(team);
        System.out.println(team.getName());
    }
    ```

    ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cefc0a43-550a-470f-9e56-31fb9a3bee5d/Untitled.png)


## 8.1.3 프록시 확인

> `PersistenceUnitUtil.isLoaded(Object entity)`를 사용하면 프록시 인스턴스의 초기화 여부를 확인할 수 있다. - 아직 초기화 되지 않은 경우 false를 반환한다.
>

```sql
public static void proxyLoadCheck(EntityManager em) {
    //
    Team team = em.getReference(Team.class, "Team1");
    boolean isLoad = em.getEntityManagerFactory()
            .getPersistenceUnitUtil().isLoaded(team);
    System.out.println("isLoad ::: " + isLoad);
}
```

> 조회한 엔티티가 진짜인지 프록시인지 확인하려면 클래스 명을 직접 출력해보면 된다.
>
>
> 클래스 명 뒤에 `..javassist..`라 되어 있으면 프록시인 것이다.
>

```sql
public static void proxyLoadCheck(EntityManager em) {
    //
    Team team = em.getReference(Team.class, "Team1");
    System.out.println("Proxy Check ::: " + team.getClass().getName());
}
```

출력결과 - 프록시를 생성하는 라이브러리에 따라 결과가 달라질 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4e063a20-759e-4af0-a23a-71ddae98cc1f/Untitled.png)

<aside>
💡 프록시 강제 초기화

`iniitalize()`메소드를 사용하면 프록시를 강제로 초기화 할 수 있다.

단, JPA표준에는 없는 메소드이므로 직접 호출하면 된다.

</aside>