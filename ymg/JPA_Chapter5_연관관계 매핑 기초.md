<aside>
๐ก ๊ฐ์

๊ฐ์ฒด์ ์ฐธ์กฐ์ ํ์ด๋ธ์ ์ธ๋ํค ๋งคํ

### ๋ฐฉํฅ

๋ ์ค ํ ์ชฝ๋ง ์ฐธ์กฐ โ ๋จ๋ฐฉํฅ ๊ด๊ณ

์๋ก ์ฐธ์กฐ โ ์๋ฐฉํฅ ๊ด๊ณ

### ๋ค์ค์ฑ

N:1 , 1:N, 1:1, N:N

### ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ

๊ฐ์ฒด๋ฅผ ์๋ฐฉํฅ ์ฐ๊ด๊ด๊ณ๋ก ๋ง๋ค๋ฉด ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ์ ์ ํด์ผ ํ๋ค.

</aside>

# 5.1 ๋จ๋ฐฉํฅ ์ฐ๊ด๊ด๊ณ

![๋ค๋์ผ (N:1) ์ฐ๊ด๊ด๊ณ ์์](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/9bf0d956-afb1-492e-b1fe-262799dd4a16/Untitled.png)

๋ค๋์ผ (N:1) ์ฐ๊ด๊ด๊ณ ์์

<aside>
๐ก ๊ฐ์ฒด ์ฐ๊ด๊ด๊ณ์ ํ์ด๋ธ ์ฐ๊ด๊ด๊ณ์ ๊ฐ์ฅ ํฐ ์ฐจ์ด

๊ฐ์ฒด **์ฐธ์กฐ**๋ฅผ ํตํ ์ฐ๊ด๊ด๊ณ๋ ์ธ์ ๋ ๋จ๋ฐฉํฅ์ด๋ค

ex) Member.team์ผ๋ก ํ์ ์ ์๋ ์์ง๋ง ํ์ ๋ฉค๋ฒ๋ฅผ ์ ์ ์๋ค.

์ฐ๊ด๊ด๊ณ๋ฅผ ์๋ฐฉํฅ์ผ๋ก ๋ง๋ค๊ณ  ์ถ๋ค๋ฉด ๋ฐ๋์ชฝ์๋ ํ๋๋ฅผ ์ถ๊ฐํด์ ์๋ก๋ฅผ ์ฐธ์กฐํ๋๋ก ํด์ผ ํ๋ค.

**ํ์ง๋ง ์ด๊ฒ์ ์๋ฐฉํฅ์ด ์๋๋ผ ์๋ก ๋ค๋ฅธ ๋จ๋ฐฉํฅ ๊ด๊ณ 2๊ฐ ์ด๋ค.**

๋ฐ๋ฉด ํ์ด๋ธ์ **์ธ๋ ํค**๋ก ์๋ฐฉํฅ ์ฐ๊ด๊ด๊ณ๋ฅผ ๊ฐ์ง๋ค.

ex) ํ์ด๋ธ์ Join์ ์ด์ฉํด์ ์๋ก๋ฅผ ์ ์ ์๋ค.

</aside>

## 5.1.1 ์์ํ ๊ฐ์ฒด ์ฐ๊ด๊ด๊ณ

ํจ์ค

## 5.1.2 ํ์ด๋ธ ์ฐ๊ด๊ด๊ณ

ํจ์ค

## 5.1.3 ๊ฐ์ฒด ๊ด๊ณ ๋งคํ

JPA๋ฅผ ์ฌ์ฉํด์ ๊ฐ์ฒด ์ฐ๊ด๊ด๊ณ์ ํ์ด๋ธ ์ฐ๊ด๊ด๊ณ๋ฅผ ๋งคํํ๋ค.

