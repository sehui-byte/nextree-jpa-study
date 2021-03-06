<aside>
π‘ κ°μ

κΈ°λ³Έν€ μμ± μ λ΅ ::  131 μ°Έμ‘° - μΈν¬μ¨

JPAλ₯Ό μ¬μ©νλ λ° κ°μ₯ μ€μν κ² - μν°ν°μ νμ΄λΈμ μ νν λ§€ννλ κ²

**λ§€ν μ΄λΈνμ΄μ**

- κ°μ²΄μ νμ΄λΈ λ§€ν : @Entity, @Table
- κΈ°λ³Έ ν€ λ§€ν : @Id
- νλμ μ»¬λΌ λ§€ν : @Column
- μ°κ΄κ΄κ³ λ§€ν : @ManyToOne, @JoinColumn
</aside>

# 4.1 @Entity

> JPAλ₯Ό μ¬μ©ν΄μ νμ΄λΈκ³Ό λ§€νν  ν΄λμ€λ νμλ‘ λΆμ¬μΌ νλ€. β `@NoArgsConstructure`
>
>
> @Entityκ° λΆμ ν΄λμ€λ JPAκ° κ΄λ¦¬νλ κ²
>

## μ μ©μ μ£Όμμ¬ν­

- κΈ°λ³Έ μμ±μ νμ
- `final`ν΄λμ€, `enum`, `interface`, `inner`ν΄λμ€μλ μ¬μ©ν  μ μλ€.
- μ μ₯ν  νλμ `final`μ¬μ©νλ©΄ μλ¨

# 4.2 @Table

> μν°ν°μ λ§€νν  νμ΄λΈ μ§μ 
>
>
> μλ΅κΈ° λ§€νν μν°ν° μ΄λ¦μ νμ΄λΈ μ΄λ¦μΌλ‘ μ¬μ©
>

# 4.3 λ€μν λ§€ν μ¬μ©

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

    //== μΆκ° ==
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

> enumμ μ»¬λΌμΌλ‘ λ§€ννκΈ° μν΄ μ¬μ©νλ μ΄λΈνμ΄μ
>

### @Temporal

> μλ°μ λ μ§ νμμ λ§€ννκΈ° μν΄ μ¬μ©νλ μ΄λΈνμ΄μ
>

### @Lob

> CLOB, BLOB νμμ λ§€ννκΈ° μν μ΄λΈνμ΄μ
>

# 4.4 λ°μ΄ν°λ² μ΄μ€ μ€ν€λ§ μλ μ μ©

## persistence.xml μμ±

```java
<property name="hibernate.hbm2ddl.auto" value="create" />
```

> β createλ‘ μΈν΄ μ νλ¦¬μΌμ΄μ μ€ν μμ μ λ°μ΄ν°λ² μ΄μ€ νμ΄λΈμ μλμΌλ‘ μμ±νλ€.
>

application.ymlμ κ²½μ°

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/030c8bcf-7da5-484b-8393-a551004f8415/Untitled.png)

### μμ±

| μ΅μ | μ€λͺ |
| --- | --- |
| create | DROP + CREATE |
| create-drop | DROP + CREATE + DROP |
| update | λ³κ²½μ¬ν­λ§ μμ  |
| validate | νμ΄λΈκ³Ό λ§€νμ λ³΄λ₯Ό λΉκ΅ν΄μ μ°¨μ΄κ° μμΌλ©΄ κ²½κ³ , μ΄νλ¦¬μΌμ΄μμ μ€ννμ§ μμ
DDLμ μμ νμ§ μλλ€. |
| none | μλμμ±κΈ°λ₯μ μ¬μ©νμ§ μμ(μ ν¨νμ§ μμ κ°) |

> μ΄μ λ¨κ³μμλ create, create-drop, updateλ₯Ό μ λ μ¬μ©νμ§ μλλ€.
>
>
> μ€νμ΄μ§, μ΄μμμλ validate, noneλ§μ μ¬μ©ν  κ²
>

---

```java
<property name="hibernate.show_sql" value="true" />
```

> β μ½μμ DDL(Data Definition Language)μ μΆλ ₯ν  μ μλ€.
>
>
> μ΄λ μλ μμ±λμ΄ μΆλ ₯λλ DDLμ dialectμ λ°λΌ λ¬λΌμ§λ€.
>

application.ymlμ κ²½μ°

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c44a64c1-94c0-4b5f-8a09-8ad6296877f3/Untitled.png)

