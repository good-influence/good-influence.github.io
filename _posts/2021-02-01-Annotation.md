---
layout: post
title: Annotaion
subtitle: 
categories: JAVA
tags: [JAVA, annotation]
---
# 애노테이션

- Java Doc
    
    자바를 개발한 사람들은 소스코드와 문서를 하나의 파일로 관리하기위해, 소스코드의 주석으로 코드에 대한 정보를 저장하고, 그 주석으로부터 HTML 문서를 생성해 내는 프로그램 (javadoc.exe)를 만들어 사용했다.
    Javadoc을 사용하여 문서화할 클래스, 필드, 메서드, 어노테이터, 인터페이스 등에 주석 및 어노테이션으로 문서를 작성하고 HTML등 다양한 포맷으로 Export 할 수 있다.
    
    장점
    - 표준에 맞춘 주석 작성으로, 구독자 간의 혼동 최소화
    - 코드에 문서를 포함시킬 수 있어 개발과 동시에 쉬운 문서화 가능
    - 다양한 지원 도구로 원하는 디자인/타입 문서를 쉽게 만들 수 있음
    

## 어노테이션이란?

어노테이션(Annotation)은 Java5부터 등장한 기능으로 사전적 정의는 주석 이지만 실제 우리가 사용하는 주석과는 차이가 있다. `@`를 이용한 주석으로, 자바코드에 주석을 달아 특별한 의미를 부여한 것이다.

어노테이션으로 인해 **데이터의 유효성 검사**를 쉽게 할 수 있고, 이와 관련한 코드가 간결해진다. 또한, 어노테이션의 본질적인 목적은 소스코드에 **메타데이터를 표현하는 것**이다.

- 유효성 검사: 데이터가 요구하는 형식에 맞는지 검사하는 것
    - ex.  이메일 형식인지 검사하는 어노테이션
    
    ```java
    public class Demo {
    
        @Email(message = "이메일 형식이 아닙니다.")
        private String email;
    }
    ```
    
- 메타데이터 : 데이터를 위한 데이터 즉, 데이터에 대한 설명을 포함한, 자신의 정보를 담고 있는 데이터
    - ex. `@Override` : 상속받은 메소드를 동일한 이름으로 새로 작성하는 것
    
    ```java
    public class Parent {
        public void method() {
            System.out.println("Parent 클래스의 method");
        }
    }
    
    public class Child extends Parent{
        // 에러 발생
        @Override                
        public void method(String test) {
            super.method();
        }
    }
    ```
    
    ![](/assets/images/java/annotation-result1.png)
    
    **어노테이션은 컴파일러에서 읽을 수 있는 주석**이기때문에 오버라이딩에 맞지 않을 때 오류가 나는것을 알려준다.
    
>    💡 - 컴파일러에게 코드 문법 에러를 체크하도록 정보 제공
>    - 런타임에 특정 기능을 실행하도록 정보 제공
    

## 어노테이션 정의

어노테이션 타입을 정의하는 방법은 인터페이스를 정의하는 것과 유사하다.

```java
public @interface AnnotationName {
    // ...
}
```

사용시에는 `@AnnotationName` 으로 사용한다. 

### 멤버를 갖는 어노테이션

```java
public @interface MyAnnotation {
    String myElement1();
    int myElement2() default 5;
    public enum WEEK {MONTHDAY, FRIDAY, SUNDAY} // enum 타입 사용 가능
    String value();
    int[] values();
    WEEK week() default WEEK.FRIDAY;            // enum 사용 방법
}
```

사용할 수 있는 엘리먼트 타입으로는 int, double 과같은 primitive type, String, Class 타입 그리고, 이들의 배열 타입을 사용할 수있다. 

위 코드에서 정의한 어노테이션을 코드에 적용할 때는 이렇게 사용한다. 

- `@Annotation(myElement1="TEST", myElement2=10);`
- `@MyAnnotation(myElement1 = "TEST");`
    - myElement2 는 default 값(5) 사용

default 값이 없으면 반드시 값을 기술해야하지만 있다면 생략할 수 있다.

```java
public @interface myAnno {
    String value();
}

public class Test {
    @myAnno("이름생략가능")
    public void doing() {
        // ...
    }
}
```

하나의 멤버만있고 멤버의 이름이 value 인경우 사용시 멤버이름을 지정하지 않아도 된다.

### java.lang.annotation

- @Inherited
    - 해당 어노테이션이 자손 클래스에도 상속되도록 한다.

- @Documented
    - javadoc으로 api문서를 작성시 해당 어노테이션에 대한 설명도 포함하도록 지정해주는것

- @Retention (RetentionPolicy.RUNTIME / SOURCE / CLASS)
    - 어느시점까지 어노테이션의 메모리를 가져갈지 설정한다.(어노테이션의 **라이프사이클 정의**)  
        - RUNTIME : 컴파일 이후에도 JVM에 의해 참조 가능
        - SOURCE : 컴파일 이후 없어짐
        - CLASS : 컴파일러가 클래스를 참조할 때가지 유효

