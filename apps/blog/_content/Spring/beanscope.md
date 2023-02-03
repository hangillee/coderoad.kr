---
title: '빈 스코프'
subtitle: 'Spring 빈의 스코프'
date: 2023-01-24 00:00:00
category: 'Spring'
---

**본 문서는 인프런에서 수강할 수 있는 [스프링 핵심 원리 - 기본편](https://inflearn.com/course/스프링-핵심-원리-기본편)을 수강한 후, 공부한 내용을 정리한 문서입니다. 본 문서의 모든 저작권은 해당 강의의 저자이신 [김영한](https://inflearn.com/users/@yh) 우아한형제들 기술이사님께 있습니다.**

문서 공사중입니다.

## Spring 빈 스코프?

Spring 빈은 Spring 컨테이너가 시작될 때 함께 생성되고, 컨테이너가 종료될 때 같이 소멸됩니다. 이렇게 **빈이 존재할 수 있는 범위**를 **스코프**라고 합니다. Spring은 다양한 스코프를 지원합니다.

- **싱글톤** : 기본 스코프로 컨테이너의 생성과 소멸까지 유지되는 가장 넓은 스코프
- **프로토타입** : 컨테이너가 빈의 생성과 의존관계 주입까지만 관여하는 매우 짧은 스코프
- **웹 관련 스코프**
  - **request** : 웹 요청이 들어오고 나갈 때까지 유지되는 스코프
  - **session** : 웹 세션이 생성되고 소멸될 때까지 유지되는 스코프
  - **application** : 웹의 Servlet 컨텍스트와 같은 범위로 유지되는 스코프

기본적으로 Spring 빈은 싱글톤 패턴으로 생성됩니다. 프로토타입이나 웹 관련 스코프를 적용하려면 `@Scope` 어노테이션을 활용해서 적용하면 됩니다.

```java
//이 빈은 프로토타입 스코프입니다.
//빈 생성과 의존관계 주입 후에는 컨테이너가 관리하지 않습니다.
@Scope("prototype")
@Bean
PrototypeBean ExampleBean() {
    private int count = 0;
    public void addCount() {
        count++;
    }
    public int getCount() {
        return count;
    }
}
```

## 프로토타입 스코프

싱글톤 스코프 빈, 즉, 일반적인 빈은 컨테이너에서 조회하면 항상 같은 인스턴스의 빈을 반환해줍니다. Spring 컨테이너가 생성된 빈을 컨테이너 소멸 시까지 관리하기 때문입니다. 반대로, 프로토타입 스코프 빈은 컨테이너가 항상 새로운 인스턴스를 생성해서 반환해주는데, **컨테이너가 빈을 생성하고 의존관계를 주입해주면 더 이상 관리하지 않아** 반환해줄 빈이 없기 때문입니다. 따라서, **클라이언트가 빈을 요청하면 항상 새로 생성해서 반환**합니다.

프로토타입 빈은 초기화 콜백 메소드인 `@PostConstruct`는 실행되지만, `@PreDestroy`와 같은 소멸 콜백 메소드는 실행되지 않습니다. 때문에, 프로토타입 빈은 빈을 조회한 클라이언트가 직접 관리해줘야합니다. 당연히 소멸 콜백 메소드에 대한 호출도 클라이언트가 직접 해야합니다.

### 프로토타입 스코프의 문제점

프로토타입 스코프 빈에는 큰 문제가 하나 존재합니다. **싱글톤 빈과 함께 사용할 때**, 프로토타입 스코프 빈이 **의도한대로 동작하지 않을 가능성이 있습니다**. 바로, 프로토타입 스코프 빈을 싱글톤 스코프 객체 안의 필드에 저장하게 되면 프로토타입의 특징을 잃어버리는 문제입니다.

우리가 프로토타입 빈을 사용할 때는 빈을 생성할 때마다 매번 새로운 인스턴스가 들어오는 것을 기대합니다. 그러나 막상 싱글톤 빈의 필드에 프로토타입 빈을 주입 받아 사용해보면 **기존에 저장된 인스턴스가 호출**됩니다. 이렇게 의도치 않게 프로토타입 빈이 싱글톤 빈과 함께 유지되는 이유는 프로토 타입 빈이 엄밀히 말하면 **의존관계가 주입 될 때** 새로 생성되는 것이지 **이미 주입된 빈을 사용할 때는 새로 생성되지 않기 때문** 입니다.

```java
@Scope("prototype")
public class PrototypeBean() {
    ...
}

//싱글톤 빈의 필드에 프로토타입 빈을 대입하는 예시입니다.
public class SingletonBean() {
    private final PrototypeBean prototypeBean;

    @Autowired
    public SingletonBean(PrototypeBean prototypeBean) {
        //의존관계를 주입할 때만 새로운 PrototypeBean 인스턴스가 생성되고,
        //이 필드를 활용할 때는 새로 생성하는 것이 아니라 저장된 인스턴스가 호출됩니다.
        this.prototypeBean = prototypeBean;
    }
}
```

### 스코프와 Provider

이렇게 싱글톤 빈과 프로토타입 빈을 같이 쓸 때 발생하는 문제를 해결하기 위해선 **싱글톤 빈이 프로토타입 빈을 사용할 때 마다 Spring 컨테이너에 새로운 인스턴스를 요청**해야합니다. 이를 **의존관계 탐색(Dependency Lookup)**이라고 하는데, 의존관계 주입과 다르게 스스로 필요한 의존관계를 위한 인스턴스를 컨테이너에서 찾아 반환받는 것입니다. DL를 활용하는 가장 간단하고 익숙한 방법은 Spring 컨테이너, 즉,`AnnotationConfigApplicationContext`의 `getBean()` 메소드를 통해 항상 새로운 프로토타입 빈을 생성해 반환받는 방법입니다.

```java
@Autowired
private ApplicationContext ac;

public int mainLogic() {
    //로직을 실행할 때마다 컨테이너로부터 항상 새로운 프로토타입 빈을 반환받습니다.
    PrototypeBean prototypeBean = ac.getBean(PrototypeBean.class);
    prototypeBean.addCount();
    int count = prototypeBean.getCount();
    return count;
}
```

우리가 지금까지 자주 다뤘던 메소드를 통해 해결하는 방법이지만 이 방법에도 문제가 있습니다. 바로 TDD(테스트 주도 개발)을 위한 단위 테스트 작성이 어렵다는 점입니다. 이 방법은 `ApplicationContext` 객체가 없이는 사용할 수 없기 때문에 테스트를 작성할 때마다 `ApplicationContext`를 주입받아야 합니다. `ApplicationContext`는 편리하기도 하지만 그만큼 많은 기능을 제공하고 이 말은 **불필요한 기능도 많다**는 것입니다. 또한, `ApplicationContext`가 꼭 필요하기 때문에 Spring에 종속적인 코드가 됩니다. 우리가 Spring을 공부하면서 계속 피해왔던 것이 특정 기술에 종속적인 코드를 작성하는 것이었던 걸 생각하면 이 방법보다 더 나은 방법을 찾아야 합니다.

다행히도 Spring에는 불필요한 기능들을 제외하고 **DL** 기능만 제공하는 **`ObjectProvider`**라는 객체가 있습니다. `ObjectProvider`는 `ObjectFactory`에 편의 기능을 추가한 객체로 DL 외에도 `ObjectFactory` 상속, 옵션, 스트림 처리 기능을 제공합니다. 별도의 라이브러리 없이 간단하게 사용할 수 있고 테스트 작성도 수월합니다. 그러나 여전히 Spring에 의존적이라는 문제는 해결하지 못합니다.

```java
@Autowired
private ObjectProvider<PrototypeBean> prototypeBeanProvider;

public int mainLogic() {
    //ObjectProvider를 통해 매번 새로운 프로토타입 빈을 반환받습니다.
    PrototypeBean prototypeBean = prototypeBeanProvider.getObject();
    prototypeBean.addCount();
    int count = prototypeBean.getCount();
    return count;
}
```

마지막 문제인 Spring에 의존적인 코드를 벗어나기 위해선 `javax.inject.Provider`라는 **JSR-330** Java 표준 Provider를 사용하는 방법입니다. 참고로 **Spring Boot 3.0**에서는 `jakarta.inject.Provider` ㅖ라이브러리를 사용합니다. 사용법은 다음과 같습니다.

```java
@Autowired
private Provider<PrototypeBean> provider;

public int mainLogic() {
    //jakarta.inject.Provider 라이브러리의 Provider를 사용합니다.
    PrototypeBean prototypeBean = provider.get();
    prototypeBean.addCount();
    int count = prototypeBean.getCount();
    return count;
}
```

`Provider`는 `get()` 메소드로 Spring의 `ObjectProvider`와 동일하게 DL을 통해 Spring 컨테이너로부터 새로운 인스턴스를 반환받습니다. 이 방법은 Java 표준이고 단순하기 때문에 Spring에 의존적이지 않고 단위 테스트를 작성하기 좋습니다.

지금까지 열심히 프로토타입 빈에 대해 알아봤습니다. 프로토타입 빈은 **'사용할 때마다 의존관계 주입이 완료된 새로운 객체가 필요할 때'** 사용하면 됩니다. 사실, 실무에서 웹 애플리케이션을 개발할 때, 싱글톤 빈으로 대부분의 문제를 해결할 수 있기 때문에 프로토타입 빈을 직접 사용하는 일은 매우 드물다고 합니다.

## 웹 스코프

웹 스코프는 웹 환경에서만 동작한다. 웹 스코프는 프로토타입과 다르게 스프링이 해당 스코프의 종료 시점까지 관리한다. (생성은 웹 요청 시에 생성된다.) 문제는 웹 요청 시에 빈이 생성되기 때문에 컨테이너가 올라가고 로직이 실행될 때 없는 빈을 참조하기 때문에 예외가 발생한다.

## 스코프와 프록시

스코프 애노티에션에 proxyMode 어트리뷰트를 활용해 프록시 방식을 적용할 수 있다. Provider 사용 시와는 다르게 싱글톤 스코프와 같은 코드로도 빈의 생성이 정상적으로 이루어진다. 이것이 가능한 이유는 CGLIB 라이브러리를 활용해 내가 작성한 클래스(빈)을 상속한 가짜 "프록시" 객체를 만들어서 대신 주입하고 HTTP Request가 왔을 때 가짜 "프록시" 객체가 내부 로직(위임)을 통해 진짜 객체(빈)의 로직을 실행한다. 이는 다형성의 장점이다.(클라이언트는 객체가 가짜 프록시 객체인지 진짜 스프링 빈인지 알 필요가 없다.) 꼭 웹 스코프가 아니더라도 프록시는 사용 가능하다.
