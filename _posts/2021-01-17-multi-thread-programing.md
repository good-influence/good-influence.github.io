---
layout: post
title: multi thread
subtitle: 멀티쓰레드 프로그래밍
categories: JAVA
tags: [JAVA, thread]
---
## 멀티쓰레드 프로그래밍

**프로세스(process)란 실행 중인 프로그램(program)이다.** 프로그램을 실행하면 OS로부터 실행에 필요한 자원(메모리)을 할당받아 프로세스가 된다.

쓰레드는 **프로세스의 자원을 이용해서 실제로 작업을 수행하는 것이 바로 쓰레드**이다. *(경량 프로세스 LWP, light-weight process 라고도 함)*

>💡 프로세스 : 운영체제로부터 자원을 할당받은 **작업**의 단위
💡쓰레드 : 프로세스가 할당받은 자원을 이용하는 **실행 흐름**의 단위

그래서, 모든 프로세스에는 최소한 하나 이상의 쓰레드가 존재하며, 둘 이상의 쓰레드를 가진 프로세스를 **멀티쓰레드 프로세스 (multi-threaded process)** 라고 한다.

- 싱글쓰레드 프로세스 = 자원 + thread
- 멀티쓰레드 프로세스 = 자원 + thread + thread + ...

하나의 프로세스가 가질 수 있는 쓰레드의 개수는 **프로세스의 메모리의 한계(호출스택의크기) 에따라** 생성할 수 있는 **쓰레드의 수가 결정**된다. (쓰레드가 작업을 수행하는데 호출스택(메모리공간)을 필요로 하기 때문)

### 멀티태스킹

OS의 스케쥴링에 의해 Task를 번갈아가며 수행하는 것을 의미한다.(Task: 운영체제에서 처리하는 작업의 단위)

윈도우나, 유닉스를 포함한 대부분의 OS는 멀티태스킹 (multi-tasking, 다중작업)을 지원한다.

그리하여 **여러개의 프로세스가 동시에 실행** 될 수 있는 것이다.

### 멀티쓰레딩

**하나의 프로세스 내에서 여러 쓰레드가 동시에 작업** 하는것으로, 실제로는 한 개의 CPU가 한 번에 단 한가지 작업만 수행할 수 있기 때문에 아주 짧은 시간 동안 여러 작업을 번갈아 가며 수행함으로써 동시에 여러 작업이 수행되는 것처럼 보이게 하는 것이다.

메신저의 경우, 채팅하면서 파일을 다운로드 받거나 음성대화를 나눌 수 있는 것이 가능한 이유가 바로 멀티쓰레드로 작성되어 있기 때문이다.

멀티쓰레딩의 장점

- CPU의 사용률을 향상시킨다
- 자원을 보다 효율적으로 사용할 수 있다.
- 사용자에 대한 응답성이 향상된다.
- 작업이 분리되어 코드가 간결해진다.

단점

- 서로 자원을 소모하다가 충돌 일어날 가능성 존재

### 쓰레드의 구현 방법

- Thread 클래스를 상속
    
    ```java
    class MyThread exthends Thread {
        public void run() {  // Thread 클래스의 run() 오버라이딩
            // 작업내용
        }
    }
    ```
    
    - 사용
    
    ```java
    MyThread myThread = new MyThread();
    myThread.start();
    ```
    
- Runnable 인터페이스를 구현
    
    ```java
    class MyThread implements Runnable {
        // Runnable 인터페이스의 추상메소드 run() 구현
        public void run() {   
            // 작업내용
        }
    }
    ```
    
    - 사용
    
    ```java
    Runnable r = new MyThread();
    Thread t = new Thread(r);
    t.start();
    ```
    
- 람다 사용
    
    ```java
    Thread thread = new Thread(() -> {
    		String name = Thread.currentThread().getName();
    		System.out.println(name);
    });
    thread.setName("Thread #1");
    thread.start();
    ```
    

