---
layout: post
title: Interface
subtitle: 인터페이스
categories: JAVA
tags: [JAVA, Interface]
---

## 인터페이스
추상클래스를 부분적으로 완성된 '미완성 설계도'라 한다면, 인터페이스는 구현된 것은 아무 것도 없고 밑그림만 그려져 있는 '**기본 설계도**' 라 할 수 있다.

완성되지 않은 불완전한 것이기 때문에 다른 클래스를 작성하는데 도움을 줄 목적으로 작성된다.

- 인터페이스 장점
    - **개발 시간 단축**
        
        밑그림만 그려져있는 기본 설계도가 미리 작성되어 있으면 구현은 뒤로 미뤄두고 일단 프로그램을 작성하는 것이 가능하기 때문에 개발 시간이 단축된다.
        
    - **표준화 가능**
        
        기본 틀을 인터페이스로 작성한 다음, 개발자들에게 작성한 인터페이스를 구현하여 프로그램을 작성하도록 하면 보다 일관되고 정형화된 프로그램의 개발이 가능하다.
        
    - **서로 관계 없는 클래스들 간의 관계를 맺어준다.**
        
        하나의 인터페이스를 공통적으로 구현하도록 하여 관계를 맺어 줄 수 있다.
        
    - **독립적인 프로그래밍 가능**
        
        인터페이스를 이용하면 클래스의 선언과 구현을 분리시킬 수 있기 때문에 실제 구현에 독립적인 프로그램을 작성하는 것이 가능하다.
        
- 추상클래스 vs 인터페이스
    - 공통점
        - 자기 자신을 객체화 할 수 없으며, 다른 객체가 상속(`extends`) 또는 구현(`implements`) 하여 객체를 생성 할 수 있다.
        - 상속, 구현을 한 하위 클래스에서는 상위에서 정의한 추상 메서드를 반드시 구현하여야 한다.
    
    - 차이점

        | 추상클래스 | 인터페이스 |
        | :-------- | :-------- |
        | 일반 메소드 포함 가능 | 모든 메서드는 추상 메서드, <br> 자바 8 : `default` `static` 메서드 추가  <br> 자바 9 : `private` 메서드 추가 |  
        | 다중상속 불가능 | 다중상속 가능 |  
        | 상속, 변수 필드 포함 가능 | 상수 필드만 포함 가능 |

    
    - **추상 클래스는 IS-A** ~는 ~이다 의 관계
        - Dog 는 동물이다.
    - **인터페이스는 HAS -A** ~는 ~를 할 수 있다.
        - 어떤 Dog는 수영을 할 수 있다.
    

추상클래스보다 더욱 강력한 **추상화를 제공하는 도구**이며, 이를 통해 **다형성을 더욱 강력**하게 해 주는 도구이다.

대표적인 예로 `List` 를 들 수 있다.

![](/assets/images/java/interface-list.png)

`ArrayList` 는 `List` 인터페이스를 구현한 클래스이다.

![](/assets/images/java/interface-arraylist.png)


## 인터페이스 정의 방법

클래스를 작성하는 것과 같지만, 키워드로 `class` 대신 `interface` 를 사용하여 작성한다. 클래스와 마찬가지로 접근제어자로 `public, default` 를 사용할 수 있다.

```java
interface 인터페이스_이름 {
    public static final String test = "";
    public abstract method1();
}

interface 인터페이스이름 extends [부모인터페이스이름 ...] {
    // 다중 상속 가능
}
```

- 상수는 `static final` 이 붙어야 하지만 생략 가능
- `public` 추상 메소드를 위한 `public abstract` 또한 생략 가능
    - `private` 나 `protected` 같은 접근제어자는 사용할 수 없으나 자바 9 부터 `private` 메소드 생성 가능
    - 구현할 때는 `public` 을 명시해주어야 한다.

인터페이스 멤버들은 제약사항들을 가지고 있다.

