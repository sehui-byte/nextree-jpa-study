<aside>
π‘ κ°μ

- μμ κ΄κ³ λ§€ν : κ°μ²΄μ μμ κ΄κ³λ₯Ό DBμ μ΄λ»κ² λ§€νν  κ²μΈμ§
- @MappedSuperclass : μ¬λ¬ μν°ν°μμ κ³΅ν΅μΌλ‘ μ¬μ©νλ λ§€ν μ λ³΄λ§ μμλ°κ³  μΆμΌλ©΄ μ΄ κΈ°λ₯μ μ¬μ©
- λ³΅ν© ν€μ μλ³ κ΄κ³ λ§€ν : DBμ μλ³μκ° νλ μ΄μμΌ λ λ§€ννλ λ°©λ², DBμ€κ³μμ μ΄μΌκΈ°νλ μλ³ κ΄κ³μ λΉμλ³ κ΄κ³
- μ‘°μΈ νμ΄λΈ : νμ΄λΈμ μΈλ ν€ νλλ‘ μ°κ΄κ΄κ³λ₯Ό λ§Ίμ μ μμ§λ§ μ°κ΄κ΄κ³λ₯Ό κ΄λ¦¬νλ μ°κ²° νμ΄λΈμ λλ λ°©λ²λ μλ€. β μ°κ²° νμ΄λΈμ λ§€ννλ λ°©λ²μ λ€λ£° κ²
- μν°ν° νλμ μ¬λ¬ νμ΄λΈ λ§€ν
</aside>

# 7.1 μμ κ΄κ³ λ§€ν

> ORMμλ μμμ΄λΌλ κ°λμ΄ μλ€ - μνΌνμ μλΈνμ κ΄κ³λΌλ λͺ¨λΈλ§ κΈ°λ²μ΄ μ μ¬ν κ°λ
>
>
> β ORMμ μμμ **κ°μ²΄μ μμ κ΅¬μ‘°μ DBμ μνΌνμ μλΈνμ κ΄κ³λ₯Ό λ§€ννλ κ²μ΄λ€.**
>

![μνΌνμ μλΈνμ λΌλ¦¬ λͺ¨λΈ](Chapter7_1_pic/img_2.png)

μνΌνμ μλΈνμ λΌλ¦¬ λͺ¨λΈ

![img_3.png](Chapter7_1_pic/img_3.png)

κ°μ²΄ μμ λͺ¨λΈ

μνΌνμ μλΈνμ λΌλ¦¬ λͺ¨λΈ β νμ΄λΈλ‘ κ΅¬ν ν  λλ μλ μΈκ°μ§ λ°©λ²μ μ νν  μ μλ€.

## 7.1.1 μ‘°μΈ μ λ΅

![img_4.png](Chapter7_1_pic/img_4.png)

> μ‘°μΈ μ λ΅μ **μν°ν° κ°κ°μ λͺ¨λ νμ΄λΈλ‘ λ§λ€κ³ ** μμ νμ΄λΈμ΄ λΆλͺ¨ νμ΄λΈμ κΈ°λ³Έ ν€λ₯Ό λ°μμ κΈ°λ³Έ ν€ + μΈλ ν€λ‘ μ¬μ©νλ μ λ΅μ΄λ€.
>
>
> λ¨, κ°μ²΄λ νμμΌλ‘ κ΅¬λΆν  μ μμ§λ§ νμ΄λΈμ νμμ κ°λμ΄ μκΈ° λλ¬Έμ νμμ κ΅¬λΆνλ μ»¬λΌ(DTYPE)μ μΆκ°ν΄μΌ νλ€.
>

### μμ  μ½λ λΆμ

`*Item*`

```java
@Getter
@Setter
@Entity
@Inheritance(strategy = InheritanceType.JOINED)
@DiscriminatorColumn(name = "DTYPE")
public abstract class Item {

    @Id @GeneratedValue
    @Column(name = "ITEM_ID")
    private Long id;

    private String name;        //μ΄λ¦
    private int price;          //κ°κ²©
```

*`Album`*

```java
@Getter
@Setter
@Entity
@DiscriminatorValue("A")
public class Album extends Item {

    private String artist;
```

*`Movie`*

```java
@Getter
@Setter
@Entity
@DiscriminatorValue("M")
public class Movie extends Item {

    private String director;
    private String actor;
```

<aside>
π‘ λ§€ν μ λ³΄ λΆμ

1. *`@Inheritance*(strategy = *InheritanceType*.JOINED)` : μμ λ§€νμ λΆλͺ¨ ν΄λμ€μ *`@Inheritance`*λ₯Ό μ¬μ©ν΄μΌ νλ€. κ·Έλ¦¬κ³  λ§€ν μ λ΅μ μ§μ ν΄μΌ νλλ° μ¬κΈ°μλ μ‘°μΈ μ λ΅μ μ¬μ©νλ€
2. *`@DiscriminatorColumn*(name = "DTYPE")` : λΆλͺ¨ ν΄λμ€μ κ΅¬λΆ μ»¬λΌμ μ§μ νλ€. μ΄ μ»¬λΌμΌλ‘ μ μ₯λ μμ νμ΄λΈμ κ΅¬λΆν  μ μλ€. κΈ°λ³Έ κ°μ΄ DTYPEμ΄λ―λ‘ μλ΅ν  μ μλ€.
3. *`@DiscriminatorValue*("M")` : κ΅¬λΆ μ»¬λΌμ μλ ₯ν  κ°μ μ§μ 
</aside>

> κΈ°λ³Έκ°μΌλ‘ μμ νμ΄λΈμ λΆλͺ¨ νμ΄λΈμ ID μ»¬λΌλͺμ κ·Έλλ‘ μ¬μ©νλλ°, μ΄λ₯Ό λ³κ²½νκ³  μΆμΌλ©΄ *`@PrimaryKeyJoinColumn`*λ₯Ό μ¬μ©νλ€.
>
>
> BOOK νμ΄λΈμ `ITEM_ID` κΈ°λ³Έ ν€ μ»¬λΌλͺμ `BOOK_ID`λ‘ λ³κ²½
>
> ```java
> @Getter
> @Setter
> @Entity
> @DiscriminatorValue("M")
> @PrimaryKeyJoinColumn(name = "BOOK_ID")
> public class Movie extends Item {
> 
>     private String director;
>     private String actor;
> ```
>

