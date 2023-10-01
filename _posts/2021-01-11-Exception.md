---
layout: post
title: 예외처리
subtitle: Exception handling
categories: JAVA
tags: [JAVA, Exception handling]
---

## 예외처리 (Exception handling)

프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우에서 이 결과를 초래하는 원인을 **프로그램** **에러** 또는 **오류** 라고 한다. 

이를 발생시점에 따라 '컴파일 에러(compile-time error)' 와 런타임 에러 (runtime error) 로 분류 하는데 자바에서는 **runtime** 에 발생할 수 있는 프로그램 오류를 **'에러' 와 '예외'** 두 가지로 구분하였다.

예외처리란 프로그램 실행 시 발생할 수 있는 예외의 발생에 대한 코드를 작성하는 것으로 그 목적은 프로그램의 비정상 종료를 막고 정상적인 실행상태를 유지하는 것에 있다.

## 에러 (Error) vs 예외 (Exception)

에러는 메모리 부족(OutOfMemoryError) 또는 스택오버플로우(StackOverflowError**)** 와 같이 일단 발생하면 **복구 할 수 없는 심각한 오류**이고 예외는 발생하더라도 수습될 수 있는 **비교적 덜 심각한 것**이다. 

>💡 에러 vs 예외  
> 에러 (error) : 프로그램 코드에 의해서 수습 될 수 없는 심각한 오류  
> 예외 (exception) : 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

![](/assets/images/java/Exception-error-vs.png)

## 자바 예외 계층구조

![](/assets/images/java/Exception-hierarchy.png)

