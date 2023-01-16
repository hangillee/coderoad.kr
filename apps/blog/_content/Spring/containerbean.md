---
title: 'Spring 컨테이너와 빈'
subtitle: '컨테이너와 빈의 개념 정리'
date: 2023-01-15 06:45:00
category: 'Spring'
---

**본 문서는 인프런에서 수강할 수 있는 [스프링 핵심 원리 - 기본편](https://inflearn.com/course/스프링-핵심-원리-기본편)을 수강한 후, 공부한 내용을 정리한 문서입니다. 본 문서의 모든 저작권은 해당 강의의 저자이신 [김영한](https://inflearn.com/users/@yh) 우아한형제들 기술이사님께 있습니다.**

## Spring 컨테이너
Spring은 **[IoC(제어의 역전)](https://hangillee.github.io/docs/techstack/spring/04)** 개념을 통해 탄생한 구성자, **컨테이너**로 프로그램의 흐름을 제어합니다. Spring에서의 컨테이너를 **"Spring 컨테이너"**라고 부릅니다. Spring 컨테이너는 **`ApplicationContext`** 인터페이스를 통해 생성하는데, 이 `ApplicationContext` 자체를 Spring 컨테이너라고 하기도 합니다. 본격적으로 Spring을 활용하기 위해선 이 Spring 컨테이너를 생성해야합니다. 방법은 매우 간단합니다.

```java
//'AppConfig.class'는 프로그래머가 작성한 구성 정보를 담은 Java 설정 클래스입니다.
ApplicationContext applicationContext = new AnnotationConfigApplicationContext(AppConfig.class);
```

위 코드가 바로 Spring 컨테이너를 생성하는 코드입니다. **`AnnotationConfigApplicationContext`**는 `ApplicationContext` 인터페이스의 구현체로, **`AppConfig`**와 같이 구성 정보를 담은 **'어노테이션'** 기반의 **'Java 설정 클래스'**를 통해 Spring 컨테이너를 생성합니다. Spring 컨테이너를 생성하는 방식은 다양합니다. 그 중, 대표적인 두 가지가 위의 코드와 같이 어노테이션 기반의 Java 설정 클래스를 통한 생성법과 XML을 기반으로한 생성법입니다. 최근에는 주로 Java 설정 클래스를 통해 Spring 컨테이너를 생성합니다.

우리는 Spring 컨테이너를 생성할 때 Java 설정 클래스(이제 설정 클래스라 작성하겠습니다.)를 활용한다는 것을 알았습니다. 그런데 Spring이 프로그램의 수많은 클래스들 중에 설정 클래스가 무엇인지 어떻게 알 수 있을까요? 또, 어노테이션을 기반으로 한다는데, 어노테이션은 무엇일까요? 당연히 모든 클래스가 설정 클래스가 되는 것은 아닙니다. 프로그램의 구성 정보(DI 정보)를 담고 있다고 해서 자동으로 설정 클래스가 되는 것도 아닙니다. 프로그래머가 설정 클래스로 사용하고자 하는 클래스를 **어노테이션**을 통해 직접 명시해줘야 Spring이 이를 확인하고 컨테이너를 생성하는데 설정 클래스로 등록된 클래스를 활용합니다.

어노테이션은 Java의 문법으로 메타데이터의 일종입니다. 쉽게 설명하자면 해당 코드가 무엇인지 설명해주는 역할을 가집니다. Spring은 여러가지 자체적인 어노테이션들을 가지고 있고, 프로그래머가 이를 사용해 Spring의 기술을 활용할 수 있도록 합니다. 그 중 가장 기초가 되는 것이 바로 순수한 클래스를 설정 클래스로 만들어주는 `@Configuration` 어노테이션입니다. 클래스 선언문 앞에 해당 어노테이션을 붙혀주면 Spring은 해당 클래스를 자동으로 설정 클래스로 인식하고 그에 맞는 동작들을 수행합니다.

```java
//이제 AppConfig 클래스는 Spring에 의해 Java 설정 클래스로 관리됩니다.
@Configuration
public class AppConfig {
    ...
}
```

이렇게 `@Configuration` 어노테이션을 통해 설정 클래스로 등록된 `AppConfig` 클래스는 Spring 컨테이너 생성 시 프로그램의 구성 정보로 활용됩니다. Spring은 `AppConfig`에 적혀진 대로 객체를 생성하고, 관리하며, 의존관계를 주입합니다. 다시 말해, `@Configuration` 어노테이션에 의해 설정 클래스로 등록된 `AppConfig` 클래스는, IoC를 통해 프로그램의 제어권을 가지게 된 Spring 컨테이너에게 해당 프로그램의 설명서 역할을 하는 것입니다. `AnnotationConfigApplicationContext` 클래스를 기반으로 Spring 컨테이너를 생성하기 위해선 `AppConfig`와 같은 설정 클래스가 필수입니다.

## Spring 빈
Spring 컨테이너는 생성될 때, 구성 정보로 등록한 Java 설정 클래스(`@Configuration`이 붙은 클래스)에 작성되어 있는 객체들을 모두 생성해서 자기 자신에 등록합니다. 이때, 컨테이너에 등록된 객체들을 **빈(Bean)**이라고 합니다. 물론 설정 클래스에 있는 모든 요소들이 빈으로 등록되는 것은 아닙니다. `@Bean` 어노테이션을 붙힌 메소드가 반환하는 객체들이 등록되는 것입니다. 빈은 정확히는 컨테이너 내부의 빈 저장소에 등록되는데, 빈을 불러올 수 있는 키(key) 역할을 하는 **'빈 이름'**과 실제 객체인 **'빈 객체'**가 같이 등록됩니다. 빈 이름은 따로 지정해주지 않으면 `@Bean`을 붙혀 Spring 빈으로 지정한 메소드 이름으로 자동 저장됩니다. Spring 빈으로 등록하는 법은 다음과 같습니다.

```java
//컨테이너 생성에 활용할 구성 정보를 가진 설정 클래스입니다.
@Configuration
public class AppConfig {
    //Spring 빈으로 등록되었습니다.
    //빈 이름 : memberRepository
    //빈 객체 : MemoryMemberRepository 클래스의 인스턴스
    @Bean
    public MemberRepository memberRepository() {
        //IoC에 의해 해당 객체는 이 곳에서만 생성되고 의존관계가 주입됩니다.
        return new MemoryMemberRepository();
    }

    //빈 이름을 직접 등록할 수도 있습니다.
    @Bean(name="memberServiceBean")
    public MemberService memberService() {
        return new MemberServiceImpl(memberRepository());
    }
}
```

빈 이름을 직접 등록할 때 꼭 주의해야할 점은 이름이 중복되면 안된다는 것입니다. 서로 다른 두 빈이 같은 이름을 가지게 될 경우, 컨테이너에서 다른 하나의 빈은 무시되거나 아예 덮어씌워질 수도 있고 설정에 따라 오류가 발생할 수도 있습니다.

컨테이너의 역할인 생성과 관리(빈을 생성하고 컨테이너에 등록)까지 알아봤습니다. 다음은 가장 중요하다고 할 수 있는 컨테이너의 DI, 의존관계 주입 방식에 대해 알아보겠습니다. Spring은 빈을 생성하고 의존관계를 주입하는 단계가 나뉘어져 있습니다. 위의 코드로 설명하면, `@Bean` 어노테이션을 확인한 Spring은 그 아래에 있는 메소드들의 이름을 빈 이름으로, 그 메소드가 반환하는 객체들을 빈 객체로 Spring 컨테이너에 등록합니다. 이후, 메소드 안에 작성되어 있는 의존관계 정보(MemberServiceImpl 클래스는 memberRepository 빈에 의존합니다.)를 확인하고 그 의존관계를 주입해줍니다. 그런데, 위 코드와 같이 Java 설정 클래스를 통해 빈을 등록하면 생성자를 호출함과 동시에 의존관계도 주입됩니다. 즉, 단계가 나누어지지 않는다는 말입니다. 자세한 내용은 의존관계 자동 주입 파트에서 설명하겠습니다. 지금 가지고 가야할 정보는 설정 클래스를 활용해서 만들어진 Spring 빈은 생성될 때 의존관계가 자동으로 주입된다는 것입니다.

## 컨테이너의 빈 조회하기
이제 Spring 컨테이너를 생성하고 Spring 빈을 등록하는 방법까지 알아봤습니다. 이제 빈이 잘 등록되어 있는지 확인하는(혹은 빈을 활용하고자 객체를 불러오는) 방법에 대해 알아보겠습니다.

### 모두 조회하기
먼저 Spring에 등록된 모든 빈 정보를 확인할 수 있는 방법입니다.

```java
//설정 클래스 AppConfig를 구성 정보로 하는 어노테이션 기반 Spring 컨테이너 생성
AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

void findAllBean() {
    //빈 이름들을 모두 받아온 후
    String[] beanDefinitionNames = ac.getBeanDefinitionNames();
    //해당 빈 이름을 가진 빈 객체를 받아와서
    for(String beanDefinitionName : beanDefinitionNames) {
        Object bean = ac.getBean(beanDefinitionName);
        //출력
        System.out.println("name = " + beanDefinitionName + " object = " + bean);
    }
}
```

현재 Spring에서 관리하고 있는 모든 Spring 빈을 출력하는 방법입니다. `ac.getBeanDefinitionNames()`을 통해 Spring에 등록된 모든 빈 이름을 조회하고, `ac.getBean()`으로 조회한 빈 이름을 가진 빈 객체(인스턴스)를 조회합니다. 여기서 `ac`는 Spring 컨테이너 객체입니다. 당연히 바뀔 수 있습니다.

그러나 조금 아쉬운 점이 있습니다. 위의 방식을 사용하면 내가 등록한 Spring 빈 뿐만 아니라 Spring 자체적으로 등록한 Spring 빈들도 모두 조회됩니다. 내가 직접 등록한 빈들만 확인하려면 어떻게 해야할까요? 방법은 다음과 같습니다.

```java
//설정 클래스 AppConfig를 구성 정보로 하는 어노테이션 기반 Spring 컨테이너 생성
AnnotationConfigApplicationContext ac = new AnnotationConfigApplicationContext(AppConfig.class);

void findApplicationBean() {
    //빈 이름들을 모두 받아온 후
    String[] beanDefinitionNames = ac.getBeanDefinitionNames();
    //해당 빈 이름을 가진 빈 객체를 받아와서
    for(String beanDefinitionName : beanDefinitionNames) {
        BeanDefinition beanDefinition = ac.getBean(beanDefinitionName);
        //해당 빈 이름을 가진 빈 객체의 속성을 확인한 후
        //직접 등록한 애플리케이션 빈일 경우
        if (beanDefinition.getRole() == BeanDefinition.ROLE_APPLICATION) {
            Object bean = ac.getBean(beanDefinitionName);
            //출력
            System.out.println("name = " + beanDefinitionName + " object = " + bean);
        }
        //Role ROLE_APPLICATION: 직접 등록한 애플리케이션 빈
        //Role ROLE_INFRASTRUCTURE: 스프링이 내부에서 사용하는 빈
    }
}
```

내가 직접 등록한 빈을 **'애플리케이션 빈'**이라고 합니다. Spring 내부의 빈과 애플리케이션 빈은 `getRole()` 메소드의 결과값으로 구분할 수 있습니다. `beanDefinition.getRole()`의 결과값은 `beanDefinition`에 저장해두었던 빈 이름을 가진 빈의 역할입니다. 만약 프로그래머가 직접 등록한 빈이라면 `ROLE_APPLICATION`이라는 결과값이 나옵니다. 이를 통해 내가 등록한 빈만 확인할 수 있습니다.

### 기본 방식

### 동일한 타입 존재 시
### 상속 관계

## BeanFactory와 ApplicationContext