ํ์ ๊ฐ์ฒด

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

    //์ฐ๊ด๊ด๊ณ ๋งคํ
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
}
```

ํ ๊ฐ์ฒด

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

> ์ธ๋ ํค ๋งคํํ  ๋ ์ฌ์ฉ
>

| ์์ฑ | ๊ธฐ๋ฅ |
| --- | --- |
| name | ๋งคํํ  ์ธ๋ ํค ์ด๋ฆ |
| referencedColumnName | ์ธ๋ ํค๊ฐ ์ฐธ์กฐํ๋ ๋์ ํ์ด๋ธ์ ์ปฌ๋ผ๋ช |
| foreignKey(DDL) | ์ธ๋ ํค ์ ์ฝ์กฐ๊ฑด์ ์ง์  ์ง์ 
ํ์ด๋ธ ์์ฑ์์๋ง ์ฌ์ฉ |
| unique, nullable๋ฑ
@Column๊ณผ ๊ฐ์ ์์ฑ๋ค | @Column์ ์์ฑ๊ณผ ๊ฐ์ |

<aside>
๐ก @JoinColumn์๋ต

โ ์๋ต ์ ๊ธฐ๋ณธ ์ ๋ต์ ์ฌ์ฉํ๋ค.

๊ธฐ๋ณธ ์ ๋ต :: ํ๋๋ช + _ + ์ฐธ์กฐํ๋ ํ์ด๋ธ์ ์ปฌ๋ผ๋ช

```java
    @ManyToOne
    private Team team;
```

์ด ๊ฒฝ์ฐ team_TEAM_ID๋ฅผ ์ธ๋ ํค๋ก ์ฌ์ฉํ๋ค.

</aside>

### @ManyToOne

> ๋ค๋์ผ ๊ด๊ณ์์ ์ฌ์ฉ
>
>
> ์์ฑ์ ๋์ค์ ์ฌ์ฉํ  ๋ ์ถ๊ฐํ  ๊ฒ
>

| ์์ฑ | ๊ธฐ๋ฅ |
| --- | --- |
| optional | false๋ก ์ค์ ํ๋ฉด ์ฐ๊ด๋ ์ํฐํฐ๊ฐ ๋ฐ๋์ ์์ด์ผ ํ๋ค. |
|  |  |

# 5.2 ์ฐ๊ด๊ด๊ณ ์ฌ์ฉ

## 5.2.1 ์ ์ฅ

### ํ์คํธ ์ฝ๋

```java
public static void testSave(EntityManager em) {
    //
    //ํ1 ์ ์ฅ
    Team team1 = new Team("team1", "ํ1");
    em.persist(team1);

    //ํ์1
    Member member1 = new Member("member1", "ํ์1", team1);
    em.persist(member1);
    //ํ์2
    Member member2 = new Member("member2", "ํ์2", team1);
    em.persist(member2);
}
```

> ํ์ด๋ธ ์์ฑ ์ฟผ๋ฆฌ โ **์ฐธ์กฐํ ํ์ ์๋ณ์ team.id๋ฅผ ์ธ๋ ํค๋ก ์ฌ์ฉํด์ ์ ์ ํ ๋ฑ๋ก ์ฟผ๋ฆฌ๋ฅผ ์์ฑํ๋ค.**
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4673861f-8ba1-4fe0-a2e4-291c51583327/Untitled.png)
>

> ์ถ๋ ฅ ํ์ธ
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b9e8ad69-3214-40da-b78b-7e5ec97528be/Untitled.png)
>

## 5.2.2 ์กฐํ

### ํ์คํธ ์ฝ๋ โ ๊ฐ์ฒด ๊ทธ๋ํ ํ์

> ์๋์ฒ๋ผ ๊ฐ์ฒด๋ฅผ ํตํด ์ฐ๊ด๋ ์ํฐํฐ๋ฅผ ์กฐํํ๋ ๊ฒ์ ๊ฐ์ฒด ๊ทธ๋ํ ํ์์ด๋ผ๊ณ  ํ๋ค.
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

### ํ์คํธ ์ฝ๋ โ ๊ฐ์ฒด ์งํฅ ์ฟผ๋ฆฌ ์ฌ์ฉ(JPQL)

> Member์ Team์ ์กฐ์ธ
>

```java
public static void finaAllJPQL(EntityManager em) {
        //
        List<Member> members = em.createQuery("select m from Member m join m.team t where " +
                "t.name =:teamName", Member.class)
                .setParameter("teamName", "ํ1")
                .getResultList();
        for (Member mem : members) {
            System.out.println(mem.getId());
            System.out.println(mem.getUsername());
            System.out.println(mem.getTeam().getName());
            System.out.println("======================");
        }
    }