### νμ€νΈμ© μ μ₯ μ½λ

```java
public static void saveAlbum(EntityManager em) {
    //
    Album album1 = new Album("Artist", "ETC");
    em.persist(album1);
}
```

μ€ν μΏΌλ¦¬

![img_5.png](Chapter7_1_pic/img_5.png)

Itemνμ΄λΈ, Album νμ΄λΈμ νλμ© λ λ²μ Insertκ° μ€νλλ€.

### μ₯λ¨μ  λ° νΉμ§

- μ₯μ 
    - νμ΄λΈ μ κ·ν
    - μΈλ ν€ μ°Έμ‘° λ¬΄κ²°μ± μ μ½μ‘°κ±΄ νμ© κ°λ₯
    - μ μ₯κ³΅κ°μ ν¨μ¨μ μΌλ‘ μ¬μ© κ°λ₯
- λ¨μ 
    - μ‘°νν  λ μ‘°μΈμ΄ λ§μ΄ λ¨ β μ±λ₯ μ ν
    - μ‘°ν μΏΌλ¦¬ λ³΅μ‘
    - λ°μ΄ν° λ±λ‘μ INSERT SQLμ΄ λ λ² μ€νλ¨
- νΉμ§
    - JPAνμ€ λͺμΈλ κ΅¬λΆ μ»¬λΌμ μ¬μ©ν λ‘ νμ§λ§ νμ΄λ²λ€μ΄νΈλ₯Ό ν¬ν¨ν λͺλͺ κ΅¬νμ²΄λ κ΅¬λΆ μ»¬λΌ(*`@DiscriminatorValue`*) μμ΄λ λμνλ€.

## 7.1.2 λ¨μΌ νμ΄λΈ μ λ΅

> νμ΄λΈ νλλ§ μ¬μ© - DTYPEμΌλ‘ μ΄λ€ μμ λ°μ΄ν°κ° μ μ₯λμλμ§ κ΅¬λΆ
>
>
> β μΌλ°μ μΌλ‘ κ°μ₯ λΉ λ₯΄λ€
>
> μ£Όμμ  :: μμ μν°ν°κ° λ§€νν μ»¬λΌ λͺ¨λ nullμ νμ©ν΄μΌ νλ€ - ν νμ΄λΈμ μ μ₯νλ©΄ λ€λ₯Έ νμ΄λΈμ nullλ‘ μ μ₯ν΄μΌνκΈ° λλ¬Έ
>

```java
@Getter
@Setter
@Entity
@Inheritance(strategy = InheritanceType.SINGLE_TABLE)
@DiscriminatorColumn(name = "DTYPE")
public abstract class Item {
		//
		...
}
//λ΄λΆ ν΄λμ€λ‘ μ§μ 
@Entity
@DiscriminatorValue("A")
public class Album extends Item {
    //
    ...
}
```

*`@Inheritance*(strategy = *InheritanceType*.SINGLE_TABLE)`λ‘ μ§μ νλ©΄ λ¨μΌ νμ΄λΈ μ λ΅μ μ¬μ©νλ€. νμ΄λΈ νλμ λͺ¨λ  κ²μ ν΅ν©νλ―λ‘ *`@DiscriminatorValue`*λ νμκ° λλ€.

### μ₯λ¨μ  λ° νΉμ§

- μ₯μ 
    - μ‘°μΈμ΄ νμ μμΌλ―λ‘ μΌλ°μ μΌλ‘ μ‘°ν μ±λ₯μ΄ λΉ λ₯΄λ€
    - μ‘°ν μΏΌλ¦¬κ° λ¨μνλ€
- λ¨μ 
    - μμ μν°ν°κ° λ§€νν μ»¬λΌμ λͺ¨λ nullμ νμ©ν΄μΌ νλ€.
    - λ¨μΌ νμ΄λΈμ λͺ¨λ  κ²μ μ μ₯ β νμ΄λΈμ΄ μ»€μ§ μ μλ€.

      **β μν©μ λ°λΌ μ‘°νμ±λ₯μ΄ μ€νλ € λ λ¦μ΄μ§ μλ μλ€.**

- νΉμ§
    - κ΅¬λΆ μ»¬λΌμ κΌ­ μ¬μ©ν΄μΌ νλ€. β *`@DiscriminatorValue`* νμ
    - *`@DiscriminatorValue`*λ₯Ό λ°λ‘ μ§μ νμ§ μμΌλ©΄ κΈ°λ³ΈμΌλ‘ μν°ν° μ΄λ¦μ μ¬μ©νλ€.

## 7.1.3 κ΅¬ν ν΄λμ€λ§λ€ νμ΄λΈ μ λ΅

![img_6.png](Chapter7_1_pic/img_6.png)

> μμ μν°ν°λ§λ€ νμ΄λΈ μμ± - μμ νμ΄λΈ κ°κ°μ νμν μ»¬λΌμ΄ λͺ¨λ μ‘΄μ¬
>

```java
@Getter
@Setter
@Entity
@Inheritance(strategy = InheritanceType.TABLE_PER_CLASS)
@DiscriminatorColumn(name = "DTYPE")
public abstract class Item {
```

*`@Inheritance*(strategy = *InheritanceType*.TABLE_PER_CLASS)`λ₯Ό μ ννλ©΄ κ΅¬ν ν΄λμ€λ§λ€ νμ΄λΈ μ λ΅μ μ¬μ©νλ€. μ΄λ μμ μν°ν°λ§λ€ νμ΄λΈμ λ§λ λ€. β **λΉμΆμ²**

### μ₯λ¨μ  λ° νΉμ§

- μ₯μ 
    - μλΈ νμμ κ΅¬λΆν΄μ μ²λ¦¬ν  λ ν¨κ³Όμ 
    - not nullμ μ¬μ©ν  μ μλ€.
- λ¨μ 
    - μ¬λ¬ μμ νμ΄λΈμ ν¨κ» μ‘°νν  λ μ±λ₯μ΄ λλ¦¬λ€
    - μμ νμ΄λΈμ ν΅ν©ν΄μ μΏΌλ¦¬νκΈ° μ΄λ ΅λ€
- νΉμ§
    - κ΅¬λΆ μ»¬λΌμ μ¬μ©νμ§ μμ

# 7.2 @MappedSuperclass

![img_7.png](Chapter7_1_pic/img_7.png)

