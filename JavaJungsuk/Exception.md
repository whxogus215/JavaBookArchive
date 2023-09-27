# Contents of 예외처리
- [프로그램 오류](#프로그램-오류)
- [예외 클래스의 계층구조](#예외-클래스의-계층구조)
  - [Exception과 RuntimeException](#Exception과-RuntimeException)
- [예외 처리하기](#예외-처리하기)
    - [try catch문](#try-catch문)
    - [try catch문 흐름](#try-catch문-흐름)
    - [예외의 발생과 catch 블럭 원리](#예외의-발생과-catch-블럭-원리)
      - [예외 메시지 출력](#예외-메시지-출력)
      - [멀티 catch 블럭](#멀티-catch-블럭)
    - [메서드에 예외 선언하기](#메서드에-예외-선언하기)
- [예외 발생시키기](#예외-발생시키기)
- [Checked Unchecked 예외](#Checked-Unchecked-예외)
- [finally 블럭](#finally-블럭)

출처 : [https://github.com/castello/javajungsuk_basic](https://github.com/castello/javajungsuk_basic)

## 프로그램 오류
프로그램 오류는 **컴파일 에러와 런타임 에러로 나뉜다.** 컴파일 에러는 컴파일 시 발생하는 에러, 런타임 에러는 컴파일 후 실행도중에 발생하는 에러를 뜻한다.
컴파일 에러는 컴파일러가 언어의 문법 상 오류 등 `.class` 파일 변환 과정에서 발생하는 여러 오류들을 체크함으로써 발생한다. 하지만 런타임 에러는 프로그램이 실행 중 발생할 수 있는
예기치 못한 에러들이 포함되어 있으며, 이는 컴파일러가 검사하기 어렵다. 따라서 런타임 에러로 인한 프로그램의 비정상적 종료를 막기 위해 **프로그램 작성 시 런타임 에러에 대한
처리를 할 수 있어야 한다.**

자바는 런타임 오류를 크게 에러(error)와 예외(exception)로 나누었다. 에러는 프로그램 코드에 의해서 수습할 수 없는 **심각한 오류를 뜻한다.** 반면에 예외는
프로그램 코드를 통해 수습할 수 있는 **다소 미약한 오류를 뜻한다.** 따라서 프로그래머는 핸들링할 수 있는 예외에 대해서는 처리를 해야만 한다.

## 예외 클래스의 계층구조
**자바는 런타임 오류인 에러와 예외를 클래스로 정의하였다.** Exception과 Error 클래스는 여느 클래스들과 마찬가지로 Object 클래스의 자손이다.

<img width="1000" alt="image" src="https://github.com/whxogus215/JavaBookArchive/assets/70999462/cf0ebec4-5910-43c5-9478-3ffbe2254a96">

### Exception과 RuntimeException
Exception 클래스는 **모든 예외의 최고 조상이며, Exception은 크게 두 가지로 나뉜다.**
1. Exception 클래스와 그 자손들 (Checked 예외이며 사용자 등 외부 요인으로 인해 발생할 수 있는 예외 - 컴파일 시 에러 발생)
  - 파일 이름을 잘못 적었을 때(`ClassNotFoundException`)
  - 입력한 데이터의 형식이 잘못됐을 때(`DataFormatException`)
2. RuntimeException 클래스와 그 자손들 (Unchecked 예외이며 프로그래머의 실수로 인해 발생할 수 있는 예외)
  - 배열의 범위를 벗어났을 때(`ArrayIndexOutOfBoundsException`)
  - 값이 null인 참조변수를 호출했을 때(`NullPointerException`)

## 예외 처리하기
프로그램의 실행도중에 발생하는 에러는 어쩔 수 없다. 하지만 예외는 프로그래머가 어느정도 핸들링할 수 있다. **예외 처리란 프로그램 실행 시 발생할 수 있는 예외를 대비한 
코드를 작성하는 것이며, 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지할 수 있도록 하는 것이다.**

발생한 예외를 처리하지 못하면, 프로그램은 비정상적으로 종료된다. 처리되지 못한 예외(Uncaught Exception)는 **JVM의 예외처리기(UncaughtExceptionHandler)가 받아서 예외의 원인을
화면에 출력해준다.**

<img width="800" alt="image" src="https://github.com/whxogus215/JavaBookArchive/assets/70999462/e6a088b6-6e2b-41f6-901c-5002aa6dd752">

### try catch문
**예외를 처리하기 위해서는 try-catch문을 사용한다.**
```java
try {
  // 예외가 발생할 수 있는 코드가 들어간다.
} catch (Exception1 e1) {
  // Exception1이 발생했을 경우, 처리되는 코드가 들어간다.
} catch (Exception2 e2) {
  // Exception2이 발생했을 경우, 처리되는 코드가 들어간다.
}
```
이처럼 여러 예외 상황들을 대비하는 catch 블럭들을 만들 수 있으며, 이 중에서 예외의 종류와 일치하는 **단 하나의 catch 블럭만 수행된다.**

### try catch문 흐름
try-catch문에서 예외가 발생한 경우와 발생하지 않았을 때 흐름이 달라진다.
- try 블럭 내에서 예외가 발생한 경우
   1. 발생한 예외와 일치하는 catch 블럭을 확인한다.
   2. **일치하는 catch 블럭이 있을 경우, try 블럭에서 벗어나 일치하는 catch 블럭으로 이동하여 해당 코드들을 수행한다.**
   3. 다 끝나면 try-catch문을 최종적으로 벗어난다. 또 다른 catch문을 실행하지는 않는다!
- try 블럭 내에서 예외가 발생하지 않은 경우
   1. 예외가 발생하지 않았기 때문에 catch 블럭으로 이동하지 않는다. 따라서 **try 문만 수행하고 try-catch문을 빠져나가게 된다.**

<img width="1000" alt="image" src="https://github.com/whxogus215/JavaBookArchive/assets/70999462/00330ee3-564d-4264-83f9-b16f10e9a2a3">

**핵심은 예외가 발생한 시점에서 그 다음 문장들은 실행이 되지 않는다는 것이다. catch 문으로 예외를 잡는다면 해당 catch 블럭으로 이동할 것이고, 예외를 처리하지 않았다면
그 상태로 프로그램은 종료된다.**

### 예외의 발생과 catch 블럭 원리
catch 블럭은 `()`와 `{}`로 나누어져 있다. 소괄호 내에는 처리하고자 하는 예외 타입의 참조 변수를 선언해야 한다. **예외가 발생할 경우, 해당 예외에 해당하는 클래스의 인스턴스가 자동으로 생성된다.**
이 때, try 블럭 내에서 예외가 발생한 것이라면 해당하는 catch 블럭을 찾게 된다.

알맞는 catch 블럭을 찾는 방법은 `instanceof 연산자`를 활용하는 것이다. catch 블럭에 선언된 예외클래스와 instanceof 연산을 한 다음 true가 나오면 해당 블럭을 수행한다.
true가 하나도 나오지 않으면 예외는 처리되지 않는다.

```java
public class Ex8_3 {
    public static void main(String[] args) {
        System.out.println(1);
        System.out.println(2);
        
        try {
            System.out.println(3);
            System.out.println(0 / 0); // ArithmeticException 발생
            System.out.println(4);
        } catch (Exception e) {
            System.out.println("에러 처리됨.");
        }

        System.out.println("프로그램 종료");
    }
}

1
2
3
에러 처리됨.
프로그램 종료
```
이처럼 `ArithmeticException`이 발생하지만 조상 클래스인 Exception을 통해서 처리가 가능하다. **그 이유는 `System.out.println(0 / 0);`로 인해 생성된 `ArithmeticException` 인스턴스가
catch 블럭에 있는 `Exception` 클래스와의 `instanceof` 연산을 한 결과 true이기 때문에 가능한 것이다.**

```java
public class Ex8_3 {
    public static void main(String[] args) {
        System.out.println(1);
        System.out.println(2);

        try {
            System.out.println(3);
            System.out.println(0 / 0);
            System.out.println(4);
        } catch (ArithmeticException arithmeticException) {
            System.out.println("Arith 예외 처리");
        } catch (Exception e) {
            System.out.println("에러 처리됨.");
        }

        System.out.println("프로그램 종료");
    }
}

1
2
3
Arith 예외 처리
프로그램 종료
```
이처럼 `ArithmeticException` catch 블럭을 먼저 놓을 경우, Exception 블럭은 실행되지 않는다. 따라서 Exception 예외를 통한 catch 블럭을 사용하면 위에서부터 구체적인 예외 처리가 다 이루어지고
난 뒤에 명시되지 않은 **나머지 예외 대해 일괄적으로 처리할 수 있게 된다.**

만약, 위 코드에서 두 catch 블럭의 순서를 뒤집으면 컴파일 에러가 발생한다. 왜냐하면 Exception 블럭이 위에 있기 때문에 모든게 다 거기에서 해결된다. 밑에 있는 `ArithmeticException`는 이미 처리된 시점에서
굳이 또 쓸 필요가 없는 것이다. **따라서 Exception 블럭은 맨 마지막에 사용해야 한다.**

#### 예외 메시지 출력
예외가 발생하면 기본적으로 해당 예외 클래스의 인스턴스가 생성된다. 그리고 이 인스턴스에는 예외에 대한 정보들이 담겨있다. catch 블럭의 괄호에 선언한 참조변수를 통해 **생성된 예외 인스턴스에
접근할 수 있다.** 주로 사용되는 메서드는 다음과 같다.
- `printStackTrace()` : 예외발생 당시에 호출스택(Call Stack)에 있던 메서드 정보와 예외 메시지를 출력한다. 스택이기 때문에 맨 밑에 main 메서드가 온다. (main 메서드 다음에 생성한 메서드가 호출됨으로써 스택에 쌓이는 구조)
- `getMessage()` : 예외 인스턴스에 저장된 메시지를 얻을 수 있다.

```java
public class Ex8_3 {
    public static void main(String[] args) {
        System.out.println(1);
        System.out.println(2);

        try {
            System.out.println(3);
            System.out.println(0 / 0);
            System.out.println(4);
        } catch (ArithmeticException arithmeticException) {
            arithmeticException.printStackTrace();
            System.out.println(arithmeticException.getMessage());
        }

        System.out.println("프로그램 종료");
    }
}

1
2
3
/ by zero
프로그램 종료
java.lang.ArithmeticException: / by zero
	at exception.Ex8_3.main(Ex8_3.java:10)

Process finished with exit code 0
```
이처럼 예외가 발생했음에도 이를 처리하였기 때문에 **프로그램이 정상적으로 종료되었다.**

#### 멀티 catch 블럭
JDK 1.7 이후로 여러 catch 블럭을 `|` 로 묶을 수 있다. 개수 제한은 없으나 조상과 자손의 관계에 있는 클래스를 연결할 경우, 컴파일 에러가 발생한다.
```java
public class Ex8_3 {
    public static void main(String[] args) {
        System.out.println(1);
        System.out.println(2);

        try {
            System.out.println(3);
            System.out.println(0 / 0);
            System.out.println(4);
        } catch ( Exception | ArithmeticException arithmeticException) { // 에러 발생 : Exception e 와 동일함
            arithmeticException.printStackTrace();
            System.out.println(arithmeticException.getMessage());
        }

        System.out.println("프로그램 종료");
    }
}
```

또한 여러 catch 블럭을 하나로 처리하기 때문에 해당 멀티 catch에 예외가 발생했을 경우, 어떤 예외 클래스가 발생한 것인지 알 수 없다. 따라서
이 때 사용되는 참조변수 e는 `|` 기호로 연결된 클래스들의 조상 예외 클래스의 멤버만 사용할 수 있다. 자손 클래스의 메서드를 사용해야 할 경우,
참조변수를 형변환 해서 사용해야 한다.

### 메서드에 예외 선언하기
예외를 처리하는 방법은 **try-catch를 통해 직접 처리하거나 예외를 메서드에 선언하는 방법이 있다.** 메서드에 예외를 선언할면 `throws`를 메서드에 붙여주면 된다.
해당 메서드에서 발생할 수 있는 예외 클래스를 `throws`로 선언하면 된다. 이를 통해 해당 메서드를 사용하는 쪽에서는 해당 메서드에서 어떠한 예외가 발생할 수 있는지
알 수 있다.

특정 예외를 메서드에 선언할 경우, 해당 예외의 자손 예외 클래스도 발생할 수 있음을 암시한다. 따라서 예외 클래스의 최고 조상인 `Exception` 클래스를 선언할 경우,
모든 종류의 예외가 발생할 수 있다는 뜻이다.

이처럼 메서드에 예외가 발생할 수 있음을 알려줌으로써 프로그래머들은 예외를 처리하기가 더 수월해졌다. **핵심은 `throws`를 선언한 메서드를 사용하는 쪽에서는
반드시 해당 에러를 처리하거나 혹은 다시 `throws`를 사용해야 한다는 것이다.**

**예외를 선언하게 되면 Exception 혹은 그 밑 자식 클래스들 같은 checked 예외의 경우라도 컴파일 에러가 발생하지 않는다.**

`throws`를 활용하게 되면 해당 예외를 메서드를 호출한 쪽에 넘길 수 있다. 즉, 특정 메서드가 에러를 자체적으로 해결할 수 있는 상황이 아니라면
메서드 선언을 통해 해결 의무를 위임할 수 있다. 이러한 기능을 통해 예외로 인한 프로그램의 비정상 종료를 보다 유연하게 대처할 수 있다.

## 예외 발생시키기
`throw`를 사용하면 프로그래머가 예외 인스턴스를 직접 생성할 수도 있다. 메서드에 예외를 선언하는 것은 throw**s**이다. 혼동해서는 안된다.
`throw new Exception("예외 메시지");` 이러한 형태로 생성하는 것이 일반적인 방법이다. 이 때, 예외의 생성자 안에 에러 메시지를 직접 지정할 수도 있다.
**이렇게 생성된 에러 메시지는 앞서 언급한 `getMessage()`를 통해 가져올 수 있다.**

## Checked Unchecked 예외
Checked 예외인, Exception 클래스와 그 자손들의 경우, 이를 예외처리하지 않으면 **컴파일조차 되지 않는다.** 반면에 Unchecked 예외인,
RuntimeException 클래스와 그 자손들의 경우, 예외처리를 하지 않아도 **컴파일은 된다.** 왜냐하면 Unchecked 예외의 경우, **프로그래머의 실수로 인해
발생할 수 있는 예외들을 정의하고 있다.** 일반적인 배열에서 발생할 수 있는 `IndexOutOfBoundsException`을 무조건 예외처리하도록 강제한다면
평범한 배열 생성에 대해서도 try-catch문을 사용해야 할 것이다. 즉, 컴파일이 된다는 것은 예외처리를 강제하지 않는다는 뜻이다.

물론 Unchecked 예외가 고의로 발생되거나 무언가에 의해 발생될 경우엔 반드시 예외처리를 해야 한다. Unchecked 예외라도 예외처리가 되어야 프로그램이 정상적으로 종료된다.

## finally 블럭
finally는 예외의 발생과 상관없이 실행되어야 할 코드들을 담는 블럭이다. try-catch문을 사용할 경우, 예외 상황에 따라 catch문이 실행되거나 실행되지 않을 수도 있으며,
try 블럭 내에 있는 코드들도 전부 실행이 안되고 넘어갈 수가 있다. 즉, 이러한 예외 발생과 관련없이 실행되어야만 하는 코드들을 감싸는 블럭이라고 생각하면 된다.

- 예외가 발생할 경우 : **try -> catch -> finally** 순으로 진행
- 예외가 발생하지 않을 경우 : **try -> finally** 순으로 진행 (예외가 발생하지 않기 때문에 catch문은 실행되지 않음)

또한 finally 블럭은 try문과 catch문에 공통적으로 들어가는 코드들을 넣는 데도 활용할 수 있다.

```java
try {
  startIntall();
  copyFiles();
  deleteTempFiles();
} catch (Exception e) {
  e.printStackTrace();
  deleteTempFiles();
}
```
이 코드에서 `deleteTempFiles()`는 try문에도 있고, catch문에도 있다. **즉, 예외와 상관없이 무조건 실행된다는 뜻이다. 그러면 이러한 상황에서는 finally 블럭을 사용해서 중복 코드를 제거하는 것이다.**

```java
try {
  startIntall();
  copyFiles();
} catch (Exception e) {
  e.printStackTrace();
} finally {
  deleteTempFiles();
}
```

