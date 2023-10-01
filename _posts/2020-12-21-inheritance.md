---
layout: post
title: Inheritance
subtitle: 상속
categories: JAVA
tags: [JAVA, Inheritance]
---

## 상속 (Inheritance)

기존의 클래스를 재사용하여 새로운 클래스를 작성하는 것으로 `extends` 키워드를 사용하여 상속 관계를 만들 수 있다. 

```java
class Child extends Parent {
    // ...
}
```

위와 같은 상속관계에 있을 때 `Child` 클래스를 자식클래스, 하위(`sub`)클래스, 파생된(`derived`)클래스라 칭하고   
`Parent` 클래스를 부모클래스, 상위(`super`)클래스, 기반(`base`)클래스라 표현한다.

하위클래스는 상위클래스의 모든 멤버를 상속받는데 만약, 상위클래스에 멤버변수가 추가된다면 하위클래스도 자동적으로 상위클래스에서 추가한 멤버변수가 추가된다. 

예를들어,

```java
class Tumbler {
    int size;   // 멤버변수 추가
}

class StarbuksTumbler extends Tumbler {}
```

Tumbler 클래스에 `size`라는 정수형 변수를 멤버변수로 추가하면 하위클래스는 상위클래스를 상속받기 때문에 `StarbuksTumbler`는 자동적으로 `size` 멤버변수가 추가된 것과 같은 효과를 얻는다.  

[ 텀블러와 스타벅스텀블러 클래스의 상속관계도 ]
![[ 텀블러와 스타벅스텀블러 클래스의 상속관계도 ]](/assets/images/java/inheritance-relation.jpg)

- 상속관계 멤버 현황  

| 클래스  | 멤버  |
| :--- | :--- |
| Tumbler  | size  |
| StarbuksTumbler  | size  |

>💡 생성자와 초기화 블럭은 상속되지 않는다. 멤버만 상속된다.  
>💡 하위 클래스의 멤버 개수는 상위 클래스보다 항상 같거나 많다.

### 포함관계 (Composite)

상속 이외로 클래스를 재사용 하는 방법으로 한 클래스의 멤버변수로 다른 클래스를 선언하는 것이다. 

```java
class bongabbang {
    String flavor1 = "red bean";
    String flavor2 = "Chou cream";
    int size;
}
```

붕어빵 클래스를 포함관계 방식으로 새로 작성한다면 다음처럼 할 수 있다. 

```java
class bongabbang {
    Flavor flavor = new Flavor();   // 맛 (슈크림, 팥, 치즈, ...)
    int size;
}
```

붕어빵 클래스의 단위 요소인 맛 클래스를 미리 작성하고 붕어빵 클래스의 멤버변수로 선언하여 포함관계를 맺어주면 다음의 장점이 있다.

- 클래스를 작성하는것이 쉽다
- 코드도 간결하여 이해하기 쉽다
- 단위클래스별로 코드가 작게 나뉘어 작성되어 코드를 관리하는데 좋다.

>💡 '~ 은 ~ 이다 (is-a)' 에 넣었을 때 자연스러우면 상속관계로 작성  
>💡 '~ 은 ~을 가지고 있다. (has-a)' 에 넣었을 때 자연스러우면 포함관계로 작성

## 자바 상속의 특징

- 단일 상속 (single inheritance)
    
    c++에서는 다중 상속 (multiple inheritance)를 허용하지만 자바에서는 하나 이상의 클래스로부터 상속을 받을 수 없다.
    
    만약 A 클래스에서 B 클래스와 C클래스를 상속받는다고 가정을 할 때, B클래스와 C클래스에서 동일한 메서드가 있다면   
    - 어떤 메서드를 상속받게 될지,  
    - 둘 다 상속받게 된다면 메소드 선언부(이름, 매개변수)만 같고 서로 다른 내용의 메서드를 구별할 길이 없는 등의 문제가 생긴다. 
    
    이러한 다중상속의 문제점때문에 자바에서는 다중상속의 장점을 포기하고 단일 상속만을 허용한다.
    
    ```java
    class A extends B,C {   // 불가능
    
    }
    
    class A extends B {     // 가능 (오로지 한 클래스만 가능)
    
    }
    ```
    