```

> ์์ฑ ์ฟผ๋ฆฌ
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fd0f8040-87fc-46a7-a22a-bb13f913e9da/Untitled.png)
>

> ์ถ๋ ฅ ํ์ธ
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b9e8ad69-3214-40da-b78b-7e5ec97528be/Untitled.png)
>

## 5.2.3 ์์ 

> update๋ ๋ณ๋ ๋ฉ์๋๊ฐ ์๋ค - ์์์ ํ๋ฌ์ ๋ฐ์
>

```java
public static void updateRelation(EntityManager em) {
    //
    //์๋ก์ด ํ
    Team team2 = new Team("team2", "ํ2");
    em.persist(team2);

    //ํ์1์ ์๋ก์ด ํ2 ์ค์ 
    Member member = em.find(Member.class, "member1");
    member.setTeam(team2);
}
```

> ์์ฑ ์ฟผ๋ฆฌ
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/faf163b5-a23a-4ab3-8a20-e1d4d602ca0b/Untitled.png)
>

> ์ถ๋ ฅ๊ฒฐ๊ณผ
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/96c455a2-3ba1-48ef-a707-b45ec6f787d0/Untitled.png)
>

## 5.2.4 ์ฐ๊ด๊ด๊ณ ์ ๊ฑฐ

> ์ฐ๊ด๊ด๊ณ๋ฅผ null๋ก ์ค์  โ ์ผ๋ฐ์ ์ผ๋ก ์ถ๋ ฅํ๋ ค๊ณ  ํ๋ฉด ๋ํฌ์ธํธ ๋ฐ์
>

```java
public static void deleteRelation(EntityManager em) {
    //

    Member member1 = em.find(Member.class, "member1");
    member1.setTeam(null);
}
```

> ์์ฑ ์ฟผ๋ฆฌ
>
>
> TEAM_ID = null๋ก ์๋ฐ์ดํธ ๋๋ค.
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/27279e64-3c6c-48d0-a51a-dd4843942f8b/Untitled.png)
>

## 5.2.5 ์ฐ๊ด๋ ์ํฐํฐ ์ญ์ 

> ์ฐ๊ด๋ ์ํฐํฐ๋ฅผ ์ญ์ ํ๋ ค๋ฉด ๊ธฐ์กด์ ์๋ ์ฐ๊ด๊ด๊ณ๋ฅผ ๋จผ์  ์ ๊ฑฐํด์ผ ํ๋ค.
>
>
> ๊ทธ๋ ์ง ์์ผ๋ฉด ์ธ๋ ํค ์ ์ฝ ์กฐ๊ฑด์ผ๋ก ์ธํด DB์ ์ค๋ฅ๊ฐ ๋ฐ์ํจ
>

```java
public static void testSave(EntityManager em) {
    //
    //ํ1 ์ ์ฅ
    Team team1 = new Team("team1", "ํ1");
    em.persist(team1);

    //ํ์1
    Member member1 = new Member("member1", "ํ์1", team1);
    em.persist(member1);
    //ํ์2
    Member member2 = new Member("member2", "ํ์2", team1);
    em.persist(member2);

    em.remove(team1);
}
```

์๋ฌ ์ฌํญ โ ํธ๋์ญ์์ ์คํจํ๋ค.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7e713da4-9a39-4565-a773-b1107d95433a/Untitled.png)

### ์ ์ ์๋์

```java
public static void testSave(EntityManager em) {
    //
    //ํ1 ์ ์ฅ
    Team team1 = new Team("team1", "ํ1");
    em.persist(team1);

    //ํ์1
    Member member1 = new Member("member1", "ํ์1", team1);
    em.persist(member1);
    //ํ์2
    Member member2 = new Member("member2", "ํ์2", team1);
    em.persist(member2);

    member1.setTeam(null); //๋ฏธ๋ฆฌ ์ฐ๊ด๊ด๊ณ๋ฅผ ๋๋๋ค
    member2.setTeam(null);
    em.remove(team1);
}
```

> ์์ฑ ์ฟผ๋ฆฌ
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e40d142c-68f2-4db5-a1f9-ee20b1d83d5f/Untitled.png)
>

# 5.3 ์๋ฐฉํฅ ์ฐ๊ด๊ด๊ณ

## 5.3.1 ์๋ฐฉํฅ ์ฐ๊ด๊ด๊ณ ๋งคํ

> member์๋ ๋ณ๊ฒฝ์  ์์
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

    //์ฐ๊ด๊ด๊ณ ๋งคํ
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
}
```

## 5.3.2. ์ผ๋๋ค ์ปฌ๋ ์ ์กฐํ