> μ§κΈκΉμ§λ λΆλͺ¨, μμ ν΄λμ€ λͺ¨λ DB νμ΄λΈκ³Ό λ§€ν
>
>
> λΆλͺ¨ ν΄λμ€λ νμ΄λΈκ³Ό λ§€ννμ§ μκ³  λΆλͺ¨λ₯Ό μμλ°λ μμ ν΄λμ€μκ² λ§€ν μ λ³΄λ§ μ κ³΅νκ³  μΆμΌλ©΄  `@MappedSuperclass`λ₯Ό μ¬μ©νλ€.
>
> `@MappedSuperclass`λ μΆμν΄λμ€μ λΉμ·νλ° `@Entity`λ μ€μ  νμ΄λΈμ λ§€ν λμ§λ§ `@MappedSuperclass`λ λ§€νλμ§ μκ³  λ§€ν μ λ³΄λ₯Ό μμν  λͺ©μ μΌλ‘ μ¬μ©λλ€.
>

## νμ¬ μ½λ μμ 

> νμ¬ μ½λμμλ `DramaEntityJpo`, `DomainEntityJpo`κ° μ¬κΈ°μ ν΄λΉλμλ€.
>
>
> ![img_8.png](Chapter7_1_pic/img_8.png)
>
> ![img_9.png](Chapter7_1_pic/img_9.png)
>

<aside>
π‘ `DramaEntityJpo`, `DomainEntityJpo`λ νμ΄λΈκ³Ό μ§μ  λ§€νν  νμκ° μκ³  μμ μν°ν°μκ² κ³΅ν΅μΌλ‘ λ§€ν μ λ³΄λ§ μ κ³΅νλ©΄ λλ€.

μμ μν°ν°κ° λΆλͺ¨λ‘ λΆν° λ¬Όλ €λ°μ

- λ§€ν μ λ³΄λ₯Ό μ¬μ μ νλ €λ©΄ `@AttributeOverrides`λ `@AttributeOverride`λ₯Ό μ¬μ©
- μ°κ΄κ΄κ³λ₯Ό μ¬μ μ νλ €λ©΄ `@AssociationOverrides`λ `@AssociationOverride`λ₯Ό μ¬μ©
</aside>

### `@AttributeOverrides` μμ  μ½λ

> λΆλͺ¨μκ² μμλ°μ idμμ±μ μ»¬λΌλͺ μ¬μ μ
>

```java
@Entity
@AttributeOverrides({
        @AttributeOverride(name = "id", column = @Column(name = "MEMBER_ID")),
        @AttributeOverride(name = "name", column = @Column(name = "MEMBER_NAME"))
})
public class Member extends BaseEntity {
```

## `@MappedSuperclass`μ νΉμ§

- νμ΄λΈκ³Ό λ§€νλμ§ μκ³  μμ ν΄λμ€μ μν°ν°μ λ§€ν μ λ³΄λ₯Ό μμνκΈ° μν΄ μ¬μ©
- `@MappedSuperclass`λ‘ μ§μ ν ν΄λμ€λ μν°ν°κ° μλλ―λ‘ em.find()λ JPQLμμ μ¬μ©ν  μκ° μλ€.
- μ΄ ν΄λμ€λ₯Ό μ§μ  μμ±ν΄μ μ¬μ©ν  μΌμ κ±°μ μμ β μΆμ ν΄λμ€λ‘ λ§λ€ κ²

# 7.3 λ³΅ν© ν€μ μλ³ κ΄κ³ λ§€ν

## 7.3.1 μλ³ κ΄κ³ vs λΉμλ³ κ΄κ³

> νμ΄λΈ μ¬μ΄μ κ΄κ³λ μΈλ ν€κ° κΈ°λ³Έ ν€μ ν¬ν¨λλμ§ μ¬λΆμ λ°λΌ μλ³ κ΄κ³μ λΉμλ³ κ΄κ³λ‘ κ΅¬λΆνλ€
>
>
> νμ΄ν λͺ¨μ μ£Όλͺ©
>

### μλ³ κ΄κ³

![img_10.png](Chapter7_1_pic/img_10.png)

> λΆλͺ¨ νμ΄λΈμ κΈ°λ³Έ ν€λ₯Ό λ΄λ €λ°μμ μμ νμ΄λΈμ κΈ°λ³Έ ν€ + μΈλ ν€λ‘ μ¬μ©νλ κ΄κ³
>

### λΉμλ³ κ΄κ³

![img_11.png](Chapter7_1_pic/img_11.png)

> λΆλͺ¨ νμ΄λΈμ κΈ°λ³Έ ν€λ₯Ό λ°μμ μμ νμ΄λΈμ μΈλ ν€λ‘λ§ μ¬μ©νλ κ΄κ³
>
>
> λΉμλ³ κ΄κ³λ μΈλ ν€μ NULLμ νμ©νλμ§μ λ°λΌ νμμ , μ νμ  κ΄κ³λ‘ λλλ€.
>
- **νμμ  λΉμλ³ κ΄κ³**(Mandatory) : μΈλ ν€μ NULLμ νμ©νμ§ μλλ€. μ°κ΄κ΄κ³λ₯Ό νμμ μΌλ‘ λ§Ίμ΄μΌ νλ€.
- **μ νμ  λΉμλ³ κ΄κ³**(Optional) : μΈλ ν€μ NULLμ νμ©, μ°κ΄κ΄κ³λ₯Ό λ§Ίμμ§ λ§μ§ μ νκ°λ₯

## 7.3.2 λ³΅ν© ν€: λΉμλ³ κ΄κ³ λ§€ν

> λ³΅ν© ν€λ₯Ό μ¬μ©ν  κ±°λΌκ³  `@Id`λ₯Ό λ λ² μ¬μ©νλ©΄ λ§€ν μμΈκ° λ°μνλ€.
>
>
> JPAλ μμμ± μ»¨νμ€νΈμ μν°ν°λ₯Ό λ³΄κ΄ν  λ μν°ν°μ μλ³μλ₯Ό ν€λ‘ μ¬μ©νλ€.
>
> κ·Έλ°λ° μλ³μ νλκ° 2κ° μ΄μμ΄λ©΄ λ³λμ μλ³μ ν΄λμ€λ₯Ό λ§λ€κ³  κ·Έκ³³μ equalsμ hashcodeλ₯Ό κ΅¬νν΄μΌ νλ€.
>
> JPAλ λ³΅ν© ν€λ₯Ό μ§μνκΈ° μν΄ λ κ°μ§ λ°©μμ μ§μνλ€.
>
> - `@IdClass` : ORMμ κ°κΉμ΄ λ°©λ²
> - `@EmbeddedId` : κ°μ²΄μ§ν₯μ κ°κΉμ΄ λ°©λ²