주의할점은, **한 번 사용한 쓰레드는 다시 재사용할 수 없다**는 점이다. 즉, 하나의 쓰레드에 대해 `start()` 가 한 번만 호출 될 수 있다는 뜻이다. 만약 한 번 더 수행되기를 원한다면 새로운 쓰레드 인스턴스를 생성한 후에 호출해야 한다. 만일, 하나의 쓰레드에 대해 두 번 이상 `start()` 를 호출하게 되면 실행시에 `IllegalThreadStateException` 이 발생한다.

```java
// IllegalThreadStateException
MyThread t = new MyThread();
t.start();
t.start();
```

```java
MyThread t = new MyThread();
t.start();
t = new MyThread();
t.start();
```

어느 방법을 사용하든 별 차이는 없지만 Thread 클래스를 상속받으면 다른 클래스를 상속받을 수 없기 때문에, 일반적으로 `Runnable` 인터페이스를 구현하여 사용한다. Thread 클래스를 상속받아 사용할 때는 run() 메소드 외에 다른 것들을 **오버라이딩 할 필요가 있을 때** 사용한다.

### start() 와 run()

- run()
    
    run()을 호출 하는 것은 생성된 쓰레드를 실행시키는 것이 아니라 단순히 클래스에 속한 메서드 하나를 호출하는 것이다.
    
    ```java
    public class MyThread extends Thread{
        @Override
        public void run() {
            throwException();
        }

        private void throwException() {
            try {
                throw new Exception();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    ```
    
    ```java
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.run();
    }
    ```
    

![](/assets/images/java/multithread-result-1.png)

![](/assets/images/java/multithread-stack-1.jpg)

- start()
    
    새로운 쓰레드가 작업을 실행하는데 필요한 호출스택을 생성한 후 run()을 호출해서 생성된 호출 스택에 run()이 첫 번째로 저장되게 한다. 
    
    모든 쓰레드는 독립적인 작업을 수행하기 위해 자신만의 호출스택이 필요하기 때문에 새로운 쓰레드를 생성하고 실행시킬때마다 **새로운 호출스택이 생성되고 종료되면 호출스택은 소멸**된다.
    
    ```java
    public class MyThread extends Thread{
        @Override
        public void run() {
            throwException();
        }

        private void throwException() {
            try {
                throw new Exception();
            } catch (Exception e) {
                e.printStackTrace();
            }
        }
    }
    ```
    
    ```java
    public static void main(String[] args) {
        MyThread myThread = new MyThread();
        myThread.start();
    }
    ```
    
    ![](/assets/images/java/multithread-result-2.png)

    ![](/assets/images/java/multithread-stack-2.jpg)
    
    호출스택의 첫 번째 메서드가 main 메서드가 아닌 이유는 `start()` 를 사용하였기 때문이다. 
    한 쓰레드에서 예외가 발생해서 종료되어도 다른 쓰레드의 실행에는 영향을 미치지 않는다.
    
    - `start()` 실행 순서
    
    ![](/assets/images/java/multithread-api-process.png)
    
    1. main 메서드에서 쓰레드의 start를 호출한다.
    2. start메서드는 쓰레드가 작업을 수행하는데 사용될 새로운 호출스택을 생성한다
    3. 생성된 호출스택에 run 메서드를 호출해서 쓰레드가 작업을 수행하도록 한다.
    4. 호출스택이 2개이기 때문에 스케줄러가 정한 순서에 의해서 번갈아 가면서 실행된다.
    
    4번 그림처럼 쓰레드가 둘 이상일 경우에는 **호출스택의 최상위에 있는 메서드라도 대기상태에 있을 수 있다**. 스케줄러는 쓰레드의 우선순위를 고려하여 실행순서와 실행시간을 결정한다.
    
    **즉, start()가 호출된 쓰레드는 바로 실행되는 것이 아닐 수도 있다는 점을 알아야 한다.**
    
    주어진 시간동안 작업을 마치지 못한 쓰레드는 자신의 차례가 돌아올때까지 대기상태에 있게되며 작업을 마친 쓰레드는 호출스택이 모두 비워지면서 이 쓰레드가 사용하던 호출스택은 사라진다.
    
    만약, 메인 메서드가 있는 쓰레드가 종료된다면?
    
    - 다른 쓰레드가 작업을 마치지 않은 상태라면 프로그램은 종료되지 않는다.
    - **실행 중인 사용자쓰레드가 하나도 없을 때 프로그램은 종료된다.**