# 4.5 DDL μμ± κΈ°λ₯

> @Columnμ λ§€ν μ λ³΄μ μμ± κ°μ μΆκ°νμ¬ μ»¬λΌμ μ μ½μ‘°κ±΄μ μΆκ°ν  μ μλ€.
>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/cc09df13-b7a0-4428-8ff3-a705dd5964e0/Untitled.png)

![L : Before, R : After](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a9e2d30b-3dac-409b-9757-9a58a15d16cd/Untitled.png)

L : Before, R : After

> @Tableμ μ λν¬ μ μ½μ‘°κ±΄μ μΆκ°ν  μλ μλ€.
>

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0dc6d339-c01d-4fe7-868c-43fe094727b6/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/990d1802-2ae0-4bba-885c-1aa6bdee1af2/Untitled.png)

<aside>
π‘ μ΄λ° μμ±κ°λ€μ λ¨μ§ DDLμ μλ μμ±ν  λλ§ μ¬μ©λκ³  JPAμ μ€ν λ‘μ§μλ μν₯μ μ£Όμ§ μλλ€.

β μ§μ  DDLμ λ§λ λ€λ©΄ μ¬μ©ν  μ΄μ κ° μλ€.

</aside>

# 4.6 κΈ°λ³Έ ν€(Primary Key) λ§€ν

## JPAκ° μ κ³΅νλ κΈ°λ³Έ ν€ μμ± μ λ΅

- μ§μ  ν λΉ : κΈ°λ³Έ ν€λ₯Ό μ νλ¦¬μΌμ΄μμμ μ§μ  ν λΉ
- μλ μμ± : λλ¦¬ ν€ μ¬μ© λ°©μ
    - IDENTITY : κΈ°λ³Έ ν€ μμ±μ λ°μ΄ν°λ² μ΄μ€μ μμ
    - SEQUENCE : λ°μ΄ν° λ² μ΄μ€ μνμ€λ₯Ό μ¬μ©ν΄μ κΈ°λ³Έ ν€ ν λΉ
    - TABLE : ν€ μμ± νμ΄λΈμ μ¬μ©

β μ λ΅μ΄ μ΄λ κ² λ€μν μ΄μ  : DBλ²€λλ§λ€ μ§μνλ λ°©μμ΄ λ€λ₯΄κΈ° λλ¬Έ

ex) μ€λΌν΄μ μνμ€ μ κ³΅ / MySQLμ AUTO_INCREMENT - κΈ°λ³Έ ν€ κ° μλμΌλ‘ μ±μμ€

<aside>
π‘ ν€ μμ± μ λ΅μ μ¬μ©νλ €λ©΄ persistence.xmlμ μλ μμ±μ λ°λμ μΆκ°ν΄μΌ νλ€.

κ³Όκ±° λ²μ κ³Όμ νΈνμ±μ μν΄ κΈ°λ³Έ κ°μ΄ falseμ΄κΈ° λλ¬Έμ΄λ€.

```java
<property name="hibernate.id.new_generator_mappings" value="true" />
```

</aside>

## 4.6.1 κΈ°λ³Έ ν€ μ§μ  ν λΉ μ λ΅

> @Idλ‘ λ§€ννλ κ²
>
>
> μ΄λ em.persist()λ‘ μν°ν°λ₯Ό μ μ₯νκΈ° μ μ μ νλ¦¬μΌμ΄μμμ κΈ°λ³Έ ν€λ₯Ό μ§μ  ν λΉνλ λ°©λ²μ΄λ€.
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/4870405c-68ec-4165-9e2b-fd6fb029f6e8/Untitled.png)
>

## 4.6.2 IDENTITY μ λ΅

> κΈ°λ³Έ ν€ μμ±μ DBμ μμνλ μ λ΅
>
>
> μ£Όλ‘ MySQL, PostgreSQL, SQL Server, DB2μμ μ¬μ©
>

### μ½λ

μλμ²λΌ `@GeneratedValue`μ΄λΈνμ΄μμ `strategy = GenerationType.IDENTITY`λ₯Ό μ΅μμΌλ‘ μλ ₯

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/d9fd5321-fb22-441d-a4a6-5634580c6d62/Untitled.png)