### `@IdClass`

![img_12.png](Chapter7_1_pic/img_12.png)

> μ κ·Έλ¦Όμμ λ³΅ν© ν€ νμ΄λΈμ λΉμλ³ κ΄κ³κ³  PARENTλ κΈ°λ³Έ ν€λ₯Ό μ¬μ©νλ€.
>

*`Parent`*

```java
@Getter
@Setter
@Entity
@IdClass(ParentId.class)
public class Parent {
    //
    @Id
    @Column(name = "PARENT_ID1")
    private String id1; //ParentId.id1κ³Ό μ°κ²°

    @Id
    @Column(name = "PARENT_ID2")
    private String id2; //ParentId.id2κ³Ό μ°κ²°

    private String name;
}
```

*`ParentId`*

```java
@NoArgsConstructor
@AllArgsConstructor
public class ParentId implements Serializable {
    //
    private String id1;
    private String id2;
}
```

<aside>
π‘ `@IdClass`λ₯Ό μ¬μ©ν  λ μλ³μ ν΄λμ€λ λ€μ μ‘°κ±΄μ λ§μ‘±ν΄μΌ νλ€.

- μλ³μ ν΄λμ€μ μμ±λͺκ³Ό μν°ν°μμ μ¬μ©νλ μλ³μμ μμ±λͺμ΄ κ°μμΌ νλ€.
- Serializable μΈν°νμ΄μ€λ₯Ό κ΅¬νν΄μΌνλ€.
- κΈ°λ³Έ μμ±μκ° μμ΄μΌ νλ€.
- μλ³μ ν΄λμ€λ publicμ΄μ΄μΌ νλ€.
</aside>

**μ μ₯ μ½λ μ€ν**

> μλ μ½λλ₯Ό λ³΄λ©΄ μλ³μ ν΄λμ€μΈ `ParentId`κ° λ³΄μ΄μ§ μλλ°, `em.persist()`λ₯Ό νΈμΆνλ©΄ μμμ± μ»¨νμ€νΈμ μν°ν°λ₯Ό λ±λ‘νκΈ° μ§μ μ λ΄λΆμμ `parent.setId1`, `parent.setId2`λ₯Ό μ¬μ©ν΄μ μλ³μ ν΄λμ€μΈ `ParentId`λ₯Ό μμ±νκ³  μμμ± μ»¨νμ€νΈμ ν€λ‘ μ¬μ©νλ€.
>

```java
public static void saveParent(EntityManager em) {
    //
    Parent parent = new Parent();
    parent.setId1("myId1");
    parent.setId2("myId2");
    parent.setName("parentName");
    em.persist(parent);
}
```

> λμ μΏΌλ¦¬
>
>
> ![img_13.png](Chapter7_1_pic/img_13.png)
>

> `ParentId` μμ± νμΈμ μν΄ μλμ²λΌ `ParentId`λ₯Ό λ³κ²½νλ€.
>
>
> ```java
> public class ParentId implements Serializable {
>     //
>     private String id1;
>     private String id2;
> 
>     public ParentId() {
>         //
>         System.out.println("ParentId NoArg μμ±μ λμ νμΈ");
>     }
> 
>     public ParentId(String id1, String id2) {
>         //
>         System.out.println("ParentId AllArg μμ±μ λμ νμΈ");
>         this.id1 = id1;
>         this.id2 = id2;
>     }
> }
> ```
>
> λμ νμΈ... μ μΈλ² μμ±λλκ±ΈκΉ?
>
> ![img_14.png](Chapter7_1_pic/img_14.png)
>

**λ³΅ν© ν€ μ‘°ν**

> μ‘°ν μ½λμμλ μλ³μ ν΄λμ€μΈ `ParentId`λ₯Ό μ¬μ©ν΄μ μν°ν°λ₯Ό μ‘°ννλ€.
>

```java
public static void findParent(EntityManager em) {
    //
    ParentId parentId = new ParentId("myId1", "myId2");
    Parent parent = em.find(Parent.class, parentId);
}
```

> λμ μΏΌλ¦¬
>
>
> ![img_15.png](Chapter7_1_pic/img_15.png)
>

**μμ ν΄λμ€ μΆκ°**

> λΆλͺ¨μ κΈ°λ³Έ ν€ μ»¬λ¦Όμ΄ λ³΅ν© ν€ μ΄λ―λ‘ μμ νμ΄λΈμ μΈλ ν€λ λ³΅ν©ν€μ΄λ€.
>

```java
@Getter
@Setter
@Entity
public class Child {
    //
    @Id
    private String id;

    @ManyToOne
    @JoinColumns({
            @JoinColumn(name = "PARENT_ID1",
	            referencedColumnName = "PARENT_ID1"),
            @JoinColumn(name = "PARENT_ID2",
	            referencedColumnName = "PARENT_ID2")
    })
    private Parent parent;
}
```

### `@EmbeddedId`

> @IdClassλ³΄λ€ λ κ°μ²΄μ§ν₯μ μΈ λ°©λ²
>

μμ  μ½λλ μ€λ³΅μ νΌνκΈ° μν΄ μμ Emμ λΆμλ€.

*`Parent`*

> `Parent`μν°ν°μμ μλ³μ ν΄λμ€λ₯Ό μ§μ  μ¬μ©νκ³  `@EmbeddedId`μ΄λΈνμ΄μμ μ¬μ©νλ©΄ λλ€.
>

```java
@Getter
@Setter
@NoArgsConstructor
@Entity
public class EmParent {
    //
    @EmbeddedId
    private EmParentId id;
    private String name;
}
```

*`ParentId`*

> `@IdClass`μλ λ€λ₯΄κ² `@EmbeddedId`λ₯Ό μ μ©ν μλ³μ ν΄λμ€λ κΈ°λ³Έ ν€λ₯Ό μ§μ  λ§€ννλ€.
>

