# Contents of 쓰레드 (13장)
- [프로세스와 쓰레드](#프로세스와-쓰레드)
  - [멀티쓰레딩의 장단점](#멀티쓰레딩의-장단점)   
- [쓰레드의 구현과 실행](#쓰레드의-구현과-실행)
  - [쓰레드의 실행 - start()](#쓰레드의-실행---start--)
  - [start()와 run()](#start---와-run--)
  - [싱글쓰레드와 멀티쓰레드](#싱글쓰레드와-멀티쓰레드)
  - [쓰레드의 I/O 블락킹](#쓰레드의-io-블락킹)
- [쓰레드의 우선순위](#쓰레드의-우선순위)
- [쓰레드 그룹](#쓰레드-그룹)
- [데몬 쓰레드](#데몬-쓰레드)
- [쓰레드의 상태](#쓰레드의-상태)
- [쓰레드의 실행제어](#쓰레드의-실행제어)
  - [sleep()](#sleep--)
  - [interrupt()](#interrupt--)
  - [join() & yield()](#join--과-yield--)
- [쓰레드의 동기화](#쓰레드의-동기화)
  - [synchronized를 이용한 동기화](#synchronized를-이용한-동기화)
  - [wait() & notify()](#wait--과-notify--)
  

    
출처 : [https://github.com/castello/javajungsuk_basic](https://github.com/castello/javajungsuk_basic)

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

### 싱글쓰레드와 멀티쓰레드
하나의 쓰레드로 두 개의 작업을 하는 경우와 두 개의 쓰레드로 두 개의 작업을 번갈아 하는 경우는 분명 차이가 있다. 전자의 경우, 하나의 작업을 마치고 다음 작업을 할 것이다.
하지만 후자의 경우, 두 쓰레드가 각각 하나의 작업을 번갈아 가면서 수행하고 이 간격은 매우 짧기 때문에 동시에 두 작업이 처리되는 것처럼 보일 것이다.
![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/4972f562-1918-427b-8322-320e329f992e)

그림에서 알 수 있듯이 두 케이스의 작업 종료 시간은 거의 동일하다. 오히려 두 개의 쓰레드를 사용했을 때 시간이 조금 더 걸린다. 왜냐하면 쓰레드 간의 Context Switching(문맥 교환)이 일어나는 데 시간이 소요되기 때문이다.
> Context Switching의 경우, 실행해야 할 다음 위치(PC, 프로그램 카운터) 등을 저장하고 읽는 데 시간이 소요된다. 쓰레드의 스위칭보다 프로세스의 스위칭이 더 많은 정보를 저장하기 때문에 더 많은 시간이 소요된다.

**따라서 단순 CPU만을 사용하는 계산 작업일 경우, 멀티쓰레드보다 싱글쓰레드가 더 효율적일 수 있다. 멀티 쓰레드가 무조건 정답은 아니라는 뜻이다!**

두 개의 쓰레드를 통해 연산하는 작업을 했을 때, 실행마다 결과가 다르게 나올 수 있다. 그 이유는 JVM의 쓰레드 스케줄러에 의해서 각 쓰레드가 실행되는 시간이 무작위로 결정되기 때문이다.
또한 JVM이 실행되는 프로세스에 대한 점유도 OS에서 결정하기 때문에 이 또한 변수가 된다. 따라서 **자바는 OS에 독립적인 언어이지만 쓰레드 부분은 OS에 종속적이다.**

### 쓰레드의 I/O 블락킹
두 쓰레드가 서로 다른 자원을 사용하는 작업의 경우에는 싱글쓰레드 프로세스보다 멀티쓰레드 프로세스가 더 효율적이다. 즉, 사용자로부터 데이터를 입력받는 작업, 네트워크로 파일을 주고받는 작업,
프린터로 파일을 출력하는 작업 등 외부기기와의 입출력을 필요로 하는 경우가 대표적인 예이다. 만약 서로 다른 두 작업을 하나의 쓰레드로 처리한다면 하나의 작업이 끝날 때 까지 다른 작업을 하지 않고
기다려야 한다. (ex. 입력받는 작업 - 입력을 받을 때 까지 계속 기다려야 한다.)
> 쓰레드가 입출력(I/O) 처리를 위해 기다리는 것을 I/O 블락킹이라고 한다.

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/1dad137c-ae79-4a73-88c8-14c55461323c)

하지만 두 개의 쓰레드로 처리한다면 사용자의 입력을 기다리는 동안 다른 쓰레드가 작업을 처리할 수 있기 때문에 효율적으로 CPU를 사용할 수 있다.
**즉, 서로 다른 자원을 사용하는 작업을 할 경우 싱글 쓰레드보다 멀티 쓰레드가 더 작업을 빨리 마친다는 뜻이다.**

```java
public class Ex13_4 {
    public static void main(String[] args) {
        String input = JOptionPane.showInputDialog("아무 값이나 입력하세요.");
        System.out.println("입력하신 값은 " + input + "입니다.");

        for (int i = 10; i > 0; i--) {
            System.out.println(i);
            try {
                Thread.sleep(1000);
            } catch (Exception e) {
                
            }
        }
    }
}
```
위 예제의 경우, 하나의 쓰레드로 값을 입력받는 작업과 숫자를 출력하는 작업을 처리하고 있다. 따라서 값을 입력받을 때까지 값을 출력하지 않는다. 그 이유는 단일 쓰레드로 작업이 되기 때문이다.
```java
public class Ex13_4 {
    public static void main(String[] args) {
        ThreadPrac threadPrac = new ThreadPrac();
        threadPrac.start();
        
        String input = JOptionPane.showInputDialog("아무 값이나 입력하세요.");
        System.out.println("입력하신 값은 " + input + "입니다.");

        for (int i = 10; i > 0; i--) {
            System.out.println(i);
            try {
                Thread.sleep(1000);
            } catch (Exception e) {

            }
        }
    }
}

class ThreadPrac extends Thread {
    @Override
    public void run() {
        for (int i = 10; i > 0; i--) {
            System.out.println("나는 다른 쓰레드임 : " + i);
            try {
                Thread.sleep(1000);
            } catch (Exception e) {

            }
        }
    }
}

// 실행 결과
나는 다른 쓰레드임 : 10
나는 다른 쓰레드임 : 9
나는 다른 쓰레드임 : 8
입력하신 값은 66입니다.
10
나는 다른 쓰레드임 : 7
9
나는 다른 쓰레드임 : 6
8
나는 다른 쓰레드임 : 5
7
나는 다른 쓰레드임 : 4
6
나는 다른 쓰레드임 : 3
5
나는 다른 쓰레드임 : 2
4
나는 다른 쓰레드임 : 1
3
2
1
```
이처럼 다른 쓰레드를 생성한 뒤, 실행시키면 입력받는 작업과는 별개로 새로 생성한 쓰레드가 작업을 진행한다. 그리고 기존에 main 메서드에 있는 출력문은 입력받는 작업이 다 끝나야 출력을 시작한다.

### 쓰레드의 우선순위
쓰레드는 우선순위라는 속성(멤버 변수)을 가지고 있는데, 이 우선순위 값에 따라 쓰레드가 얻는 실행시간이 달라진다. 쓰레드가 수행하는 작업의 중요도에 따라 우선순위를 결정하여 작업시간을 결정할 수 있다.
**사용자에게 빠르게 반응해야 하는 작업을 하는 쓰레드는 우선순위가 비교적 높아야 할 것이다.**
```java
class Thread implements Runnable {
    /* Make sure registerNatives is the first thing <clinit> does. */
    private static native void registerNatives();
    static {
        registerNatives();
    }

    private volatile String name;
    // 자바의 쓰레드는 우선순위를 멤버로 갖는다. 
    private int priority;

    /* Whether or not the thread is a daemon thread. */
    private boolean daemon = false;
	...
}
```
- `void setPriority(int newPriority)` : 쓰레드의 우선순위를 지정한 값으로 변경한다.
- `int getPriority()` : 쓰레드의 우선순위를 반환한다.
```java
    /**
     * The minimum priority that a thread can have.
     */
    public static final int MIN_PRIORITY = 1;

   /**
     * The default priority that is assigned to a thread.
     */
    public static final int NORM_PRIORITY = 5;

    /**
     * The maximum priority that a thread can have.
     */
    public static final int MAX_PRIORITY = 10;	// 숫자가 높을수록 우선순위가 높다.
```
- main 메서드를 수행하는 쓰레드의 경우, 우선순위가 5이다. 따라서 main 메서드 내에서 생성하는 쓰레드는 우선순위가 자동적으로 5가 된다.

쓰레드의 우선순위는 해당 OS의 스케쥴링에 따라서 그리고 멀티 코어 환경의 경우, 제대로 반영이 안될 수도 있다. 쓰레드는 OS에 종속적인 요소이기 때문에 예측하기가 어렵다. 매 실행마다 달라질 수 있기 때문이다.

### 쓰레드 그룹
쓰레드 그룹은 서로 관련된 쓰레드를 그룹으로 다루기 위한 기능이다. 쓰레드 그룹은 **보안상의 이유로 도입된 개념이다.** 따라서 자신이 속한 쓰레드 그룹이나 하위 쓰레드 그룹은 변경할 수 있지만
다른 쓰레드 그룹의 쓰레드를 변경할 수는 없다. 모든 쓰레드는 반드시 쓰레드 그룹에 포함되어야 있어야 하기 때문에 그룹을 지정하지 않으면 자신을 생성한 쓰레드와 같은 쓰레드 그룹에 자동으로 속하게 된다.

자바 어플리케이션이 실행되면, JVM은 main과 system이라는 쓰레드 그룹을 만들고, JVM 운영에 필요한 쓰레드들을 생성해서 이 쓰레드 그룹에 포함시킨다. main 메서드를 수행하는 main이라는 이름의 쓰레드는 main 쓰레드 그룹에
포함되며, GC를 수행하는 Finalizer 쓰레드는 system 쓰레드 그룹에 속한다. 따라서 사용자가 생성하는 쓰레드는 모두 main 쓰레드 그룹의 하위 쓰레드 그룹이 되며, 지정하지 않아도 자동으로 main 쓰레드 그룹에 포함된다.

### 데몬 쓰레드
데몬 쓰레드는 일반 쓰레드의 작업을 돕는 보조적인 역할을 하는 쓰레드이다. 따라서 일반 쓰레드가 종료되면 강제적으로 자동 종료된다. 일반 쓰레드를 보조하는 업무를 하기 때문에 일반 쓰레드가 없으면 더 이상 존재할 필요가
없기 때문이다. 데몬 쓰레드의 예로는 가비지 컬렉터, 워드 프로세서의 자동저장, 화면자동갱신 등이 있다.  
**쓰레드를 생성한 다음에 `start()`를 하기 전에 `setDaemon(true)`를 호출해야 한다. 그리고 데몬 쓰레드가 생성한 쓰레드는 자동으로 데몬 쓰레드가 된다.**
```java
public class Ex13_7 implements Runnable {
    static boolean autoSave = false;

    public static void main(String[] args) {
        Thread t = new Thread(new Ex13_7());
        t.setDaemon(true);
        t.start();

        for (int i = 1; i < 10; i++) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {

            }
            System.out.println(i);
            if(i==5) autoSave = true;
        }

        System.out.println("프로그램을 종료합니다.");
    }

    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(3 * 1000);
            } catch (InterruptedException e) {

            }

            if(autoSave) autoSave();
        }
    }

    public void autoSave() {
        System.out.println("작업파일이 자동저장되었습니다.");
    }
}

///
1
2
3
4
5
작업파일이 자동저장되었습니다.
6
7
8
작업파일이 자동저장되었습니다.
9
프로그램을 종료합니다.
```
이처럼 데몬 쓰레드의 경우, 메인 쓰레드가 종료하면서 같이 종료된다. **만약, `setDaemon(true)`를 호출하지 않고, 데몬 쓰레드로 만들지 않는다면 main 쓰레드가 종료하더라도 계속해서 실행된다.**
위에서 main 쓰레드가 종료되더라도 끝나지 않은 쓰레드가 있다면 계속해서 실행이 된다고 하였다. 하지만 데몬 쓰레드일 경우에는 main 쓰레드가 끝나면 같이 종료가 된다.

#### 데몬 쓰레드 설정을 하지 않았을 경우
```java
public class Ex13_7 implements Runnable {
    static boolean autoSave = false;

    public static void main(String[] args) {
        Thread t = new Thread(new Ex13_7());
//        t.setDaemon(true);
        t.start();

        for (int i = 1; i < 10; i++) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {

            }
            System.out.println(i);
            if(i==5) autoSave = true;
        }

        System.out.println("프로그램을 종료합니다.");
    }

    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(3 * 1000);
            } catch (InterruptedException e) {

            }

            if(autoSave) autoSave();
        }
    }

    public void autoSave() {
        System.out.println("작업파일이 자동저장되었습니다.");
    }
}

///
1
2
3
4
5
작업파일이 자동저장되었습니다.
6
7
8
작업파일이 자동저장되었습니다.
9
프로그램을 종료합니다.
작업파일이 자동저장되었습니다.
작업파일이 자동저장되었습니다.
작업파일이 자동저장되었습니다.

... 무한반복 ...
```

### 쓰레드의 상태
![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/587c82f6-948e-407d-a029-d68452e9dffe)
- 쓰레드는 생성되고 `start()`를 호출했을 때, 바로 실행되지 않고 실행대기열에 저장되어 자신의 차례를 기다린다. 실행대기열은 Queue 구조로 FIFO이다.
- 주어진 실행시간이 다 되거나 `yield()`를 만나면 다시 실행대기상태가 되고, 다음 차례의 쓰레드가 실행된다.
- I/O block, wait(), join(), sleep()에 의해 일시정지 상태가 된다면 실행대기열로 다시 들어가지 않고, 일시정지 Pool에서 대기한다.
  - 일시정지시간이 다 되거나, notify(), interrupt()가 호출되면 다시 실행대기열로 들어가 자신의 차례를 기다린다.
- 실행을 모두 마치거나 stop()이 호출되면 쓰레드는 소멸된다.

## 쓰레드의 실행제어
![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/6c2ecb0a-907d-4262-b309-1c6e208796b0)
위와 같은 쓰레드의 메서드를 사용하여 쓰레드의 스케쥴링을 제어할 수 있다. 보다 효율적인 스케쥴링은 더 효율적으로 프로그램이 실행되도록 할 것이다. 멀티 쓰레드 프로그램은
주어진 쓰레드를 최대한 활용하여 낭비되는 쓰레드가 없도록 작성되어야 한다. 따라서 해당 쓰레드 메서드를 효과적으로 사용하는 것은 매우 중요하다.
> resume(), stop(), suspend()는 쓰레드를 교착상태(dead-lock)로 만들기 쉽기 때문에 deprecated 되었다.  
> suspend()와 sleep()을 비교했을 때, 무한한 시간동안 쓰레드를 정지시키는 suspend()는 일정시간만 정지시키는 sleep()에 비해 교착상태로 만들기 더 쉬울 것이다.  
> deprecated는 하위 버전과의 호환을 위해 삭제하지 않고, 사용 금지를 권하는 의미이다. 따라서 사용하지 않는 것이 좋다.

### sleep()
`sleep()`은 **지정된 시간동안 쓰레드를 멈추게 한다.** 밀리 세컨드 및 나노 세컨드 단위로 시간을 조절할 수 있지만 약간의 오차는 발생할 수 있다. `sleep()`에 의해 일시정지 된 쓰레드는
시간이 다 되거나 `interrupt()`가 호출되면, `InterruptedException`이 발생되어 꺠어나면서 실행대기(Runnable) 상태가 된다. 따라서 `sleep()`을 호출할 때는 항상 try-catch문으로 예외를 처리해줘야 한다.
```java
public class Ex13_8 {
    public static void main(String[] args) {
        ThreadEx8_1 th1 = new ThreadEx8_1();
        ThreadEx8_2 th2 = new ThreadEx8_2();
        th1.start(); th2.start();

        try {
            Thread.sleep(2000);
        } catch (InterruptedException e) {

        }
        System.out.println("<<main 종료>>");
    }
}

class ThreadEx8_1 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print("-");
        }
        System.out.println("<<th1 종료>>");
    }
}

class ThreadEx8_2 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print("|");
        }
        System.out.println("<<th2 종료>>");
    }
}

///
---------------------------------------------------------------------------------------------------------------------------------------------------
---------------------------------------------------------------------------------------------------------------------------------------------------------|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||<<th1 종료>>
|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||<<th2 종료>>
<<main 종료>>

```
쓰레드 th1과 th2를 실행시킨 후, 2초 동안 실행중이던 main 쓰레드를 중지시켰다. 만약에 `sleep()` 코드가 없다면 어떻게 될까?
```java
public class Ex13_8 {
    public static void main(String[] args) {
        ThreadEx8_1 th1 = new ThreadEx8_1();
        ThreadEx8_2 th2 = new ThreadEx8_2();
        th1.start(); th2.start();

//        try {
//            Thread.sleep(2000);
//        } catch (InterruptedException e) {

//        }
        System.out.println("<<main 종료>>");
    }
}

class ThreadEx8_1 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print("-");
        }
        System.out.println("<<th1 종료>>");
    }
}

class ThreadEx8_2 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print("|");
        }
        System.out.println("<<th2 종료>>");
    }
}

///
<<main 종료>>
|||||||||||||||||||||||||||||||||||||||||||||||||||||||---------------------------------------------------------------------------------------------------
-------------------------------------------------------------------------------------||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||-------|||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||------------------<<th2 종료>>
-------------------------------------------------------------------------------------------<<th1 종료>>
```
이처럼 main 쓰레드가 먼저 종료되고, 그 다음에 th1과 th2가 번갈아가며 수행되는 것을 알 수 있다.
```java
public class Ex13_8 {
    public static void main(String[] args) {
        ThreadEx8_1 th1 = new ThreadEx8_1();
        ThreadEx8_2 th2 = new ThreadEx8_2();
        th1.start();

        try {
            Thread.sleep(2000);
            System.out.println(Thread.currentThread().getName());
        } catch (InterruptedException e) {

        }

        th2.start();

        System.out.println("<<main 종료>>");
    }
}

class ThreadEx8_1 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print("-");
        }
        System.out.println("<<th1 종료>>");
    }
}

class ThreadEx8_2 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print("|");
        }
        System.out.println("<<th2 종료>>");
    }
}

///
------------------------------------------------------------------------------------------------------------------------------------------
------------------------------------------------------------------------------------------------------------------------------------------------------------------<<th1 종료>>
main
<<main 종료>>
||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||
||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||||<<th2 종료>>
```
이처럼 `sleep()`을 중간에 넣게 되면 th1이 실행이 끝나면 main 쓰레드가 2초 동안 멈추게 된다.

### interrupt()
진행 중인 쓰레드를 중간에 중단시켜야 할 때 사용하는 메서드이다. 이는 쓰레드를 강제로 종료시키지는 못하며 단순히 하던 작업을 중지시키는 것이다. 쓰레드는 인스턴스 변수로 interrupted 상태 값을 가지고 있다.
따라서 `interrupt()`를 호출하면 interrupted 상태 값을 true로 바꾸는 것이다. 마찬가지로 `interrupted()`를 호출하면 현재 interrupted 상태를 반환하고, false로 변경한다. (비슷한 메서드인 `isInterrupted()`는
상태만 반환하고 값을 바꾸지는 않는다.)
```java
public class Ex13_9 {
    public static void main(String[] args) {
        ThreadEx9_1 th1 = new ThreadEx9_1();
        th1.start();

        String input = JOptionPane.showInputDialog("아무 값이나 입력하세요.");
        System.out.println("입력하신 값은 " + input + "입니다.");
        th1.interrupt();
        
        System.out.println("isInterrupted(): " + th1.isInterrupted());
    }
}

class ThreadEx9_1 extends Thread {
    @Override
    public void run() {
        int i = 10;

        while (i != 0 && !isInterrupted()) {
            System.out.println(i--);
            for(long x = 0; x<2500000000L;x++);
        }
        System.out.println("카운트가 종료되었습니다.");
    }
}
```
위 코드처럼 `interrupt()`를 통해 쓰레드의 실행흐름을 결정할 수도 있다. 실제로 쓰레드가 `sleep()`, `wait()`, `join()`에 의해 일시정지 상태에 있을 때, 이 쓰레드에 대해 `interrupt()`를 호출하면 `sleep()`, `wait()`, `join()`에서
`Interrupted Exception`이 발생하고 쓰레드가 Runnable 상태로 바뀐다. **즉, 멈춰있던 쓰레드를 깨워서 실행가능한 상태로 만들 수 있는 것이다.** `sleep()`을 사용할 때, 예외처리를 해야하는 이유가 다음과 같이 `interrupt()`에 의해서
예외를 발생하고 깨어날 수 있기 때문이다.

### join()과 yield()
작업 중에 다른 쓰레드의 작업이 먼저 수행되어야할 필요가 있을 때 `join()`을 사용한다.
```java
try {
  th1.join(); // 현재 실행중인 쓰레드가 쓰레드 th1의 작업이 끝날 때까지 기다린다.
} catch (InterruptedException e) {}
```
`join()`도 `sleep()`처럼 `interrupt()`에 의해 대기상태에서 벗어날 수 있기 때문에 예외 처리를 해야 한다. **`sleep()`은 현재 실행중인 쓰레드를 중지시키므로 static 메서드이다. 하지만 `join()`은 특정 쓰레드를 지칭하여 실행되기 때문에
static 메서드가 아니다.** `join()`은 **자신의 작업 중간에 다른 쓰레드의 작업을 참여(join)시킨다는 의미로 지어진 것이다.** 즉, `th1.join()`일 경우, th1이 쓰레드를 수행하도록 하는 것이다.

`yield()`는 쓰레드 자신에게 주어진 시간을 다음 차례의 쓰레드에게 양보(yield)하는 것이다. 쓰레드가 할당받은 시간을 사용하다가 중간에 `yield()`를 사용하면 남은 시간을 다른 쓰레드에게 양보하게 된다.
**따라서 `yield()`와 `interrupt()`를 적절히 사용하면, 프로그램의 응답성을 높이고 보다 효율적인 실행이 가능하게 할 수 있다.**
```java
public class Ex13_11 {
    static long startTime = 0;

    public static void main(String[] args) {
        ThreadEx11_1 th1 = new ThreadEx11_1();
        ThreadEx11_2 th2 = new ThreadEx11_2();
        th1.start();
        th2.start();
        startTime = System.currentTimeMillis();

        try {
            th1.join(); // main 쓰레드가 th1의 작업이 끝날 때까지 기다린다.
            th2.join(); // main 쓰레드가 th2의 작업이 끝날 때까지 기다린다.
        } catch (InterruptedException e) {

        }

        System.out.println("소요시간:" + (System.currentTimeMillis() - startTime));
    }
}

class ThreadEx11_1 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print(new String("-"));
        }
    }
}

class ThreadEx11_2 extends Thread {
    @Override
    public void run() {
        for (int i = 0; i < 300; i++) {
            System.out.print(new String("|"));
        }
    }
}
```
`join()`을 사용하지 않은 상태에서 코드를 실행하면 main 쓰레드가 바로 종료되기 때문에 소요시간을 측정할 수 없게 된다. 그러나 th1과 th2가 종료될 때까지 기다리기 때문에 소요시간을 출력할 수 있다.

## 쓰레드의 동기화
**멀티쓰레드 프로세스의 경우 여러 쓰레드가 같은 프로세스의 자원을 공유해서 작업하기 때문에 서로의 작업에 영향을 줄 수 있다.** 여러 쓰레드가 특정 공유자원을 무분별하게 그리고 동시에 작업할 경우 발생하는 문제들을
막기 위해 도입된 개념이 임계 영역(Critical Section)과 잠금(Lock)이다. 따라서 공유 데이터가 사용되는 코드 영역을 임계 영역으로 지정하고, 공유 데이터(객체)가 가지고 있는 Lock을 획득한 단 하나의 쓰레드만 공유 자원의
코드를 수행할 수 있게 한다. 그리고 Lock을 반납함으로써 다른 쓰레드가 공유 자원을 사용할 수 있도록 통제하는 것이다. 이처럼 **한 쓰레드가 진행 중인 작업을 다른 쓰레드가 간섭하지 못하도록 막는 것을 쓰레드의 동기화(synchronization)라고 한다.** - 한 쓰레드의 진행 작업을 다른 쓰레드가 간섭하지 못하도록 막는 것

### synchronized를 이용한 동기화
`synchronized`는 임계 영역(Critical Section)을 설정하기 위해서 사용되는 키워드이다.
- 메서드 전체를 임계영역으로 설정할 수 있다.
  - ```java
    public synchronized void method() { ... }
    ```
  - 쓰레드는 메서드를 실행한 시점부터 Lock을 얻게 되고, 메서드가 종료되면 Lock을 반환한다.
- 클래스 내 특정 영역을 임계 영역으로 설정할 수 있다.
  - ```java
    synchronized(객체의 참조변수) { ... }
    ```
  - 여기서 참조변수는 Lock을 걸고자 하는 객체를 참조하는 참조변수이어야 한다. 마찬가지로 이 영역 내에 들어가게 되면 Lock을 얻게 되고, 영역을 벗어나면 Lock을 반환한다.

두 방법 모두 Lock을 자동으로 반납하기 때문에 `synchronized`를 통해 임계 영역만 설정해주면 된다. 또한 모든 객체는 하나의 Lock을 갖고 있으며 해당 객체의 Lock을 가진 쓰레드만이 임계 영역의 코드를
수행할 수 있다. 그리고 다른 쓰레드들은 Lock을 얻을 때까지 기다리게 된다.  
임계 영역은 멀티쓰레드 프로그램의 성능을 좌우하기 때문에 메서드 전체를 임계 영역으로 설정하기 보다는 블럭을 통해 최소화하는 것이 더 좋다.

```java
public class Ex13_12 {
    public static void main(String[] args) {
        Runnable r = new RunnableEx12();
        new Thread(r).start();
        new Thread(r).start();
    }
}

class Account {
    private int balance = 1000;

    public int getBalance() {
        return balance;
    }

    public void withdraw(int money){
        if (balance >= money) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {

            }
            balance -= money;
        }
    }
}

class RunnableEx12 implements Runnable {

    Account acc = new Account();

    @Override
    public void run() {
        while (acc.getBalance() > 0) {
            int money = (int) (Math.random() * 3 + 1) * 100;
            acc.withdraw(money);
            System.out.println("balance:" + acc.getBalance());
        }
    }
}

///
balance:800
balance:900
balance:700
balance:600
balance:500
balance:200
balance:0
balance:-100
```
위처럼 `balance`라는 공유 자원에 대한 동기화 처리가 되어있지 않으면 음수 값이 나오게 된다. 왜냐하면 잔고가 0보다 큰 상황에서 한 쓰레드가 작업을 하려던 찰나에 다른 쓰레드가 끼어들어서 출금할 수 있다.
그렇게 되면, 쓰레드 A와 쓰레드 B가 접근한 자원의 상태는 서로 다르게 되므로 다음과 같은 잘못된 결과 값이 나오게 되는 것이다.

```java
public class Ex13_12 {
    public static void main(String[] args) {
        Runnable r = new RunnableEx12();
        new Thread(r).start();
        new Thread(r).start();
    }
}

class Account {
    private int balance = 1000;

    public int getBalance() {
        return balance;
    }
    
    // 메서드를 동기화
    public synchronized void withdraw(int money){
        if (balance >= money) {
            try {
                Thread.sleep(1000);
            } catch (InterruptedException e) {

            }
            balance -= money;
        }
    }
}

class RunnableEx12 implements Runnable {

    Account acc = new Account();

    @Override
    public void run() {
        while (acc.getBalance() > 0) {
            int money = (int) (Math.random() * 3 + 1) * 100;
            acc.withdraw(money);
            System.out.println("balance:" + acc.getBalance());
        }
    }
}

///
balance:800
balance:600
balance:400
balance:200
balance:200
balance:100
balance:0
balance:0
```
위처럼 공유 자원에 접근하는 메서드 영역을 `synchronized` 동기화하게 되면 음수 값이 나오지 않게 된다. **주의해야할 점은 공유 자원인 balance가 private이라는 점이다. 만약 private이 아니면 외부에서 접근할 수 있기 때문에
아무리 동기화를 하더라도 이 값의 변경을 막을 수 없다. `synchronized`를 사용하는 것은 단순히 하나의 쓰레드만 접근할 수 있도록 하는 것이지 공유 자원 그 자체를 보호하는 것은 아니기 때문이다.**

### wait()과 notify()
`synchronized`로 동기화를 한 상태에서 특정 쓰레드가 Lock을 계속 갖고 있다면 작업이 계속 진행되지 않을 수 있다. **따라서 Lock을 가지고 작업을 수행할 상황이 되지 않는다면 Lock을 반납하는 것으로 문제를 해결할 수 있다.**
`wait()`을 호출하게 되면, 갖고 있던 Lock을 반납하고 대기를 한다ㅏ. 그러면 다른 쓰레드가 Lock을 얻어 작업을 수행할 수 있다. 나중에 다시 작업을 할 수 있는 상황이 된다면 `notify()`를 호출해서 중단된 쓰레드를 깨울 수 있다.

`wait()`과 `notify()`는 특정 객체에 대한 것이므로 Object 클래스에 정의되어 있다. 또한 객체마다 Lock 뿐만 아니라 Waiting Pool이 존재한다. 따라서 특정 `notify()`를 호출하면 특정 객체에 대한 Waiting Pool에 있는 쓰레드를 
깨우게 되는 것이다. 당연하게도, **`wait(), notify(), notifyAll()`은 동기화 블록(synchronized 블록) 내에서만 사용할 수 있다.**

```java
public class Ex13_14 {
    public static void main(String[] args) throws InterruptedException {
        Table table = new Table();

        // Runnable 인터페이스를 구현한 쓰레드를 참조변수 없이 생성 + 이름 지정
        new Thread(new Cook(table), "COOK").start();
        new Thread(new Customer(table, "donut"), "CUST1").start();
        new Thread(new Customer(table, "burger"), "CUST2").start();

        Thread.sleep(5000);
        System.exit(0);
    }
}

class Customer implements Runnable {

    private Table table;
    private String food;

    public Customer(Table table, String food) {
        this.table = table;
        this.food = food;
    }

    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {

            }
            String name = Thread.currentThread().getName();

            if(eatFood())
                System.out.println(name + " ate a " + food);
            else
                System.out.println(name + " failed to eat. :(");
        }
    }

    boolean eatFood() {
        return table.remove(food);
    }
}

class Cook implements Runnable {

    private Table table;

    public Cook(Table table) {
        this.table = table;
    }

    @Override
    public void run() {
        while (true) {
            int idx = (int) (Math.random() * table.dishNum());
            table.add(table.dishNames[idx]);
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {

            }
        }
    }
}

class Table {
    String[] dishNames = {"donut", "donut", "burger"};
    final int MAX_FOOD = 6;
    private ArrayList<String> dishes = new ArrayList<>();

    public synchronized void add(String dish) {
        if(dishes.size() >= MAX_FOOD) return;
        dishes.add(dish);
        System.out.println("Dishes:" + dishes.toString());
    }

    public boolean remove(String dishName) {
        synchronized (this) {
            while (dishes.size() == 0) {
                String name = Thread.currentThread().getName();
                System.out.println(name + " is waiting.");
                try {
                    Thread.sleep(500);
                } catch (InterruptedException e) {

                }
            }

            for (int i = 0; i < dishes.size(); i++) {
                if (dishName.equals(dishes.get(i))) {
                    dishes.remove(i);
                    return true;
                }
            }
        }

        return false;
    }

    public int dishNum() {
        return dishNames.length;
    }
}

///
Dishes:[burger]
CUST1 is waiting.
CUST2 ate a burger
CUST1 is waiting.
CUST1 is waiting.
CUST1 is waiting.
CUST1 is waiting.
CUST1 is waiting.
CUST1 is waiting.
CUST1 is waiting.
CUST1 is waiting.
CUST1 is waiting.
```
위 결과에서 알 수 있듯이 CUST1이 동기화된 remove 메서드를 수행하면서 Lock을 계속 쥐고 있다. 따라서 COOK은 Lock을 계속 갖지 못해 add라는 동기화된 메서드를 호출할 수 없게 된다.
Lock을 계속 쥐고 있게 될 때 다른 쓰레드가 작업을 수행할 수 없음을 보여주는 예제이다.

```java
public class Ex13_14 {
    public static void main(String[] args) throws InterruptedException {
        Table table = new Table();

        // Runnable 인터페이스를 구현한 쓰레드를 참조변수 없이 생성 + 이름 지정
        new Thread(new Cook(table), "COOK").start();
        new Thread(new Customer(table, "donut"), "CUST1").start();
        new Thread(new Customer(table, "burger"), "CUST2").start();

        Thread.sleep(5000); // main 쓰레드가 5초 동안 멈추도록 한다. = 5초 동안 여러 쓰레드가 동작할 수 있다.
        System.exit(0); // main 쓰레드가 5초 뒤에 다시 실행되면 exit을 통해 프로그램을 종료시킨다.
        // 이 두 코드가 없으면, COOK과 CUST 쓰레드는 계속 실행된다.
    }
}

class Customer implements Runnable {

    private Table table;
    private String food;

    public Customer(Table table, String food) {
        this.table = table;
        this.food = food;
    }

    @Override
    public void run() {
        while (true) {
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {

            }
            String name = Thread.currentThread().getName();

            table.remove(food);
            System.out.println(name + " ate a " + food);
        }
    }
}

class Cook implements Runnable {

    private Table table;

    public Cook(Table table) {
        this.table = table;
    }

    @Override
    public void run() {
        while (true) {
            int idx = (int) (Math.random() * table.dishNum());
            table.add(table.dishNames[idx]);
            try {
                Thread.sleep(10);
            } catch (InterruptedException e) {
            }
        }
    }
}

class Table {
    String[] dishNames = {"donut", "donut", "burger"};
    final int MAX_FOOD = 6;
    private ArrayList<String> dishes = new ArrayList<>();

    public synchronized void add(String dish) {
        while (dishes.size() >= MAX_FOOD) {
            String name = Thread.currentThread().getName();
            System.out.println(name + " is waiting.");
            try {
                wait(); // COOK 쓰레드가 대기하도록 한다.
                Thread.sleep(500);
            } catch (InterruptedException e) {}
        }
        dishes.add(dish);
        notify(); // 대기중이던 CUST 쓰레드를 깨우기 위해 사용한다.
        System.out.println("Dishes:" + dishes.toString());
    }

    public void remove(String dishName) {
        synchronized (this) {
            String name = Thread.currentThread().getName();

            while (dishes.size() == 0) {
                System.out.println(name + " is waiting.");
                try {
                    wait(); // CUST 쓰레드가 대기하도록 한다.
                    Thread.sleep(500);
                } catch (InterruptedException e) {}
            }

            while (true) {
                for (int i = 0; i < dishes.size(); i++) {
                    if (dishName.equals(dishes.get(i))) {
                        dishes.remove(i);
                        notify(); // 대기 중인 COOK 쓰레드를 깨우기 위해 사용한다.
                        return;
                    }
                }

                try {
                    System.out.println(name + " is waiting.");
                    wait(); // 원하는 음식이 없으면 CUST 쓰레드를 기다리도록 한다.
                    Thread.sleep(500);
                } catch (InterruptedException e) {}
            }
        }
    }

    public int dishNum() {
        return dishNames.length;
    }
}

///
Dishes:[donut]
CUST2 is waiting.
CUST1 ate a donut
Dishes:[burger]
CUST2 ate a burger
CUST1 is waiting.
Dishes:[burger]
CUST1 is waiting.
CUST2 ate a burger
Dishes:[donut]
CUST1 ate a donut
Dishes:[burger]
  ... 생략 ...
```
`wait()`과 `notify()`를 통해 Lock을 넘겨줄 수 있도록 작성한 코드이다. 만약 `wait()`을 호출하게 되면 해당 쓰레드는 Table 객체의 Waiting Pool에서 대기하게 된다.
즉, **공유 자원을 갖고 있는 객체의 Waiting Pool에서 대기한다는 것이 핵심이다.** 여기서는 CUST 쓰레드와 COOK 쓰레드가 같은 Waiting Pool에서 기다리기 때문에
`notify()`를 하게 되면, 두 쓰레드 중 하나가 Lock을 얻게 된다. **이처럼 여러 종류의 쓰레드가 같은 Waiting Pool에 있으면 특정 쓰레드를 깨우기가 어렵다는 문제점이 존재한다.**
