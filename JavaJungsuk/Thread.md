# Contents of 쓰레드 (13장)0
- [프로세스와 쓰레드](#프로세스와-쓰레드)
  - [멀티쓰레딩의 장단점](#멀티쓰레딩의-장단점)   
- [쓰레드의 구현과 실행](#쓰레드의-구현과-실행)
  - [쓰레드의 실행 - start()](#쓰레드의-실행---start--)
  - [start()와 run()](#start---와-run--)

    
출처 : [https://github.com/expert0226/oopinspring](https://github.com/castello/javajungsuk_basic)

## 프로세스와 쓰레드
프로세스란 **실행중인 프로그램**이다. 프로그램을 실행하면 OS로부터 실행에 필요한 자원(메모리)을 할당받아 프로세스가 된다.
![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/be838a93-1ff3-4e32-98c0-2d56d5c73176)
프로세스는 프로그램을 수행하는 데 필요한 자원(데이터 및 메모리) 그리고 쓰레드로 구성된다. 프로세스의 자원을 이용해서 **실제로 작업을 수행하는 것이 쓰레드**이다.
**모든 프로세스에는 최소 한 개의 쓰레드가 존재하며, 둘 이상의 쓰레드를 가진 프로세스를 멀티쓰레드 프로세스**라고 한다.
> 프로세스가 하나의 작업공간이라면, 쓰레드는 그 작업공간에서 일하는 일꾼이다.

### 멀티쓰레딩의 장단점
1. 장점
  - CPU의 사용률을 향상 시킨다. (다중작업을 진행하기 때문에 CPU가 노는 시간이 적어진다.)
  - 자원을 보다 효율적으로 사용할 수 있다. (위와 동일)
  - 사용자에 대한 응답성이 향상된다.
  - 작업이 분리되어 코드가 간결해진다.
2. 단점
  - 동기화 및 교착상태 같은 문제들이 발생한다. (여러 쓰레드가 같은 프로세스 내에서 자원을 공유하면서 작업을 하기 때문에 발생한다.)

우리가 PC를 통해 메신저로 채팅하면서 파일을 다운로드 받거나 음성대화를 나눌 수 있는 것이 가능한 이유가 바로 멀티쓰레드로 작성되어 있기 떄문이다.
만약, 싱글쓰레드로 작성되어 있다면 파일을 다운로드 받는 동안에는 다른 일(채팅)을 전혀 할 수 없을 것이다.  
**여러 사용자에게 서비스를 해주는 서버 프로그램의 경우 멀티쓰레드로 작성하는 것은 필수적이다. 하나의 서버 프로세스가 여러 개의 쓰레드를 생성해서
쓰레드와 사용자의 요청이 일대일로 처리되도록 프로그래밍해야 한다.** 만약 싱글쓰레드로 서버 프로그램을 작성한다면 사용자의 요청마다 **새로운 프로세스를
생성해야 한다.** 프로세르를 생성하는 것은 쓰레드를 생성하는 것에 비해 더 많은 시간과 메모리 공간이 필요하기 때문에 많은 수의 사용자 요청을 서비스하기 어렵다.
> 쓰레드를 가벼운 프로세스, 경량 프로세스(LWP, light-weight process)라고 부르기도 한다.

## 쓰레드의 구현과 실행
자바에서 쓰레드를 구현하는 방법은 크게 두 가지가 있다.
1. **Thread 클래스를 상속받는다.**
   - 자바는 다중상속이 안되기 때문에, Thread 클래스를 상속받을 경우 다른 클래스를 상속받을 수 없다.
   - Thread 클래스의 `run()`을 오버라이딩
2. **Runnable 인터페이스를 구현한다.**
   - 인터페이스를 구현하는 것은 **재사용성이 높고 코드의 일관성을 유지할 수 있기 때문에 보다 객체지향적인 방법이다.**
   - Runnable 인터페이스의 `run()`을 구현
**두 방법의 공통점은 쓰레드를 통해 작업하고자 하는 것을 `run()`을 통해 구현하는 것이다.**

```java
public class EX13_1 {

    public static void main(String[] args) {
        ThreadEx1_1 t1 = new ThreadEx1_1();

        Runnable r = new ThreadEx1_2();
        Thread t2 = new Thread(r); // Runnable 인터페이스 객체를 생성자의 매개변수 사용
        // Thread t2 = new Thread(new ThreadEx1_2());

        t2.start();
        t1.start();
    }
}

class ThreadEx1_1 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            System.out.println("나는 t1 " + getName()); // 상위 클래스인 Thread의 getName() 호출
        }
    }
}

class ThreadEx1_2 implements Runnable {

    @Override
    public void run() {
        for (int i = 0; i < 5; i++) {
            // Thread.currentThread() - 현재 실행 중인 Thread를 반환한다.
            System.out.println("나는 t2 " + Thread.currentThread().getName());
        }
    }
}


/// 결과 화면
나는 t2 Thread-1
나는 t2 Thread-1
나는 t2 Thread-1
나는 t2 Thread-1
나는 t2 Thread-1
나는 t1 Thread-0
나는 t1 Thread-0
나는 t1 Thread-0
나는 t1 Thread-0
나는 t1 Thread-0
```
Thread 클래스를 상속받는 방법과 Runnable 인터페이스를 구현하는 방법은 쓰레드 인스턴스 생성방법이 다르다.  
- Thread 클래스를 상속받은 경우, 상속받은 하위 클래스의 인터페이스로 바로 쓰레드를 생성할 수 있다.
- Runnable 인터페이스를 구현하는 경우, Runnable을 구현한 클래스의 인스턴스를 생성한 다음 Thread 생성자의 매개변수에 집어넣어 쓰레드를 생성할 수 있다.
  - 물론, 하나의 코드로 축약해서 생성할 수도 있다. (코드 참고)

- `static Thread currentThread()` : 현재 실행중인 쓰레드의 참조를 반환한다. 해당 참조를 통해 쓰레드의 메서드인 `getName()` 호출가능
- `String getName()` : 쓰레드의 이름을 반환한다.

### 쓰레드의 실행 - start()
쓰레드를 생성했다고 해서 자동으로 실행되는 것은 아니다. `start()`를 호출해야만 쓰레드가 실행된다. `start()`가 호출되었다고 해서 바로 실행되는 것이 아니라, **일단 실행대기 상태에 있다가
자신의 차례가 되어야 실행된다.** 물론 실행대기 중인 쓰레드가 하나도 없으면 곧바로 실행상태가 된다. (쓰레드의 실행순서는 OS의 스케쥴러가 작성한 스케쥴에 의해 결정된다.)

**한 번 실행이 종료된 쓰레드는 다시 실행할 수 없다는 것이 핵심이다. 즉, 하나의 쓰레드에 대해 `start()`가 한 번만 호출될 수 있다는 뜻이다.**
```java
public static void main(String[] args) {
        ThreadEx1_1 t1 = new ThreadEx1_1();

        t1.start();

        t1.start();
    }

/// 결과 화면
Exception in thread "main" java.lang.IllegalThreadStateException ----------------> IllegalThreadStateException 발생
	at java.base/java.lang.Thread.start(Thread.java:794)
	at thread.EX13_1.main(EX13_1.java:10)
나는 t1 Thread-0
나는 t1 Thread-0
나는 t1 Thread-0
나는 t1 Thread-0
나는 t1 Thread-0
```
```java
 public static void main(String[] args) {
        ThreadEx1_1 t1 = new ThreadEx1_1();
        t1.start();

        t1 = new ThreadEx1_1(); // 새로운 쓰레드 생성
        t1.start();
    }

/// 결과 화면
나는 t1 Thread-1
나는 t1 Thread-1
나는 t1 Thread-0
나는 t1 Thread-1
나는 t1 Thread-0
나는 t1 Thread-0
나는 t1 Thread-0
나는 t1 Thread-0
나는 t1 Thread-1
나는 t1 Thread-1
-> 위 결과는 실행마다 매번 달라진다 -> OS의 스케쥴러에 따라 Thread의 실행 순서 및 시간이 결정됨.
```

### start()와 run()
쓰레드를 실행시킬 때 `run()`이 아닌 `start()`를 호출해야 한다는 점에 주의해야 한다. `start()`와 `run()`에는 분명한 차이점이 존재하기 때문이다.
main 메서드에서 `run()`을 호출하는 것은 단순히 구현된 메서드를 호출하는 것에 불과하다. 하지만 `start()`는 새로운 쓰레드가 작업을 실행하는 데 필요한 호출스택(call stack)을 생성한다.
그 다음 `run()`을 호출해서, 생성된 호출스택에 `run()`이 올가가게 한다.  
모든 쓰레드는 독립적인 작업을 수행하기 위해 자신만의 호출스택을 필요로 하기 때문에, 새로운 쓰레드를 생성하고 실행시킬 때마다 새로운 호출스택이 생성되고 쓰레드가 종료되면 작업에 사용된 호출스택은 소멸된다.
![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/e829e419-858a-44f3-9ef4-ff7bf521db95)  
(출처 : https://joont92.github.io/java/%EC%93%B0%EB%A0%88%EB%93%9C-%EA%B8%B0%EB%B3%B8/)

**main 메서드의 작업을 수행하는 것도 쓰레드이다.** 이를 main 쓰레드라고 한다. 프로그램이 실행되기 위해서는 적어도 하나의 쓰레드가 필요하다. 따라서 main 메서드를 통해 프로그램을 실행시키는 것도 쓰레드의 역할 덕분인 것이다.
프로그램을 실행하면 기본적으로 하나의 쓰레드를 생성하고, 그 쓰레드가 main 메서드를 호출해서 작업이 수행되도록 하는 것이다.
![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/92234009-f0aa-4f9f-86c3-1effbfc3a09b)

쓰레드의 `start()`는 main 메서드 내에서 실행되기 때문에 main 쓰레드의 호출스택에 올라간다. 그리고 `start()`에 의해 생성된 또 다른 호출스택에 새로운 쓰레드의 `run()`이 올라가게 되는 것이다.  

main 메서드가 끝나더라도 아직 작업을 마치지 않은 쓰레드가 존재한다면 **프로그램은 종료되지 않는다. 즉, 실행 중인 사용자 쓰레드가 하나도 없을 때 프로그램은 종료된다.**