```java
@Getter
@Setter
@NoArgsConstructor
@EqualsAndHashCode
@Embeddable
public class EmParentId implements Serializable {
    //
    @Column(name = "PARENT_ID1")
    private String id1;
    @Column(name = "PARENT_ID2")
    private String id2;
}
```

<aside>
π‘ `@EmbeddedId`λ₯Ό μ μ©ν μλ³μ ν΄λμ€μ μ‘°κ±΄

- `@Embeddable` μ΄λΈνμ΄μμ λΆμ¬μ£Όμ΄μΌ νλ€.
- *`Serializable`* μΈν°νμ΄μ€λ₯Ό κ΅¬νν΄μΌ νλ€.
- equals, hashCodeλ₯Ό κ΅¬νν΄μΌ νλ€. β *`@EqualsAndHashCode`*
- κΈ°λ³Έ μμ±μκ° μμ΄μΌ νλ€. β *`@NoArgsConstructor`*
- μλ³μ ν΄λμ€λ publicμ΄μ΄μΌ νλ€.
</aside>

μ μ₯ μ½λ

```java
public static void saveEmbedded(EntityManager em) {
    //
    EmParent parent = new EmParent();
    EmParentId parentId = new EmParentId("myId1", "myId2");
    parent.setId(parentId);
    parent.setName("parentName");
    em.persist(parent);
}
```

> μ€ν μΏΌλ¦¬
>
>
> ![img.png](Chapter7_2_pic/img.png)
>

μ‘°ν μ½λ

```java
public static void findemParent(EntityManager em) {
    //
    EmParentId parentId = new EmParentId("myId1", "myId2");
    EmParent parent = em.find(EmParent.class, parentId);
    System.out.println(parent.getId().getId1());
    System.out.println(parent.getId().getId2());
    System.out.println(parent.getName());
}
```

### λ³΅ν© ν€μ `equals()`, `hashCode()`

> μμμ± μ»¨νμ€νΈλ μν°ν°μ μλ³μλ₯Ό ν€λ‘ μ¬μ©ν΄μ μν°ν°λ₯Ό κ΄λ¦¬νλ€.
>
>
> μλ³μλ₯Ό λΉκ΅ν  λ equals(), hashCode()λ₯Ό μ¬μ©νλ€.
>
> β μλ³μμ λλ±μ±(equalsλΉκ΅)κ° μ§μΌμ§μ§ μμΌλ©΄ μμκ³Ό λ€λ₯Έ μν°ν°κ° μ‘°νλκ±°λ μν°ν°λ₯Ό μ°Ύμ μ μλ λ±μ λ¬Έμ κ° λ°μνλ€.
>
> β `equals()`, `hashCode()`λ₯Ό νμλ‘ κ΅¬νν΄μΌ νλ€.
>

<aside>
π‘ μ°Έκ³ 

λ³΅ν© ν€μλ @GenerateValueλ₯Ό μ¬μ©ν  μ μλ€

</aside>

## 7.3.3 λ³΅ν© ν€: μλ³ κ΄κ³ λ§€ν

![img_2.png](Chapter7_2_pic/img_2.png)

> λΆλͺ¨, μμ, μμκΉμ§ κ³μ κΈ°λ³Έ ν€λ₯Ό μ λ¬νλ μλ³ κ΄κ³
>

### `@IdClass`μ μλ³ κ΄κ³

*`Parent`*

```java
@Getter
@Setter
@Entity
public class Parent {
    //
    @Id
    @Column(name = "PARENT_ID")
    private String id1; //ParentId.id1κ³Ό μ°κ²°
    private String name;
}
```

*`Child`*

```java
@Getter
@Setter
@Entity
@IdClass(ChildId.class)
public class Child {
    //
    @Id
    @ManyToOne
    @JoinColumn(name = "PARENT_ID")
    private Parent parent;

    @Id
    @Column(name = "CHILD_ID")
    private String childId;

    private String name;
}
```

*`ChildId`*

```java
@Getter
@Setter
@EqualsAndHashCode
public class ChildId implements Serializable {
    //
    private String parent; // Child.parent λ§€ν
    private String childId; // Child.childId λ§€ν
}
```

*`GrandChild`*

```java
@Getter
@Setter
@Entity
public class GrandChild {
    //
    @Id
    @ManyToOne
    @JoinColumns ({
            @JoinColumn (name = "PARENT_ID"),
            @JoinColumn (name = "CHILD_ID")
    })
    private Child child;

    @Id
    @Column(name = "GRANDCHILD_ID")
    private String id;

    private String name;
}
```

*`GrandChildId`*

```java
@Getter
@Setter
@EqualsAndHashCode
public class GrandChildId implements Serializable {
    //
    private ChildId childId; // GrandChild.parent λ§€ν
    private String id;       // GrandChild.id λ§€ν
}
```

> μλ³ κ΄κ³λ κΈ°λ³Έ ν€μ μΈλ ν€λ₯Ό κ°μ΄ λ§€νν΄μΌ νλ€.
>
>
> λ°λΌμ μλ³μ λ§€νμΈ `@Id`μ μ°κ΄κ΄κ³ λ§€νμΈ `@ManyToOne`μ κ°μ΄ μ¬μ©νλ©΄ λλ€.
>
> ```java
> @Id // => κΈ°λ³Έ ν€ λ§€ν
> @ManyToOne // => μΈλ ν€ λ§€ν
> 
> @JoinColumn(name = "PARENT_ID")
> private Parent parent;
> ```
>

### `@EmbeddedId`μ μλ³ κ΄κ³

*`Parent`*

```java
@Getter
@Setter
@Entity
public class EmParent {
    //
    @Id
    @Column(name = "PARENT_ID")
    private String id;
    private String name;
}
```

*`Child`*

```java
@Getter
@Setter
@Entity
@IdClass(ChildId.class)
public class Child {
    //
    @Id
    @ManyToOne
    @JoinColumn(name = "PARENT_ID")
    private Parent parent;

    @Id
    @Column(name = "CHILD_ID")
    private String childId;

    private String name;
}
```

*`ChildId`*

```java
@Getter
@Setter
@EqualsAndHashCode
@Embeddable
public class EmChildId implements Serializable {
    //
    private String parentId; //@MapsId("parentID")λ‘ λ§€ν

    @Column(name = "CHILD_ID")
    private String id;

}
```

*`GrandChild`*

