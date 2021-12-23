<aside>
💡 개요

객체의 참조와 테이블의 외래키 매핑

### 방향

둘 중 한 쪽만 참조 → 단방향 관계

서로 참조 → 양방향 관계

### 다중성

N:1 , 1:N, 1:1, N:N

### 연관관계의 주인

객체를 양방향 연관관계로 만들면 연관관계의 주인을 정해야 한다.

</aside>

# 5.1 단방향 연관관계

![다대일 (N:1) 연관관계 예시](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9bf0d956-afb1-492e-b1fe-262799dd4a16/Untitled.png)

다대일 (N:1) 연관관계 예시

<aside>
💡 객체 연관관계와 테이블 연관관계의 가장 큰 차이

객체 **참조**를 통한 연관관계는 언제나 단방향이다

ex) Member.team으로 팀을 알 수는 있지만 팀은 멤버를 알 수 없다.

연관관계를 양방향으로 만들고 싶다면 반대쪽에도 필드를 추가해서 서로를 참조하도록 해야 한다.

**하지만 이것은 양방향이 아니라 서로 다른 단방향 관계 2개 이다.**

반면 테이블은 **외래 키**로 양방향 연관관계를 가진다.

ex) 테이블은 Join을 이용해서 서로를 알 수 있다.

</aside>

## 5.1.1 순수한 객체 연관관계

패스

## 5.1.2 테이블 연관관계

패스

## 5.1.3 객체 관계 매핑

JPA를 사용해서 객체 연관관계와 테이블 연관관계를 매핑한다.

회원 객체

```java
@Getter
@Setter
@NoArgsConstructor
@Entity
public class Member {
    //
    @Id
    @Column(name = "MEMBER_ID")
    private String id;

    private String username;

    //연관관계 매핑
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
}
```

팀 객체

```java
@Getter
@Setter
@Entity
@NoArgsConstructor
public class Team {
    //
    @Id
    @Column(name = "TEAM_ID")
    private String id;

    private String name;
}
```

### @JoinColumn

> 외래 키 매핑할 때 사용
>

| 속성 | 기능 |
| --- | --- |
| name | 매핑할 외래 키 이름 |
| referencedColumnName | 외래 키가 참조하는 대상 테이블의 컬럼명 |
| foreignKey(DDL) | 외래 키 제약조건을 직접 지정
테이블 생성시에만 사용 |
| unique, nullable등
@Column과 같은 속성들 | @Column의 속성과 같음 |

<aside>
💡 @JoinColumn생략

⇒ 생략 시 기본 전략을 사용한다.

기본 전략 :: 필드명 + _ + 참조하는 테이블의 컬럼명

```java
    @ManyToOne
    private Team team;
```

이 경우 team_TEAM_ID를 외래 키로 사용한다.

</aside>

### @ManyToOne

> 다대일 관계에서 사용
>
>
> 속성은 나중에 사용할 때 추가할 것
>

| 속성 | 기능 |
| --- | --- |
| optional | false로 설정하면 연관된 엔티티가 반드시 있어야 한다. |
|  |  |

# 5.2 연관관계 사용

## 5.2.1 저장

### 테스트 코드

```java
public static void testSave(EntityManager em) {
    //
    //팀1 저장
    Team team1 = new Team("team1", "팀1");
    em.persist(team1);

    //회원1
    Member member1 = new Member("member1", "회원1", team1);
    em.persist(member1);
    //회원2
    Member member2 = new Member("member2", "회원2", team1);
    em.persist(member2);
}
```

> 테이블 생성 쿼리 ⇒ **참조한 팀의 식별자 team.id를 외래 키로 사용해서 적절한 등록 쿼리를 생성한다.**
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4673861f-8ba1-4fe0-a2e4-291c51583327/Untitled.png)
>

> 출력 확인
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b9e8ad69-3214-40da-b78b-7e5ec97528be/Untitled.png)
>

## 5.2.2 조회

### 테스트 코드 ⇒ 객체 그래프 탐색

> 아래처럼 객체를 통해 연관된 엔티티를 조회하는 것을 객체 그래프 탐색이라고 한다.
>

```java
public static void findAllObject(EntityManager em) {
    //
    Member member1 = em.find(Member.class, "member1");
    System.out.println(member1.getId());
    System.out.println(member1.getUsername());
    System.out.println(member1.getTeam().getName());
    System.out.println("======================");

    Member member2 = em.find(Member.class, "member2");
    System.out.println(member2.getId());
    System.out.println(member2.getUsername());
    System.out.println(member2.getTeam().getName());
    System.out.println("======================");
}
```