๋ฐ์ดํฐ ์๋ ฅ ๋ก์ง ์์ -team์ ์กฐํํด์ ์ถ๋ ฅ๋๊ฒ ํ๋ ค๋ฉด team1์๋ Member๋ฅผ ๋ฃ์ด์ persistํด์ฃผ์ด์ผ ํ๋ค

```java
public static void testSave(EntityManager em) {
        //
        //ํ1 ์ ์ฅ
        Team team1 = new Team("team1", "ํ1");

        //ํ์1
        Member member1 = new Member("member1", "ํ์1", team1);
        em.persist(member1);
        //ํ์2
        Member member2 = new Member("member2", "ํ์2", team1);
        em.persist(member2);

        // ์ด๊ฑธ ์ถ๊ฐํด์ผํ๋ค๊ณ ??
        List<Member> members = new ArrayList<>();
        members.add(member1);
        members.add(member2);
        team1.setMembers(members);
        em.persist(team1);

    }
```

์ถ๋ ฅ๋ก์ง

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

> ์ถ๋ ฅ๊ฒฐ๊ณผ
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/445a278d-dfa4-4d9a-abe8-d01291f3a8a6/Untitled.png)
>

# 5.4 ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ

> ๊ฐ์ฒด์๋ ์๋ฐฉํฅ ์ฐ๊ด๊ด๊ณ๋ผ๋ ๊ฒ์ด ์๋ค - ํ๋ด๋ง ๋ผ ๋ฟ
>
>
> ํ์ด๋ธ์ ์ธ๋ ํค ํ๋๋ง์ผ๋ก ์๋ฐฉํฅ ์ฐ๊ด๊ด๊ณ๋ฅผ ๋งบ๋๋ค.
>
> โ ์ด๋ฐ ์ฐจ์ด๋ก ์ธํด JPA๋ ๋ ๊ฐ์ฒด ์ฐ๊ด๊ด๊ณ ์ค ํ๋๋ฅผ ์ ํด์ ํ์ด๋ธ์ ์ธ๋ ํค๋ฅผ ๊ด๋ฆฌ ํด์ผ ํ๋๋ฐ ์ด๊ฒ์ `์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ(Owner)`๋ผ๊ณ  ํ๋ค.
>

## 5.4.1 ์๋ฐฉํฅ ๋งคํ์ ๊ท์น : ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ

> ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ๋ง์ด DB์ฐ๊ด๊ด๊ณ์ ๋งคํ๋๊ณ  ์ธ๋ ํค๋ฅผ ๊ด๋ฆฌ(CUD)ํ  ์ ์๋ค.
>
>
> ์ฃผ์ธ์ด ์๋ ์ชฝ์ ์ฝ๊ธฐ(R)๋ง ํ  ์ ์๋ค.
>
> โ ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ์ ์ ํ๋ค๋ ๊ฒ์ ์ธ๋ ํค์ ๊ด๋ฆฌ์๋ฅผ ์ ํํ๋ ๊ฒ์ด๋ค.
>

Member entity