```java
@Getter
@Setter
@Entity
public class EmGrandChild {
    //
    @EmbeddedId
    private EmGrandChildId id;

    @MapsId("childId")
    @ManyToOne
    @JoinColumns({
            @JoinColumn(name = "PARENT_ID"),
            @JoinColumn(name = "CHILD_ID")
    })
    private EmChild child;
    private String name;
}
```

*`GrandChildId`*

```java
@Getter
@Setter
@EqualsAndHashCode
public class EmGrandChildId implements Serializable {
    //
    private EmChildId childId; //@MapsId("childId") λ‘ λ§€ν

    @Column(name = "GRANDCHILD_ID")
    private String id;
}
```

> `@EmbeddedId` λ‘ μλ³ κ΄κ³λ₯Ό κ΅¬μ±ν  λλ @MapsIdλ₯Ό μ¬μ©ν΄μΌ νλ€.
>
>
> Childμν°ν°μ parent νλ νμΈ
>
> ```java
> @Id
> @ManyToOne
> @JoinColumn(name = "PARENT_ID")
> private Parent parent;
> ```
>
> `@IdClass`μμ μ°¨μ΄μ μ `@Id`λμ μ `@MapsId`λ₯Ό μ¬μ©ν μ μ΄λ€.
>
> `@MapsId`λ μΈλ ν€μ λ§€νν μ°κ΄κ΄κ³λ₯Ό κΈ°λ³Έ ν€μλ λ§€ννκ² λ€λ λ»μ΄λ€.
>
> `@MapsId`μ μμ± κ°μ `@EmbeddedId`λ₯Ό μ¬μ©ν μλ³μ ν΄λμ€μ κΈ°λ³Έ ν€ νλλ₯Ό μ§μ νλ©΄ λλ€.
>

## 7.3.4 λΉμλ³ κ΄κ³λ‘ κ΅¬μ±

![img_3.png](Chapter7_2_pic/img_3.png)

> μλ³ κ΄κ³μ λΉκ΅νλ©΄ λ§€ν, μ½λκ° λ¨μνλ€. λν λ³΅ν© ν€κ° μμΌλ―λ‘ λ³΅ν© ν€ ν΄λμ€λ₯Ό λ§λ€μ§ μμλ λλ€.
>

*`Parent`*

```java
@Getter
@Setter
@Entity
public class Parent {
    //
    @Id
    @GeneratedValue
    @Column(name = "PARENT_ID")
    private Long id;
    private String name;
}
```

*`Child`*

```java
@Getter
@Setter
@Entity
public class Child {
    //
    @Id
    @GeneratedValue
    @Column(name = "CHILD_ID")
    private Long id;
    private String name;

    @ManyToOne
    @JoinColumn(name = "PARENT_ID")
    private Parent parent;
}
```

*`GrandChild`*

```java
@Getter
@Setter
@Entity
public class GrandChild {
    //
    @Id
    @GeneratedValue
    @Column(name = "GRANDCHILD_ID")
    private Long id;
    private String name;

    @ManyToOne
    @JoinColumn(name = "CHILD_ID")
    private Child child;
}
```

## 7.3.5 μΌλμΌ μλ³ κ΄κ³

![img_4.png](Chapter7_2_pic/img_4.png)

> μΌλμΌ μλ³ κ΄κ³λ μμ νμ΄λΈμ κΈ°λ³Έ ν€ κ°μΌλ‘ λΆλͺ¨ νμ΄λΈμ κΈ°λ³Έ ν€ κ°λ§ μ¬μ©νλ€.
>
>
> κΈ°λ³Έ ν€κ° λ³΅ν© ν€κ° μλλ©΄ μμ νμ΄λΈμ κΈ°λ³Έ ν€λ λ³΅ν© ν€λ‘ κ΅¬μ±νμ§ μμλ λλ€.
>

*`Board`*

```java
@Getter
@Setter
@Entity
public class Board {
    //
    @Id
    @GeneratedValue
    @Column(name = "BOARD_ID")
    private Long id;

    private String title;

    @OneToOne(mappedBy = "board")
    private BoardDetail boardDetail;
}
```

*`BoardDetail`*

```java
@Getter
@Setter
@Entity
public class BoardDetail {
    //
    @Id
    private Long boardId;

    @MapsId // BoardDetail.boardIdλ§€ν
    @OneToOne
    @JoinColumn(name = "BOARD_ID")
    private Board board;

    private String content;
}
```

## 7.3.6 μλ³, λΉμλ³ κ΄κ³μ μ₯λ¨μ 

### DB κ΄μ 

> DB μ€κ³ κ΄μ μμλ μλμ μ΄μ λ‘ μλ³ κ΄κ³λ³΄λ€λ λΉμλ³ κ΄κ³λ₯Ό λ μ νΈνλ€.
>
- μλ³ κ΄κ³λ λΆλͺ¨ νμ΄λΈμ κΈ°λ³Έ ν€λ₯Ό μμ νμ΄λΈλ‘ μ ννλ©΄μ μμ νμ΄λΈμ κΈ°λ³Έ ν€ μ»¬λΌμ΄ μ μ  λμ΄λλ€
- μλ³ κ΄κ³λ 2κ° μ΄μμ μ»¬λΌμ ν©ν΄μ λ³΅ν© κΈ°λ³Έ ν€λ₯Ό λ§λ€μ΄μΌ νλ κ²½μ°κ° λ§λ€.
- μλ³ κ΄κ³λ₯Ό μ¬μ©ν  λ κΈ°λ³Έ ν€λ‘ λΉμ¦λμ€ μλ―Έκ° μλ μμ° ν€ μ»¬λΌμ μ‘°ν©νλ κ²½μ°κ° λ§λ€.

  λ°λ©΄ λΉμλ³ κ΄κ³μ κΈ°λ³Έ ν€λ λΉμ¦λμ€μ μ ν κ΄κ³μλ λλ¦¬ ν€λ₯Ό μ£Όλ‘ μ¬μ©νλ€.

  λΉμ¦λμ€μ μκ΅¬μ¬ν­μ μΈμ  κ° λ³νλλ°, μλ³ κ΄κ³μ μμ° ν€ μ»¬λΌλ€μ΄ μμμ μμκΉμ§ μ νλλ©΄ λ³κ²½νκΈ° νλ€λ€(Cube, Cardμ μ΄λ¦μ λ°κΎΌλ€κ³  μκ°νλ©΄?)

