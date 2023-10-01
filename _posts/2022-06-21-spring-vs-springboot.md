---
layout: post
title: Spring vs Spring Boot
subtitle: 
categories: Spring
tags: [Spring, Spring Boo]
---
## Spring Framework
Java 플랫폼을 위한 Application Framework
애플리케이션을 개발하기 위한 모든 기능을 종합적으로 제공하는 애플리케이션 프레임워크

## Spring Framework 특징

### 1) 경량 컨테이너 & IOC (Inversion of Control)
개발자를 대신하여 경량 컨테이너로서 자바 객체를 직접 관리한다.객체 생성, 소멸과 같은 라이프 사이클을 관리하며 필요한 객체를 얻어올 수 있다.
예를들어, 아래와 같은 독립적인 코드를 작성해서 Annotation만 넘겨주면 Container가 개발자가 원하는 상황에 코드를 실행한다.
``` java
@GetMapping("/greeting")
public Greeting greeting(@RequestParam(value = "name", defaultValue="yesol") String name) {
  return new Greeting(counter.incrementAndGet(), name);
}
```
위와 같은 코드만 작성하면 개발자는 해당 메서드가 언제, 어디서 호출되어야 하는지 메서드를 호출하기 위해 필요한 매개변수를 준비해서 전달하지 않아도 된다. Container가 개발자 대신 알아서 호출한다.  
이렇게 Container가 개발자를 대신하여 메서드가 호출될 때와 메서드가 필요한 자원을 전달하는 설계 구조를 Inversion of Control (IOC)라 한다.  
IOC는 메서드가 필요로 하는 자원(ex. name)을 코드가 실행되는 시점에 전달하는데, 이를 Dependency Injection (DI)라 한다.

### 2) AOP 지원
Aspect Oriented Programming 의 약자로, 대부분의 시스템에서 보안, 로그, 트랜잭션과 같은 비즈니스 로직은 아니지만 반드시 처리가 필요한 부분을 횡단 관심사라 한다.   
스프링에서는 이러한 횡단관심사를 비즈니스 로직과 분리하여 중복된 코드를 줄이고 개발자가 비즈니스 로직에 집중하도록 도와준다.

### 3) WAS에 독립적인 개발환경
단순한 서버 환경인 톰캣(Tomcat)이나 제티(Jetty)에서도 동작한다.

## Spring boot vs Spring
스프링 프레임워크는 기능이 많은만큼 환경설정이 복잡한 편으로, 이에 많은 부분을 자동화하여 개발자가 편하게 스프링을 활용할 수 있도록 나온것이 Spring Boot 이다.

## Spring Boot
- 간편한 설정
- 편리한 의존성 관리 & 자동 권장 버전 관리
- 내장 서버로 인한 간단한 배포 서버 구축
- 스프링 Security, Data JPA 등의 다른 스프링 프레임워크 요소를 쉽게 사용 가능