## Thread 우선순위

쓰레드클래스는 **priority(우선순위)** 라는 멤버변수를 가지고 있는데, 이 우선순위의 값에 따라 쓰레드가 얻는 **실행시간이 달라진다**.

즉, 수행하는 작업의 중요도에 따라 우선순위를 서로 다르게 지정하여 원하는 쓰레드가 더 많은 작업시간을 갖도록 조정할 수 있다.

- `void setPriority(int newPriority)` : 쓰레드의 우선순위를 지정된 값으로 변경
- `int getPriority()` : 쓰레드의 우선순위를 반환

쓰레드가 가질 수 있는 우선순위의 범위는 **1~10** 이며 숫자가 높을수록 우선순위가 높다. 그러나 상대적이므로 예를들어,  
t1 t2 우선순위를 1과 2로 설정하는 것과 9와 10으로 설정하는것은 같은 결과를 얻는다. 

프로세스에게 주어진 실행시간을 두 쓰레드에게 **어떠한 비율로 나누어 할당할 것인지는 쓰레드간의 우선순위 차이**에 의해서 결정된다.

```java
public static final int MAX_PRIORITY = 10  // 최대우선순위
public static final int MIN_PRIORITY = 1   // 최소우선순위
public static final int NORM_PRIORITY = 5  // 보통우선순위
```

또한, **쓰레드의 우선순위는 쓰레드를 생성한 쓰레드로부터 상속받는다.**

예를들어, main 메서드를 수행하는 쓰레드는 우선순위가 5이므로 main 메서드에서 생성하는 쓰레드의 우선순위는 자동적으로 5가된다. (설정 안할시)

## Thread State

기본적인 쓰레드는 다음 구조를 갖고 있다.

![](/assets/images/java/multithread-structure.png)


쓰레드 객체를 생성하고 `start()` 메소드를 호출하면 실행 대기 상태로 가고, 스케줄링으로 선택된 쓰레드가 CPU를 점유하고 `run()`메소드를 실행하는데 이를 Running 실행 상태라고 한다. 

JDK 1.5부터 `getState()` 메소드로 쓰레드의 상태를 가져올 수 있다. 

- 객체 생성
    - **NEW :** 쓰레드 객체가 생성 아직 `start()` 메소드가 호출되지 않은 상태
- 실행 대기
    - **RUNNABLE :** 실행 중 또는 실행 가능한 상태
- 일시 정지
    - **WAITING** : 쓰레드의 작업이 종료되지는 않았지만 실행가능하지 않은(unrunnable) 일시정지 상태
    - **TIMED_WAITING** : 일시정지시간이 지정된 경우
    - **BLOCKED** : 동기화블럭에 의해서 일시정지된 상태 (Lock이 풀릴 때까지 기다리는 상태)
- 종료
    - **TERMINATED** : 쓰레드의 작업이 종료된 상태

![](/assets/images/java/multithread-structure-2.png)

1. 쓰레드를 생성하고 `start()` 를 호출하면 실행 대기열에 저장되어 차례를 기다린다. 
큐와 같은 구조로 먼저 실행대기열에 들어온 쓰레드가 먼저 실행된다.
2. 실행대기상태에 있다가 자신의 차례가 되면 실행상태가 된다.
3. 주어진 실행시간이 다되거나 `yield()` 를 만나면 다시 실행대기상태가 되고 다음 차례의 쓰레드가 실행상태가 된다.
4. 실행중에 `suspend()`, `sleep()`, `wait()`, `join()`, `I/O block`에 의해 일시정지상태가 될 수 있다. 
    - I/O block : 입출력작업에서 발생하는 지연상태