- @Target : 사용될 위치 지정
    - ElementType.METHOD
    - ElementType.PACKAGE
    - ElementType.TYPE
        - class, interface, annotation, enum 에 적용 가능
    - ElementType.ANNOTATION_TYPE
    - ElementType.FIELD
        - 필드, enum 상수
    - ElementType.CONSTRUCTOR
    - ElementType.LOCAL_VARIABLE
    - ElementType.MODULE
    - 자바 8부터 추가
        - ElementType.TYPE_PARAMETER
            - `static class Test<@TypeAnno T> { }`
        - ElementType.TYPE_USE
            
            타입 파라미터를 포함해서 타입을 포함하는 모든곳에 사용 가능
            
            ```java
            @TypeAnno
            public class App {
            
                public static void main(@TypeAnno String[] args) throws @TypeAnno RuntimeException {
                    List<@TypeAnno String> names = Arrays.asList("test");
                }
                static class Test<@TypeAnno T> {
                    // 타입 파라미터, 타입
                    public static <@TypeAnno C> void print(@TypeAnnoC c) {
                
                    }
                }
            }
            ```
            
- @Repeatable
    - 이전에는 어노테이션의 중복이 불가능했지만 이 어노테이션을 이용하면 가능하다.
        - @Repeatable() 안에 여러 어노테이션(중복애노테이션)을 감싸고 있을 컨테이너 어노테이션 타입을 선언
        - 이때, **컨테이너어노테이션이 가지고 있는 @Retention과 @target 정보는 중복 어노테이션보다 같거나 넓어야 한다.**
        - 쓰는방법
            1. 중복애노테이션타입으로 가져오는 방법
                
                ```java
                Words[] words = App.class.getAnnotationsByType(Words.class);	
                Arrays.stream(words).forEach(w -> {
                    System.out.println(w.value());
                });
                ```
                
            2. 컨테이너 타입으로 가져오는 방법 
                
                ```java
                MyRepeatedAnnos container = App.class.getAnnotation(MyRepeatedAnnos.class);
                Arrays.stream(container.value()).forEach(w -> {
                    System.out.println(w.value());
                });
                ```
                
    - 사용 예제
    
    ```java
    @Retention(RetentionPolicy.RUNTIME)
    @Repeatable(MyRepeatedAnnos.class)
    @interface Words {
        String word() default "Hello";
        int value() default 0;
    }
    
    @Retention(RetentionPolicy.RUNTIME)
    @interface MyRepeatedAnnos {            // 컨테이너 어노테이션 
        Words[] value();
    }
    
    public class App {
    
        @Words(word = "First", value = 1)
        @Words(word = "Second", value = 2)
        public static void newMethod() {
            App obj = new App();
            try {
                Class<?> c = obj.getClass();
                // Obtain the annotation for newMethod
                Method m = c.getMethod("newMethod");
                // Display the repeated annotation
                Annotation anno = m.getAnnotation(MyRepeatedAnnos.class);
                System.out.println(anno);
            } catch (NoSuchMethodException e) {
                System.out.println(e);
            }
        }

        public static void main(String[] args) {
                newMethod();
        }
    }
    ```
    ![](/assets/images/java/annotation-result2.png)

### java.lang

- @Deprecated
    - 해당 메소드를 사용하지 않도록 유도한다. (컴파일 경고)
- @Override
    - 메소드가 오버라이드 되었는지 검증한다. 만약, 부모 클래스 또는 구현해야할 인터페이스에서 해당 메소드를 찾을 수 없다면 컴파일 에러 발생
- @SuppressWarnings
    - 컴파일 경고를 무시하도록 하는 기능

## 애노테이션 프로세서

Annotation Processing은 javac에 속한 빌드툴로, **컴파일타임에** 어노테이션을 분석하여 붙여진 어노테이션에 따라 **클래스를 만들어 낸다**.

어떤 순서로 동작하는가?

1. 어노테이션 클래스를 생성한다.
2. 어노테이션 Parser 클래스를 생성한다.
3. 어노테이션을 사용한다.
4. 컴파일하면 어노테이션 Parser가 어노테이션을 처리한다.
5. 자동생성된 클래스가 빌드 폴더에 추가된다.

그리고, **Annotation Parser classes 는 프로젝트를 컴파일 할 때만 필요하고, 빌드가 끝나면 쓰이지 않는다.**

## ServiceLoader

인터페이스의 구현체를 모르는데 jar 파일만 바꿔끼어 동작할 수 있게,

즉, 해당 인터페이스 타입을 구현한 구현체의 인스턴스를 가져 올 수 있게 해주는 것 

---

## Reference

[https://docs.oracle.com/javase/tutorial/java/annotations/index.html](https://docs.oracle.com/javase/tutorial/java/annotations/index.html)

[https://www.nextree.co.kr/p5864/](https://www.nextree.co.kr/p5864/)

[https://codediver.tistory.com/71](https://codediver.tistory.com/71)

[https://www.geeksforgeeks.org/annotations-in-java/](https://www.geeksforgeeks.org/annotations-in-java/)