>💡 - 모든 멤버변수는 **`public static final`** 이어야 하며, 이를 **생략** 할 수 있다.
> - 모든 메서드는 **`public abstract`** 이어야 하며, 이를 **생략 할 수 없다**.  
>    - 모든 메소드들은 추상 메소드로 간주된다.

## 인터페이스 구현 방법

추상클래스와 마찬가지로 그 자체로는 인스턴스를 생성할 수 없으며, 추상클래스는 클래스를 확장한다는 의미의 `extends` 를 사용하지만, 인터페이스는 **구현한다는 의미**의 키워드 `implements`를 사용한다.

```java
class 클래스이름 implements 인터페이스이름1, 인터페이스이름2 {
    // 인터페이스에 정의된 추상메서드를 구현해야 한다.
    public void method1();
    public void method2(int x, int y);
}
```

여러 개의 인터페이스도 구현할 수 있다. 인터페이스에서 추상 메소드 시점에 내용이 결정되는 것이 아니라, 구현시점에 어떤 내용이 들어갈지 결정되므로 이름이 같아도 다이아몬드 문제(다중상속문제)가 발생하지 않는다. 

만약, 구현하는 인터페이스의 메서드 중 **일부만 구현한다면 추상클래스로 선언**되어야 한다.

```java
abstract class 클래스이름 implements 인터페이스이름 {
    public void method1() {
        // 구현
    }
}
```

또한, **상속과 구현을 동시에** 할 수도 있다.

```java
class 클래스이름 extends 상위클래스 implements 인터페이스이름 {
    public void method1() {
        // 구현
    }

    public void method2(int x, int y) {
        // 구현
    }
}
```

### 익명 구현 객체

인터페이스는 원칙적으로 객체화 될 수 없지만, 인터페이스를 구현하는 일회용 객체를 만들어 사용할 수 있다. 

```java
Flyable fly = new Flyable() {
    @Override
    public void fly() {
        System.out.println("Anomynous's flying");
    }
};
```

익명 객체를 만들어 변수 `fly` 에 주입시켜 준다. 

java의 인터페이스에는 생성자가 없으므로 항상 flyable()의 괄호 안에는 빈 상태이다.

단순히 인터페이스의 구현 역할을 하는 익명객체일 뿐, 상속이나 또 다른 인터페이스를 구현하는 것은 불가능하다.

익명 구현 객체를 만드는 방법은 expression의 일종이므로 뒤에 세미 콜론을 붙이는 것을 잊지 않아야 한다. 

```java
List<Runnable> actions = new ArrayList<Runnable>();
actions.add(new Runnable() {
    @Override
    public void run() {
        // todo
    }
});
```

## 인터페이스 레퍼런스를 통해 구현체를 사용하는 방법

```java
public interface Drinkable {
    public void drink();
}

public interface Chewable {
    public void chew();
}
```

```java
public class Eat implements Drinkable, Chewable {

    @Override
    public void chew() {
        System.out.println("eating " + this.vitamin);
    }

    @Override
    public void drink() {
        System.out.println("drinking " + this.water);
    }
}
```

```java
Drinkable d = new Eat();
d.drink();
d.chew(); // 컴파일 에러 cannot resolve method 'chew' in 'drinkable'
```

`Drinkable` 을 통해서 d를 선언하게 되면 `Drinkable` 가 가질 수 있는 메소드만 사용할 수 있다. 

이는 자바의 다형성의 대표적인 예시로 활용된다. 

```java
List<Drinkable> list = ArrayList<Drinkable>();
list.add(new Water());
list.add(new Juice());

for (Drinkable element : list) {
    element.drink();    // drink 메소드로 통일하여 호출 할 수 있다.
}
```

## 인터페이스의 상속

인터페이스는 **인터페이스로부터만 상속받을 수 있으며**, 클래스와는 달리 **다중상속**, 즉 여러개의 인터페이스로부터 상속을 받는 것이 가능하다. (여러 개의 계층 구조를 가질 수 있음)

