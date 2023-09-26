# Contents of 예외처리
- [프로그램 오류](#프로그램-오류)
- [예외 클래스의 계층구조](#예외-클래스의-계층구조)
  - [Exception과 RuntimeException](#Exception과-RuntimeException)
- [예외 처리하기](#예외-처리하기)
    - [try catch문](#try-catch문)
    - [try catch문 흐름](#try-catch문-흐름)
    - [예외의 발생과 catch 블럭 원리](#예외의-발생과-catch-블럭-원리)
      - [예외 메시지 출력](#Set-인터페이스)
      - [멀티 catch 블럭](#Map-인터페이스)
    - [메서드에 예외 선언하기]
- [예외 발생시키기](#스택과-큐)
- [Checked Unchecked 예외](#Iterator)
  - [Map과 Iterator](#Map과-Iterator)
- [Arrays](#Arrays)
- [Comparator와 Comparable](#Comparator와-Comparable)

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
1. Exception 클래스와 그 자손들 (Unchecked 예외이며 사용자 등 외부 요인으로 인해 발생할 수 있는 예외)
  - 파일 이름을 잘못 적었을 때(`ClassNotFoundException`)
  - 입력한 데이터의 형식이 잘못됐을 때(`DataFormatException`)
2. RuntimeException 클래스와 그 자손들 (Checked 예외이며 프로그래머의 실수로 인해 발생할 수 있는 예외 - 컴파일 시 에러 발생)
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

알맞는 catch 블럭을 찾는 방법은 `instanceof 연산자`를 활용하는 것이다. catch 블럭에 선언된 예외클래스 참조변수와 instanceof 연산을 한 다음 true가 나오면 해당 블럭을 수행한다.
true가 하나도 나오지 않으면 예외는 처리되지 않는다.
