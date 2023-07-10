---
title: '고급 매핑'
subtitle: '상속관계 매핑과 @MappedSuperClass'
date: 2023-07-07 00:35:00
category: 'JPA'
---

## 상속관계 매핑

객체와 관계형 데이터베이스의 차이가 또 하나 있습니다. 바로, **데이터베이스에는 상속관계가 존재하지 않는다**는 것입니다. 상속이 객체지향 프로그래밍에서 아주 중요한 개념임을 생각하면 데이터베이스에서도 테이블 간의 상속관계를 표현할 방법이 있어야 합니다. 다행히도 상속관계와 유사한 데이터베이스 모델링 기법이 있습니다. **슈퍼타입 서브타입 관계**라는 모델링 기법으로, 상속관계 매핑은 객체의 상속과 구조를 DB의 슈퍼타입 서브타입 관계를 매핑하는 것을 말합니다.

<img width="1160" alt="supersubtype" src="https://github.com/GDSC-SKHU/moida-backend/assets/14046092/b80d8291-ac2f-4260-9991-dd520c565e09">

<div align="center"><I>DB의 슈퍼타입 서브타입 관계와 객체의 상속관계 </I></div>
<br>

슈퍼타입 서브타입 논리 모델을 물리 모델로 구현하는 방법에는 여러 전략들이 있습니다. 전략들을 살펴보기 전, 상속관계 매핑을 위해 꼭 알아둬야 하는 어노테이션이 있습니다.

- `@Inheritance(strategy = InheritanceType.타입)`
  - `JOINED` : 조인 전략
  - `SINGLE_TABLE` : 단일 테이블 전략
  - `TABLE_PER_CLASS` : 구현 클래스 별 테이블 전략
- `@DiscriminatorColumn(name = "DTYPE")`
- `@DiscriminatorValue("구분자 이름")`

첫 번째 어노테이션 `@Inheritance`는 `JOINED`, `SINGLE_TABLE`과 같은 상속 전략 타입과 함께 **부모 클래스에 입력**해 상속관계 매핑 시, 원하는 전략을 사용할 수 있도록 해줍니다. `@Inheritance` 어노테이션의 기본 전략은 `SINGLE_TABLE`입니다.

다음은 각 자식 클래스를 데이터베이스 상에서 구분하기 위해 **부모 클래스에 사용**되는 **구분자 컬럼 설정**을 위한 `@DiscriminatorColumn` 어노테이션입니다. 컬럼명을 직접 정할 수도 있으며 기본 이름은 `DTYPE`입니다.

마지막 어노테이션인 `@DiscriminatorValue`는 `@DiscriminatorColumn`에 저장될 값을 지정하는 자식 클래스용 어노테이션입니다. 해당 어노테이션에 적어둔 값이 부모 테이블의 구분자 컬럼에 저장됩니다. 기본값은 자식 엔티티의 이름입니다.

```java
// 부모 클래스(엔티티)
@Entity
@Inheritance(strategy = InheritanceType.JOINED) // 조인 전략으로 상속관계 매핑
@DiscriminatorColumn // 구분자 컬럼 DTYPE
public class Item {

    @Id @GeneratedValue
    private Long id;

    private String name;

    private int price;
    ...
}
```

```java
// 자식 클래스(엔티티)
@Entity
@DiscriminatorValue("MOVIE") // DTYPE MOVIE
public class Movie extends Item {

    private String director;

    private String actor;
    ...
}
```

위 코드가 상속관계 매핑을 완료한 코드입니다. 이렇게 JPA를 활용하면 간단하게 데이터베이스에서 상속관계를 표현할 수 있습니다. `Item` 엔티티의 테이블에는 `DTYPE`이라는 컬럼이 추가될 것이고, **조인 전략**이라는 것과, 구분자 값 `MOVIE`가 `Item`의 `DTYPE` 컬럼에 입력될 것이라는 것도 알 수 있습니다.

## 조인 전략

위의 예제를 통해 잠깐 알아본 **조인 전략**에 대해서 더 자세히 알아보겠습니다. 조인 전략은 말 그대로 SQL `JOIN` 연산을 활용하는 전략으로, 슈퍼타입과 서브타입 테이블을 각각 만들고, 자식 테이블이 부모 테이블의 기본 키를 받아서 자신의 기본 키이자 외래 키로 사용합니다. 당연히 데이터 조회 시, `JOIN` 연산을 통해 두 테이블을 함께 조회합니다.

<img width="1240" alt="joinstrategy" src="https://github.com/GDSC-SKHU/moida-backend/assets/14046092/e0508c38-9cd5-445c-9078-8cdff7b03627">