### 테스트 코드 ⇒ 객체 지향 쿼리 사용(JPQL)

> Member와 Team을 조인
>

```java
public static void finaAllJPQL(EntityManager em) {
        //
        List<Member> members = em.createQuery("select m from Member m join m.team t where " +
                "t.name =:teamName", Member.class)
                .setParameter("teamName", "팀1")
                .getResultList();
        for (Member mem : members) {
            System.out.println(mem.getId());
            System.out.println(mem.getUsername());
            System.out.println(mem.getTeam().getName());
            System.out.println("======================");
        }
    }
```

> 생성 쿼리
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fd0f8040-87fc-46a7-a22a-bb13f913e9da/Untitled.png)
>

> 출력 확인
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b9e8ad69-3214-40da-b78b-7e5ec97528be/Untitled.png)
>

## 5.2.3 수정

> update는 별도 메소드가 없다 - 알아서 플러시 발생
>

```java
public static void updateRelation(EntityManager em) {
    //
    //새로운 팀
    Team team2 = new Team("team2", "팀2");
    em.persist(team2);

    //회원1에 새로운 팀2 설정
    Member member = em.find(Member.class, "member1");
    member.setTeam(team2);
}
```

> 생성 쿼리
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/faf163b5-a23a-4ab3-8a20-e1d4d602ca0b/Untitled.png)
>

> 출력결과
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/96c455a2-3ba1-48ef-a707-b45ec6f787d0/Untitled.png)
>

## 5.2.4 연관관계 제거

> 연관관계를 null로 설정 → 일반적으로 출력하려고 하면 널포인트 발생
>

```java
public static void deleteRelation(EntityManager em) {
    //

    Member member1 = em.find(Member.class, "member1");
    member1.setTeam(null);
}
```

> 생성 쿼리
>
>
> TEAM_ID = null로 업데이트 된다.
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27279e64-3c6c-48d0-a51a-dd4843942f8b/Untitled.png)
>

## 5.2.5 연관된 엔티티 삭제

> 연관된 엔티티를 삭제하려면 기존에 있던 연관관계를 먼저 제거해야 한다.
>
>
> 그렇지 않으면 외래 키 제약 조건으로 인해 DB에 오류가 발생함
>

```java
public static void testSave(EntityManager em) {
    //
    //팀1 저장
    Team team1 = new Team("team1", "팀1");
    em.persist(team1);

    //회원1
    Member member1 = new Member("member1", "회원1", team1);
    em.persist(member1);
    //회원2
    Member member2 = new Member("member2", "회원2", team1);
    em.persist(member2);

    em.remove(team1);
}
```

에러 사항 → 트랜잭션에 실패한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7e713da4-9a39-4565-a773-b1107d95433a/Untitled.png)

### 정상 작동시

```java
public static void testSave(EntityManager em) {
    //
    //팀1 저장
    Team team1 = new Team("team1", "팀1");
    em.persist(team1);

    //회원1
    Member member1 = new Member("member1", "회원1", team1);
    em.persist(member1);
    //회원2
    Member member2 = new Member("member2", "회원2", team1);
    em.persist(member2);

    member1.setTeam(null); //미리 연관관계를 끊는다
    member2.setTeam(null);
    em.remove(team1);
}
```

> 생성 쿼리
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e40d142c-68f2-4db5-a1f9-ee20b1d83d5f/Untitled.png)
>

# 5.3 양방향 연관관계

## 5.3.1 양방향 연관관계 매핑

> member에는 변경점 없음
>

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
public class Member {
    //
    @Id
    @Column(name = "MEMBER_ID")
    private String id;

    private String username;

		public Team(String id, String name) {
        //
        this.id = id;
        this.name = name;
    }

    //연관관계 매핑
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
}
```

## 5.3.2. 일대다 컬렉션 조회

```java
public static void biDirection(EntityManager em) {
    //
    Team team = em.find(Team.class, "team1");
    List<Member> members = team.getMembers();
    for (Member mem : members) {
        System.out.println(mem.getId());
        System.out.println(mem.getUsername());
        System.out.println(mem.getTeam().getName());
        System.out.println("======================");
    }
}
```