λ°μ΄ν°λ₯Ό μλ ₯ν  λ λ³λλ‘ Idλ₯Ό μ§μ ν΄μ£Όμ§ μμλ

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/366e65da-064e-4e5f-96ec-446cfcf57e16/Untitled.png)

DBμλ Idκ° μλμΌλ‘ μλ ₯λλ€.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/14b69182-4d85-4140-b641-18ffd90aff6a/Untitled.png)

β `em.persist()`λ₯Ό νΈμΆν΄μ μν°ν°λ₯Ό μ μ₯ν μ§νμ ν λΉλ μλ³μ κ°μ μΆλ ₯

μ¬κΈ°μ μΆλ ₯λ κ° 1,2λ **μ μ₯ μμ μ DBκ° μμ±ν κ°μ JPAκ° μ‘°νν κ²**μ΄λ€

<aside>
π‘ κ°λ°μκ° μν°ν°μ μ§μ  μλ³μλ₯Ό ν λΉνλ©΄ `@Id`λ§ μμΌλ©΄ λμ§λ§ μλ³μλ₯Ό μλμΌλ‘ μμ±νκ³  μΆμ κ²½μ°λ `@GeneratedValue`λ₯Ό μ¬μ©νκ³  μλ³μ μμ± μ λ΅μ μ νν΄μΌ νλ€.

IDENTITYμ λ΅μ λ°μ΄ν°λ₯Ό DBμ INSERTν νμ κΈ°λ³Έ ν€ κ°μ μ‘°νν  μ μλ°. λ°λΌμ μν°ν°μ μμ‘μ κ°μ ν λΉνλ €λ©΄ JPAλ μΆκ°λ‘ DBλ₯Ό μ‘°νν΄μΌ νλ€.

</aside>

<aside>
β μν°ν°κ° μμ μνκ° λλ €λ©΄ μλ³μκ° λ°λμ νμνλ€.

κ·Έλ°λ° IDENTITY μλ³μ μμ± μ λ΅μ μν°ν°λ₯Ό DBμ μ μ₯ν΄μΌ μλ³μλ₯Ό κ΅¬ν  μ μμΌλ―λ‘ em.persist()λ₯Ό νΈμΆνλ μ¦μ INSERT SQLμ΄ μ λ¬λλ€. **β  νΈλμ­μμ μ§μνλ μ°κΈ° μ§μ°μ΄ λμνμ§ μλλ€.**

</aside>

## 4.6.3 SEQUENCE μ λ΅

> μνμ€ : μ μΌν κ°μ μμλλ‘ μμ±νλ νΉλ³ν DB μ€λΈμ νΈ
>
>
> μ€λΌν΄, PostgreSQL, DB2, H2 μμ μ¬μ©
>

### μ½λ

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/acc55dd6-2886-4b20-84a9-935fb03a87f3/Untitled.png)

> `@SequenceGenerator`λ‘ μνμ€ μμ±κΈ° λ±λ‘
>
>
> `sequenceName` : JPAλ μ΄ μμ±μ μ΄λ¦μΌλ‘ μνμ€ μμ±κΈ°λ₯Ό μ€μ  DBμ μνμ€μ λ§€ννλ€.
>
> `@GeneratedValue` μ `strategy`λ‘ μ λ΅μ μ ννκ³ , `generator` λ‘ μνμ€ μμ±κΈ°λ₯Ό μ ν
>

κ°μ set νκ³  `em.persist()` νΈμΆ

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/98925bfb-022d-4425-bee5-6fc780ca2518/Untitled.png)

μΆλ ₯κ°

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b3d44779-b543-4520-b413-b32870c038b7/Untitled.png)

<aside>
π‘ μ¬κΈ°μ μΆλ ₯ νμμ IDENTITYμ κ°μ§λ§ λ΄λΆ λμ λ°©μμ λ€λ₯΄λ€

SEQUENCEλ em.persist()λ₯Ό νΈμΆν  λ λ¨Όμ  DB μνμ€λ₯Ό μ¬μ©ν΄μ μλ³μλ₯Ό μ‘°ννλ€.

κ·Έλ¦¬κ³  μ‘°νν μλ³μλ₯Ό μν°ν°μ ν λΉ ν νμ μμμ± μ»¨νμ€νΈμ μ μ₯

μ΄ν νΈλμ­μ μ»€λ° - νλ¬μκ° μΌμ΄λλ©΄ μν°ν°λ₯Ό DBμ μ μ₯

</aside>