5. 지정된 일시정지시간이 다 되거나 (`time-out`), `notify()`, `resume()`, `interrupt()`가 호출되면 일시정지 상태를 벗어나 다시 실행대기열에 저장되어 차례를 기다리게 된다.
6. 실행을 모두 마치거나 `stop()` 이 호출되면 쓰레드는 소멸된다.
    - `stop()` 은 현재 deprecated 되어 다음 방법으로 종료를 구현해야 한다.
        - flag 방식
        - Interrupt 방식
            - `interrupt()` 를 사용하면 InterruptedException을 발생시켜서 sleep(), join(), wait()에 의해 일시정지 상태인 쓰레드를 실행대기상태로 만든다.
            
            ```java
            public class MyThreadRunnable implements Runnable {
            
                @Override
                public void run() {
                    while (true) {
                        try {
                            System.out.println("Thread is running");
                            Thread.sleep(1000);
                        } catch (InterruptedException e) {
                            System.out.println("Thread was interrupted");
                            System.out.println("Release some resources");
                            System.out.println("Done");
                            return;
                        }
                    }
                }
            }
            ```
            
            ```java
            Thread thread = new Thread(new MyThreadRunnable());
            thread.start();
            Thread.sleep(5000);
            System.out.println("isInterrupted() = " + thread.isInterrupted());
            System.out.println("Interrupt this thread");
            thread.interrupt();
            System.out.println("isInterrupted() = " + thread.isInterrupted());
            ```
            
            ![](/assets/images/java/multithread-result-3.png)
            

### Thread 상태 제어

쓰레드의 상태는 메서드를 통해 제어할 수 있다.

- `void interrupt()` : `sleep()` 이나 `join()` 에 의해 일시정지상태인 쓰레드를 실행대기상태로 만든다. 해당 쓰레드에서는 InterruptedException이 발생하면서 일시정지상태를 벗어나게 된다.
- `static boolean interrupted()` : 현재 쓰레드가 중지되었는지를 확인하여 준다.
- `void join()`, `void join(long millis)`, `void join(long millis, int nanos)`
지정된 시간동안 쓰레드가 실행되도록 함, 지정된 시간이 지나거나 작업이 종료되면 join()을 호출한 쓰레드로 다시 돌아와 실행을 계속한다.
- `void resume` : `suspend()` 에 의해 일시정지상태에 있는 쓰레드를 실행대기상태로 만든다.
    - 교착상태를 일으킨 가능성이 높아 deprecated 됨
    - `notify()` 를 사용하여야 함
- `static void sleep(long mills)`, `static void sleep(long mills, int nanos)` :
지정된 시간 동안 쓰레드를 일시정지시킨다. 지정한 시간이 지나고 나면, 자동적으로 다시 실행대기상태가 된다.   
    - **실행중인 쓰레드에 대해서만 동작한다. → 참조변수로 sleep 메서드 호출 하면 안됨.**
    - sleep 사용할 때 예외를 처리해주어야 하는데 예외를 잡아도 개발자가 해줄 수 있는 처리가 `throw new RuntimeException(e)` 하는 방법정도밖에 없기 때문에 롬복의 `@sneakyThrows` 를 사용하면 코드를 줄일 수 있다.
- `void stop()` : 쓰레드를 즉시 종료시키는데 교착상태(dead-lock)에 빠지기 쉽기 때문에 **deprecated 되었다.**
- `void suspend()` : 쓰레드를 일시정지시킨다. `resume()`을 호출하면 다시 실행대기상태가 된다.
    - 교착상태를 일으킨 가능성이 높아 deprecated 됨
    - `wait()` 을 사용하여야 함