[https://madplay.github.io/post/java-checked-unchecked-exceptions](https://madplay.github.io/post/java-checked-unchecked-exceptions)

모든 예외의 최고 조상은 Exception 클래스이며, RuntimeException(Unchecked Exception)과 Checked Exception 으로 나눠지는 것을 볼 수 있다. 

### RuntimeException (Unchecked Exception) vs Checked Exception

RuntimeException 클래스들은 주로 개발자의 실수에 의해서 발생될 수 있는 예외들로서, 자바 프로그래밍 요소들과 관계가 깊다. 예를들어, 배열의 범위를 벗어났다던가 (`IndexOutOfBoundsException`) 값이 null인 멤버를 호출하려 했다던가 (`NullPointerException`), 클래스간의 형변환을 잘못했다던가(`ClassCastException`), 정수를 0으로 나누려 했다던가 (`ArithemeticException`) 등의 경우에 발생하는 예외들이다. 

**RuntimeException 예외들이 발생할 가능성이 있는 코드들은 try-catch 문을 사용하기보다는 주의 깊게 코딩하여 예외가 발생하지 않도록 해야하는 것이 더 좋다.**   
**단, RuntimeException 클래스들은 예외처리를 해 주지 않아도 컴파일러가 문제삼지 않는다는 사실을 잊지말자.**

Checked Exception 에 해당하는 예외 클래스들은 주로 외부의 영향으로 발생할 수 있는 것들로서, 프로그램의 사용자들의 동작에 의해서 발생하는 경우가 많다. (=클라이언트쪽=메서드를 호출하는쪽)

예를들어, 존재하지 않는 파일의  이름을 입력했다던가 (`FileNotFoundException`), 실수로 클래스의 이름을 잘못 적었다던가 (`ClassNotFoundException`) , 또는 입력한 데이터 형식이 잘못된 경우(`DataFormatException`)에 발생하는 예외들이다. 이러한 종류들의 예외는 **반드시 처리를 해주어야 한다. 그렇지 않으면 컴파일 에러가 발생한다.**

> 클라이언트가 exception을 적절히 회복할 수 있을것으로 예상 되면 
    → checked exception     
> 그렇지 않은 경우 unchecked exception 으로 만드는 것이 좋다.

## 예외처리 방법

### try - catch

기본 구조는 다음과 같다. 

```java
try {
    // try_statements
} catch (Exception1 e1) {
    // catch_statements
} catch (Exception2 e2) {
    // catch_statements
} catch (ExceptionN eN) {
    // catch_statements
}
```

- try_statements 공간에는 실행될 코드를 작성하고 catch_statements 공간에는 해당 예외가 발생하였을 때 처리할 내용들을 작성한다.
- 좁은 그물망을 위에 넓은 그물망을 아래의 catch 블럭에 작성해야 한다.
    - 위에 넓은 그물망을 잡을 경우 아래의 catch블럭은 쓸모 없게 되기 때문이다.
- catch 블럭은 괄호 () 와 블럭 {} 두 부분으로 나눠져 있는데, 괄호 () 안에는 처리하고자 하는 예외와 같은 타입의 참조변수를 하나 선언해야 한다.
    - 예외가 발생하면 **발생한 예외에 해당하는 클래스의 인스턴스가 만들어진다.**
        - 첫 번째 catch블럭부터 차례로 내려가면서 `instanceof` 연산자를 이용하여 true 즉, 발생한 예외와 맞는 catch 블럭을 찾을 때까지 계속되는데, **찾게되면 블럭에 있는 문장들을 모두 수행 후에 try-catch문을 빠져나가고**, 예외는 처리되지만 true인 catch 블럭이 하나도 없을 경우 예외는 처리 되지 않는다.
        - 예외가 발생했을 때 생성되는 예외클래스의 인스턴스에는 발생한 예외에 대한 정보가 담겨져 있어 다음 메소드를 통해서 이 정보들을 얻을 수 있다.
            - `printStackTrace()` : 예외 발생 당시의 호출 스택(Call Stack) 에 있었던 메서드 정보와 예외 메시지를 화면에 출력한다.
            - `getMessage()` : 발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.
            
            ```java
            try	{
                System.out.println(0/0);       // 예외 발생
            } catch (ArithmeticException ae) {
                ae.printStackTrace();
                System.out.println("예외 메시지 : " + ae.getMessage());
            }
            ```
            
            ```xml
            java.lang.ArithmeticException: / by zero   // printStackTrace()
            	at App.main(App.java:30)
            
            예외 메시지 : / by zero                     // getMessage()
            ```
            
            - `printStackTrace(PrintStream s)`, `printStackTrace(PrintWriter s)` 로 발생한 예외에 대한 정보를 파일에 저장 할 수도 있다.
- catch 블럭 안에 있는 참조변수 들은 (e1, e2, eN ..) **각 catch 블럭 내에서만 유효**하다.
    - 만약, catch블럭 안에 try-catch 구문을 또 작성하려면 해당 catch블럭 참조변수 이름을 제외한 이름의 참조변수를 지정해야 한다.

try 선언에는 세 가지 형식이 존재한다.

1. `try ... catch`
    
    ```java
    System.out.println(1);
    System.out.println(2);
    try	{
        System.out.println(3);
        System.out.println(0/0);       // 예외 발생
        System.out.println(4);
    } catch (ArithmeticException ae) {
        System.out.println(5);
    }
    System.out.println(6);
    ```
    
    ```java
    결과 : 
        1
        2
        3
        5
        6
    ```
    
    결과로 보아 `try-catch` 문의 흐름은 다음과 같음을 알 수 있다. 
    
    try 블럭 내에서 예외가 발생한 경우,
    
    1. 발생한 예외와 일치하는 catch블럭이 있는지 확인한다.
    2. 일치하는 catch블럭을 찾게 되면 해당 catch블럭 내의 문장들을 수행하고 전체 try-catch 문을 빠져나가서 그 다음 문장을 계속해서 수행한다.
        - 만일, 일치하는 catch 블럭을 찾지 못하면 예외는 처리되지 못한다.
    
    try 블럭 내에서 예외가 발생하지 않은 경우, 
    
    1. catch 블럭을 거치지 않고 전체 try-catch 문을 빠져나가서 수행을 계속한다. 
    
2. `try ... finally`
    
    ```java
    System.out.println(1);
    try {
        System.out.println(2);
    } finally {
        System.out.println(3);
    }
    System.out.println(4);
    ```
    
    ```xml
    결과 : 
        1
        2
        3
        4
    ```
    
    try 블럭 수행 → finally 블럭 수행 → try문 빠져나와 다음 문장 수행 순으로 이어진다.
    
    `finally` 블럭은 `try-catch` 문과 함께 예외의 발생여부에 상관없이 실행되어야 할 코드를 포함 시킬 목적으로 사용된다.
    
    또한 finally 블럭 안에서는 return 을 하지 말아야 한다. (**Anti-Pattern**)
    
    - try 블록 안에서 발생한 예외는 무시되고 finally 에서 무조건 return 되니까 try문이 정상 종료되어 **예외를 알 수 없게 된다.**
    
3. `try ... catch ... finally`
    
    ```java
    System.out.println(1);
    try	{
    	System.out.println(2);
    	System.out.println(0/0);       // 예외 발생
    	System.out.println(3);
    } catch (ArithmeticException ae) {
    	System.out.println(4);
    } finally {
    	System.out.println(5);
    }
    System.out.println(6);
    ```
    
    ```xml
    결과 : 
        1
        2
        4
        5
        6
    ```
    
    예외가 발생한 경우에는 try → catch → finally 순으로 발생하지 않은 경우에는 try → finally 순으로 실행된다. 
    
    catch 블럭 문장 수행 중에 `return` 문을 만나도 finally 블럭의 문장들은 수행된다.
    

### Multicatch block

java 7 이전에는 catch 블록에서 하나의 예외 타입만 작성할 수 있어 하나 이상의 예외가 있고, 동일한 조치를 취해야 하는 상황에서 동일한 코드가 있는 catch블럭 여러개를 작성해야 했다.

그러나  java 7 부터 `|` (파이프)로 구분하여 단일 catch 블록이 여러 예외를 처리할 수 있다.

- 기존

```java
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    try {
        int n = Integer.parseInt(scanner.nextLine());
        if (99%n == 0)
            System.out.println(n + " is a factor of 99");
    } catch (NumberFormatException ex) {
        System.out.println("Number Format Exception " + ex);
    } catch (ArithmeticException ex) {
        System.out.println("Arithmetic " + ex);
    }
}
```

- 자바 7 이후

```java
public static void main(String[] args) {
    Scanner scanner = new Scanner(System.in);
    try {
        int n = Integer.parseInt(scanner.nextLine());
        if (99%n == 0)
            System.out.println(n + " is a factor of 99");
    } catch (NumberFormatException | ArithmeticException ex) {
        System.out.println("Exception encountered " + ex);
    }
}
```

- 단, 상위클래스와 하위클래스는 하나의 catch 문에서 처리할 수 없다.
    - `catch(NuberFormatException | Exception ex)` // 컴파일 에러

### try-with-resources

java 7이상을 사용하고, 리소스를 해제해야 한다면, try-with-resources 구문을 사용하는 것이 좋다

- exception 시 resources를 자동으로 close() 해준다.
- 사용 로직을 작성할 객체는 `AutoCloseable` 인터페이스를 구현한 객체여야 한다.

보통은 다음과 같이finally 블럭에서 close를 할 것이다.

```java
static void copy(String src, String dest) throws IOException {
    InputStream in = null;
    OutputStream out = null;
    
    try {
        in = new FileInputStream(src);
        out = new FileOutputStream(dest);
        byte[] buf = new byte[1024];
        int n;
        while ((n = in.read(buf)) >= 0) {
            out.write(buf, 0, n);
        }
    } finally {
        if (in != null) in.close();
        if (out != null) out.close();
    }
}
```

위 코드에서 문제점은 finally 블럭에서 **close를 할 때, exception이 발생할 수** 있다는 것이다. 만약, 발생하게 된다면 out 스트림이 닫히지 않고 코드가 된다.

그러면 위 문제를 방지하기 위해 다음과 같이 코드를 작성할 것이다. 

```java
try {
    in = new FileInputStream(src);
    out = new FileOutputStream(dest);
    byte[] buf = new byte[1024];
    int n;
    while ((n = in.read(buf)) >= 0) {
        out.write(buf, 0, n);
    }
} finally {
    if (in != null) {
        try {
            in.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
    if (out != null) {
        try {
            out.close();
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

이렇게 작성하게 되면 또 문제가 생길 여지가 있다. catch 블럭에서 IOException만 캐치하기 때문에 위의 if문에서 RuntimeException이 발생한다면 아래의 if문은 실행하지 않아 out은 close가 되지 않는다. 

```java
try {
      try {
          in = new FileInputStream(src);
          out = new FileOutputStream(dest);
          byte[] buf = new byte[1024];
          int n;
          while ((n = in.read(buf)) >= 0) {
              out.write(buf, 0, n);
          }
      } finally {
          if (in != null) {
              try {
                  in.close();
              } catch (IOException e) {
                  e.printStackTrace();
              }
          }
      }
  } finally {
      if (out != null) {
          try {
              out.close();
          } catch (IOException e) {
              e.printStackTrace();
          }
      }
  }
```

이렇게 작성하면 in을 닫는데 문제가 생기더라도 out을 닫는 코드는 실행된다. 그러나, 이렇게 작성하면 코드가 굉장히 길고 가독성도 떨어진다. 이러한 경우에 try-with-resource  를 사용하여 작성하면 안전하고, 짧게 코드를 작성할 수 있다. 

```java
static void copy(String src, String dest) throws IOException {
    try (InputStream in = new FileInputStream(src); 
        OutputStream out = new FileOutputStream(dest)){

        byte[] buf = new byte[1024];
        int n;
        while ((n = in.read(buf)) >= 0) {
            out.write(buf, 0, n);
        }
    }
}
```

위와같이 try-with-resource 를 이용하여 작성하여 바이트 코드를 보면 내부적으로는 catch 블록이 하나 더 생기는 것을 볼 수 있다. 

```java
static void copy(String var0, String var1) throws IOException {
    FileInputStream var2 = new FileInputStream(var0);

    try {
        FileOutputStream var3 = new FileOutputStream(var1);

        try {
            byte[] var4 = new byte[1024];

            int var5;
            while((var5 = var2.read(var4)) >= 0) {
                var3.write(var4, 0, var5);
            }
        } catch (Throwable var8) {
            try {
                var3.close();
            } catch (Throwable var7) {
                var8.addSuppressed(var7);
            }

            throw var8;
        }

        var3.close();
    } catch (Throwable var9) {
        try {
            var2.close();         // 예외가 났을 경우
        } catch (Throwable var6) {
            var9.addSuppressed(var6);
        }

        throw var9;
    }

    var2.close();                 // 예외가 나지 않았을 경우
}
```

굳이 close를 해주는 코드를 작성하지 않아도, 컴파일러가 알아서 close를 해주니 try-with-resource 를 안쓸 이유는 없다.!

### throws

메서드에 예외를 선언하는 방법으로 `throws` 키워드를 사용하여 선언한다. 여러개일 경우에는 쉼표(,) 로 구분한다.

```java
void method() throws Exception1, Exception2, ... ExceptionN {

}
```

메서드의 선언부에 예외를 선언함으로써 메서드를 사용하려는 사람이 메서드의 선언부를 보았을 때, **이 메서드를 사용하기 위해서는 어떠한 예외들이 처리되어져야 하는지 알 수 있다**. 

![](/assets/images/java/Exception-wait.png)

예를들어, wait() 메서드의 선언부에는 `InterruptedException` 이 적혀 있는 것을 볼 수 있는데 이것은 이 메서드를 사용하려면 `InterruptedException` 을 처리 해주어야 한다는 것을 의미한다. 

결론적으로,

예외가 발생한 **메서드 내에서 자체적으로 처리해도 되는 것**은 메서드 내에서 `try-catch문`을 사용해서 처리하고, 

**메서드에 호출 시 넘겨 받아야 할 값을 다시 받아야 하는 경우** 즉, 메서드 내에서 자체적으로 해결이 안되는 경우에는 예외를 메서드에 선언(`throws`)해서 호출한 메서드에서 처리해야 한다. 

### throw

키워드 `throw` 를 사용하면 프로그래머가 고의로 예외를 발생시킬 수 있다. 

1. 연산자 new 를 이용하여 발생시키려는 예외 클래스의 객체를 만든다.
    
    ```java
    Exception e = new Exception("고의로 발생시킴");
    ```
    
2. 키워드 `throw` 를 이용해서 예외를 발생시킨다.
    
    ```java
    throw e;
    ```
    

위와같이 임의로 예외를 발생시킬 수도 있고,

예외 되던지기 (exception re-throwing) 에도 사용된다.

이 방법은 **하나의 예외에 대해서 예외가 발생한 메서드와 이를 호출한 메서드 양쪽 모두에서 처리해줘야** 할 작업이 있을 때 사용되는 방법으로,

주의해야할 점은 예외가 발생할 메서드에서는 `try-catch문` 을 사용해서 예외처리를 해줌과 동시에 메서드의 선언부에 발생할 예외를 `throws` 에 지정해줘야 한다.

```java
public static void main(String[] args) {
    try {
        method1();
    } catch (Exception e) {
        System.out.println("main 에서 예외가 처리됨.");
    }
}

static void method1() throws Exception {
    try	{
        throw new Exception();
    } catch (Exception e) {
        System.out.println("method1 에서 예외가 처리됨.");
        throw e;
    }
}
```

```xml
method1 에서 예외가 처리됨.
main 에서 예외가 처리됨.
```

위와같이 예외를 처리한 후에 인위적으로 다시 발생시키는 방법을 **예외 되던지기(exception re-throwing)**이라고 한다.

### e.printStackTrace()

catch 블록에 잡힌 객체는 예외가 발생하기 까지의 이력을 프린트 해주는 메소드를 가지고 있는데 이것이 `printStackTrace();` 이다.

이 이력은 어떻게 알 수 있을까?

먼저, JVM 구조를 다시 보자.

![](/assets/images/java/jvm-structure.png)

메소드가 실행되면 메모리의 **스택(stack) 영역**에 **Stack Frame** 이 차곡 차곡 쌓이게 된다. 예를들어, 메소드1 에서 메소드2 를 호출한다면 그 위에 쌓이는 식으로 차곡차곡 쌓인다. 

스택 영역은 Stack Frame을 저장하는 영역으로, jvm은 스택에서 Stack Frame을 `push` 하고 `pop` 하는 작업을 한다.

즉, e.printStackTrace() 메서드는 메모리 영역의 스택에 저장되어 있는 Stack Frame을 `pop`  하여 출력하는 것이다.

### 예외처리비용

위 jvm 구조 그림에서 보았듯이 예외는 메모리 영역의 스택에 담기는것이기 때문에 비용이 든다. 그러므로, **무작정 예외를 던져서 처리하기 보다는 logic으로 처리 할 수 있는 경우(리턴값, 입력값 등..) 코드로 처리하는것이 좋다.**

## 사용자정의 예외 작성

기존의 정의된 예외 클래스 외에 필요에 따라 개발자가 새로운 예외 클래스를 작성하여 사용할 수 있다.

보통 `Exception` 클래스로부터 상속받는 클래스를 만들지만, 필요에 따라 알맞은 예외 클래스를 선택할 수 있다.

```java
public class MyException extends Exception{

    private final int ERR_CODE;

    public MyException(String msg, int err_code) {
        super(msg);
        ERR_CODE = err_code;
    }

    public MyException(String msg) {
        this(msg, 100);
    }

    public int getErrCode() {
        return ERR_CODE;
    }
}

public static void main(String[] args) {
    try {
        throw new MyException("custom exception occur");
    } catch (MyException e) {
        System.out.println(e.getErrCode());
        System.out.println(e.getMessage());
    }
}
```

```xml
100                       // error code
custom exception occur    // error msg
```

### custom exception 만들때 유의해야 할 점

1. **Always Provide a Benefit**
    
    이미 JDK가 제공하고 있는 다양한 장점을 가진 자바 표준 예외와 비교했을 때 본인이 만들고자 하는 커스텀 예외가 아무런 장점을 제공하지 못한다면, 커스텀 예외를 만드는 이유를 다시 생각해 보아야 한다. 
    
    장점을 제공할 수 없는 예외를 만드는 것 보다 오히려, `UnsupportedOprerationException` 또는, `IllegalArgumentException`과 같은 표준 예외 중 하나를 사용하는 것이 더 낫다.
    
2. **Follow the Naming Convention**
    
    JDK가 제공하는 예외 클래스들을 보면 클래스 이름이 모두 *Exception 으로 끝난다. 이러한 네이밍 규칙은 자바 생태계 전체에 사용되는 규칙으로 커스텀 예외 클래스도 이 네이밍 규칙을 따라야 한다. 
    
3. **Provide javadoc Comments for Your Exception Class**
    
    기본적으로 API의 모든 클래스, 멤버변수, 생성자들에 대해서는 문서화 하는 것이 일반적인 Best Practices로 문서화되지 않은 API는 사용하기가 어렵다. 
    
    예외처리에도 문서화가 필요하나 생각이 들 수 있겠지만, 클라이언트와 직접 관련된 메소드들 중 하나가 예외를 던지면 그 예외도 api의 일부가 되기 때문에 문서화가 필요하다.
    
    JavaDoc은 예외가 발생할 수도 있는 상황과 예외의 일반적인 의미를 기술한다. 목적은 다른 개발자들이 API를 이해하고 일반적인 에러 상황들을 피하도록 돕는 것이다. 
    
    ```java
    /**
     * The MyBusinessException wraps all checked standard Java exception and enriches them with a custom error code.
     * You can use this code to retrieve localized error messages and to link to our online documentation.
     * 
     * @author TJanssen
     */
    public class MyBusinessException extends Exception { ... }
    ```
    
4. **Provide a Constructor That Sets the Cause**
    
    커스텀 예외를 던질 때 표준 예외를 Catch 하는 경우 이 표준예외를 함께 던져주지 않으면 중요한 정보(오류를 분석하는데 도움이 되는)를 잃게된다.
    
    ```java
    public void wrapException(String input) throws MyBusinessException {
        try {
            // do something
        } catch (NumberFormatException e) {
            throw new MyBusinessException("A message that describes the error.", e, ErrorCode.INVALID_PORT_CONFIGURATION);
        }
    }
    ```
    
    NumberFormatException 은 표준예외이고 본인이 만든 예외를 던질 때 이 객체도 포함하여 던지라는 뜻이다.
    

[참고] [https://dzone.com/articles/implementing-custom-exceptions-in-java?fromrel=true](https://dzone.com/articles/implementing-custom-exceptions-in-java?fromrel=true)

## 예외의 전파

```java
try {
    doSomething();
    System.out.println("Normal statement.");
} finally {
    System.err.println("From finally block.");
}
```

만약, 위와 같은 코드에서 doSomething 메서드 수행시 Exception이 발생하면 어떻게 될까 ? (상위로 throw가 될까 skip이 되는 것일까?)

![](/assets/images/java/Exception-throw.png)

[https://docs.oracle.com/javase/tutorial/essential/exceptions/definition.html](https://docs.oracle.com/javase/tutorial/essential/exceptions/definition.html)

자바에서 Exception이 발생했을 경우 Exception Handler가 처리하게 되는데 , Exception Handler를 찾아 가는 방식이 위 그림과 같다. 

- 에러가 발생한 메소드에서 찾고 거기서도 없으면
- call stack 을 이용하여 역방향으로 Exception Handler를 탐색한다.
- 탐색하면서 Exception Handler가 처리할 수 있는 Exception 타입인지 체크하고 맞다면 처리하고, 아니라면 상위로 전달된다.

즉, 발생한 Exception에 대해서 처리가 가능한 Handler를 찾을 때까지 상위로 전달되면서 찾아가게 된다.

적절한 Handler를 찾지 못한다면 JVM까지 전달되며, 최종적으로 JVM이 Exception을 처리하게 된다. 

```java
public class App {
    public static void main(String[] args) {
        System.out.println("Main Start");
        App exceptionEx = new App();

        try {
            exceptionEx.doSomething();
        } catch (Exception e){
            System.out.println("Catch exception");
            e.printStackTrace();
        }

    }

    private void doSomething() {
        System.out.println("execute doSomething");
        try {
            occursException();
        } finally {
            System.out.println("doSomething finally block");
        }
    }

    private void occursException() {
        System.out.println("Exception occurs");
        String code = null;
        code.length();          // NullPointException occur
    }
}
```

```xml
Main Start
execute doSomething
Exception occurs
doSomething finally block
Catch exception

BUILD SUCCESSFUL in 428ms
2 actionable tasks: 2 executed
java.lang.NullPointerException
	at dispatch.App.occursException(App.java:29)
	at dispatch.App.doSomething(App.java:20)
	at dispatch.App.main(App.java:9)
```

occursException 에서 exception이 발생했지만 처리해줄 handler가 없으니까 main의 catch블럭에서 처리 해준 것을 볼 수 있다.

>💡 발생한 Exception은 적절한 catch 구문 (Exception Handler)를 만날때까지 상위로 전달되며, 최종적으로 JVM까지 전달되어 처리될 수 있다. 
그러므로 **예상가능한 Exception에 대해서는 꼭 catch구문을 통해 적절한 처리를 해야 한다.**