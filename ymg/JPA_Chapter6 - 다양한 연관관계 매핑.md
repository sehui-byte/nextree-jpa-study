<aside>
๐ก ์ด๋ฒ ์ฅ์์ ์ค์ํ ๊ฒ

- ๋ค์ค์ฑ
    - ๋ค๋์ผ(@ManyToOne)
    - ์ผ๋๋ค(@OneToMany)
    - ์ผ๋์ผ(@OneToOne)
    - ๋ค๋๋ค(@ManyToMany)
- ๋ฐฉํฅ
    - ๋จ๋ฐฉํฅ
    - ์๋ฐฉํฅ
- ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ
</aside>

# 6.1 ๋ค๋์ผ

> DB์ 1 : N ๊ด๊ณ์์ ์ธ๋ ํค๋ ํญ์ N์ชฝ์ ์๋ค.
>

## 6.1.1 ๋ค๋์ผ ๋จ๋ฐฉํฅ [N : 1]

![img_3.png](Chapter6_pic/img_b.png)

*`@JoinColumn`*์ ์ฌ์ฉํด์ [Member.team](http://Member.team) ํ๋๋ฅผ `TEAM_ID`์ ๋งคํํจ

```sql
//์ฐ๊ด๊ด๊ณ ๋งคํ
@ManyToOne
@JoinColumn(name = "TEAM_ID")
private Team team;
```

## 6.1.2 ๋ค๋์ผ ์๋ฐฉํฅ [N:1, 1:N]

![img_4.png](Chapter6_pic/img_c.png)

### Member

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
public class Member {
    //
    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private long id;

    private String username;

    //์ฐ๊ด๊ด๊ณ ๋งคํ
    @ManyToOne
    @JoinColumn(name = "TEAM_ID")
    private Team team;

    //setter์์ 
    public void setTeam(Team team) {
        //
        this.team = team;

        //๋ฌดํ๋ฃจํ ๋ฐฉ์ง
        if (!team.getMembers().contains(this)) {
            team.getMembers().add(this);
        }
    }
}
```

### Team

```java
@Getter
@Setter
@Entity
@NoArgsConstructor
@AllArgsConstructor
public class Team {
    //
    @Id
    @Column(name = "TEAM_ID")
    private Long id;

    private String name;

    @OneToMany(mappedBy = "team")
    private List<Member> members = new ArrayList<>();

    public void addMember (Member member) {
        //
        this.members.add(member);
        if (member.getTeam() != this) { //๋ฌดํ๋ฃจํ ๋ฐฉ์ง
            member.setTeam(this);
        }
    }
}
```

> Point
>
> - **์๋ฐฉํฅ์ ์ธ๋ ํค๊ฐ ์๋ ์ชฝ์ด ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ์ด๋ค.**
    >
    >     ์ฃผ์ธ์ด ์๋ ์ชฝ์ JPQL, ๊ฐ์ฒด ๊ทธ๋ํ ํ์์์ ์ฌ์ฉํ๋ค.
>
> - **์๋ฐฉํฅ ์ฐ๊ด๊ด๊ณ๋ ํญ์ ์๋ก๋ฅผ ์ฐธ์กฐํด์ผํ๋ค.**
    >
    >     ๋จ, ๋ฌดํ๋ฃจํ ๋ฐฉ์ง ๋ฉ์๋๋ฅผ ์์ฑํด์ฃผ์ด์ผ ํ๋ค. (ex `setTeam`, `addMember`)
>

# 6.2 ์ผ๋๋ค

> ์ํฐํฐ๋ฅผ ํ๋ ์ด์ ์ฐธ์กฐํ  ์ ์๋ค โ ์๋ฐ ์ปฌ๋ ์์ ์ฌ์ฉํด์ผํ๋ค.
>

## 6.2.1 ์ผ๋๋ค ๋จ๋ฐฉํฅ [1:N]

![img_5.png](Chapter6_pic/img_d.png)

> ์ผ๋๋ค ๋จ๋ฐฉํฅ ๊ด๊ณ๋ ์ฝ๊ฐ ํน์ดํ๋ค
>
>
> ์ธ๋ ํค๋ ํญ์ ๋ค์ชฝ ํ์ด๋ธ์ ์๋ค
>
> ํ์ง๋ง ๋ค ์ชฝ์ธ Member ์ํฐํฐ์๋ ์ธ๋ ํค๋ฅผ ๋งคํํ  ์ ์๋ ์ฐธ์กฐ ํ๋๊ฐ ์๋ค.
>

### *`Member`*

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
public class Member {
    //
    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private long id;

    private String username;
```

### *`Team`*

```java
@Getter
@Setter
@Entity
@NoArgsConstructor
@AllArgsConstructor
public class Team {
    //
    @Id
		@GeneratedValue
    @Column(name = "TEAM_ID")
    private Long id;

    private String name;

    @OneToMany
    @JoinColumn(name = "TEAM_ID") // Memberํ์ด๋ธ์ TEAM_ID(FK)
    private List<Member> members = new ArrayList<>();
```

> ์ผ๋๋ค ๋จ๋ฐฉํฅ ๊ด๊ณ๋ฅผ ๋งคํํ  ๋๋ *`@JoinColumn`*์ ๋ช์ํด์ผํ๋ค.
>
>
> ๊ทธ๋ ์ง ์์ผ๋ฉด JPA๋ ์ฐ๊ฒฐ ํ์ด๋ธ์ ์ค๊ฐ์ ๋๊ณ  ์ฐ๊ด๊ด๊ณ๋ฅผ ๊ด๋ฆฌํ๋ ์กฐ์ธํ์ด๋ธ ์ ๋ต์ ๊ธฐ๋ณธ์ผ๋ก ์ฌ์ฉํด์ ๋งคํํ๋ค.
>

### ์ผ๋๋ค ๋จ๋ฐฉํฅ ๋งคํ์ ๋จ์ 

> ๋งคํํ ๊ฐ์ฒด๊ฐ ๊ด๋ฆฌํ๋ ์ธ๋ ํค๊ฐ ๋ค๋ฅธ ํ์ด๋ธ์ ์๋ค.
>
>
> ๋นจ๊ฐ ์ค์ ๋ถ๋ถ์์ ๋ณด์ด๋ ๊ฒ ์ฒ๋ผ ๋ณธ์ธ ํ์ด๋ธ์ ์ธ๋ ํค๊ฐ ์์ผ๋ฉด ์ ์ฅ๊ณผ ์ฐ๊ด๊ด๊ณ ์ฒ๋ฆฌ๋ฅผ insertํ๋ฒ์ ๋๋ผ ์ ์์ง๋ง, ๋ค๋ฅธ ํ์ด๋ธ์ ์๊ธฐ ๋๋ฌธ์ update๊ฐ ํ ๋ฒ ๋ ๋์ด์ผ ํ๋ค.
>

```java
public static void testSave(EntityManager em) {
    //
    Member member1 = new Member("member1");
    Member member2 = new Member("member2");

    Team team1 = new Team("team1");
    team1.getMembers().add(member1);
    team1.getMembers().add(member2);

    em.persist(member1);
    em.persist(member2);
    em.persist(team1);
}
```

์คํ ์ฟผ๋ฆฌ

![img_2.png](Chapter6_pic/img_2.png)

<aside>
๐ก โ ์ฑ๋ฅ, ๊ด๋ฆฌ๋ฌธ์ ๊ฐ ์์ผ๋ ๋ค๋์ผ ์๋ฐฉํฅ ๋งคํ์ ์ฌ์ฉํ์

</aside>

## 6.2.2 ์ผ๋๋ค ์๋ฐฉํฅ [1:N, N:1]  - ์ด๋ก ๋ง ์ ๋ฆฌ

> ์ธ๋ ํค๋ ํญ์ ๋ค ์ชฝ์ ์๋ค
>
>
> ํ์ง๋ง ์ผ ์ชฝ์๋ ์ธ๋ ํค๋ฅผ ์ถ๊ฐํ  ์๋ ์๋ค.
>
> @OneToMany์ชฝ์ @JoinColumn์ ์ถ๊ฐํ ๋ค์, @ManyToOne์ @JoinColumn์๋ ์ฝ๊ธฐ ์ ์ฉ ์ต์๋ง ๋ถ์ด๋ ์
>
> ```java
> @ManyToOne
> @JoinColumn(name = "TEAM_ID", insertable = false,
>         updatable = false)
> private Team team;
> ```
>
> (๋ ๋ค ์ธ๋ ํค๋ฅผ ๊ด๋ฆฌํ๋ ๋ฌธ์ ๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด)
>

# 6.3 ์ผ๋์ผ [1:1]

<aside>
๐ก ํน์ง

- ์ผ๋์ผ ๊ด๊ณ๋ ๊ทธ ๋ฐ๋๋ ์ผ๋์ผ
- ์ผ๋์ผ ๊ด๊ณ๋ ์ฃผ ํ์ด๋ธ์ด๋ ๋์ ํ์ด๋ธ ๋ ์ค ์ด๋ ๊ณณ์ด๋ ์ธ๋ ํค๋ฅผ ๊ฐ์ง ์ ์๋ค.

  โ ์ผ๋์ผ ๊ด๊ณ๋ ์ฃผ ํ์ด๋ธ์ด๋ ๋์ ํ์ด๋ธ ์ค์ ๋๊ฐ ์ธ๋ํค๋ฅผ ๊ฐ์ง์ง ์ ํํด์ผ ํ๋ค.

    - ์ฃผ ํ์ด๋ธ์ ์ธ๋ ํค

      ์ฃผ ํ์ด๋ธ๋ง ํ์ธํด๋ ๋์ ํ์ด๋ธ๊ณผ ์ฐ๊ด๊ด๊ณ๊ฐ ์๋์ง ์ ์ ์๋ค.

    - ๋์ ํ์ด๋ธ์ ์ธ๋ ํค

      ์ผ๋์ผ โ ์ผ๋๋ค ๋ณ๊ฒฝ ์ ํ์ด๋ธ ๊ตฌ์กฐ๋ฅผ ๊ทธ๋๋ก ์ ์งํ  ์ ์๋ค.

</aside>

## 6.3.1 ์ฃผ ํ์ด๋ธ์ ์ธ๋ ํค

### ๋จ๋ฐฉํฅ

> ๋ค๋์ผ ๋จ๋ฐฉํฅ๊ณผ ์ ์ฌ
>

*`Member`*

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
public class Member {
    //
    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private long id;

    private String username;

    @OneToOne
    @JoinColumn(name = "LOCKER_ID")
    private Locker locker;
```

*`Locker`*

```java
@Getter
@Setter
@Entity
@NoArgsConstructor
@AllArgsConstructor
public classLocker{
//
@Id
    @GeneratedValue
    @Column(name = "LOCKER_ID")
privateLongid;

privateStringname;
}
```

ํ์ด๋ธ ์์ฑ ์ฟผ๋ฆฌ

![img_3.png](Chapter6_pic/img_3.png)

### ์๋ฐฉํฅ

> locker์์ member๋ฅผ ๋งคํํ๋๋ก ๋ณ๊ฒฝ
>
>
> ์๋ฐฉํฅ์ด๋ฏ๋ก ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ์ ์ ํด์ผ ํ๋ค. Memberํ์ด๋ธ์ด ์ธ๋ ํค๋ฅผ ๊ฐ์ง๊ณ  ์์ผ๋ฏ๋ก `Member.locker`๊ฐ ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ์ด๋ค.
>
> ๋ฐ๋ํธ์ `Locker.member`๋ `mappedBy`๋ฅผ ์ ์ธํด์ ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ์ด ์๋๋ผ๊ณ  ์ค์ ํ๋ค.
>

*`Locker`*

```java
@Getter
@Setter
@Entity
@NoArgsConstructor
@AllArgsConstructor
public class Locker {
    //
    @Id
    @GeneratedValue
    @Column(name = "LOCKER_ID")
    private Long id;

    private String name;

    @OneToOne(mappedBy = "locker")
    private Member member;
}
```

## 6.3.2 ๋์ ํ์ด๋ธ์ ์ธ๋ ํค

### ๋จ๋ฐฉํฅ

> ๋์ ํ์ด๋ธ์ ์ธ๋ ํค๊ฐ ์๋ ๋จ๋ฐฉํฅ ๊ด๊ณ๋ JPA์์ ์ง์ํ์ง ์๋๋ค.
>

### ์๋ฐฉํฅ

![img_4.png](Chapter6_pic/img_4.png)

*`Member`*

```java
@Getter
@Setter
@NoArgsConstructor
@AllArgsConstructor
@Entity
public class Member {
    //
    @Id
    @GeneratedValue
    @Column(name = "MEMBER_ID")
    private long id;

    private String username;

    @OneToOne(mappedBy = "member")
    private Locker locker;
```

*`Locker`*

```java
@Getter
@Setter
@Entity
@NoArgsConstructor
@AllArgsConstructor
public class Locker {
    //
    @Id
    @GeneratedValue
    @Column(name = "LOCKER_ID")
    private Long id;

    private String name;

    @OneToOne
    @JoinColumn(name = "MEMBER_ID")
    private Member member;
}
```

์์ฑ ์ฟผ๋ฆฌ

![img_5.png](Chapter6_pic/img_5.png)

# 6.4 ๋ค๋๋ค [N:N]

> ๋ค๋๋ค ๊ด๊ณ๋ฅผ ํ์ด๋ธ๋ก ํํ ํ  ๋ ์ ๊ทํ๋ ํ์ด๋ธ 2๊ฐ๋ก ํํํ  ์ ์๋ค. โ ์ฐ๊ฒฐ ํ์ด๋ธ์ด ํ์ํจ
>
>
> ํ์ง๋ง ๊ฐ์ฒด๋ 2๊ฐ๋ก ๋ฌ์ฌํ  ์ ์๊ณ  ์ด๋ฌํ ๊ฐ์ฒด๋ฅผ `@ManyToMany`๋ก ๋งคํํ  ์ ์๋ค
>

## 6.4.1 ๋จ๋ฐฉํฅ

### ์ํฐํฐ ์ค๊ณ

*`Member`*

```java
@Entity
public class Member {
    //
    @Id
    @Column(name = "MEMBER_ID")
    private String id;

    private String username;

    @ManyToMany
    @JoinTable(name = "MEMBER_PRODUCT",
    joinColumns = @JoinColumn(name = "MEMBER_ID"),
    inverseJoinColumns = @JoinColumn(name = "PRODUCT_ID"))
    private List<Product> product = new ArrayList<>();
```

*`Product`*

```java
@Entity
public class Product {
    //
    @Id
    @Column(name = "PRODUCT_ID")
    private String id;

    private String name;
```

์์ฑ ์ฟผ๋ฆฌ

![img_6.png](Chapter6_pic/img_6.png) 

### *`@JoinTable`* ์์ฑ

- name : ์ฐ๊ฒฐ ํ์ด๋ธ์ ์ง์ 
- joinColumns : ํ์ฌ ๋ฐฉํฅ์ธ ํ์๊ณผ ๋งคํํ  ์กฐ์ธ ์ปฌ๋ผ ์ ๋ณด๋ฅผ ์ ์ฅ
- inverseJoinColumns : ๋ฐ๋ ๋ฐฉํฅ์ธ ์ํ๊ณผ ๋งคํํ  ์กฐ์ธ ์ปฌ๋ผ ์ ๋ณด๋ฅผ ์ง์ 

### save ๋ก์ง

```java
public static void testSave(EntityManager em) {
    //
    Product productA = new Product();
    productA.setId("product1");
    productA.setName("์ํA");
    em.persist(productA);

    Member member1 = new Member("ํ์1");
    member1.setId("member1");
    member1.getProduct().add(productA);
    em.persist(member1);
}
```

์์ฑ ์ฟผ๋ฆฌ

![img_7.png](Chapter6_pic/img_7.png)

### find ๋ก์ง

```java
public static void find(EntityManager em) {
    //
    Member member = em.find(Member.class, "member1");
    List<Product> products = member.getProduct();
    for (Product product : products) {
        System.out.println("Product name ::: " + product.getName());
    }
}
```

find๊ฒฐ๊ณผ

![img_8.png](Chapter6_pic/img_8.png)

## 6.4.2 ์๋ฐฉํฅ

> ๋ค๋๋ค ๋งคํ์ด๋ฏ๋ก ์ญ๋ฐฉํฅ๋ `@ManyToMany`๋ฅผ ์ฌ์ฉํ๋ค. + mappedBy๋ก ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ์ ์ค์ 
>

### ์ํฐํฐ

*`Product`*

```java
public class Product {
    //
    @Id
    @Column(name = "PRODUCT_ID")
    private String id;
    private String name;

    @ManyToMany(mappedBy = "products")
    private List<Member> member;
}
```

### ๊ฐ์ฒด ๊ทธ๋ํ ํ์

> ์๋ฐฉํฅ ์ฐ๊ด๊ด๊ณ๋ฅผ ๋ง๋ค์์ผ๋ฏ๋ก ์ญ๋ฐฉํฅ์ผ๋ก ๊ฐ์ฒด๊ทธ๋ํ๋ฅผ ํ์ํ  ์ ์๋ค.
>

```java
public static void findInverse(EntityManager em){
    //
    Product product = em.find(Product.class, "product1");
    List<Member> members = product.getMembers();
    for (Member member : members) {
        System.out.println("member = " + member.getUsername());
    }
}
```

## 6.4.3 ๋ค๋๋ค ๋งคํ์ ํ๊ณ์ ๊ทน๋ณต, ์ฐ๊ฒฐ ์ํฐํฐ ์ฌ์ฉ

> `@ManyToMany`๋ฅผ ์ฌ์ฉํ๋ฉด ์ฐ๊ฒฐ ํ์ด๋ธ์ ์๋์ผ๋ก ์ฒ๋ฆฌํด์ฃผ๋ฏ๋ก ๋๋ฉ์ธ ๋ชจ๋ธ์ด ๋จ์ํด์ ธ ํธ๋ฆฌ
>
>
> ํ์ง๋ง ์ค๋ฌด์์ ์ฌ์ฉํ๊ธฐ์๋ ํ๊ณ๊ฐ ์๋๋ฐ, ์ฐ๊ฒฐ ํ์ด๋ธ์ ์ปฌ๋ผ์ ์ถ๊ฐํ  ๊ฒฝ์ฐ ๋ค๋ฅธ ์ํฐํฐ์๋ ์ถ๊ฐํ ์ปฌ๋ผ๋ค์ ๋งคํํ  ์ ์๊ธฐ ๋๋ฌธ
>

![img_9.png](Chapter6_pic/img_9.png)

> ์ด๋ฅผ ํด๊ฒฐํ๊ธฐ ์ํด ๊ฒฐ๊ตญ ์ฐ๊ฒฐ ์ํฐํฐ๋ฅผ ๋ง๋ค๊ณ  ํด๋น ์ํฐํฐ๋ฅผ ์ค์ฌ์ผ๋ก ๋ค๋์ธ, ์ผ๋๋ค ๊ด๊ณ๋ก ํ์ด๋ด์ผ ํ๋ค.
>

### ์ํฐํฐ

*`Member`*

> ํ์๊ณผ ํ์_์ํ์ ์๋ฐฉํฅ
>
>
> ํ์_์ํ ์ชฝ์ด ์ธ๋ ํค๋ฅผ ๊ฐ์ง๊ณ  ์์ผ๋ฏ๋ก ์ฐ๊ด๊ด๊ณ์ ์ฃผ์ธ
>

```java
public class Member {
    //
    @Id
    @Column(name = "MEMBER_ID")
    private String id;

    //์ญ๋ฐฉํฅ
    @OneToMany(mappedBy = "member")
    private List<MemberProduct> memberProducts;
}
```

*`Product`*

> ์ํ ์์ ํ์_์ํ์ ํ์ํ  ํ์๊ฐ ์์ผ๋ฏ๋ก ์ฐ๊ด๊ด๊ณ ์์
>

```java
public class Product {
    //
    @Id
    @Column(name = "PRODUCT_ID")
    private String id;

    private String name;
}
```

*`MemberProduct`*

> @Id์ @JoinColumn์ ๋์์ ์ฌ์ฉํด์ ๊ธฐ๋ณธ ํค + ์ธ๋ ํค๋ฅผ ํ๋ฒ์ ๋งคํ
>
>
> @IdClass๋ฅผ ์ฌ์ฉํด์ ๋ณตํฉ ๊ธฐ๋ณธํค๋ฅผ ๋งคํ
>

```java
@Entity
@IdClass(MemberProductId.class)
public class MemberProduct {
    //
    @Id
    @ManyToOne
    @JoinColumn(name = "MEMBER_ID")
    private Member member; //MemberProductId.member์ ์ฐ๊ฒฐ

    @Id
    @ManyToOne
    @JoinColumn(name = "PRODUCT_ID")
    private Product product; //MemberProductId.product์ ์ฐ๊ฒฐ

    private int orderAmount;
}
```

*`MemberProductId`*

```java
public class MemberProductId implements Serializable {
    //
    private String member; //MemberProduct.member ์ ์ฐ๊ฒฐ
    private String product; //MemberProduct.product ์ ์ฐ๊ฒฐ

}
```

### ๋ณตํฉ ๊ธฐ๋ณธ ํค

> ํ์ ์ํ ์ํฐํฐ๋ ๊ธฐ๋ณธ ํค๊ฐ MEMBER_ID์ PRODUCT_ID๋ก ์ด๋ฃจ์ด์ง ๋ณตํฉ ํค์ด๋ค.
>
>
> ์ด๋ ๊ฒ ์ฌ์ฉํ๋ ค๋ฉด ๋ณ๋์ ์๋ณ์ ํด๋์ค๋ฅผ ๋ง๋ค์ด์ผ ํ๋ค. โ ์ฌ๊ธฐ์๋ *`MemberProductId`*
>

์๋ณ์ ํด๋์ค์ ํน์ง

- ๋ณตํฉ ํค๋ ๋ณ๋์ ์๋ณ์ ํด๋์ค๋ก ๋ง๋ค์ด์ผ ํ๋ค.
- Serializable์ ๊ตฌํํด์ผ ํ๋ค.
- equals์ hashCode๋ฉ์๋๋ฅผ ๊ตฌํํด์ผ ํ๋ค. - ๋๋ถ๋ถ ์๋ฐ IDE์๋ ์๋ ์์ฑ๊ธฐ๋ฅ์ด ์์
- ๊ธฐ๋ณธ ์์ฑ์๊ฐ ์์ด์ผ ํ๋ค.
- ์๋ณ์ ํด๋์ค๋ public
- `@IdClass` ์ธ์๋ `@EmbeddidId`๋ฅผ ์ฌ์ฉํ๋ ๋ฐฉ๋ฒ๋ ์๋ค

<aside>
โก๏ธ ๊ฐ์ด ๋ณด๊ธฐ

[๋ณตํฉํค](https://www.notion.so/861c56e896e94c0ca19d9a054cab0ec2)

</aside>

### ์๋ณ ๊ด๊ณ

> ๋ถ๋ชจ ํ์ด๋ธ์ ๊ธฐ๋ณธ ํค๋ฅผ ๋ฐ์์ ์์ ์ ๊ธฐ๋ณธ ํค + ์ธ๋ ํค๋ก ์ฌ์ฉํ๋ ๊ฒ์ ์๋ณ ๊ด๊ณ(Identifying Relationship) ์ด๋ผ ํ๋ค.
>
>
> โ MemberProduct๋ MemberProductId ์๋ณ์ ํด๋์ค๋ก ๋ ๊ธฐ๋ณธ ํค๋ฅผ ๋ฌถ์ด์ ์์ ์ ํค - ๋ณตํฉ ๊ธฐ๋ณธ ํค๋ก ์ฌ์ฉํ๋ค.
>

### ํ์ด๋ธ ์์ฑ ์ฟผ๋ฆฌ

MemberProduct์์ ์์ชฝ์ FK๋ฅผ ๊ฐ์ง๊ณ  ์๋ค

![img_10.png](Chapter6_pic/img_10.png)

### save ์ฝ๋

```java
public static void testSave(EntityManager em) {
    //ํ์ ์ ์ฅ
    Member member1 = new Member();
    member1.setId("member1");
    member1.setUserName("ํ์1");
    em.persist(member1);

    //์ํ ์ ์ฅ
    Product productA = new Product();
    productA.setId("productA");
    productA.setName("์ํA");
    em.persist(productA);

    //ํ์์ํ ์ ์ฅ
    MemberProduct memberProduct = new MemberProduct();
    memberProduct.setMember(member1);
    memberProduct.setProduct(productA);
    memberProduct.setOrderAmount(2);
    em.persist(memberProduct);
}
```

์ถ๋ ฅ ์ฟผ๋ฆฌ

![img_10.png](Chapter6_pic/img_10.png)

### find ์ฝ๋

> ๋ณตํฉ ํค๋ ํญ์ ์๋ณ์ ํด๋์ค๋ฅผ ๋ง๋ค์ด์ ์กฐํํด์ผ ํ๋ค.
>
>
> โ ๋ณต์กํจ
>

```java
public static void testFind(EntityManager em) {
    //๊ธฐ๋ณธ ํค ๊ฐ ์์ฑ
    MemberProductId memberProductId = new MemberProductId();
    memberProductId.setMember("member1");
    memberProductId.setProduct("productA");

    MemberProduct memberProduct = em.find(MemberProduct.class,
            memberProductId);

    Member member = memberProduct.getMember();
    Product product = memberProduct.getProduct();

    System.out.println("member :::: " + member.getUserName());
    System.out.println("product :::: " + product.getName());
    System.out.println("orderAmount :::: " + memberProduct.getOrderAmount());

}
```

์ถ๋ ฅ ๊ฒฐ๊ณผ

![img_11.png](Chapter6_pic/img_11.png)

## 6.4.4 ๋ค๋๋ค : ์๋ก์ด ๊ธฐ๋ณธ ํค ์ ์ฉ

> DB์์ ์๋์ผ๋ก ์์ฑํด์ฃผ๋ ๋๋ฆฌ ํค๋ฅผ Long ๊ฐ์ผ๋ก ์ฌ์ฉํ๋ ๊ฒ
>
>
> โ ORM๋งคํ์ ๋ณตํฉ ํค๋ฅผ ๋ง๋ค์ง ์์๋ ๋๋ฏ๋ก ๊ฐ๋จํ ์์ฑ ๊ฐ๋ฅ
>

Order๋ก ์ด๋ฆ ๋ณ๊ฒฝ - ์๋ก์ด ๊ธฐ๋ณธํค ์ ์ฉ

![img_12.png](Chapter6_pic/img_12.png)

*`Orders`*

> DB์๋ฌ ๋๋ฌธ์ Orders๋ก ๋ช์นญ์ ๋ฐ๊ฟ์ผ ํ๋ค
>

```java
@Entity
public class Orders {
    //
    @Id
    @GeneratedValue
    @Column(name = "ORDERS_ID")
    private Long id;

    @ManyToOne
    @JoinColumn(name = "MEMBER_ID")
    private Member member;

    @ManyToOne
    @JoinColumn(name = "PRODUCT_ID")
    private Product product;

    private int orderAmount;
}
```

๊ทธ์ธ ์ํฐํฐ, ์ ์ฅ ์ฝ๋๋ ๋์ผ

### find ์ฝ๋

> ์๋ณ์ ํด๋์ค๋ฅผ ์ฌ์ฉํ์ง ์์ ์ฝ๋๊ฐ ํจ์ฌ ๊ฐ๊ฒฐํด์ก๋ค
>

```java
public static void testFind(EntityManager em) {
    //
    Long ordersId = 1L;
    Orders orders = em.find(Orders.class, ordersId);

    Member member = orders.getMember();
    Product product = orders.getProduct();

    System.out.println("member :::: " + member.getUserName());
    System.out.println("product :::: " + product.getName());
    System.out.println("orderAmount :::: " + orders.getOrderAmount());
}
```

## 6.4.5 ๋ค๋๋ค ์ฐ๊ด๊ด๊ณ ์ ๋ฆฌ

- **์๋ณ ๊ด๊ณ** : ๋ฐ์์จ ์๋ณ์๋ฅผ ๊ธฐ๋ณธ ํค + ์ธ๋ ํค๋ก ์ฌ์ฉ
- **๋น์๋ณ ๊ด๊ณ** : ๋ฐ์์จ ์๋ณ์๋ฅผ ์ธ๋ ํค๋ก๋ง ์ฌ์ฉํ๊ณ  ์๋ก์ด ์๋ณ์ ์ถ๊ฐ