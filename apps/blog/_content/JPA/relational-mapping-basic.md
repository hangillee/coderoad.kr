---
title: '연관관계 매핑 - 기초'
subtitle: '연관관계의 방향과 연관관계의 주인'
date: 2023-01-17 00:00:00
category: 'JPA'
---

## 연관관계?

JPA와 같은 ORM 기술을 공부하다 보면, '**연관관계**'라는 단어를 자주 마주칠 수 있습니다. 이 단어는 먼저 객체지향 설계의 목표를 먼저 알아야 완벽하게 이해할 수 있습니다. [객체지향의 사실과 오해](https://blog.coderoad.kr/essence-oop)라는 책에선 객체지향 설계의 목표를 다음과 같이 설명합니다.

> 객체지향 설계의 목표는 자율적인 객체들의 협력 공동체를 만드는 것이다.

만약 우리가 객체지향 설계를 통해 서비스를 만들고자 한다면 **자율적인 객체**들이 **협력**을 통해 사용자가 원하는 결과를 도출해내야합니다. 당연하게도 우리가 데이터베이스 테이블과 매핑한 엔티티들 역시 객체이며, 협력을 통해 우리가 원하는 결과를 만들어내도록 해야합니다. 즉, 객체지향 설계를 위해서 우리는 엔티티들에게 적절한 연관관계를 부여하여 엔티티들끼리 협력하게 해야 합니다. 그런데, 우리가 데이터베이스 테이블에 맞춰서 매핑한 엔티티들 간에는 연관관계가 전혀 없습니다. 아래는 연관관계가 없이 설계한 엔티티의 다이어그램과 코드입니다.

<img width="970" alt="object" src="https://github.com/SKHUMEET/skhumeet-backend/assets/14046092/fb8b2fc1-f023-4b1c-b0fa-25d940a086f6">
<div align="center"><I>데이터베이스 테이블에 맞춰서 설계한 엔티티</I></div>

```java
@Entity
public class Member {
    @Id @GeneratedValue
    private Long id;

    @Column(name = "USERNAME")
    private String name;

    @Column(name = "TEAM_ID")
    private Long teamId;

}
```

<img width="922" alt="table" src="https://github.com/SKHUMEET/skhumeet-backend/assets/14046092/d75f41e7-7e22-459a-8953-e0bb04fc8c6b">
<div align="center"><I>데이터베이스 테이블에 맞춰서 설계한 엔티티</I></div>

```java
@Entity
public class Team {
    @Id @GeneratedValue
    private Long id;
    private String name;
}
```

얼핏 보면 Member 엔티티에 있는 외래키 속성 덕분에 연관관계가 있는 것처럼 보일 수도 있지만 두 엔티티 객체 사이에는 아무런 연관관계도 없습니다. Member에서 Team을 바로 참조할 방법도, Team에서 Member를 바로 참조할 방법도 없습니다. 데이터베이스 테이블 스키마대로 엔티티를 설계했기 때문에 데이터 저장에는 문제가 없지만 이 방법은 두 객체가 협력할 방법이 전혀 없는 객체지향스럽지 않은 설계입니다. 즉, 객체를 테이블에 맞춰서 데이터를 중심으로 설계하면 협력 관계를 구축할 수 없습니다. 이러한 문제가 발생한 이유는 객체와 테이블이 서로 자신과 연관된 요소를 찾는 방법에 큰 차이가 있기 때문입니다.

## 단방향 연관관계

## 양방향 연관관계

## 연관관계의 주인

```

```

```

```