| IDENTITY | SEQUENCE |
| --- | --- |
| μν°ν°λ₯Ό DBμ μ μ₯ν ν μλ³μ μ‘°ν - μν°ν°μ μλ³μμ ν λΉ | DB μνμ€λ₯Ό μ¬μ©ν΄μ μλ³μ μ‘°ν - μ‘°νν μλ³μλ₯Ό μν°ν°μ ν λΉν ν μν°ν°λ₯Ό μμμ± μ»¨νμ€νΈμ μ μ₯ - μ΄ν νλ¬μκ° μΌμ΄λλ©΄ μν°ν°λ₯Ό DBμ μ μ₯ |

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/133d5ebc-b0d1-4023-99f2-d4006f490920/Untitled.png)

### @SequenceGenerator

> νΉμ΄μ¬ν­
>
>
> `allocationSize`λ μνμ€ ν λ² νΈμΆμ μ¦κ°νλ μλ₯Ό μ§μ νλ λ³μμΈλ°, **κΈ°λ³Έκ°μ΄ 50μΌλ‘ μ€μ λμ΄ μλ€.**
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/44a0a3c8-4edb-4dd2-b48a-ef614c72d084/Untitled.png)
>
> 5λ² persistν  λμ `allocationSize`λ₯Ό λΊ κ²½μ°
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/b91e2871-2adb-4863-bb0b-0fd3eff520c4/Untitled.png)
>
> 5λ² persistν  λμ `allocationSize`λ₯Ό 2λ‘ μ€ κ²½μ° λ λ§μ SEQνΈμΆμ΄ νμνλ€
>
> ![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/2f2cd7d8-cb86-4c7e-b4b3-3328ed92b6c9/Untitled.png)
>

<aside>
β μ μ₯ λλ DBμμ νμ€νΈ ν΄λ³Ό κ²!!

</aside>

<aside>
π‘ JPAλ μνμ€μ μ κ·Όνλ νμλ₯Ό μ€μ΄κΈ° μν΄ `allocationSize`λ₯Ό μ¬μ©νλ€.

κΈ°λ³Έ κ°(50)μ΄λΌλ©΄ μνμ€λ₯Ό ν λ²μ 50 μ¦κ° μν¨ λ€μ 1~50κΉμ§λ **λ©λͺ¨λ¦¬μμ μλ³μλ₯Ό ν λΉ**νλ€. κ·Έλ¦¬κ³  51μ΄ λλ©΄ μνμ€ κ°μ 100μΌλ‘ μ¦κ°μν¨ λ€μ 51~100κΉμ§ λ©λͺ¨λ¦¬μμ μλ³μλ₯Ό ν λΉνλ€.

β μ΄λ κ² νλ©΄ μνμ€ κ°μ μ μ νλ―λ‘ μ¬λ¬ JVMμ΄ λμμ λμν΄λ κΈ°λ³Έ ν€ κ°μ΄ μΆ©λνμ§ μμ§λ§, DBμ **μ§μ  μ κ·Όν΄μ λ°μ΄ν°λ₯Ό λ±λ‘ν  λ μνμ€ κ°μ΄ ν λ²μ λ§μ΄ μ¦κ°**νλ€λ κ²μ μΌλν΄μΌ νλ€.

INSERT μ±λ₯μ΄ μ€μμΉ μμΌλ©΄ `allocationSize`κ°μ 1λ‘ μ€ κ²

</aside>

## 4.6.4 TABLE μ λ΅

> ν€ μμ± μ μ© νμ΄λΈμ νλ λ§λ€κ³  μ¬κΈ°μ μ΄λ¦κ³Ό κ°μΌλ‘ μ¬μ©ν  μ»¬λΌμ λ§λ€μ΄ DB μνμ€λ₯Ό νλ΄λ΄λ μ λ΅ β νμ΄λΈμ μ¬μ©νλ―λ‘ λͺ¨λ  DBμ μ μ© κ°λ₯
>

### μ½λ

`pkColumnValue`λ‘ μνμ€ μ»¬λΌμ μΆκ°νλ€.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a1d3f29b-dd03-4992-a2b9-53a45b69e32f/Untitled.png)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/032d8d74-4827-4395-b692-a5c806585f44/Untitled.png)

λ΄λΆ λμ λ°©μμ SEQUENCEμ λ΅κ³Ό λμΌνλ€ β μλ³μ μ‘°ν - μν°ν° ν λΉ - DBμ μ₯