- `static void yield()` : 실행 중에 다른 쓰레드에게 양보(yield)하고 실행대기상태가 된다.
- `void checkAccess()` : 현재 수행중인 쓰레드가 해당 쓰레드를 수정할 수 있는 권한이 있는지 확인한다. 만약 권한이 없다면 SecurityException 발생
- `boolean isAlive()` : 쓰레드가 살아있는지를 확인한다. 해당 쓰레드의 run()메소드가 종료 여부를 확인
- `boolean isInterrupted()` : run() 메소드가 정상적으로 종료되지 않고, interrupt() 메소드의 호출을 통해서 종료되었는지를 확인하는데 사용한다.

### Thread static 메소드

- `static int activeCount()` : 현재 쓰레드가 속한 쓰레드 그룹의 쓰레드 중 살아 있는 쓰레드의 개수를 리턴
- `static Thread currentThread()` : 현재 수행중인 쓰레드의 객체를 리턴
- `static void dumpStack()` : 현재 쓰레드의 스택 정보를 출력

## wait() 와 notify()

`wait()`와 `notify()`를 적절히 사용하면 더 효율적인 작업을 수행할 수 있다. 

`wait()`를 호출해서 다른 쓰레드에게 제어권을 넘겨주고, 다른 쓰레드에 의해서 `notify()`가 호출되면 다시 실행상태로 되도록 하는것이다. 

또한, 이 메서드들은 Thread 클래스가 아닌 **Object 클래스에 정의된 메서드 이므로 모든 객체에서 호출이 가능하다.**

쓰레드가 `wait()` 을 호출하면 자신이 객체에 걸어 놓았던 모든 lock을 풀고, `wait()`이 호출된 객체의 waiting pool 에서 기다리게 된다. 그러다가 다른 쓰레드에 의해서 그 객체에 대해 `notify()` 를 호출하면 객체의 waiting pool 을 벗어나서 다시 실행대기상태로 전환된다. (자신이 실행될 차례를 기다리는 상태)

`notify()` 와 `notifyAll()` 은 waiting pool 안에 있는 쓰레드 중 하나만을 깨우거나 모든 쓰레드를 깨운다는 차이점만 있다. 그러나 notify()에 의해 어떤 쓰레드가 깨워지게 될지는 알 수 없어서 우선순위가 높은 특정 쓰레드가 오랫동안 객체의 waiting pool 에 머물 수 있기 때문에 모든 쓰레드를 깨워놓고 JVM의 쓰레드 스케줄링에 의해서 처리되도록 하는 것이 안전하다. 

또한 waiting pool은 객체마다 존재하는 것이기 때문에 notify를 호출한다고 모든 객체의 waiting pool에 있는 쓰레드들이 깨워지는것은 아니다. 

>💡 `wait()` 와 `notify()`
- Object 에 정의되어 있다.
- 동기화 블록 (synchronized 블록) 내에서만 사용할 수 있다.
- 보다 효율적인 동기화를 가능하게 한다.


## Main 쓰레드

- Java 는 **JVM(Java Virtual Machine)** 에서 돌아가는데, 이것은 하나의 프로세스고, **모든 자바 어플리케이션은 반드시 하나의 메인쓰레드를 가진다**.
- 메인쓰레드가 `public static void main(String[] args){}` 를 실행하면서 시작된다.  (메인쓰레드의 시작점)
    - 메인쓰레드 외 별도의 쓰레드를 실행하지 않고 `main()` 메소드만 실행하는 것을 싱글쓰레드 어플리케이션이라함.
        
        ![](/assets/images/java/multithread-main-thread.png)
        