또한 클래스와 달리 Object클래스와 같은 최상위조상은 없다.

```jsx
interface 인터페이스 extends 인터페이스2, 인터페이스3 { }
```

클래스상속과 마찬가지로 하위 인터페이스는 상위인터페이스 (인터페이스2,3) 에 정의된 멤버를 모두 상속받는다.

### 다중상속

- 다이아몬드문제
    
    ![](/assets/images/java/interface-diamond.png)

    
    자바에서 다중상속을 지원하지 않는 이유는 충돌이 생길 수 있기 때문이다. 위 와 같은 상속 구조가 있다고 가정할 때, 아빠 엄마가 할아버지한테 있는 print 메소드를 상속받아 각각 오버라이딩 하여 구현하였다면 최하위 자식인 딸은 어떤 print 메소드를 사용할지 충돌이 생긴다.  
    

*자바에서는 다중상속을 허용하지 않지만 인터페이스를 통해 다중상속을 할 수 있는데 인터페이스의 등장 목적이 다중상속을 위해 생겨난 것은 아니다.* 

인터페이스는 상수만 정의할 수 있으므로 조상클래스의 멤버변수와 충돌하는 경우는 극히 드물고, 추상메서드는 구현내용이 전혀 없으므로 조상클래스의 메서드와 선언부가 일치하는 경우에는 조상클래스의 메서드를 상속받으면 되므로 문제되지 않는데 이렇게 되면 다중상속의 장점을 잃게 된다. 

고로, 만일 두 개의 클래스로부터 상속을 받아야 할 상황이라면, **두 조상클래스 중에서 비중이 높은 쪽을 선택하고 다른 한쪽은 클래스 내부에 멤버로 포함시키는 방식으로 처리하거나 어느 한쪽의 필요한 부분을 뽑아서 인터페이스로 만든 다음 구현하도록 한다.**

- 다중상속 예제

```java
interface GrandFather {
    void myMethod();
}

interface Father extends GrandFather {
    @Override
    void myMethod();
}

interface Mather extends GrandFather {
    @Override
    void myMethod();
}

interface Dauther extends Father, Mather{
    @Override
    void myMethod();
}
```

인터페이스는 기능에 대한 선언만 하기 때문에 다이아몬드 상속이 되더라도 충돌한 여지가 없어 문제가 발생하지 않는다. (추상메소드의 경우 추상메소드 선언 시점에 결정되는 것이 아니라 구현시점에 결정되기 때문)

### 자바 8 default 메소드

해당 인터페이스를 구현한 모든 클래스에 공통적으로 추가해주어야 할 메서드가 있는 경우 기존에는 일일이 해당 클래스에 추가해주어야 했지만 **default method를 활용하면 추가할 필요 없이 구현한 클래스에서 해당 메소드 사용이 가능하다.**  (하위 호환성)

```java
public interface Test {
    void printName();

    String getName();

    default void printNameUpperCase() {
        System.out.println(getName().toUpperCase());
    } 
}

public class TestImpl implements Test{
    String name;

    public TestImpl(String name) {
        this.name = name;
    }

    @Override
    public void printName() {
        System.out.println(this.name);
    }
}
```

```java
@Test
void Test() {
    Test test = new TestImpl("test");
    test.printName();
    test.printNameUpperCase();
}
```

```xml
test
TEST
```

Test 인터페이스를 구현한 TestImpl 클래스를 깨트리지 않고 새 기능(대문자 출력)을 추가할 수 있다.

그러나, **구현체가 모르게 추가된 기능이므로 위험도가 있다.**

예를들어, `getName()` 에서 무슨 값이 올지 모르는 상황이기 때문에 위험하다. 만약 어떤 구현체가 null 을 리턴하게 된다면 컴파일 에러는 안나더라도 **런타임 에러가 발생**할 수 있다.

이러한 것을 방지하려면 최소한으로 문서화를 잘 해야 한다. 

- `@implSpec` 자바독 태그 사용하기

