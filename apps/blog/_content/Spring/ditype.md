---
title: '의존관계 자동 주입'
subtitle: '다양한 DI 방식들'
date: 2023-01-15 06:45:07
category: 'Spring'
---
**본 문서는 인프런에서 수강할 수 있는 [스프링 핵심 원리 - 기본편](https://inflearn.com/course/스프링-핵심-원리-기본편)을 수강한 후, 공부한 내용을 정리한 문서입니다. 본 문서의 모든 저작권은 해당 강의의 저자이신 [김영한](https://inflearn.com/users/@yh) 우아한형제들 기술이사님께 있습니다.**

### 다양한 의존관계 주입 방법
Spring의 의존관계 주입 방법에는 크게 4가지 방법이 있습니다.
* 생성자 주입
* 수정자 주입
* 필드 주입
* 일반 메소드 주입

#### 생성자 주입
생성자 주입 방식은 말 그대로 클래스의 생성자(Constructor)를 통해 의존관계를 주입 받습니다. 생성자를 매개체로 사용하기 때문에 생성자 호출 시점에 **딱 1번만 호출되는 것이 보장**되며, 변하지 않고 필수적인 의존관계에 사용합니다. 즉, 생성자 주입 방식을 사용하면 의존관계 주입된 인스턴스를 중간에 변경할 수 없습니다.

```java
public class MemberServiceImpl implements MemberService {
    //한 번 주입된 의존관계는 외부에서 변경할 수 없습니다!
    private final MemberRepository memberRepository;

    @Autowired //의존관계 자동 주입 기능을 사용했습니다.
    public MemberServiceImpl(MemberRepository memberRepository) {
        //Spring 컨테이너가 memberRepository 이름을 가진 빈을 주입해줍니다.
        this.memberRepository = memberRepository;
    }
}
```

만약, 생성자가 딱 1개만 존재하면 `@Autowired`가 없어도 의존관계가 자동 주입됩니다. 당연히 주입하려는 의존관계가 Spring 빈이어야만 가능합니다.

#### 수정자 주입
수정자, 흔히 `Setter`라고 불리는 **필드 값을 변경하는 수정자 메소드를 통해 의존관계를 주입**하는 방법입니다. 생성자 주입 방식과 다르게 외부에서 언제든 접근이 가능하기 때문에 선택적이고 변경 가능성이 있는 의존관계에 사용하는 방식입니다. `Setter`를 사용하여 필드에 접근하는 방식을 **Java 빈 프로퍼티 규약**이라 하는데, 수정자 주입 방식이 해당 규약을 따른다고 생각하면 됩니다.

```java
public class MemberServiceImpl implements MemberService {
    //final로 선언되지 않았기 때문에 수정될 수 있습니다.
    private MemberRepository memberRepository;

    @Autowired //의존관계 자동 주입 기능을 사용했습니다.
    public void setMemberRepository(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```

만약, 빈의 의존관계를 필수가 아닌 선택적으로 주입되도록 하려면, `@Autowired`를 `@Autowired(required = false)`와 같이 작성해야합니다.

#### 필드 주입
필드 주입은 위의 두 방식보다 훨씬 간단합니다.

```java
public class MemberServiceImpl implements MemberService {
    //이 한 줄이면 자동으로 의존관계가 주입됩니다!
    @Autowired
    private MemberRepository memberRepository;
}
```

너무나도 간단해서 많은 개발자들을 현혹시켰고, 실제로 많은 프로젝트에서 필드 주입 방식을 사용했다고 합니다. 그러나 필드 주입 방식에는 치명적인 단점들이 있어 **사용을 지양해야합니다.**

가장 큰 문제는 [SOLID](https://blog.coderoad.kr/solid)의 SRP를 위반할 가능성이 크다는 것입니다. Spring을 관통하는 **좋은 OOP를 포기하면서**까지 필드 주입 방식을 사용할 이유가 없습니다!

또한, 클래스의 코드만 봐서는 의존관계가 한 눈에 보이지 않는 **숨겨진 의존관계**(Hidden dependency) 문제가 발생합니다. 실제로 어떤 인스턴스(Spring 빈)가 의존관계 주입되어야 하고, 어떤 의존관계가 필수인지, 어떤 의존관계가 변하면 안 되는지 코드를 하나하나 뜯어봐야하는 매우 비효율적인 상황이 생기는 것입니다.

더군다나 필드를 `final`로 선언할 수 없어 언제든 변경될 수 있습니다. 이 경우에 변경된 의존관계로 인해 예상치 못한 에러가 발생해도 쉽게 알아차릴 수 없습니다. 바로 전에 설명한 문제점에서 알 수 있듯, 필드 주입 방식은 의존관계가 감춰져 있기 때문입니다!

TDD(Test Driven Development, 테스트 주도 개발)에도 악영향을 끼칩니다. 필드 주입 방식은 의존관계를 주입할 때, `@Autowired` 어노테이션을 보고 Spring 컨테이너가 자동으로 주입 해주는 방식입니다. 이는, 단위 테스트 시 DI 컨테이너, 다시 말해 Spring 컨테이너가 없으면 의존관계를 주입할 방법이 없다는 이야기입니다. `@Autowired`를 보고 의존관계를 주입해줘야 하는데 순수한 Java 테스트 코드는 Spring 컨테이너가 없으니 제대로 동작할리가 없습니다.

Spring이 등장한 이유가 특정 기술(EJB)에 종속적이었던 Java 개발 생태계를 개선하려던 것이었음을 생각하면, Spring에 종속적인 코드는 매우 모순적인 상황이 아닐 수 없습니다. 그러니 우리는 필드 주입 방식을 멀리해야합니다.

#### 일반 메소드 주입
일반적인 Java 메소드를 통해 의존관계를 주입 받을 수도 있습니다. 그러나, 일반적으로 잘 사용하지 않습니다.

```java
public class MemberServiceImpl implements MemberService {
    private MemberRepository memberRepository;

    @Autowired
    //사실 수정자 주입 방식과 별반 다를게 없습니다.
    public void dependencyInject(MemberRepository memberRepository) {
        this.memberRepository = memberRepository;
    }
}
```

### 옵션 처리

### 생성자 주입을 선택해야하는 이유

### Lombok

### 중복 빈이 존재할 때

### 조회한 빈이 모두 필요할 때

### 자동과 수동 빈 등록, 올바른 실무 운영법