- `main()` 메소드의 마지막 코드를 실행하거나 return 문을 만나면 종료된다.
- 메인쓰레드는 여러 개의 작업 쓰레드를 생성하여 병렬로 코드를 실행 할 수 있다.
    - 멀티쓰레드 어플리케이션은 메인쓰레드가 종료되더라도 아직 실행 중인 다른 쓰레드가 하나라도 있으면 프로세스를 종료시키지 않는다.

## 데몬쓰레드 (daemon thread)

데몬쓰레드가 아닌 일반 쓰레드의 작업을 돕는 **보조적인 역할을 수행**하는 쓰레드이다. 

**일반스레드가 모두 종료되면 데몬쓰레드는 강제적으로 자동 종료**되는데, 데몬쓰레드는 일반 쓰레드의 보조역할을 수행하므로 일반쓰레드가 모두 종료되고 나면 존재 의미가 없기 때문이다. 
자동 종료된다는 점을 제외하면 일반쓰레드와 다르지 않다.

- 사용 예제
    - 가비지 컬렉터
    - 워드프로세서의 자동저장
    - 화면 자동갱신

## 동기화

멀티쓰레드 프로세스의 경우 여러 쓰레드가 같은 프로세스 내의 **자원을 공유해서 작업하기 때문에** 서로의 작업에 영향을 주게 된다.

예를들어, A쓰레드가 작업하던 도중에 B쓰레드에게 제어권이 넘어갔을때 A가 작업하던 공유데이터를 B가 변경할수 있기 때문에 **lock을 걸어야 한다.**

### synchronized 이용한 동기화

키워드 `synchronized` 를 사용하면 해당 작업과 관련된 공유데이터에 lock을 걸어 다른 쓰레드에게 제어권이 넘어가더라도 데이터가 변경되지 않도록 보호함으로써 쓰레드의 동기화를 가능하게 한다.

- 특정한 객체에 lock을 걸고자 할 때

```java
synchronized(객체의 참조변수) {
    // ...
}
```

`synchronized` **블럭의 시작부터 lock이 걸렸다가 블록이 끝나면 lock이 풀린다.**

이 블록을 수행하는 동안에는 지정된 객체에 lock이 걸려서 다른 쓰레드가 **이 객체에 접근할 수 없게 된다.**

- 메서드에 lock을 걸고자 할 때

```java
public synchronized void method() {
    // ...
}
```

한 쓰레드가 `synchronized` 메서드를 호출해서 수행하고 있으면, 이 메서드가 종료될 때까지 다른 쓰레드가 이 메서드를 호출하여 수행할 수 없게 된다.

예를들어, Account (계좌)에서 잔고(balance)를 확인하고 임의의 금액을 출금(withdraw) 하는 예제 코드를 작성한다 했을때,

```java
public class Account {
    int balance = 1000;

    public void withdraw(int money) {
        if (balance >= money) {
            try {
                Thread.sleep(1000);
            } catch (Exception e) {
            }
            balance -= money;
        }
    }
}
public class MyThreadRunnable implements Runnable {
	Account account = new Account();
	@Override
	public void run() {
		while (account.balance > 0) {
			int money = (int) (Math.random() * 3 + 1) * 100;
			account.withdraw(money);
			System.out.println("balance : " + account.balance);
		}
	}
}

public class App {
	public static void main(String[] args) {
		MyThreadRunnable r = new MyThreadRunnable();
		Thread t1 = new Thread(r);
		Thread t2 = new Thread(r);
		t1.start();
		t2.start();
	}
}
```

위 코드를 상상으로 분석해보면 balance는 절대 음수가 나올것 같지 않다. (출금하려는 금액보다 큰 경우에만 출금하게끔 짜여있기 때문에) **그러나 결과를 보면** 

![](/assets/images/java/multithread-result-4.png)

잔고가 어떻게 음수가 나올 수 있었을까? 
한 쓰레드가 if문의 조건식을 통과하고 출금하기(withdraw) 바로 직전에 다른쓰레드에게 제어권이 넘어가서 다른 쓰레드가 출금을 먼저 했기 때문이다.

