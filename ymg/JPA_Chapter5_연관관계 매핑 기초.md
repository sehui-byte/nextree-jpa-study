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

데이터 입력 로직 수정-team을 조회해서 출력되게 하려면 team1에도 Member를 넣어서 persist해주어야 했다

```java
public static void testSave(EntityManager em) {
        //
        //팀1 저장
        Team team1 = new Team("team1", "팀1");

        //회원1
        Member member1 = new Member("member1", "회원1", team1);
        em.persist(member1);
        //회원2
        Member member2 = new Member("member2", "회원2", team1);
        em.persist(member2);

        // 이걸 추가해야한다고??
        List<Member> members = new ArrayList<>();
        members.add(member1);
        members.add(member2);
        team1.setMembers(members);
        em.persist(team1);

    }
```

출력로직

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

> 출력결과
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/445a278d-dfa4-4d9a-abe8-d01291f3a8a6/Untitled.png)
>

# 5.4 연관관계의 주인

> 객체에는 양방향 연관관계라는 것이 없다 - 흉내만 낼 뿐
>
>
> 테이블은 외래 키 하나만으로 양방향 연관관계를 맺는다.
>
> ⇒ 이런 차이로 인해 JPA는 두 객체 연관관계 중 하나를 정해서 테이블의 외래 키를 관리 해야 하는데 이것을 `연관관계의 주인(Owner)`라고 한다.
>

## 5.4.1 양방향 매핑의 규칙 : 연관관계의 주인

> 연관관계의 주인만이 DB연관관계와 매핑되고 외래 키를 관리(CUD)할 수 있다.
>
>
> 주인이 아닌 쪽은 읽기(R)만 할 수 있다.
>
> ⇒ 연관관계의 주인을 정한다는 것은 외래 키의 관리자를 선택하는 것이다.
>

Member entity

```java
public class Member {
    //연관관계 매핑
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
```

Team entity

```java
public class Team {
		//
    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<>();
```

> 위의 경우 Member entity를 주인으로 선택하면 Member 테이블의 team_id 외래 키를 관리하면 된다.
>
>
> 반면 Team을 주인으로 선택할 경우 물리적으로 전혀 다른 테이블의 외래 키를 관리해야 한다.
>

## 5.4.2 연관관계의 주인은 외래 키가 있는 곳

> 연관관계의 주인만 DB연관관계와 매핑되고 외래키를 관리할 수 있다.
>
>
> 반대편은 읽기만 가능하다.
>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4fe62713-88d1-4668-bf40-0f7d52dc3b84/Untitled.png)

<aside>
💡 DB에서 테이블의 관계는 항상 다 쪽이 외래 키를 가진다.

다 쪽인 @ManyToOne은 항상 연관관계의 주인이 되므로 mappedBy를 설정할 수 없다.

</aside>

# 5.5 양방향 연관관계 저장

> Team에 Member를 별도로 저장하지 않아도 Member에서 저장한 Team값으로 인해 Member테이블에는 Team이 저장된다.
>

입력

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bde43116-8ba7-4053-be4c-57999ae4f568/Untitled.png)

출력

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a6549244-1596-42c3-9f6d-9ad967db5633/Untitled.png)

> 실행 결과
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3f12e243-4990-45cb-8818-db82dcd6197a/Untitled.png)
>

# 5.6 양방향 연관관계의 주의점

> 양방향 관계를 설정하고 흔히 하는 실수는 주인에는 값을 입력하지 않고 주인이 아닌 곳에만 입력하는 것이다. DB에 외래 키 값이 정상적이지 않다면 이것을 의심할 것.
>

## 5.6.1 순수한 객체까지 고려한 양방향 연관관계

> 객체 관점에서 양쪽 방향 모두 값을 입력해주는 것이 안전하다.
>
>
> 위의 5.3.2에서 한 것처럼 Member, Team 양쪽 다 데이터를 넣어주어야 진짜 양뱡향 연관관계라 할 수 있다. → Member에서 검색하든 Team에서 검색하든 같은 결과가 나옴
>

입력 일부 수정

```java
public static void testSave(EntityManager em) {
    //
    //팀1 저장
    Team team1 = new Team("team1", "팀1");

    //회원1
    Member member1 = new Member("member1", "회원1", team1);
    em.persist(member1);
    //회원2
    Member member2 = new Member("member2", "회원2", team1);
    em.persist(member2);
		
		//team에 Member의 데이터들을 추가해주었다
    team1.getMembers().add(member1);
    team1.getMembers().add(member2);
    em.persist(team1);

}
```

## 5.6.2 연관관계 편의 메소드

> 위처럼 `member.set(team1);`(나는 생성자로 대체)  `team1.getMembers().add(member1);` 를 각각 호출하다 보면 실수로 둘 중 하나만 호출해서 양방향이 깨질 수 있다.
>

이럴 때는 Member클래스의 setTeam()을 수정해서 사용하는 것이 안전하다.

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

    //연관관계 매핑
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
		
		//생성자 추가
    public Member (String id, String username) {
        //
        this.id = id;
        this.username = username;
    }
		
		//setter수정
    public void setTeam(Team team) {
        //
        this.team = team;
        team.getMembers().add(this);
    }
}
```

수정 출력 로직

```java
public static void testSave2(EntityManager em) {
    //
    //팀1 저장
    Team team1 = new Team("team1", "팀1");
    em.persist(team1);
    
    //회원1
    Member member1 = new Member("member1", "회원1");
    member1.setTeam(team1);
    em.persist(member1);
    //회원2
    Member member2 = new Member("member2", "회원2");
    member2.setTeam(team1);
    em.persist(member2);
}
```

## 5.6.3 연관관계 편의 메소드 작성 시 주의사항

> team을 변경하는 경우 기존의 관계가 제거되지 않는다 → 제거 코드를 추가해주어야 함
>

```java
//setter수정
    public void setTeam(Team team) {
        //
        //기존 관계 제거
        if (this.team != null) {
            this.team.getMembers().remove(this);
        }
        this.team = team;
        team.getMembers().add(this);
    }
```

<aside>
❗ 사실 teamA → member1 관계가 제거되지 않아도 문제는 없음

Team.members는 연관관계의 주인이 아니기 때문

또한 새로운 영속성 컨텍스트에서 teamA를 조회해서 `teamA.getMembers()`를 호출해도 외래 키에는 관계가 끊어져 있으므로 조회되지 않는다.

다만 관계를 변경하고 영속성 컨텍스트가 살아있는 상태라면 `teamA.getMembers()`를 실행할 경우 member1이 조회되어버린다.

</aside>

# 5.7 정리

> 양방향은 단방향과 비교해서 반대방향으로 객체 그래프 탐색 기능을 추가 한 것 뿐
>
>
> 객체 그래프 탐색이 필요할 때 양방향을 사용하도록 한다.
>

<aside>
❗ 연관관계의 주인은 외래 키의 위치와 관련해서 정해야지 비즈니스 중요도로 접근해서는 안된다.

또한 양방향 매핑 시 무한루프를 주의할 것 - 서로를 참조할 수 있기 때문이다.

</aside>