νμ§λ§ μνμ€ μμ²΄λ₯Ό νμ΄λΈμ μ μ₯νλ λ°©μμ΄κΈ° λλ¬Έμ μνμ€ νμ΄λΈ μμμ SELECT - UPDATEλ₯Ό λ°λ³΅ν΄μ μνμ€λ₯Ό μλ°μ΄νΈ ν ν ν λΉνλ€ β ν¨μ¨μ μΈμ§λ μ λͺ¨λ₯΄κ² μ

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e3d6fca4-3750-4450-ac1f-8a5ded55377c/Untitled.png)

μ΄ ν μ¬μ©μ μλ ₯κ° κ°μ -1 λ§νΌ SELECT UPDATEλ₯Ό λ°λ³΅νλ€.

## 4.6.5 AUTOμ λ΅

> μ νν DBλ°©μΈμ λ°λΌ μ μΈκ°μ§ μ λ΅ μ€ νλλ₯Ό μλμΌλ‘ μ ννλ€.
>
>
> β DBλ₯Ό μ€κ°μ λ³κ²½ν΄λ μ½λλ₯Ό μμ ν  νμκ° μλ€.
>
> λ¨, SEQUENCEλ TABLE μ λ΅μ΄ μ νλλ©΄ μνμ€λ ν€ μμ±μ νμ΄λΈμ λ―Έλ¦¬ λ§λ€μ΄λμ΄μΌ νλ€. - μ€ν€λ§ μλ μμ± κΈ°λ₯μ μ¬μ©νλ©΄ Hibernateκ° κΈ°λ³Έκ°μ μ¬μ©ν΄μ μλμΌλ‘ λ§λ€μ΄ μ€ κ²
>

# 4.7 νλμ μ»¬λΌ λ§€ν: λ νΌλ°μ€

| λ§€ν μ΄λΈνμ΄μ | μ€λͺ |
| --- | --- |
| @Column | μ»¬λΌ λ§€ν |
| @Enumerated | μλ°μ enum νμμ λ§€ννλ€ |
| @Temporal | λ μ§ νμμ λ§€ννλ€ |
| @Lob | BLOB. CLOB νμμ λ§€ννλ€. |
| @Transient | νΉμ  νλλ₯Ό λ°μ΄ν°λ² μ΄μ€μ λ§€ννμ§ μλλ€. |
| @Access | JPAκ° μν°ν°μ μ κ·Όνλ λ°©μμ μ§μ νλ€. |

## 4.7.1 @Column

μ±145p μμ±μ λμ€μ μ¬μ©ν  λλ§λ€ μ λ¦¬ν  κ²

νΉμ΄ν κ²½μ°λ§ μ λ¦¬νλ€.

### unique

> ν μ»¬λΌμ μ λν¬ μ μ½μ‘°κ±΄
>
>
> λ§μ½μ λ μ»¬λΌ μ΄μμ μ¬μ©ν΄μ μ μ½μ‘°κ±΄μ μ¬μ©νλ €λ©΄ ν΄λ μ€ λ λ²¨μμ `@Table.uniqueConstraint`λ₯Ό μ¬μ©ν  κ²
>

### columnDefinition

> λ°μ΄ν°λ² μ΄μ€ μ»¬λΌ μ λ³΄λ₯Ό μ§μ  μ€ μ μλ€.
>

μμ 

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

`@Enumerated(EnumType.*STRING*)` : enum μ΄λ¦μ DBμ μ μ₯ β κΆμ₯

- μ μ₯λ enumμ μμκ° λ°λκ±°λ enumμ΄ μΆκ°λμ΄λ μμ 
- λ¨, DBμμ μ μ₯λλ λ°μ΄ν° ν¬κΈ°κ° ORDINALμ λΉν΄ ν¬λ€

`@Enumerated(EnumType.*ORDINAL*)` : enum μμλ₯Ό DBμ μ μ₯ β κΈ°λ³Έκ°

- DBμ μ μ₯λλ λ°μ΄ν° ν¬κΈ°κ° μλ€
- λ¨, μ΄λ―Έ μ μ₯λ enumμ μμλ₯Ό λ³κ²½ν  μ μλ€

<aside>
β ORDINALμ¬μ©μ μ£Όμμ 

1λ²(0), 2λ²(1) μ¬μ΄μ enumμ΄ νλ μΆκ° λλ©΄ 1λ²(0), 3λ²(1), 2λ²(2) μ΄ λλλ°,