이와같은 사태를 막기 위해서는 잔고를 확인하는 조건문과 출금하는 문장은 하나로 **동기화 블록으로 묶여져야 한다.** 

```java
public class Account {
    int balance = 1000;

    public synchronized void withdraw(int money) {
        if (balance >= money) {
            try {
                Thread.sleep(1000);
            } catch (Exception e) {
            }
            balance -= money;
        }
    }
}
```

![](/assets/images/java/multithread-result-5.png)

한 쓰레드가 synchronized 블럭에 들어가면 Account 객체 전체에 lock이 걸려서 블럭을 벗어날 때까지 다른 쓰레드는 이 객체에 접근할 수 없다. 

## 데드락 (교착상태, Deadlock)

![](/assets/images/java/multithread-deadlock.png)

[https://coding-factory.tistory.com/311](https://coding-factory.tistory.com/311)

**두 개 이상의 프로세스가 더 이상 진행을 할 수 없는 상태**로, 둘 이상의 프로세스들이 자원을 점유한 상태에서 서로 다른 프로세스가 점유하고 있는 자원을 요구하며 무한정 기다리는 현상이다. 

예를들어,  A가 ◐ , ◑ 를 사용해서 작업을 해야한다. ◑를 가진 B의 작업이 끝날때까지 기다리는데, B는 ◐ 가 작업중에 필요한상황이와서 A가 ◐를 주기를 기다리는 상태에 빠져 서로 무한정 기다리게 되는 현상이다.

교착상태 발생할 수 있는 조건

- 교착상태가 발생하기 위해서는 다음 네가지 조건이 충족되어야 한다.
- 네가지 조건중 하나라도 충족되지 않으면 교착상태가 발생하지 않는다.
1. **상호배제**  
자원은 한 번에 한 프로세스만이 사용할 수 있어야한다.
2. **점유대기**  
최소한 하나의 자원을 점유하고 있으면서 다른 프로세스에 할당되어 사용하고 있는 자원을 추가로 점유하기 위해 대기하는 프로세스가 있어야한다.
3. **비선점**  
다른 프로세스에 할당된 자원은 사용이 끝날 때까지 강제로 빼앗을 수 없어야 한다.
4. **순환대기**  
공유자원과 공유자원을 사용하기 위해 대기하는 프로세스들이 원형으로 구성되어 있어 자신에게 할당된 자원을 점유하면서 앞이나 뒤에 있는 프로세스의 자원을 요구해야 한다.

<aside>
💡 두 쓰레드가 lock을 건 상태에서 서로 lock을 풀리기를 기다리는 상황

</aside>

## 동시성(concurrency) vs 병렬성(parallelism)

|동시성                      | 병렬성                                           |
|:---------------------------| :---------------------------------------------- |
| 동시에 실행되는 것 같아 보이는 것 | 실제로 동시에 여러 작업이 처리되는 것 |  
| 싱글 코어에서 멀티쓰레드를 동작시키는방식 | 멀티코어에서 멀티쓰레드를 동작시키는 방식 |  
| 한번에 많은 것을 처리 | InputStream에서 현재 위치를 표시 해줌 |
| 논리적인 개념  | 물리적인 개념  |



![](/assets/images/java/multithread-concurrency-parallelism.png)

[https://seamless.tistory.com/42](https://seamless.tistory.com/42)

싱글코어에서는 2개의 작업을 동시에 실행하는 것처럼 보이기 위해 번갈아가며 작업을 수행하는데, 다른작업으로 바꾸어 실행할 때 내부적으로 Context Switch 가 일어난다.

- critical path
    
    ![](/assets/images/java/multithread-cirticalpath.png)
    
    - 하얀색이 critical path 임
        - 동시에 실행하는 것 중에 가장 긴 시간이 걸리는 작업 즉, 전체 실행시간에 영향을 미치는 작업임
            - 이것을 개선해야 함.
            

---