```java
public class Member {
    //์ฐ๊ด๊ด๊ณ ๋งคํ
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

> ์์ ๊ฒฝ์ฐ Member entity๋ฅผ ์ฃผ์ธ์ผ๋ก ์ ํํ๋ฉด Member ํ์ด๋ธ์ team_id ์ธ๋ ํค๋ฅผ ๊ด๋ฆฌํ๋ฉด ๋๋ค.
>
>
> ๋ฐ๋ฉด Team์ ์ฃผ์ธ์ผ๋ก ์ ํํ  ๊ฒฝ์ฐ ๋ฌผ๋ฆฌ์ ์ผ๋ก ์ ํ ๋ค๋ฅธ ํ์ด๋ธ์ ์ธ๋ ํค๋ฅผ ๊ด๋ฆฌํด์ผ ํ๋ค.
>

## 5.4.2 ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ์ ์ธ๋ ํค๊ฐ ์๋ ๊ณณ

> ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ๋ง DB์ฐ๊ด๊ด๊ณ์ ๋งคํ๋๊ณ  ์ธ๋ํค๋ฅผ ๊ด๋ฆฌํ  ์ ์๋ค.
>
>
> ๋ฐ๋ํธ์ ์ฝ๊ธฐ๋ง ๊ฐ๋ฅํ๋ค.
>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4fe62713-88d1-4668-bf40-0f7d52dc3b84/Untitled.png)

<aside>
๐ก DB์์ ํ์ด๋ธ์ ๊ด๊ณ๋ ํญ์ ๋ค ์ชฝ์ด ์ธ๋ ํค๋ฅผ ๊ฐ์ง๋ค.

๋ค ์ชฝ์ธ @ManyToOne์ ํญ์ ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ์ด ๋๋ฏ๋ก mappedBy๋ฅผ ์ค์ ํ  ์ ์๋ค.

</aside>

# 5.5 ์๋ฐฉํฅ ์ฐ๊ด๊ด๊ณ ์ ์ฅ

> Team์ Member๋ฅผ ๋ณ๋๋ก ์ ์ฅํ์ง ์์๋ Member์์ ์ ์ฅํ Team๊ฐ์ผ๋ก ์ธํด Memberํ์ด๋ธ์๋ Team์ด ์ ์ฅ๋๋ค.
>

์๋ ฅ

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/bde43116-8ba7-4053-be4c-57999ae4f568/Untitled.png)

์ถ๋ ฅ

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a6549244-1596-42c3-9f6d-9ad967db5633/Untitled.png)

> ์คํ ๊ฒฐ๊ณผ
>
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3f12e243-4990-45cb-8818-db82dcd6197a/Untitled.png)
>

# 5.6 ์๋ฐฉํฅ ์ฐ๊ด๊ด๊ณ์ ์ฃผ์์ 

> ์๋ฐฉํฅ ๊ด๊ณ๋ฅผ ์ค์ ํ๊ณ  ํํ ํ๋ ์ค์๋ ์ฃผ์ธ์๋ ๊ฐ์ ์๋ ฅํ์ง ์๊ณ  ์ฃผ์ธ์ด ์๋ ๊ณณ์๋ง ์๋ ฅํ๋ ๊ฒ์ด๋ค. DB์ ์ธ๋ ํค ๊ฐ์ด ์ ์์ ์ด์ง ์๋ค๋ฉด ์ด๊ฒ์ ์์ฌํ  ๊ฒ.
>

## 5.6.1 ์์ํ ๊ฐ์ฒด๊น์ง ๊ณ ๋ คํ ์๋ฐฉํฅ ์ฐ๊ด๊ด๊ณ

> ๊ฐ์ฒด ๊ด์ ์์ ์์ชฝ ๋ฐฉํฅ ๋ชจ๋ ๊ฐ์ ์๋ ฅํด์ฃผ๋ ๊ฒ์ด ์์ ํ๋ค.
>
>
> ์์ 5.3.2์์ ํ ๊ฒ์ฒ๋ผ Member, Team ์์ชฝ ๋ค ๋ฐ์ดํฐ๋ฅผ ๋ฃ์ด์ฃผ์ด์ผ ์ง์ง ์๋ฑกํฅ ์ฐ๊ด๊ด๊ณ๋ผ ํ  ์ ์๋ค. โ Member์์ ๊ฒ์ํ๋  Team์์ ๊ฒ์ํ๋  ๊ฐ์ ๊ฒฐ๊ณผ๊ฐ ๋์ด
>

์๋ ฅ ์ผ๋ถ ์์ 

```java
public static void testSave(EntityManager em) {
    //
    //ํ1 ์ ์ฅ
    Team team1 = new Team("team1", "ํ1");

    //ํ์1
    Member member1 = new Member("member1", "ํ์1", team1);
    em.persist(member1);
    //ํ์2
    Member member2 = new Member("member2", "ํ์2", team1);
    em.persist(member2);
		
		//team์ Member์ ๋ฐ์ดํฐ๋ค์ ์ถ๊ฐํด์ฃผ์๋ค
    team1.getMembers().add(member1);
    team1.getMembers().add(member2);
    em.persist(team1);

}
```

## 5.6.2 ์ฐ๊ด๊ด๊ณ ํธ์ ๋ฉ์๋

> ์์ฒ๋ผ `member.set(team1);`(๋๋ ์์ฑ์๋ก ๋์ฒด)  `team1.getMembers().add(member1);` ๋ฅผ ๊ฐ๊ฐ ํธ์ถํ๋ค ๋ณด๋ฉด ์ค์๋ก ๋ ์ค ํ๋๋ง ํธ์ถํด์ ์๋ฐฉํฅ์ด ๊นจ์ง ์ ์๋ค.
>

์ด๋ด ๋๋ Memberํด๋์ค์ setTeam()์ ์์ ํด์ ์ฌ์ฉํ๋ ๊ฒ์ด ์์ ํ๋ค.

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

    //์ฐ๊ด๊ด๊ณ ๋งคํ
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;
		
		//์์ฑ์ ์ถ๊ฐ
    public Member (String id, String username) {
        //
        this.id = id;
        this.username = username;
    }
		
		//setter์์ 
    public void setTeam(Team team) {
        //
        this.team = team;
        team.getMembers().add(this);
    }
}
```