- 상속에 횟수를 제한하지 않는다.
    - 다시말해 상위클래스는 여러개의 하위 클래스에게 상속이 가능하다.
    
- 최상위 클래스는  **Object 클래스** 이다.
    - Object 클래스만이 유일하게 super class를 가지지 않으며 자바의 모든 클래스들은 Object 클래스의 자손이다.
    

## super

하위클래스에서 상위클래스로부터 상속받은 멤버를 참조하는데 사용되는 **참조변수**

모든 인스턴스메서드에는 자신이 속한 인스턴스의 주소가 지역변수로 저장되는데 이것이 참조변수인 `this`와 `super`의 값이 된다.

```java
class Parent {
    int test = 10;
    String testStr;
    
    int getTestInt() {
        return test;
    }
}

class Child extends Parent {
    int test = 100;
    System.out.println("parent member :: " + super.test);   // 10
    System.out.println("child member :: " + this.test);    // 100
    this.testStr;     // this로도 Parent member 참조 가능

    int getTestInt() {
        return super.getTestInt();  // 상위클래스 메서드 호출
    }
}
```

상위클래스에서 상속받은 멤버도 하위클래스의 멤버이므로 `this` 키워드도 사용 가능하다. 상위클래스와 하위클래스의 멤버가 중복 정의되어 서로 구별되어야 할 때 `super` 를 사용하는 것이 좋다.

## super()

`this()` 와 마찬가지로 `super()` 도 생성자이다. 상위클래스의 생성자를 호출하는데 사용한다.

하위클래스의 인스턴스를 생성하면 상위클래스의 멤버와 하위클래스의 멤버가 모두 합쳐진 하나의 인스턴스가 생성된다. 이 때 조상(상위) 클래스의 멤버의 생성과 초기화 작업이 수행되어야 하기 때문에 자손 클래스의 생성자에서 조상클래스의 생성자가 호출되어야 한다. 

- 첫 줄에서 상위클래스의 생성자를 호출해야 하는 이유
    - **하위클래스의 멤버가 상위클래스의 멤버를 사용할 수도 있으므로** 상위클래스 멤버들이 먼저 초기화 되어 있어야 한다.
    

상위클래스의 생성자 호출은 클래스의 상속관계를 거슬러 올라가면서 계속 반복되고 최고 조상인 Object 클래스의 생성자인 `Object()` 까지 호출되면 끝이 난다. 

> 💡 Object 클래스를 제외한 모든 클래스의 생성자 첫 줄에는 아래의 생성자를 호출해야 한다.
>    - 같은 클래스의 다른 생성자
>    - 상위 클래스의 생성자 
>
> 호출하지 않으면 컴파일러가 자동적으로 생성자의 첫 줄에 'super()' 를 삽입한다.


```java
public class Tumbler {
    
    long size = 100L;
    String color;

    public Tumbler(long size, String color) {
        // 생성자 첫줄에서 다른 생성자를 호출하지 않기 때문에 컴파일시 super();가 삽입된다
        // super()는 Tumbler의 상위클래스인 Object 클래스의 기본 생성자 Object() 이다.
        this.size = size;
        this.color = color;
    }
}

public class StarbuksTumbler extends Tumbler{

    boolean isOfferCoupon;

    public StarbuksTumbler() {
        this(200L, "white", true);
    }

    public StarbuksTumbler(long size, String color, boolean isOfferCoupon) {
        super(size, color);
        this.isOfferCoupon = isOfferCoupon;
    }
}
```

```java
@Test
void printMember() {
    StarbuksTumbler starbuksTumbler = new StarbuksTumbler();
    System.out.println(starbuksTumbler.size);                // 200
    System.out.println(starbuksTumbler.isOfferCoupon);       // true
    System.out.println(starbuksTumbler.color);               // white
}
```

위 테스트 코드처럼 인스턴스를 생성하면 다음 순서대로 생성자가 호출된다.

![](/assets/images/java/inheritance-call-constructor.jpg)

1. `StarbuksTumbler()` 가 호출되고
2. `StarbuksTumbler(long size, String color, boolean isOfferCoupon)` 이 호출되고
3. `Tumbler(long size, String color)`가 호출되고
4. 마지막으로 `Object()` 가 호출됨을 알 수 있다.