```java
/**
*  @implSpec 이 구현체는 getName()으로 가져온 문자열을 대문자로 바꾸어 출력한다. 
*/
```

또한 default 메서드는 **재정의** 하여 사용할 수 있다. 

```java
public interface Test {
    void printName();

    String getName();

    default void printNameUpperCase() {
        System.out.println(getName().toUpperCase());
    } 
}
public class TestImpl implements Test{
    String name;

    public TestImpl(String name) {
        this.name = name;
    }

    @Override
    public void printName() {
        System.out.println(this.name);
    }

    @Override
    public void printNameUpperCase() {
        System.out.println(this.name.toUpperCase());
    }
	
}
```

단, **Object 에서 제공하는 메소드들은 default 메소드로 제공할 수 없다**. 

```java
public interface Test {
    void printName();

    String getName();

    default void printNameUpperCase() {
        System.out.println(getName().toUpperCase());
    }

    // 컴파일 에러 발생 !!!!
    default String toString() {
        // Default method 'toString' overrides a member of 'java.lang.Object'
    } 
}
```

추상메서드로 선언하여 구현체가 재정의하는 것은 가능하다. 그러나 Object안에 있는 메소드들은 모든 object들이 구현하는 메서드이기 때문에 굳이 이 인터페이스가 가지는 특별한 제약사항이 없는 한 사용할 필요가 없다. 

만약, Test 인터페이스에서 갖고있는 default 메소드를 사용하고 싶지 않다면 다음과 같이 할 수 있다.

```jsx
public interface Test2 extends Test{

    void printNameUpperCase();        // 추상메소드로 선언
}
```

위와같이 **인터페이스를 상속받는 인터페이스에서 다시 추상 메소드로 변경할 수 있다.**

단, Test2를 구현하는 구현체들은 전부 다 저 추상메소드를 구현해야 한다.

만약, 두개이상의  인터페이스를 상속받는데 그 인터페이스들에 **같은 default 메소드가 존재한다면 컴파일 에러가 발생하기 때문에 구현체에서 직접 override 해야 한다.** 

```java
public interface JoinMember {

    default void preJoin() {
        System.out.println("pre join group");
    }

    default void afterJoin() {
        System.out.println("after join group");
    }
}

public interface JoinGroup {

    default void preJoin() {
        System.out.println("pre join group");
    }

    default void afterJoin() {
        System.out.println("after join group");
    }
}

```

![](/assets/images/java/interface-compile-error.png)

재정의를 해야 한다. 

```java
public class HelloJoinMember implements JoinMember, JoinGroup{
    @Override
    public void preJoin() {
        JoinMember.super.preJoin();
    }

    @Override
    public void afterJoin() {
        System.out.println("새로 정의하거나");
    }
}
```

새로 정의할 수도 있고 상위 인터페이스에 있는 메소드를 super 키워드로 호출 할 수도 있다.

### 자바 8 static 메소드

default method는 인스턴스가 사용할 수 있는 거라면, **static method는 해당 인터페이스를 구현한 모든 인스턴스 또는 해당 타입에 관련되어 있는 유틸리티 또는 helper 메소드를 제공하고 싶은 경우에 사용**한다.

```java
public interface staticMethod {
    static void print() {
        System.out.println("static method 입니다.");
    }
}

public static void main(String[] args) {
    staticMethod.print();
}
```

```xml
static method 입니다.
```

### 자바 8 API 기본 메소드와 스태틱 메소드

![](/assets/images/java/interface-default-static.jpg)