- μλ³ κ΄κ³λ λΆλͺ¨ νμ΄λΈμ κΈ°λ³Έ ν€λ₯Ό μμ νμ΄λΈμ κΈ°λ³Έ ν€λ‘ μ¬μ© β νμ΄λΈ κ΅¬μ‘°κ° μ μ°νμ§ λͺ»νλ€.

### κ°μ²΄ κ΄μ 

> κ°μ²΄ κ΄κ³ λ§€νμ κ΄μ μμλ μλμ μ΄μ λ‘ λΉμλ³ κ΄κ³λ₯Ό λ μ νΈνλ€.
>
- μλ³ κ΄κ³λ 2κ° μ΄μμ μ»¬λΌμ λ¬Άμ λ³΅ν© κΈ°λ³Έ ν€λ₯Ό μ¬μ©νλ€(μΌλμΌ μ μΈ) β λ³΅ν© ν€ ν΄λμ€ νμ β νλ¦
- λΉμλ³ κ΄κ³λ μ£Όλ‘ λλ¦¬ ν€λ₯Ό κΈ°λ³Έ ν€λ‘ μ¬μ©νλλ°, JPAλ `@GenerateValue`λ‘ νΈνκ² μμ±ν  μ μλ€.

### μλ³ κ΄κ³λ§μ κ΄μ 

> λ°λ©΄ μλ³ κ΄κ³κ° κ°μ§λ μ₯μ λ μλ€.
>
- κΈ°λ³Έ ν€ μΈλ±μ€ νμ©μ΄ μ©μ΄
- μμ νμ΄λΈλ€μ κΈ°λ³Έ ν€ μ»¬λΌμ μμ, μμκ° κ°μ§κ³  μμ

  β νΉμ  μν©μ μ‘°μΈ μμ΄ νμ νμ΄λΈλ§μΌλ‘λ κ²μ κ°λ₯


### μ λ¦¬

> **κ°λ₯νλ©΄ λΉμλ³ κ΄κ³ μ¬μ©, κΈ°λ³Έ ν€λ Long νμμ λλ¦¬ ν€ μ¬μ©**
>
>
> Integerλ 20μ΅ / Longμ μ½920κ²½ β ν¨μ¬ μμ 
>
> μ νμ  λΉμλ³ λ³΄λ€λ νμμ  λΉμλ³ κ΄κ³κ° μΆμ²
>
> β νμμ  λΉμλ³ κ΄κ³λ NOT NULLλ‘ ν­μ κ΄κ³κ° μλ€λ κ²μ λ³΄μ₯ β **λ΄λΆ μ‘°μΈμΌλ‘ ν΄κ²°κ°λ₯**

# 7.4 μ‘°μΈ νμ΄λΈ

DBλ°μ΄ν° λ² μ΄μ€ νμ΄λΈμ μ°κ΄κ΄κ³λ₯Ό μ€κ³νλ λ°©λ²μ ν¬κ² λκ°μ§λ€

- μ‘°μΈ μ»¬λΌ μ¬μ©

  ![img_5.png](Chapter7_2_pic/img_5.png)

  > κ°κ°μ νμ΄λΈμ λ°μ΄ν°λ₯Ό λ±λ‘ νλ€κ° νμμ΄ μν  λ μ¬λ¬Όν¨μ μ ν ν  μ μλ€κ³  κ°μ 
  >
  >
  > νμμ΄ μ¬λ¬Όν¨μ μ¬μ©νκΈ° μ μλ μμ§ λ μ¬μ΄μ κ΄κ³κ° μμΌλ―λ‘ LOCKER_ID μΈλ ν€μ nullμ μλ ₯ν΄λμ΄μΌ νλ€. β μΈλ ν€μ nullμ νμ© νλ κ΄κ³λ₯Ό **μ νμ  λΉμλ³ κ΄κ³**λΌ νλ€.
  >

  ![img_6.png](Chapter7_2_pic/img_6.png)

  > μ νμ  λΉμλ³ κ΄κ³λ μΈλ ν€μ nullμ νμ©νλ―λ‘ μ‘°μΈ μ **μΈλΆ μ‘°μΈ**μ μ¬μ©ν΄μΌ νλ€
  >
  >
  > λ΄λΆ μ‘°μΈμ μ¬μ©νλ©΄ μ¬λ¬Όν¨κ³Ό κ΄κ³κ° μλ νμμ μ‘°ν λμ§ μλλ€.
>

- μ‘°μΈ νμ΄λΈ μ¬μ©

  ![img_7.png](Chapter7_2_pic/img_7.png)

  > μ‘°μΈ νμ΄λΈμ΄λΌλ λ³λμ νμ΄λΈμ μ¬μ©ν΄μ μ°κ΄κ΄κ³λ₯Ό κ΄λ¦¬νλ€.
  >
  >
  > μ‘°μΈ νμ΄λΈμμ λ νμ΄λΈμ μΈλ ν€λ₯Ό κ°μ§κ³  μ°κ΄κ΄κ³λ₯Ό κ΄λ¦¬νλ€.
  >

  ![img_8.png](Chapter7_2_pic/img_8.png)

  > memberνμ΄λΈμ κ±΄λλ¦΄ νμ μμ΄ memberμμ lockerλ₯Ό λ±λ‘νκ³  μΆλ€λ©΄ μ‘°μΈ νμ΄λΈλ§ μμ νλ©΄ λλ€.
  >
  >
  > νμ§λ§ member, lockerνμ΄λΈμ μ‘°μΈ νλ €λ©΄ **μ‘°μΈ νμ΄λΈκΉμ§ ν¨κ» μ‘°μΈν΄μ£Όμ΄μΌ νλ λ¨μ **μ΄ μλ€.
>
- νμΈ νμ

  ![img_9.png](Chapter7_2_pic/img_9.png)

  μλκ° μ‘°μΈ νμ΄λΈ μΈμ€ μμμ§λ§ μ½λ μμΌλ‘λ *`@ElementCollection`, `@CollectionTable`*μ μ¬μ©νλ€.

  μΌλλ€ κ²½μ°μ μ¬μ©λλ κ²μΈλ―

  https://medium.com/nerd-for-tech/elementcollection-vs-onetomany-in-hibernate-7fb7d2ac00ea

  ![img_10.png](Chapter7_2_pic/img_10.png)