> 💡 어떤 클래스의 인스턴스를 생성하면 클래스 상속관계의 최상위클래스인 Object 클래스까지 거슬러 올라가면서,  
모든 상위클래스의 생성자가 순서대로 호출된다.

## Object 클래스

모든 클래스 상속계층도의 제일 위에 위치하는 조상클래스이다. 

```java
// 아무것도 상속받지 않은 클래스도
class A {

}
// 컴파일 하게 되면 자동으로 Object 클래스를 상속받는다.
class A extends Object {

}
```

만일, 다른 클래스로부터 상속을 받는다고 하더라도 상속계층도를 따라 상위클래스를 찾아 올라가면 마지막 최상위 조상은 Object 클래스가 된다.

따라서 자바의 모든 클래스들은 Object 클래스에 정의된 멤버들을 사용할 수 있다.

[참고 https://docs.oracle.com/javase/10/docs/api/java/lang/Object.html](https://docs.oracle.com/javase/10/docs/api/java/lang/Object.html)

- Object class  

| Object 클래스 메서드 | 설명  |
|:---|:---|
| `protected Object clone()` | 객체 자신의 복사본을 반환  |
| `public boolean equals (Object obj)` | 객체 자신과 객체 obj가 같은 객체인지 알려준다. (같으면 `true`)  |
| `protected void finalize()` | 객체가 소멸될 때 가비지 컬렉터에 의해 자동적으로 호출된다. 이때 수행되어야 하는 코드가 있는 경우에만 오버라이딩하여 사용한다. |
| `public Class getClass()` |  객체 자신의 클래스 정보를 담고 있는 Class 인스턴스를 반환한다.  |
| `public int hashCode()` |  객체 자신의 해시코드를 반환한다. |
| `public String toString()` |  객체 자신의 정보를 문자열로 반환한다.  |
| `public void notify()` |  객체 자신을 사용하려고 기다리는 스레드를 하나만 깨운다.  |
| `public void notifyAll()` | 객체 자신을 사용하려고 기다리는 스레드를 모두 깨운다.  |
| `public void wait()`  |  다른 쓰레드가 notify()나 notifyAll()을 호출할 때까지 기다린다.  |
| `public void wait(long timeout)`  | 현재 쓰레드를 무한히 또는 지정된 시간 동안 기다리게 한다.  |
| `public void wait(long timeout, int nanos)`  | (timeout : 천분의 1초, nanos는 10^9 분의 1초)  |


## final

'마지막의', '변경될 수 없는' 의 의미로 `final` 키워드가 사용될 수 있는 곳은 클래스, 메서드, 멤버변수, 지역변수이다.

변수에 사용되면 값을 변경할 수 없는 상수가 되며,  메서드에 사용되면 오버라이딩을 할 수 없게 되고, 클래스에 사용되면 자신을 확장(`extends`)하는 하위클래스를 정의하지 못하게 된다.

대표적으로 `String` 클래스와 `Math` 클래스가 있다.

- final (제어자)

| 대상 | 설명 |
|:---|:---|
| 클래스 | 변경될 수 없는 클래스, 확장될 수 없는 클래스가 된다.<br> (final로 지정된 클래스는 다른 클래스의 조상이 될 수 없다.)  |
| 메서드 | 변경될 수 없는 메서드, 오버라이딩을 통해 재정의 될 수 없다.  |
| 멤버변수, 지역변수 | 변수 앞에 final이 붙으면 값을 변경 할 수 없는 상수가 된다. |


```java
// 조상이 될 수 없는 클래스
final class FinalTest {                   

    // 값을 변경할 수 없는 상수 (멤버변수)
    final int TEST_MEMBER = 100;          
    
    // 오버라이딩할 수 없는 메서드
    final void getTest() {    
        // 값을 변경할 수 없는 상수 (지역변수)            
        final int TEST_LOCAL = TEST_MEMBER; 
        return TEST_LOCAL;
    }
}
```

이처럼 `final`이 키워드가 붙으면 추후 변경이 될 수 없다. 그렇다면 인스턴스마다 다른 값을 가지고 싶고 추후 그 값이 변경되지 않으려면 어떻게 해야 할까?

**생성자를 이용하여 final 멤버변수를 초기화 하면 된다.**

```java
public class Card {

    final int NUMBER;        
    final String KIND;
    static int width = 100;
    static int height = 250;

    public Card(String kind, int number) {
        NUMBER = number;
        KIND = kind;
    }

    public Card() {
        this("HEART", 1);
    }
    
    public String toString() {
        return KIND + " " + NUMBER;
    }
}
```

```java
Card card = new Card("SPACE", 10);
// card.NUMBER = 10; // Cannot assign a value to final variable 'NUMBER'
System.out.println(card.KIND);     // SPACE
System.out.println(card.NUMBER);   // 10
```

상수는 선언과 함께 초기화 하는게 일반적이지만 생성자에서 단 한번만 초기화 하여 사용할 수도 있다. 생성자에서 매개변수로 넘겨받은 값으로 멤버변수들을 초기화 한다.

**단, 생성자로 한번 초기화 한 멤버변수를 변경할 수는 없다.**

### 메서드 오버라이딩 (Overriding)

상위클래스가 가지고 있는 메서드를 하위 클래스가 재정의해서 사용하는 것을 오버라이딩이라 한다.

```java
class Point {

    int x;
    int y;
    
    String getLocation() {
        return "x : " + x + ", y : " + y;
    }
}

class Point3D extends Point {

    int z;

    String getLocation() {  // 오버라이딩
        return "x :" + x + ", y :" + y + ", z :" + z;
    }
}
```

- 오버라이딩 조건
    - 하위클래스에서 오버라이딩 하는 메서드는 상위클래스의 메서드와
        - 이름이 같아야한다.
        - 매개변수가 같아야한다.
        - 리턴타입이 같아야한다.
        - 즉, 선언부가 서로 일치해야 한다.
    - 접근제어자와 예외(Execption)는 제한된 조건 하에서 다르게 변경할 수 있다.
        - 접근제어자는 상위클래스의 메서드보다 좁은 범위로 변경 할 수 없다.
        - 조상클래스의 메서드보다 많은 수의 예외를 선언할 수 없다.
        

상위클래스에 `static` 메소드가 있을 때 하위클래스에서 `static` 메소드를 `override` 할 수 있을까?

```java
public class Overriding {

    public static void method1() {
        System.out.println("A");
    }

    public void method2() {
        System.out.println("A");
    }
}

public class Overriding2 extends Overriding{
    public static void method1() {
        System.out.println("B");
    }

    public void method2() {
        System.out.println("B");
    }
}
```

```java
@Test
void overridingTest() {
    Overriding o = new Overriding2();
    o.method1();
    o.method2();
}
```

결과는 A 와 B 가 출력된다. 

**static method는 클래스가 컴파일 되는 시점에 결정**되지만 **override는 런타임 시점에 사용될 메서드가 결정**이 된다.   
⇒ static 메소드는 오버라이딩 될 수 없다. 또한 **static은 클래스 단위**로 만들어지기 때문에 **객체 단위로 형성되는 override에 성립될 수 없다**.

![](/assets/images/java/inheritance-compile-error.png)

>💡 오버라이딩은 런타임 시점에 이루어지는 dynamic dispath

## 다형성 (polymorphism)

'여러 가지 형태를 가질 수 있는 능력' 이란 의미로 자바에서는 한 타입의 참조변수로 여러 타입의 객체를 참조할 수 있도록 함으로써 다형성을 프로그램적으로 구현하였다.

다시말해, **상위클래스 타입의 참조변수로 하위클래스의 인스턴스를 참조할 수 있도록 하였다**는 것이다.

```java
public class Tumbler {

    int count;

    void increase() {
        ++count;
    }
    void decrease() {
        --count;
    }
}

public class StarbuksTumbler extends Tumbler{

    boolean isOfferCoupon;

    void setOfferCoupon() {
        isOfferCoupon = !isOfferCoupon;
    }
}
```

클래스 Tumbler와 StarbuksTumbler는 서로 상속관계에 있으며 두 클래스의 인스턴스를 생성하는 방법은 다음과 같다.

```java
Tumbler tumbler = new Tumber();
StarbuksTumbler starBuks = new StarbuksTumbler();
```

위 방법은 인스턴스의 타입과 참조변수의 타입이 일치하게끔 인스턴스를 생성하는 방법이고, 

두 클래스가 상속관계에 있을 경우 상위클래스 타입의 참조변수로 하위클래스의 인스턴스를 참조하도록 하는 것이 가능하다. 

```java
Tumbler t = new StarbuksTumbler();
```

그렇다면 두 방법의 차이는 무엇일까?

```java
StarbuksTumbler s = new StarbuksTumbler();
Tumbler t = new StarbuksTumbler();
```
![상속관계도](/assets/images/java/inheritance-relation-2.png "상속관계도")


![](/assets/images/java/inheritance-instance.png)


실제 인스턴스가 `StarbuksTumbler` 라 할지라도 참조변수가 `Tumbler`이면 `StarbuksTumbler` 인스턴스의 모든 멤버를 사용할 수 없다. `Tumbler` 타입의 참조변수로는 `StarbuksTumbler` 인스턴스 중에서 **`Tumbler` 클래스의 멤버들만 사용할 수 있다.**

**둘 다 같은 타입의 인스턴스지만 참조변수의 타입에 따라 사용할 수 있는 멤버의 개수가 달라진다.** 

하위클래스의 참조변수로 상위클래스타입의 인스턴스를 참조하는 것은 존재하지 않는 멤버를 사용하고자 할 가능성이 있으므로 허용하지 않는다. (상속관계도 그림 참고)

```java
StarbuksTumbler error = new Tumbler(); // 컴파일 에러 발생
```

즉, **참조변수가 사용할 수 있는 멤버의 개수는 인스턴스의 멤버 개수보다 같거나 적어야 한다**.

### 참조변수의 형변환

기본형 변수의 형변환에서 작은 자료형에서 큰 자료형의 형변환은 생략 가능하듯이 참조형 변수의 형변환에서는 **자손타입의 참조변수를 조상타입으로 형변환 하는 경우에는 형변환을 생략**할 수 있다.

```java
Tumbler parent = null;
StarbuksTumbler child = new SartbuksTumbler();
parent = (StarbuksTumbler) child;     // 생략 불가능    
child = parent;   
```

`Tumbler` 타입의 참조변수 parent를 `StarbuksTumbler` 타입으로 변환하는 것은 참조변수가 다룰 수 있는 멤버의 개수를 늘이는 것이므로 실제 인스턴스의 멤버 개수보다 참조변수가 사용할 수 있는 멤버의 개수가 더 많아지므로 문제가 발생할 가능성이 있어 생략할 수 없다.

>💡 자손타입 —> 조상타입 (up - casting)  : 형변환 생략 가능
>💡 조상타입 —> 자손타입 (down - casting) : 형변환 생략 불가

- 참고
    1. `Tumbler parent = null;`
        
        ![](/assets/images/java/inheritance-picture-1.jpg)
        
    2. `StarbuksTumbler child = new SartbuksTumbler();`
        
        ![](/assets/images/java/inheritance-picture-2.jpg)

    3. `parent = (StarbuksTumbler) child;` 
        
        ![](/assets/images/java/inheritance-picture-3.jpg)

        

### instanceof 연산자

참조변수가 참조하고 있는 **인스턴스의 실제 타입을 알아보기 위해 사용**한다.

`instanceof` 왼쪽에는 참조변수를 오른쪽에는 타입이 피연산자로 위치한다. 연산의 결과는 `boolean` 값으로 반환된다.

## 다이나믹 (메소드) 디스패치

자바에는 compile time 다형성과 runtime 다형성이 있는데 정적(`static`) 메소드를 오버로딩하면 컴파일 타임의 다형성이고 런타임 다형성은 메서드 오버라이드를 통해 자바의 다형성을 수행할 수 있다. 

Dynamic method dispatch는 컴파일 타임이 아닌 런타임에 오버라이딩 된 메서드에 대한 호출이 결정되는 메커니즘이다.

- Static Method Dispatch

```java
public class A {
    void method1() {
        System.out.println("A class method1");
    }

    void method1(int value) {
        System.out.println("A class method1" + value);
    }
}

public class Dispatch {
    public static void main(String[] args) {
        A a = new A();
        a.method1();
        a.method1(10);
    }
}
```

위와 같은 경우 컴파일 시점에 알 수 있는 정보로 어떤 메소드를 호출할지 정할 수 있어 컴파일시점에 정하여 호출하는 방법이다. (컴파일러가 A클래스의 `method1` 을 호출시킬 것을 명확히 알고 있음)

- Dynamic method dispatch

```java
public class Shape {
    void draw() {
        System.out.println("drawing...");
    }
}

public class Rectangle extends Shape{
    void draw() {
        System.out.println("drawing rectangle..");
    }
}

public class Dispatch {
    public static void main(String[] args) {
        Shape shape;
        shape = new Rectangle();
        shape.draw();
    }
}
```

shape는 상위클래스의 참조변수이고 이 참조변수가 어떤 `draw()`를 호출할지는 참조변수가 참조하는 객체 (Rectangle) 를 기반으로 한다. 그렇기 때문에 컴파일러는 어떤 함수가 실행될지는 모르는 것이다. 컴파일된 바이트 코드에 있는 정보를 토대로 런타임 시점에 Rectangle 클래스의 객체를 생성하고 Rectangle 클래스의 draw() 메소드가 호출되는 것이 결정된다.

- 런타임 전에는 객체 생성이 되지 않음
- `Shape shape = new Rectangle();` 컴파일러는 Rectangle 인스턴스가 생성되는 것을 모름
    - 그러므로 Shape 의 메소드만 접근 가능 (런타임시 결정됨)

### 의문점

컴파일 타임은 컴파일러가 java 언어로 짠 코드를 기계어로 변환하여 class 파일로 만드는 시점으로 알고있고 런타임은 jvm이 구동되면서 class 파일을 읽어 실행하는 시점 (응용프로그램이 실행되는 시점) 으로 알고있는데,  

Dynamic method dispatch 은 런타임 시점에 어떤 메소드가 호출될지 결정되고 static dispatch는 컴파일 타임에 결정된다면 바이트코드가 달라야 하지 않나라는 생각이 들어 바이트 코드를 출력해 보았다.

- static method dispatch 
![static method dispatch](/assets/images/java/inheritance-static-dispatch.png)

- dynamic method dispatch
![dynamic method dispatch](/assets/images/java/inheritance-dynamic-dispatch.png)


바이트코드상의 정적메소드디스패치와 다이나믹 메소드 디스패치의 차이는 없었다. 

- TODO : 라이브스터디 도중 어느분이 말씀하시길 virtual table에 메소드가 담기고 호출 시(런타임) 그 테이블에서 호출할 메서드를 가져오는 것(주소 접근)이라 하셨다. 추후 이부분에 대해서는 공부해야겠다. 

## 더블 (메소드) 디스패치

dynamic dispath를 두번 하는 것이다.

예를들어 sns 별로 post를 보내야 한다고 가정했을  때 

1. 구현체(sns)에 따라 로직이 다르지 않은 경우 
    
    ```java
    interface Post {
        void postOn(SNS sns);
    }
    class Text implements Post {
        public void postOn(SNS sns) {
            // text -> sns
            System.out.println("Text " + sns);
        }
    }
    class Picture implements Post {
        public void postOn(SNS sns) {
            // picture -> sns
            System.out.println("Picture " + sns);
        }
    }
    
    interface SNS {};
    class Facebook implements SNS {
    
    }
    class Twitter implements SNS {
    
    }
    ```
    
    ```java
    public static void main(String[] args) {
        List<Post> posts = Arrays.asList(new Text(), new Picture());
        List<SNS> sns = Arrays.asList(new Facebook(), new Twitter());

        posts.forEach(p -> sns.forEach(s -> p.postOn(s)));
    }
    ```
    
    ```java
    // 결과
    Text dispatch.Facebook@13221655
    Text dispatch.Twitter@2f2c9b19
    picture dispatch.Facebook@13221655
    picture dispatch.Twitter@2f2c9b19
    ```
    
    sns마다 text와 picture를 보내는 방식이 다르지 않을 때 이렇게 구현하여 사용할 수 있다.
    
    그렇다면 보내는 방식이 다르다면 ? (로직이 다르다면)  두 가지 방법이 있다.
    
2. 분기문 사용
    
    ```java
    interface Post {
        void postOn(SNS sns);
    }
    class Text implements Post {
        public void postOn(SNS sns) {
            if (sns instanceof Facebook) {
                System.out.println("Facebook Text " + sns);
            } else if (sns instanceof Twitter) {
                System.out.println("Twitter Text " + sns);
            } else {
                throw new IllegalArgumentException();
            }
        }
    }
    class Picture implements Post {
        public void postOn(SNS sns) {
            if (sns instanceof Facebook) {
                System.out.println("Facebook Picture " + sns);
            } else if (sns instanceof Twitter) {
                System.out.println("Twitter Picture " + sns);
            } else {
                throw new IllegalArgumentException();
            }
        }
    }
    
    interface SNS {};
    class Facebook implements SNS {
    
    }
    class Twitter implements SNS {
    
    }
    ```
    
    ```java
    public static void main(String[] args) {
        List<Post> posts = Arrays.asList(new Text(), new Picture());
        List<SNS> sns = Arrays.asList(new Facebook(), new Twitter());

        posts.forEach(p -> sns.forEach(s -> p.postOn(s)));
    }
    ```
    
    ```java
    // 결과
    Facebook Text dispatch.Facebook@13221655
    Twitter Text dispatch.Twitter@2f2c9b19
    Facebook Picture dispatch.Facebook@13221655
    Twitter Picture dispatch.Twitter@2f2c9b19
    ```
    
    이렇게 코드를 짠다면 구현체별로 다른 로직을 사용할 수 있지만 실수로 분기문을 추가 안한 경우 의도치 않게 exception이 발생할 수 있다.
    
    메소드 오버로딩(static dispatch 방법)을 사용하게된다면 컴파일 시점에 에러가 날 것이다. 왜냐하면 SNS는 인터페이스 타입이기때문에 어떤 구현체(Facebook, Twitter) 타입인지 컴파일러가 알 수 없기 때문이다.
    
3. Double Dispath 사용
    
    ```java
    interface Post {
        void postOn(SNS sns);
    }
    class Text implements Post {
        public void postOn(SNS sns) {
            sns.post(this);
        }
    }
    class Picture implements Post {
        public void postOn(SNS sns) {
            sns.post(this);
        }
    }
    interface SNS {
        void post(Text text);
        void post(Picture picture);
    };
    class Facebook implements SNS {
        System.out.println("Facebook " + text);
    }
    class Twitter implements SNS {
        System.out.println("Facebook " + picture);
    }
    ```
    
    ```java
    public static void main(String[] args) {
        List<Post> posts = Arrays.asList(new Text(), new Picture());
        List<SNS> sns = Arrays.asList(new Facebook(), new Twitter());

        posts.forEach(p -> sns.forEach(s -> p.postOn(s)));
    }
    ```
    
    dynamic dispatch를 두번 사용
    
    - Post중 어떤 구현체의 postOn 메소드를 실행할지
    - postOn 메소드 내부에서 SNS 중 어떤 구현체의 post 메소드를 실행할지
    
    만약 새로운 구현체가 생기는 경우 다음과 같이 구현체에 대한 코드만 작성하면 된다.
    
    ```java
    class Instagram implements SNS {
        public void post(Text text) {
            System.out.println("Instagram " + text);
        }

        public void post(Picture picture) {
            System.out.println("Instagram " + picture);
        }
    }
    ```
    
    기존에 의존하던 코드에 직접적으로 영향을 주지 않으니 구현체를 새로 추가하는 것이 자유롭다.  
    
### Visitor Pattern

방문자 패턴으로 **실제 로직을 가지고 있는 객체(Visitor)가 로직을 적용할 객체(Element)를 방문하면서 실행**하는 패턴 이다. 로직과 구조를 분리하는 패턴으로 구조를 수정하지 않아도 새로운 동작을 기존 객체 구조에 추가할 수 있다.

위의 코드로 보자면 다음과 같다.

- Post 클래스는 postOn() 메소드 (accept)을 제공한다. (어떤 SNS 타입의 구현체가 들어오는지 상관없음) - **구조**
- SNS 구현체 (Visitor) 의 post() 메소드(visit)을 통해 실제 로직을 실행 -**로직**

만약 SNS 구현체가 새로 추가되어도 Post 클래스 (구조)에는 영향을 미치지 않는다.

그러면 Post 구현체가 추가된다면? SNS에 Post 구현체를 처리하는 메소드를 모두 추가해주어야 한다. 단 SNS 구현체에 따라 로직이 다르지 않고 모두 공통 로직을 사용한다면 SNS를 추상클래스로 작성하여 사용해도 된다.
    
     
    

## 추상클래스 (abstract class)

클래스는 '객체를 정의해 놓은 것' 또는 '객체의 설계도 또는 틀' 이라 할 수 있는데 추상클래스는 미완성 설계도를 의미한다. 

미완성이라는 것은 멤버의 개수가 모자란다는것이 아니라, 단지 미완성메서드 (추상메서드)를 포함하고 있다는 뜻으로 미완성 설계도로 완성된 제품을 만들 수 없듯이 **추상클래스로는 인스턴스를 생성할 수 없다**. (상속을 통해서 하위클래스에 의해서만 완성될 수 있음)

예를 들어, 같은 크기의 노트북이라도 기능의 차이에 따라 여러 종류의 모델이 있는데 이들의 설계도는 약간의 기능을 제외하고 거의 동일할 것이다. 그렇다면 종류마다 서로 다른 설계도를 작성하는 것보다 공통부분만을 그린 미완성 설계도를 만들어 놓고 이 미완성 설계도를 이용해서 각각의 설계도를 완성하는것이 효율적일 것이다. 

추상클래스 자체로는 클래스의 역할을 다 하지 못하지만 새로운 클래스를 작성하는데 있어서 바탕이 되는 조상클래스로서 중요한 의미를 갖는다.

### 추상 클래스 작성방법

키워드 `abstract` 을 붙이면 완성이다. 

```java
abstract class IncompleteClass {
    // ...
}
```

클래스 선언부의 `abstract` 키워드를 보고 이 클래스는 추상메서드가 있으니 상속을 통해 구현해주어야 한다는 것을 알 수 있다.

>💡 추상클래스는 추상메서드를 포함하고 있다는 것 외엔 일반클래스와 동일하다.
추상클래스에도 생성자가 있으며, 멤버변수와 메서드 또한 가질 수 있다.

### 추상메서드 (abstract method)

메서드는 선언부와 구현부로 구성되어 있는데 선언부만 작성하고 구현부는 작성하지 않은 채로 남겨 둔 것이 추상메서드이다. 

이렇게 하면 메서드의 내용이 상속받는 클래스에 따라 다르게 구현될 수 있다.

추상메서드를 작성하는 방법 역시 키워드 `abstract` 를 붙여주고 구현부가 없으므로 `{}` 대신 문장의 끝을 알리는 `;` 을 적어준다. 

```java
/* 이메서드는 ... 한 기능을 수행할 목적으로 작성된 메서드이다 */
abstract 리턴타입 메서드이름();
```

추상클래스로부터 상속받는 하위클래스는 오버라이딩을 통해 조상인 추상클래스의 추상메서드를 모두 구현해주어야 한다. **만일, 하나라도 구현하지 않는다면 하위클래스 역시 추상클래스로 지정해 주어야 한다**.

### abstract

> 추상[抽象 뽑을 추, 코끼리 상] 낱낱의 구체적 표상이나 개념에서 **공통된 성질을 뽑아** 이를 일반적인 개념으로 파악하는 정신 작용
> 

상속이 자손 클래스를 만드는데 조상 클래스를 사용하는 것이고, 반대로 추상화는 기존의 클래스의 공통부분을 뽑아내서 조상 클래스를 만드는 것이라고 할 수 있다.

>💡 추상화 - 클래스간의 공통점을 찾아내어 공통의 조상을 만드는 일  
>💡 구체화 - 상속을 통해 클래스를 구현, 확장하는 일

---

### 참조

[https://dbbymoon.tistory.com/9](https://dbbymoon.tistory.com/9)