<div align="center"><I>객체의 상속처럼 부모 테이블에 공통 속성을 두고 자식 클래스에 구체적인 속성을 둔다.</I></div>
<br>

한 눈에 보기에도 객체와 테이블의 구조가 유사해보입니다. 실제로 조인 전략은 가장 정석적인 전략이라고 합니다.

이제 데이터베이스 테이블 구성도 완료했습니다. 조인 전략을 통해 모든 엔티티의 테이블들을 각각 생성했고, 자식 테이블들이 부모 테이블의 기본 키를 가져와 자신의 기본 키와 외래 키로 설정했습니다. 만약, 테이블에 엔티티를 저장할 때, 엔티티가 어떻게 저장되는지도 알아보겠습니다.

```java
Movie movie = new Movie();
movie.setName("JPA는 아름다워");
movie.setPrice(202307);
movie.setDirector("Hibernate");
movie.setActor("CodeRoad");
em.persist(movie);
```

위와 같이 `Movie` 엔티티를 영속화하면 JPA는 다음과 같은 SQL 쿼리를 작성합니다.

```java
Hibernate:
    /* insert hellojpa.Movie
        */ insert
        into
            Item
            (name, price, DTYPE, id)
        values
            (?, ?, 'MOVIE', ?)
Hibernate:
    /* insert hellojpa.Movie
        */ insert
        into
            Movie
            (actor, director, id)
        values
            (?, ?, ?)
```

분명히 우리는 `Movie` 엔티티 하나만 저장했는데 `INSERT` 쿼리가 2개 작성되어 데이터베이스로 향했습니다. 부모 테이블 `Item`과 자식 테이블 `Movie`에 하나씩 전송된 쿼리는 각각 데이터를 저장했습니다. 사실, 이 쿼리는 정확하게 잘 작성된 쿼리입니다. 그 이유는, 객체 입장에서는 `Item`을 상속한 `Movie`가 `Item`의 필드와 메소드들을 알고 있고 활용할 수 있지만, 데이터베이스 테이블 입장에서는 두 클래스의 속성들은 완전히 별개이기 때문입니다.

데이터베이스에서는 우리가 아무리 상속관계 매핑을 해준다고 하더라도 객체와 같이 자식인 `Movie` 테이블이 부모 테이블 `Item`의 컬럼을 상속 받아 자유자재로 다룰 수 있는 것은 아닙니다. 부모 테이블에는 다른 자식 테이블들과 공유되는 공통 속성만 있고, 각 자식 테이블은 자신 고유의 속성들만 가지고 있습니다. 이후, 자식 엔티티 `Movie`를 조회할 때, 부모와 자식 테이블을 `JOIN` 연산을 통해 합친 후, 온전한 `Movie`의 데이터를 반환하는 것입니다.

`Movie` 엔티티를 조회할 때 `Item` 테이블과 `Movie` 테이블을 `JOIN`한 후, `Item` 테이블의 `id`, `name`, `price`와 `Movie` 테이블의 `actor`, `director` 속성들을 하나로 모아 `Movie` 엔티티에 대한 정보로 반환하는 것을 볼 수 있습니다.

```java
Hibernate:
    select
        movie0_.id as id2_5_0_,
        movie0_1_.name as name3_5_0_,
        movie0_1_.price as price4_5_0_,
        movie0_.actor as actor1_7_0_,
        movie0_.director as director2_7_0_
    from
        Movie movie0_
    inner join
        Item movie0_1_
            on movie0_.id=movie0_1_.id
    where
        movie0_.id=?
```

즉, 부모 클래스의 속성은 부모 테이블에만, 자식 클래스의 속성은 자식 테이블에만 저장하고 조회할 때 `JOIN` 연산으로 합쳐 하나로 보이게 하는 것이 이 조인 전략의 동작 방법입니다.

조인 전략의 장점은 각 테이블에 중복되는 컬럼이 없기 때문에(공통 속성을 부모 테이블이 가지고 있기 때문에), 잘 **정규화**되어 있고 **외래 키 무결성 제약 조건을 활용**할 수 있습니다. 또한, 각 테이블의 모든 행이 **저장 공간을 효율적으로 사용**할 수 있습니다.

단점은 아무래도 매 조회마다 `JOIN` 연산을 사용하기 때문에 성능 저하가 있을 수 있다는 것과 조회 쿼리가 단순히 `SELECT` 연산만 사용하는 것이 아니기 때문에 복잡하다는 것입니다. 거기다, 한 엔티티를 저장할 때, 부모와 자식 테이블에 각각 데이터를 나눠서 저장해야하기 때문에 `INSERT` 쿼리가 두 번 나간다는 것도 단점입니다.

## 단일 테이블 전략

## 구현 클래스 별 테이블 전략

## 부록 : @MappedSuperClass