## 7.4.1 μΌλμΌ μ‘°μΈ νμ΄λΈ

> μΌλμΌ κ΄κ³λ₯Ό λ§λ€λ €λ©΄ μ‘°μΈ νμ΄λΈμ μΈλ ν€ μ»¬λΌ κ°κ°μ μ΄ 2κ°μ μ λν¬ μ μ½ μ‘°κ±΄μ κ±Έμ΄μΌ νλ€.
>

*`Parent`*

```java
@Getter
@Setter
@Entity
public class Parent {
    //
    @Id
    @GeneratedValue
    @Column(name = "PARENT_ID")
    private long id;
    private String name;

    @OneToOne
    @JoinTable(name = "PARENT_CHILD",
        joinColumns = @JoinColumn(name = "PARENT_ID"),
        inverseJoinColumns = @JoinColumn(name = "CHILD_ID"))
    private List<Child> children = new ArrayList<>();
}
```

*`Child`*

```java
@Getter
@Setter
@Entity
public class Child {
    //
    @Id
    @GeneratedValue
    @Column(name = "CHILD_ID")
    private Long id;
    private String name;
}
```

### *`@JoinTable`*μμ±

- `name` : λ§€νν  μ‘°μΈ νμ΄λΈ μ΄λ¦
- `joinColumns` : νμ¬ μν°ν°λ₯Ό μ°Έμ‘°νλ μΈλ ν€
- `inverseJoinColumns` : λ°λ λ°©ν₯ μν°ν°λ₯Ό μ°Έμ‘°νλ μΈλ ν€

## 7.4.2 μΌλλ€ μ‘°μΈ νμ΄λΈ

> μΌλλ€ κ΄κ³λ₯Ό λ§λλ €λ©΄ μ‘°μΈ νμ΄λΈμ μ»¬λΌ μ€ λ€(N)κ³Ό κ΄λ ¨λ μ»¬λΌμΈ CHILD_IDμ μ λν¬ μ μ½ μ‘°κ±΄μ κ±Έμ΄μΌ νλ€.
>

*`Parent`*

```java
@Getter
@Setter
@Entity
public class Parent {
    //
    @Id
    @GeneratedValue
    @Column(name = "PARENT_ID")
    private long id;
    private String name;

    @OneToMany
    @JoinTable(name = "PARENT_CHILD",
        joinColumns = @JoinColumn(name = "PARENT_ID"),
        inverseJoinColumns = @JoinColumn(name = "CHILD_ID"))
    private List<Child> children = new ArrayList<>();
}
```

> λ€λμΌμ Childμͺ½μ @JoinTableμ κ±Έμ΄μ€λ€
>

*`Parent`*

```java
@Getter
@Setter
@Entity
public class Parent {
    //
    @Id
    @GeneratedValue
    @Column(name = "PARENT_ID")
    private long id;
    private String name;

    @OneToMany(mappedBy = "parent")
    private List<Child> children = new ArrayList<>();
}
```

*`Child`*

```java
@Getter
@Setter
@Entity
public class Child {
    //
    @Id
    @GeneratedValue
    @Column(name = "CHILD_ID")
    private Long id;
    private String name;

    @ManyToOne(optional = false)
    @JoinTable(name = "PARENT_CHILD",
            joinColumns = @JoinColumn(name = "CHILD_ID"),
            inverseJoinColumns = @JoinColumn(name = "PARENT_ID"))
    private Parent parent;
}
```

## 7.4.4 λ€λλ€ μ‘°μΈ νμ΄λΈ

> λ€λλ€ κ΄κ³λ μ‘°μΈ νμ΄λΈμ λ μ»¬λΌμ ν©ν΄μ νλμ λ³΅ν© μ λν¬ μ μ½μ‘°κ±΄μ κ±Έμ΄μΌ νλ€.
>

*`Parent`*

```java
@Getter
@Setter
@Entity
public class Parent {
    //
    @Id
    @GeneratedValue
    @Column(name = "PARENT_ID")
    private long id;
    private String name;

    @ManyToMany
    @JoinTable(name = "PARENT_CHILD",
        joinColumns = @JoinColumn(name = "PARENT_ID"),
        inverseJoinColumns = @JoinColumn(name = "CHILD_ID"))
    private List<Child> children = new ArrayList<>();
}
```

*`Child`*

```java
@Getter
@Setter
@Entity
public class Child {
    //
    @Id
    @GeneratedValue
    @Column(name = "CHILD_ID")
    private Long id;
    private String name;
}
```

# 7.5 μν°ν° νλμ μ¬λ¬ νμ΄λΈ λ§€ν

> @SecondaryTableμ μ¬μ©νλ©΄ ν μν°ν°μ μ¬λ¬ νμ΄λΈμ λ§€ν ν  μ μλ€.
>

![img_11.png](Chapter7_2_pic/img_11.png)

```java
@Getter
@Setter
@Entity
@Table(name = "BOARD")
**@SecondaryTable(name = "BOARD_DETAIL",
    pkJoinColumns = @PrimaryKeyJoinColumn(name = "BOARD_DETAIL_ID"))**
public class Board {
    //
    @Id
    @GeneratedValue
    @Column(name = "BOARD_ID")
    private Long id;

    private String title;

    **@Column(table = "BOARD_DETAIL")**
    private String content;
}
```

### *`@SecondaryTable`* μμ±

- `@SecondaryTable.name` : λ§€νν  λ€λ₯Έ νμ΄λΈμ μ΄λ¦
- `@SecondaryTable.pkJoinColums` : λ§€νν  λ€λ₯Έ νμ΄λΈμ κΈ°λ³Έ ν€ μ»¬λΌ μμ±

```java
@Column(table = "BOARD_DETAIL")
private String content;
```

> contentνλλ `BOARD_DETAIL` νμ΄λΈμ μ»¬λΌμ λ§€ννλ€.
>
>
> νμ΄λΈμ μ§μ νμ§ μμΌλ©΄ κΈ°λ³Έ νμ΄λΈμΈ `BOARD`μ λ§€νλλ€.
>

**β λ νμ΄λΈμ νλμ μν°ν°μ λ§€ννλ κ² λ³΄λ€λ νμ΄λΈ λΉ μν°ν°λ₯Ό κ°κ° λ§λλ κ²μ κΆμ₯νλ€. - μ΅μ νκ° νλ€κΈ° λλ¬Έμ΄λ€**