- Iterable 의 기본 메소드
    
    ```java
    List<String> country = new ArrayList<>();
        country.add("Korea, North");
        country.add("Korea, South");
        country.add("Slovakia");
        country.add("Spain");
    ```
    
    - forEach()
        
        ```java
            country.forEach(c -> {
                System.out.println(c);
            });
    
            // method reference 사용 하면 더 간결해짐
            country.forEach(System.out::println)
        ```
        
    - spliterator()
        
        쪼갤 수 있는 기능을 갖고 있는 iterator
        
        ```java
            Spliterator<String> spliterator = country.spliterator();
            Spliterator<String> spliterator2 = spliterator.trySplit();
            // 다음 string이 없으면 false 리턴
            while (spliterator.tryAdvance(System.out::println));
            System.out.println("===============================");
            while (spliterator2.tryAdvance(System.out::println));
        ```
        
        ```xml
        Slovakia
        Spain
        ===============================
        Korea, North
        Korea, South
        ```
        
        단, 순서는 보장 안된다.
        
- Collection 의 기본 메소드
    - stream()
    
    ```java
    List<String> countryFilter = country.stream()
                .filter(s -> s.startsWith("K"))
                .collect(Collectors.toList());
    System.out.println(countryFilter);
    ```
    
    ```xml
    [Korea, North, Korea, South]
    ```
    
    - removeIf()
    
    ```java
    country.removeIf(s -> s.startsWith("K"));
    country.forEach(System.out::println);
    ```
    
    ```
    Slovakia
    Spain
    ```
    

### 자바 9 private method

java 9 이후부터는 인터페이스에서 `private method`와 `private static method`를 사용할 수 있다.

중복 코드를 피하고 `interface`에 대한 캡슐화를 유지 할 수 있다.

- `private method`

```java
public interface MyInterface {
		
    public default void print() {
        printPrivate();
        System.out.println("default method");
    }
    
    private void printPrivate() {
        System.out.println("private method");
    }
}
```

```xml
private method
default method
```

- `private static method`

```java
public interface TempI { 
  
    public abstract void mul(int a, int b); 
    
    public default void add(int a, int b) { 
        // private method inside default method 
        sub(a, b);  

        // static method inside other non-static method 
        div(a, b); 
        System.out.print("Answer by Default method = "); 
        System.out.println(a + b); 
    } 
  
    public static void mod(int a, int b) { 
        div(a, b); // static method inside other static method 
        System.out.print("Answer by Static method = "); 
        System.out.println(a % b); 
    } 
  
    private void sub(int a, int b) {  
        System.out.print("Answer by Private method = "); 
        System.out.println(a - b); 
    } 
  
    private static void div(int a, int b) { 
        System.out.print("Answer by Private static method = "); 
        System.out.println(a / b); 
    } 
} 
  
class Temp implements TempI { 
  
    @Override
    public void mul(int a, int b) { 
        System.out.print("Answer by Abstract method = "); 
        System.out.println(a * b); 
    } 
  
    public static void main(String[] args) { 
        TempI in = new Temp(); 
        in.mul(2, 3); 
        in.add(6, 2); 
        TempI.mod(5, 3); 
    } 
}
```

```xml
    Answer by Abstract method = 6              // mul(2, 3) = 2*3 = 6
    Answer by Private method = 4               // sub(6, 2) = 6-2 = 4 
    Answer by Private static method = 3        // div(6, 2) = 6/2 = 3
    Answer by Default method = 8               // add(6, 2) = 6+2 = 8
    Answer by Private static method = 1        // div(5, 3) = 5/3 = 1 
    Answer by Static method = 2                // mod(5, 3) = 5%3 = 2
```

### 추상클래스는 더이상 의미가 없나 ?

- 인터페이스에서 정말 모든 것을 다 할 수 있나?
    - 아니다 (추상 클래스에서만 할 수 있는 것들이 아직도 있다.)
        - 추상 클래스는 하나의 mutable 한 상태를 가질 수 있다.
        - 인터페이스는 상태를 가질 수 있다.
        - 또한 java8을 쓰는 경우 `private` 메서드가 없으니 추상 클래스로 만들어서 중복코드를 제거 할 수 있다.
        
## Reference

[https://www.notion.so/Java-8-0cc8c251d5374ac882a4f22fa07c4e6a](https://www.notion.so/8-0cc8c251d5374ac882a4f22fa07c4e6a)