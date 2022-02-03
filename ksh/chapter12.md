[TOC]



# Chapter12. Spring-Data-JPA



- spring data JPA :  spring framework에서 JPA를 편리하게 사용할 수 있도록 지원하는 프로젝트.

  > The goal of the Spring Data repository abstraction is to significantly reduce the amount of boilerplate code required to implement data access layers for various persistence stores.

  - **https://spring.io/projects/spring-data-jpa**
  - https://docs.spring.io/spring-data/jpa/docs/current/reference/html/#reference

- CRUD를 처리하기 위한 공통 interface 제공.

- repository 개발시에 interface만 작성하면 실행 시점에 spring data JPA 가 구현 객체를 동적으로 생성해서 주입한다. 따라서 **데이터 접근 계층을 개발할 때 구현 클래스 없이 interface만 작성해도 개발을 완료할 수 있다**.

  

``` java
package jpa.example.repository;

import jpa.example.entity.Member;
import org.springframework.data.domain.Page;
import org.springframework.data.domain.Pageable;
import org.springframework.data.jpa.repository.JpaRepository;

public interface MemberRepository extends JpaRepository<Member, String> {

    Page<Member> findAll(Pageable pageable);
}

```



---



## 제공하는 Repository interface

<img src="https://mblogthumb-phinf.pstatic.net/MjAxOTA3MTZfMjQ3/MDAxNTYzMjc0NDkzMTM4.GzXFfLL4_KDQsFZ5lqJ8LpzpAKeXeXkaf4tdRLrABLcg.5t2-LUUnQekg4kBICyVId9iT0A8wdVpUvU2tWc8odggg.PNG.writer0713/%EC%8A%A4%ED%81%AC%EB%A6%B0%EC%83%B7_2019-07-16_%EC%98%A4%ED%9B%84_7.54.42.png?type=w800" style="zoom:50%;" />



---



- spring data JPA는 유연한 반환 타입을 지원하는데 결과가 1건 이상이면 collection interface를 사용하고, 단건이면 반환 타입을 지정한다. 만약 **조회 결과가 없으면 컬렉션은 빈 컬렉션을 반환**한다. 만약 단건을 기대했는데 2건 이상이 조회되면 NoUniqueResultException이 발생한다.



---



## Paging

- Sort :  정렬기능
- Pageable  : 페이징 기능
  - `Pageable`을 사용하면 반환타입으로 `List`나 `org.springframework.data.domain.Page`를 사용할 수 있다.

- `Page`를 사용하면 spring data JPA는 페이징 기능을 제공하기 위해 검색된 전체 데이터 건수를 조회하는 **`count` 쿼리를 추가로 호출**한다.

  - ```jAva
    public interface MemberRepository extends JpaRepository<Member, String> {
        Page<Member> findAll(Pageable pageable);
    }
    ```

- 참고로 페이지는 0부터 시작한다.

- `Pageable`은 인터페이스이고 실제 사용할 때는 `PageRequest`객체를 사용한다.

  - ```java
    Pageable pageable = PageRequest.of(offset, limit, getMemberSort(memberSort));
    ```

    

아래는 내가 테스트로 작성했던 코드의 일부다.

```java
import org.springframework.data.domain.Page;
import org.springframework.data.domain.PageRequest;
import org.springframework.data.domain.Pageable;
import org.springframework.data.domain.Sort;


public class MemberLogic {
    public Page<Member> findAll(int offset, int limit, MemberSort memberSort) {
        Pageable pageable = PageRequest.of(offset, limit, getMemberSort(memberSort));

        // count쿼리 추가로 호출
        return memberRepository.findAll(pageable);

    }

    private Sort getMemberSort(MemberSort memberSort) {
        switch (memberSort) {
            case UsernameAsc:
                return Sort.by(new Sort.Order(Sort.Direction.ASC, "username"));
            case UsernameDesc:
                return Sort.by(new Sort.Order(Sort.Direction.DESC, "username"));
            case AgeDesc:
                return Sort.by(new Sort.Order(Sort.Direction.DESC, "age"));
            case AgeAsc:
                return Sort.by(new Sort.Order(Sort.Direction.ASC, "age"));
            default:
                return Sort.by(new Sort.Order(Sort.Direction.ASC, "username"));
        }
    }
}
```





---



## 사용자 정의 repository 구현

- spring data JPA는 필요한 메서드만 구현할 수 있는 방법을 제공한다.

- 아래 코드와 같이 클래스 이름을 지을 때는 <u>repository interface 이름 + Impl</u> 로 지어야 한다. 이렇게 하면 spring data JPA가 사용자 정의 구현 클래스로 인식한다.

  

1. 사용자 정의 인터페이스 생성

```java
public interface MemberRepositoryCustom {
    //
    public List<Member> findMemberCustom();
}
```

2. implements해서 구현

```java
public class MemberRepositoryImpl implements MemberRepositoryCustom {
    
    @Override
    public List<Member> findMemberCustom() {
        //사용자 정의 구현
    }
}
```