์์  ์ถ๋ ฅ ๋ก์ง

```java
public static void testSave2(EntityManager em) {
    //
    //ํ1 ์ ์ฅ
    Team team1 = new Team("team1", "ํ1");
    em.persist(team1);
    
    //ํ์1
    Member member1 = new Member("member1", "ํ์1");
    member1.setTeam(team1);
    em.persist(member1);
    //ํ์2
    Member member2 = new Member("member2", "ํ์2");
    member2.setTeam(team1);
    em.persist(member2);
}
```

## 5.6.3 ์ฐ๊ด๊ด๊ณ ํธ์ ๋ฉ์๋ ์์ฑ ์ ์ฃผ์์ฌํญ

> team์ ๋ณ๊ฒฝํ๋ ๊ฒฝ์ฐ ๊ธฐ์กด์ ๊ด๊ณ๊ฐ ์ ๊ฑฐ๋์ง ์๋๋ค โ ์ ๊ฑฐ ์ฝ๋๋ฅผ ์ถ๊ฐํด์ฃผ์ด์ผ ํจ
>

```java
//setter์์ 
    public void setTeam(Team team) {
        //
        //๊ธฐ์กด ๊ด๊ณ ์ ๊ฑฐ
        if (this.team != null) {
            this.team.getMembers().remove(this);
        }
        this.team = team;
        team.getMembers().add(this);
    }
```

<aside>
โ ์ฌ์ค teamA โ member1 ๊ด๊ณ๊ฐ ์ ๊ฑฐ๋์ง ์์๋ ๋ฌธ์ ๋ ์์

Team.members๋ ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ์ด ์๋๊ธฐ ๋๋ฌธ

๋ํ ์๋ก์ด ์์์ฑ ์ปจํ์คํธ์์ teamA๋ฅผ ์กฐํํด์ `teamA.getMembers()`๋ฅผ ํธ์ถํด๋ ์ธ๋ ํค์๋ ๊ด๊ณ๊ฐ ๋์ด์ ธ ์์ผ๋ฏ๋ก ์กฐํ๋์ง ์๋๋ค.

๋ค๋ง ๊ด๊ณ๋ฅผ ๋ณ๊ฒฝํ๊ณ  ์์์ฑ ์ปจํ์คํธ๊ฐ ์ด์์๋ ์ํ๋ผ๋ฉด `teamA.getMembers()`๋ฅผ ์คํํ  ๊ฒฝ์ฐ member1์ด ์กฐํ๋์ด๋ฒ๋ฆฐ๋ค.

</aside>

# 5.7 ์ ๋ฆฌ

> ์๋ฐฉํฅ์ ๋จ๋ฐฉํฅ๊ณผ ๋น๊ตํด์ ๋ฐ๋๋ฐฉํฅ์ผ๋ก ๊ฐ์ฒด ๊ทธ๋ํ ํ์ ๊ธฐ๋ฅ์ ์ถ๊ฐ ํ ๊ฒ ๋ฟ
>
>
> ๊ฐ์ฒด ๊ทธ๋ํ ํ์์ด ํ์ํ  ๋ ์๋ฐฉํฅ์ ์ฌ์ฉํ๋๋ก ํ๋ค.
>

<aside>
โ ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ์ ์ธ๋ ํค์ ์์น์ ๊ด๋ จํด์ ์ ํด์ผ์ง ๋น์ฆ๋์ค ์ค์๋๋ก ์ ๊ทผํด์๋ ์๋๋ค.

๋ํ ์๋ฐฉํฅ ๋งคํ ์ ๋ฌดํ๋ฃจํ๋ฅผ ์ฃผ์ํ  ๊ฒ - ์๋ก๋ฅผ ์ฐธ์กฐํ  ์ ์๊ธฐ ๋๋ฌธ์ด๋ค.

</aside>