μ΄λκΉμ§ 2λ²μΌλ‘ μ μ₯λ κ°μ DBμμ 2λ‘ λ³κ²½λλ κ²μ΄ μλλΌ 1λ‘ κ·Έλλ‘ λ¨μλ€

</aside>

## 4.7.3 @Temporal

> λ μ§ νμ λ§€νμ μ¬μ©
>

| μ΅μ TemporalType | κΈ°λ₯ |
| --- | --- |
| DATE | λ μ§, λ°μ΄ν°λ² μ΄μ€ dateνμκ³Ό λ§€ν |
| TIME | μκ°, λ°μ΄ν°λ² μ΄μ€ timeνμκ³Ό λ§€ν |
| TIMESTAMP | λ μ§μ μκ°, λ°μ΄ν°λ² μ΄μ€ timestampνμ |

<aside>
π‘ μλ΅ν  κ²½μ°

λ°μ΄ν°λ² μ΄μ€ λ°©μΈμ λ°λΌμ DDLμ΄ μλ μμ±λλ€.

datetime : MySQL

timestamp : H2, μ€λΌν΄, PostgreSQL

</aside>

### μ¬μ©μ

```java
@Getter
@Setter
@Entity
@SequenceGenerator(
        name = "BOARD_SEQ_GENERATOR",
        sequenceName = "BOARD_SEQ", // λ§€νν  DB μνμ€ μ΄λ¦
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

μΆλ ₯

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/16f7c409-752d-48c7-92f9-12ade9cb516e/Untitled.png)

## 4.7.4 @Lob

> νλνμμ΄ λ¬Έμλ©΄ CLOB, κ·Έ μΈμλ BLOBμΌλ‘ λ§€ννλ€.
>

## 4.7.5 @Transient

> μ΄ νλλ λ§€ννμ§ μλλ€
>
>
> β DBμ μ μ₯νμ§λ μκ³  μ‘°ννμ§λ μλλ€. - κ°μ²΄μ μμλ‘ μ΄λ€ κ°μ λ³΄κ΄νκ³  μΆμ λ μ¬μ©νλ€.
>

### μ¬μ©μ

> @Transientλ₯Ό νλμ λΆμ¬μ£Όκ³ 
>

```java
@Getter
@Setter
@Entity
@SequenceGenerator(
        name = "BOARD_SEQ_GENERATOR",
        sequenceName = "BOARD_SEQ", // λ§€νν  DB μνμ€ μ΄λ¦
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

κ°κΉμ§ μλ ₯ν΄μ£Όμμ§λ§

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7698acee-1651-4ff1-b867-f9621413ade9/Untitled.png)

νμ΄λΈ μμ±μμλ ν΄λΉ νλκ° μ μΈλλ€.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/60302191-2779-479f-a896-757eca39973a/Untitled.png)

## @Access

> JPAκ° μν°ν° λ°μ΄ν°μ μ κ·Όνλ λ°©μμ μ§μ 
>

### νλ μ κ·Ό

> AccessType.FIELDλ‘ μ§μ νλ€.
>
>
> νλμ μ§μ  μ κ·Ό
>
> νλ μ κ·ΌκΆνμ΄ privateμ¬λ μ κ·Ό ν  μ μλ€.
>
> Accessλ₯Ό μ€μ νμ§ μμΌλ©΄ @Idμ μμΉλ₯Ό κΈ°μ€μΌλ‘ μ κ·Όλ°©μμ΄ μ€μ λλ€. β μΌλ°μ μΈ κ²½μ°
>

### νλ‘νΌν° μ κ·Ό

> Idκ° νλ‘νΌν°μ μμλλ μλ΅ν  μ μλ€.
>

```java
private String id;

@Id
public String getId(){
	return id;
}
```

### νλ + νλ‘νΌν° μ κ·Ό λ°©μ

> ν¨κ» μ¬μ©νλ κ²½μ°
>
>
> μ± μμ μμ λΉΌ λ¨Ήμ κ² β setterλ₯Ό κΌ­ λ£μ΄μ£Όμ΄μΌ νλ€
>

μν°ν°

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

κ²°κ³Ό

β μλμΌλ‘ fullNameμ΄ μ»¬λΌμΌλ‘ μμ±λλ€.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04f7fa94-4968-4ff6-a3d7-199242935cd3/Untitled.png)