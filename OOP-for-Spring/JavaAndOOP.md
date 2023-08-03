# Contents
- [기계어에서 객체지향 언어로](#기계어에서-객체지향-언어로)
- [자바와 절차적/구조적 프로그래밍](#자바와-절차적구조적-프로그래밍)
  - [자바를 이해하기 위한 메모리 구조](#자바를-이해하기-위한-메모리-구조)
  - [블록을 통해 구분되는 지역변수](#블록을-통해-구분되는-지역변수)
  - [전역변수의 사용을 지양할 것](#전역변수의-사용을-지양할-것)
  - [멀티 스레드 / 멀티 프로세스의 이해](#멀티-스레드-멀티-프로세스의-이해)
- [자바와 객체 지향](#자바와-객체-지향)
  - [클래스와 객체의 관계 : 붕어빵틀 과 붕어빵??](#클래스와-객체의-관계--붕어빵틀-과-붕어빵)
  - [추상화: 모델링](#추상화--모델링)
  - [상속: 재사용 + 확장](#상속--재사용--확장)
  - [다형성: 사용편의성](#다형성--사용편의성)
  - [캡슐화: 정보 은닉](#캡슐화--정보-은닉)
- [자바가 확장한 객체지향](#자바가-확장한-객체지향)
  - [abstract 키워드 - 추상 메서드와 추상 클래스](#abstract-키워드)
  - [생성자](#생성자)
  - [클래스 생성 시 실행되는 static 블록](#클래스-생성-시-실행되는-static-블록)
  - [final](#final)
  - [instanceof 연산자](#instanceof-연산자)
  - [package](#package)
  - [실제로 호출되는 것은 클래스의 메서드](#실제로-호출되는-것은-클래스의-메서드)
- [자바8 람다와 인터페이스 스펙 변화](#자바8-람다와-인터페이스-스펙-변화)
  - [람다란 무엇인가?](#람다란-무엇인가?)
  - [함수형 인터페이스](#함수형-인터페이스)
  - [람다식을 변수에 담아서 사용하기](#람다식을-변수에-담아서-사용하기)
  - [람다식을 반환값으로 사용하기](#람다식을-반환값으로-사용하기)
  - [자바8 API에서 제공하는 함수형 인터페이스](#자바8-API에서-제공하는-함수형-인터페이스)

출처 : https://github.com/expert0226/oopinspring


## 기계어에서 객체지향 언어로
**기계어란 0과 1로만 이루어져 있어 컴퓨터만이 이해할 수 있는 언어이다.** 이 기계어의 단점은 일단 사람이 이해하기 힘든다는 점과 각 CPU 회사마다 다르다는 것이다. 즉,
코드의 이식성이 좋지 않기 때문에 기계어 프로그래밍을 하던 개발자들은 매우 힘들었을 것이다.

![001](https://github.com/whxogus215/JavaBookArchive/assets/70999462/90bc39ac-bb3b-44d4-ae5b-18a8b12edf11)

이를 해결하기 위해 등장한 언어가 바로 어셈블리어이다. 이는 기존의 0과 1의 나열에서 벗어나 좀 더 인간친화적인 언어 형태를 갖추었다. ADD, PLUS 등등이 대표적이다.
그러나 어셈블리어도 치명적인 단점이 있었는데, **바로 각 운영체제, 하드웨어 종류에 따라 고유의 어셈블리어가 존재했던 것이다.** 즉, A사의 어셈블리어와 B사의 어셈블리어는
서로 호환이 되지 않았다. 어셈블리어는 기계어보다는 인간이 다루기 쉬운 언어였으나 이식성의 문제는 해결하지 못했다.

![002](https://github.com/whxogus215/JavaBookArchive/assets/70999462/458d1c54-74d0-4fbc-8c7a-191d5193cc85)
![003](https://github.com/whxogus215/JavaBookArchive/assets/70999462/2a4b92b9-d357-464b-b2d5-daa4040d1007)

이처럼 하나의 로직을 수행하기 위해 여러 언어로 작성해야 하는 번거로움은 C언어를 통해 해결되었다. 즉, C언어는 하나의 소스코드를 작성하면 각 기종(OS)에 맞는
컴파일러를 통해 기계어 목적 파일로 변환된다. **즉, 기종마다 소스코드를 작성하지 않고 하나의 소스코드를 통해 여러 기종에서 실행이 가능한 것이 C언어의 장점이다.**

가상머신, **JVM의 등장으로 인해 자바 언어에서는 여러 종류의 컴파일러를 갖출 필요가 사라졌다.** 하나의 소스코드를 하나의 컴파일러를 통해 목적 파일로 변환된다.
그리고 각 기종에 맞는 JVM에서 실행된다. 이 때 각 기종에 맞는 JRE를 통해 JVM이 동작하게 된다.

![004](https://github.com/whxogus215/JavaBookArchive/assets/70999462/b302d2ab-c5c4-4867-8595-6a4887dc90a7)

### 책에서 소개한 진정한 객체 지향 언어 - 자바
저자는 자바를 진정한 객체지향 언어로 소개한다. **자바는 객체, 즉 Class가 반드시 있어야 한다.** Class를 기반으로 동작하는 언어이기 때문에 클래스 외부에서 자바는 존재할 수 없다.
또한 모든 메서드들은 클래스를 통해서만 접근할 수 있다. ex) 클래스.메서드명(), 객체.메서드명() 심지어 main 함수 또한 하나의 클래스 안에서만 존재할 수 있다.  
**반면에 C++의 main 함수는 클래스와 별개로 존재할 수 있다.**

## 자바와 절차적/구조적 프로그래밍
자바를 이해하기 위해서는 먼저 JVM에 대해 알아야 한다. JVM은 말 그대로 가상기계이다. 컴퓨터를 통해 어떠한 기능을 수행하기 위해서는 하드웨어, 소프트웨어, 그리고 운영체제 이 세 가지 요소가
기본적으로 필요하다. 자바가 실행되기 위해서도 마찬가지로 이 세 가지 요소가 필요한데, JVM은 이 중 하드웨어에 속한다. 그리고 이 JVM 내에서 돌아가는 소프트웨어가 JDK이며, 이를 가능하게 하는
운영체제 역할을 하는 것이 JRE이다. JDK 내에는 JRE와 JVM이 포함되어 있다. JDK는 자바 컴파일러인 javac.exe를 포함하고 있으며, JRE는 자바 프로그램 실행기인 java.exe를 포함하고 있다.

우리는 JDK 설치를 통해 자바를 실행할 수 있다. 이것이 가능한 이유는 JDK 내에 있는 컴파일러가 동작하며, JRE가 JVM에서 해당 목적 파일을 실행시키기 때문이다. 스프링 개발에 비유하자면,
우리는 각자의 로컬 컴퓨터에서 각 OS에 맞는 JVM을 사용해 소스코드를 개발한다. 그리고 이것이 배포가 되면 다른 로컬 컴퓨터에서 다른 OS일지라도 각기 다른 JVM 및 JRE를 통해 같은 소스코드를
실행할 수 있는 것이다.

![001](https://github.com/whxogus215/JavaBookArchive/assets/70999462/e5b5dddd-20b6-4b2c-9854-26a110518e1e)

### 자바를 이해하기 위한 메모리 구조
자바 프로그램이 메모리를 사용할 때는 코드 영역과 데이터 저장 영역으로 나눈다. 이 때 **데이터 저장 영역은 크게 스태틱 영역, 스택 영역, 힙 영역으로 나뉜다.**
자바의 main 메서드가 실행되기 위해서는 먼저 JRE가 main 메서드를 확인한다. 그리고 프로그램을 실행하기 위한 사전 준비에 들어간다. 먼저, JVM을 부팅하고 컴파일러를 통해 받은 목적 파일을 실행한다.
이 때, 모든 자바 프로그램이 반드시 포함하게 되는 패키지인 `java.lang`을 스태틱 영역에 로딩한다. 그리고 개발자가 작성한 모든 클래스와 import 패키지도 스태틱 영역에 로딩한다.  
**즉, 스태틱 영역은
클래스들의 놀이터이다.** 이제 모든 전처리 과정이 끝났고, 본격적으로 main 메서드가 실행되면, 이제는 스태틱 영역이 아닌 스택 영역에 main 메서드의 스택 프레임이 할당된다. **즉, 스택 영역은
메서드들의 놀이터이다.** 메서드 단위별로 스택 영역에 프레임 단위로 할당된다. 블럭의 시작(`{`)과 끝(`}`)으로 구분할 수 있다.

![003](https://github.com/whxogus215/JavaBookArchive/assets/70999462/f00d8ab6-b197-44b2-8a64-bc11db484c02)

![009](https://github.com/whxogus215/JavaBookArchive/assets/70999462/62063927-5cd4-4345-9b3a-40717d140503)

### 블록을 통해 구분되는 지역변수
우리는 main 메서드 내에서 다양한 블럭들을 생성하게 된다. 특히, 제어문(조건문, 반복문)의 경우 다양한 블럭 단위로 실행된다. 이 떄 블럭 내에서 생성하는 변수들은 해당 블럭이 종료되면 메모리에서 사라진다.
만약, main 메서드 내에서 if문이 실행된다면 스택 영역에 있는 main 스택 프레임이 생성되고, 그 안에 if문 스택 프레임이 생성되는 것이다. 그리고 if문의 블럭 끝인 `}`을 만나면 해당 if문에 있는 변수들은 소멸된다.

![AnyConv com__020](https://github.com/whxogus215/JavaBookArchive/assets/70999462/f0322566-f7fc-4697-8b68-e1d38cc04f94)
![AnyConv com__023](https://github.com/whxogus215/JavaBookArchive/assets/70999462/feefc438-19af-432a-9d2d-912f936f31d1)

이처럼 **지역 변수는 스택 영역 내에 있는 스택 프레임 안에서만 생명 주기를 갖는다. 그에 반면에 스태틱 영역에 존재하는 클래스 멤버 변수들은 JVM이 종료될 때까지 메모리에 존재한다.**

**외부 블럭에서 내부 블록의 변수에는 접근할 수 없지만 그 역은 가능하다.** & **외부 스택 프레임에서 내부 스택 프레임의 변수에 접근하는 것은 불가능하나 그 역은 가능하다.**
이 말은 지역 변수를 접근할 수 있는 것이 외부에서는 불가능하다는 것이다. 우리는 스택 영역 내에 있는 스택 프레임에서의 지역변수에 대해 배웠기 때문에 이제 이 말의 뜻을 이해할 수 있다.

### 메서드가 실행될 때마다 다른 메서드 프레임이 생성된다.
```java
public class Start4 {
  public static void main(String[] args){
    int k = 5;
    int m;

    m = square(k);
  }

  private static int square(int k){
    int result;
    k = 25;
    result = k;
    return result;
  }
}
```
스택 영역 내에 main 메서드 프레임이 생성되고, square라는 메서드를 호출했을 때, 스택 영역 내에 또 다른 square 메서드 스택 프레임이 생성된다. 그리고 매개변수인 k와 result를 해당 프레임 내에 저장한다.
따라서 main 메서드 내에서 선언된 k와 square 메서드 내에서 생성된 k는 전혀 다른 것이다. main 메서드 내에서 전달한 k 값의 경우, 그저 값만 넘겨주는 것이므로, **이를 Call By Value라고 한다.
메서드를 호출하면서 인자로 전달되는 것은 변수 자체가 아니라 변수가 저장한 값만을 복제해서 전달하는 것이다.**

![AnyConv com__030](https://github.com/whxogus215/JavaBookArchive/assets/70999462/5322272b-60c8-4713-abfc-168fcaa89f91)

![AnyConv com__032](https://github.com/whxogus215/JavaBookArchive/assets/70999462/d5969007-2afa-43fe-bd66-5afd30bc7d19)

### 전역변수의 사용을 지양할 것
두 메서드 사이에 값을 전달하기 위해 메서드를 호출할 때 전역변수를 사용하여 값을 공유할 수 있다. 
```java
public class Start5 {
  static int share = 10;

  public static void main(String[] args){
    share = 55;
    int m;

    m = square(share);
  }

  private static int square(int k){
    share = k + 25;
    return share;
  }
}
```
![AnyConv com__038](https://github.com/whxogus215/JavaBookArchive/assets/70999462/18dd7acc-b383-4720-800e-2f72d5425877)

이처럼 `share`라는 static 변수가 존재할 때, 메모리의 변화는 어떻게 될까? 먼저 스태틱 영역에 클래스에 관한 것들이 로딩된 후, `share`라는 변수도 스태틱 영역에 할당된다.
그리고 main 메서드 프레임과 square 메서드 프레임이 각각 스택 영역에 할당된다. main 메서드와 square 메서드는 서로 전역 변수인 `share`에 접근하고 있는데, 이로 인해 초기 값이 10이었던
`share` 변수가 최종적으로는 80으로 변경된다.

전역 변수는 코드 어느 곳에서나 접근이 가능하기 때문에 전역 변수라고 하며, 여러 메서드들이 공유해서 사용하기 때문에 공유 변수라고도 한다. 프로젝트의 규모에 따라 여러 메서드들이 전역 변수의
값을 변경하게 되면, 전역 변수의 값을 추적하기 어려워질 것이다. 위 예제에서도 전역변수 `share`가 두 번이나 변경이 되었는데 실제 프로젝트 규모에서 전역 변수를 건드리게 되면 값을 파악하기 매우 힘들 것이다.
**따라서 전역 변수를 사용하려면 단순히 읽기 전용으로 값을 공유하는 것을 추천한다. (static final)**

### 멀티 스레드/ 멀티 프로세스의 이해
**멀티 스레드란, 하나의 메모리 내에 스택 영역을 스레드 개수 만큼 분할해서 사용하는 것이다. 반면에 멀티 프로세스란, 여러 개의 메모리를 갖는 구조이다.**
따라서 멀티 프로세스는 각 프로세스마다 메모리가 있고, 각자의 고유 공간이기 때문에 서로 참조할 수 없다. 하지만 멀티 스레드는 하나의 메모리만 사용한다.

- 멀티 프로세스 : 서로의 영역을 침범할 수 없기 때문에 안전한 구조이지만, 메모리 사용량이 많다.
- 멀티 스레드 : 힙과 스태틱 영역은 공유하지만 서로의 스택 영역은 접근할 수 없다. 따라서 메모리 사용량이 적다.

![AnyConv com__040](https://github.com/whxogus215/JavaBookArchive/assets/70999462/fa08044f-8226-4b95-9cc7-28c18ce844e9)

![AnyConv com__041](https://github.com/whxogus215/JavaBookArchive/assets/70999462/ec6cd111-d774-40a1-85b6-a8bf17bc9f8a)

스프링 MVC1을 배우면서 서블릿은 요청마다 스레드를 생성해서 처리한다라고 배웠다. **이처럼 요청마다 프로세스를 생성하는 것이 아닌 공유 자원을 활용해 다중 스레드를 지원하는 것이
보다 메모리를 효율적으로 사용하는 방법임을 알 수 있다.**

그렇다면 멀티 스레드 환경에서 전역 변수를 사용하는 것이 왜 문제가 될까? 스레드1에서 전역변수 A에 10을 할당하고, 스레드2에서 전역변수 A에 다시 20으로 할당한 뒤 스레드1에서 전역변수 A를 출력한다면
스레드1이 할당한 10이 아닌 20이 출력된다. 이처럼 멀티 스레드 환경에서 전역변수를 사용하면 스레드 안정성이 깨질 수 있다.

## 자바와 객체 지향
### 객체 지향은 인간 지향이다
프로그래밍 언어의 발전사를 보면 개발자를 더욱 편하고 이롭게 하기 위한 과정임을 알 수 있다. 기계어에서 어셈블리어 그리고 C/C++, 자바까지 모두 로우 레벨의 기계가 아닌 하이 레벨의 인간을 배려하기 위한
과정이다. 하지만 절차적/구조적 프로그래밍에서 사용한 포인터 개념은 기계 수준으로 낮춰야 이해할 수 있는 부분이다.  

**자바에서는 이러한 기계 종속적인 프로그래밍이 아닌 인간 중심에서
우리 눈으로 보고 직관적으로 이해할 수 있는 사물이라는 개념을 통해 마치 현실세계와 동일한 수준으로 프로그래밍하는 방법을 선택했다.** 그것이 바로 객체 지향 프로그래밍인 것이다.
기존의 0과 1로 대볃뇌는 기계(컴퓨터)에 맞춰 사고하던 방식을 버리고 현실세계를 인지하는 방식으로 프로그램을 만든 것이다. 따라서 객체 지향은 직관적이다.

### 클래스와 객체의 관계 : 붕어빵틀 과 붕어빵??
클래스와 객체의 관계를 이해할 때 붕어빵틀과 붕어빵이라는 개념을 많이 사용한다. 우리는 객체를 생성할 때, `클래스 객체명 = new 클래스();` 로 코드를 작성한다.
그렇다면 `붕어빵틀 붕어빵 = new 붕어빵틀();`이라는 코드가 과연 맞는 말인지 우리는 의심할 수 있다. 논리에 맞지 않는 것이다. 붕어빵틀은 붕어빵 모양을 만들도록 하는
모형틀인 것이지 붕어빵의 개념이 아니다. 붕어빵에 들어가는 반죽과 팥 혹은 슈크림을 표현할 수 없는 것이다.

**즉, 클래스는 분류에 대한 개념이지 실체가 아니다. 하지만 객체는 실체다.** 클래스는 사람이고, 객체는 손흥민이다. 클래스는 펭귄이고, 객체는 뽀로로이다. 클래스와 객체를 구분하는 방법 중 하나는
나이를 물어보는 것이다.
- 사람의 나이는 몇 살인가? VS 손흥민의 나이는 몇 살인가?
- 펭귄의 나이는 몇 살인가? VS 뽀로로의 나이는 몇 살인가?

자바의 정석에서는 클래스를 일종의 설계도라고 표현하고 있다. 설계도는 해당 객체에 관한 정보들을 담고 있는 추상화된 개념이기에 붕어빵틀보다는 옳은 예시라고 생각한다. 어쨌든 클래스를 실체가 아닌 개념으로써 받아들여야
자바 프로그래밍에서 혼동하지 않게 된다. **우리가 실제로 자바에서 실행하는 많은 기능들은 클래스를 기반으로 생성된 객체 덕분이지 클래스 그 자체만으로 존재할 수는 없는 것이다.** (물론, 객체를 생성하지 않고 변수나 메서드를 사용하는
방법도 존재하긴 한다. - static)

![AnyConv com__002](https://github.com/whxogus215/JavaBookArchive/assets/70999462/ba8bfda6-84d3-4758-ac6e-4c094f0ecf86)

### 추상화: 모델링
**객체 지향의 추상화는 곧 모델링이다.** 추상화란 구체적인 것을 분해해서 관심있는 특성만 가지고 재조합하는 것이다. 우리는 구현하고자 하는 것을 하나의 클래스로 모델링, 추상화하게 되는데 여기서 클래스는 특정 객체를 통칭할 수 있는
집합적 개념(분류)이다. 그리고 클래스를 통해 생성된 객체에 대해 **인스턴스**라는 표현을 더 자주 사용한다. 우리가 클래스를 설계할 때는 UML 다이어그램을 보통 사용한다. 여기에는 해당 클래스가 갖는 여러 속성들이 정의된다.

만약, 사람이라는 클래스를 설계한다면 키, 몸무게, 혈액형, 나이, 그리고 각종의 행위들을 정의할 수 있다. 이처럼 사람을 정의할 수 있는 건 수백, 수천가지가 존재한다. 하지만 어플리케이션 개발자에게 있어서 사람이라는 클래스에 필요한
속성들은 유한할 것이다. 이 때 사용되는 개념이 **컨텍스트(경계)**이다. 내가 설계한 클래스가 어디에 사용될 지를 명확하게 구분하는 것이다. 만약 병원 어플리케이션을 만들고 있다면 사람이라는 클래스는 환자로 구체화 될 것이며,
은행 어플리케이션을 만든다면 고객으로 구체화될 것이다. 이처럼 추상화(모델링) 과정에서도 구체화되는 부분이 분명 존재한다. **즉, 추상화란 구체적인 것을 분해해서 관심 영역에 대한 특성만을 가지고 재조합한 것**이라 할 수 있다.

**추상화란 구체적인 것을 분해해서 관심영역(애플리케이션 경계)에 있는 특성만 가지고 재조합하는 것 = 모델링**

![AnyConv com__006](https://github.com/whxogus215/JavaBookArchive/assets/70999462/e9dcc877-3951-4903-9bb6-97820bb76340)

모델링한 클래스가 실제 코드로 동작할 때 메모리에서는 어떤 일이 벌어질까? 일단 클래스들의 놀이터인 스태틱 영역에 해당하는 클래스가 다 로딩된다. 만약, 이 때 인스턴스 멤버가 있을 경우 해당 변수들은 스태틱 영역에 저장공간을 갖지 않는다.
이들은 객체가 생성되어야 존재하는 영역이기 때문에 스태틱이 아닌 힙 영역에서 동적으로 생성되기 때문이다. 객체 생성에 따른 메모리의 변화는 해당 책을 참고하는 것이 더 낫다고 생각해서 중요한 포인트만 적도록 하겠다.

먼저, `Mouse mickey = new Mouse();`라는 코드가 있을 때, 여기서 Mouse라는 클래스는 이미 스태틱 영역에 로딩되어 있다. 그리고, new 생성자를 통해 실제 인스턴스를 생성하였고 이는 힙 영역에 저장된다. 그리고 저장된 메모리 주소를
`mickey`라는 참조 변수에 대입하였다. 이제부터는 `mickey`라는 변수를 통해 해당 객체에 접근할 수 있다. 이 후에 `mickey = null`로 하면 해당 참조변수는 생성된 객체를 참조하지 않는다. 그러면 가비지 컬렉터가 해당 인스턴스를 힙 영역에서
제거한다. 

![AnyConv com__008](https://github.com/whxogus215/JavaBookArchive/assets/70999462/e1806ce1-e782-41b0-9815-5fa7d9c56807)

![AnyConv com__011](https://github.com/whxogus215/JavaBookArchive/assets/70999462/e7eedd15-2749-4558-af5f-77dd7c23014a)

![AnyConv com__016](https://github.com/whxogus215/JavaBookArchive/assets/70999462/d48ac7be-d0aa-4cea-9844-7a352b4a8af8)

#### static을 사용하는 법 : 클래스 멤버 변수
우리가 사람이라는 클래스를 설계한다면, 해당 클래스로 생성된 객체들이 갖는 공통적인 특성이 분명 존재한다. 바로 눈의 개수, 귀의 개수, 팔다리의 개수 등등 인간으로서 갖는 공통적인 성질이 존재한다. 힙 영역에 생성된 여러 사람 인스턴스들이
똑같은 값들을 담는 변수를 여러 공간 생성해 놓는다면 이는 매우 비효율적이다. 즉, 객체마다 공통으로 갖는 특성을 클래스 멤버로 분류하는 것인데 이 때 `static`을 사용한다. 그러면 해당 객체를 생성하지 않고도 클래스 멤버에 접근할 수 있다.
main 메서드가 `public static`인 이유도 여기에 있다. JVM 구동 시 스태틱 영역에 바로 배치되는 것이 클래스 및 static 변수/메서드인데, main 메서드는 특정 객체 생성과 관계없이 실행될 수 있어야 하기 때문이다.

![AnyConv com__020](https://github.com/whxogus215/JavaBookArchive/assets/70999462/a8700335-7287-4578-8b02-d4a711325d79)

![AnyConv com__021](https://github.com/whxogus215/JavaBookArchive/assets/70999462/f15660b7-469a-4564-a3a0-2bc537d4a488)

따라서 클래스 멤버 변수의 경우, 스태틱 영역에서 클래스가 로딩될 때 저장공간도 같이 생긴다. 반면에 인스턴스 멤버의 경우는 생성되지 않는다. 지역변수는 사용자가 초기화 하지 않으면 쓰레기 값을 갖는다고 하였다. 하지만 클래스 변수 및 인스턴스 변수는 여러 메서드 혹은 객체가 공유하는 변수들이다. 따라서 이들을 누가 초기화할지 정하지 않고 자동으로 초기화하도록 설정되어 있다. 

- static 변수 : (= 클래스 [멤버] 속성, 정적 변수, 정적 속성), 스태틱 영역에 존재
- 인스턴스 변수 : (= 객체 [멤버] 속성, 객체 변수), 힙 영역에 존재
- local 변수 : (= 지역 변수), 스택 영역(스택 프레임 내부)에 존재

### 상속: 재사용 + 확장
상속이라고 하는 개념은 상속받는다는 말로 이해하면 혼동하기 쉽다. 객체 지향의 상속은 상속이 아닌 **재사용과 확장**으로 이해해야 한다. 실제로 자바에서 상속의 키워드는 `extends`이다.
상위 클래스가 동물이고, 하위 클래스가 포유류일 때, 동물이 포유류의 부모 개념이 아닌 것이다. 포유류는 동물을 좀 더 세분화하고 구체화한 개념(클래스)인 것이다. 동물이 가지고 있는 특성과 함께
포유류로써 갖는 특성을 추가하여 **확장(extends)**하는 것이다. 즉, 기존의 코드를 재사용하면서 확장하는 것이 상속의 주된 기능인 것이다.

![AnyConv com__024](https://github.com/whxogus215/JavaBookArchive/assets/70999462/c23dfa29-d325-4862-ac5d-e284a688a6cf)

**상위 클래스로 갈수록 추상화, 일반화됐다고 하며, 하위 클래스로 갈수록 구체화, 특수화 됐다고 말한다.** 상속관계에서 반드시 만족하는 문장이 있는데, 바로 **하위 클래스는 상위 클래스다.**
클래스 계층도가 동물<-포유류<-고래 일때, 포유류는 동물이다, 고래는 포유류다, 고래는 동물이다. 모두 말이 된다.

![AnyConv com__025](https://github.com/whxogus215/JavaBookArchive/assets/70999462/c6dc7f8e-8916-4d1e-affc-86b33e463bc5)

![AnyConv com__026](https://github.com/whxogus215/JavaBookArchive/assets/70999462/a80afbae-3b49-4eb9-8be0-f25b1a0ba035)

#### 객체 이름 명명 시 주의사항
클래스 이름은 클래스 답게, 객체 이름은 객체 답게 이름을 지어야 한다. 만약 Bird라는 클래스가 있다면 이를 바탕으로 생성되는 객체 명은 aBird, theBird 등이어야 한다.

상속의 큰 장점 중 하나는 기존의 코드를 반복해서 사용하지 않아도 된다는 것이다. 공통되는 부분은 상위 클래스로부터 가져올 수 있으며, 해당 변수/메서드 등이 해당된다. 또한 **하위 클래스는 상위 클래스다**를 만족하기 때문에
상위 클래스 타입의 참조변수로 하위 클래스 인스턴스를 참조할 수 있다. (ex. `동물 animal = new 포유류();`,`동물 whale = new 고래();`)

#### 상속은 is a 관계가 아닌 is a kind of 관계
- 김연아 is a 사람 -> 김연아는 한 명의 사람이다.
- 뽀로로 is a 펭귄 -> 뽀로로는 한 마리 펭귄이다.  
이처럼 상속의 관계를 단순히 `is a`로 표현하는 것은 와닿지 않고 무언가 어색하다. 하지만 이를 `is a kind of`로 바꾸면 어떻게 될까?
- 김연아 is a kind of 사람 -> 김연아는 사람 중 한 명이다.
- 뽀로로 is a kind of 펭귄 -> 뽀로로는 펭귄의 한 분류이다.

정리하자면 객체 지향의 상속은 다음 세 가지로 정의할 수 있다.
1. 객체 지향의 상속은 상위 클래스의 특성을 **재사용**하는 것이다.
2. 객체 지향의 상속은 상위 클래스의 특성을 **확장**하는 것이다.
3. 객체 지향의 상속은 **is a kind of** 관계를 만족해야 한다.

#### 다중상속 대신 선택한 방법 : 인터페이스
인터페이스는 보통 **~할 수 있는, be able to**의 의미를 갖고 있다. 실제로 자바 인터페이스 중에서는 `able`이 붙은 인터페이스가 많다. (ex. Comparable, Runnable)
상속과는 다르게 특정 클래스들이 공통적으로 갖는 부분을 인터페이스로 묶어주면, 유지 보수에 용이하며 확장성 있는 코드를 만들 수 있다.

![AnyConv com__026](https://github.com/whxogus215/JavaBookArchive/assets/70999462/a80afbae-3b49-4eb9-8be0-f25b1a0ba035)

![AnyConv com__029](https://github.com/whxogus215/JavaBookArchive/assets/70999462/87564503-3fa0-42e9-8cfa-a182eff56ac3)

#### 상속과 메모리
만약, 동물이라는 클래스와 포유류라는 클래스가 있고, 포유류의 상위 클래스가 동물 클래스라고 하자. 이 때, 포유류 객체를 생성한다면 힙 영역에는 동물 클래스와 포유류 클래스가 같이 생성된다.
즉, 상위 클래스 인스턴스와 하위 클래스 인스턴스가 같이 생기는 것이다. 물론 최상위 클래스인 Object 클래스의 인스턴스도 함께 생성된다.

![AnyConv com__030](https://github.com/whxogus215/JavaBookArchive/assets/70999462/8594f7ca-8462-433b-8bed-3c4cdcc7e004)

![AnyConv com__031](https://github.com/whxogus215/JavaBookArchive/assets/70999462/1cdcbabb-f8a9-4677-b640-6f9304de7636)

### 다형성: 사용편의성
보통 다형성이라고 하면 상위 클래스와 하위 클래스에 관한 걸 얘기하지만, 이 책에서는 오버로딩과 오버라이딩을 객체 지향의 다형성이라 이야기하고 있다.
- 오버라이딩 : 상위 클래스의 메서드를 같은 이름과 인자로 구현부를 바꾸는 것
- 오버로딩 : 같은 이름과 다른 인자로 메서드를 여러 개 구현하는 것  
둘의 기능은 다르지만 어쨌든 개발자의 편의성을 고려해 생긴 기능이라는 것이다. 우리는 하위 클래스의 인스턴스 타입이 상위 클래스 인스턴스 타입으로 형변환 하는 경우, 형변환이 생략가능함을 알고 있다.
그 이유는 하위 클래스 인스턴스가 갖고 있는 멤버가 더 많기 때문에 상위 클래스 타입으로 형변환하는 것이 보다 안전하기 때문이다.

```java
public class Parent {
    public void show() {
        System.out.println("I'm a Parent");
    }
}

public class Child extends Parent {
    @Override
    public void show() {
        System.out.println("I'm a Child ");
    }
}

public static void main(String[] args) {
        Parent parent = new Parent();
        Child child = new Child();

        parent.show();
        child.show();

        System.out.println();

        Parent child2 = new Child();
        child2.show();

        Parent parent3 = new Parent();
        parent3.show();

    }
```
만약, `Parent child2 = new Child();`처럼 부모 타입 참조변수로 자식 인스턴스를 참조하더라도 실제로 호출되는 메서드는 자식(하위) 클래스의 메서드가 호출된다. 이 점을 기억해야 한다.
오버라이딩의 경우, **아무리 부모 타입의 참조변수를 사용했다고 하더라도 실제 인스턴스 타입의 메서드가 호출된다.** 물론 상위 클래스 타입의 참조변수를 사용하면, 하위 클래스의 멤버를 호출할 수는 없다.
하지만 **오버라이딩의 경우엔 상위 클래스든 하위 클래스든 어쨌든 메서드가 반드시 존재한다. (상위 클래스의 메서드를 하위 클래스가 재정의하는 것이기 때문에 둘 다 같은 메서드가 있을 수 밖에 없다.)**
따라서 오버라이딩의 경우에는 실제 인스턴스 타입의 메서드가 호출되는 것이다. (오버라이딩에 의해 상위 클래스의 메서드가 가려졌다고 이해하기)

```java
class Animal
class Penguin extends Animal

Penguin pororo = new Penguin();
Animal pingu = new Penguin();
```

![AnyConv com__034](https://github.com/whxogus215/JavaBookArchive/assets/70999462/ce4df669-74ae-429d-9cd1-4c9379131619)

![AnyConv com__035](https://github.com/whxogus215/JavaBookArchive/assets/70999462/75a21e0b-d2f8-426f-a428-11c5b2b0e8a8)

**만약 `Animal pororo = new Animal();` 이었다면, 힙 영역에 Penguin 인스턴스는 생성되지 않았을 것이다. 따라서 이 때는 `showName()`이 Animal의 `showName()`이 호출될 것이다.**
다만 `Animal pororo = new Penguin();` 이나 `Penguin pororo = new Penguin();`일 경우엔 Animal과 Penguin 인스턴스가 같이 힙 영역에 생성된다. **따라서 이 때는 오버라이딩 된 Penguin의 `showName()`에 의해
기존 Animal의 `showName()`이 가려지게 된다.** 따라서 이 때는 Penguin의 `showName()`이 호출되는 것이다.

그렇다면 이 점을 활용하여 우리는 상위 클래스 타입으로 참조변수를 사용하여, 여러 하위 클래스 타입의 메서드들을 호출할 수 있을 것이다.
```java
class Driver {
  public static void main(String[] args){
    동물[] 동물들 = new 동물[5];

    동물들[0] = new 쥐();
    동물들[1] = new 고양이();
    동물들[2] = new 강아지();
    동물들[3] = new 송아지();

    for(int i = 0; i < 동물들.length; i++){
      동물들[i].울어보세요();
    }
  }
}
```
이처럼 `쥐, 고양이, 강아지, 송아지` 클래스가 동물 클래스의 하위 클래스라면, **우리는 오버라이딩된 `울어보세요()`라는 메서드를 상위 클래스 타입인 `동물` 타입 단 하나로 호출이 가능하다.**
이것이 바로 객체지향의 다형성이 사용자에게 편의성을 제공하는 부분이다. 특히, 클래스를 설계할 때 상속을 사용해야 하는 이유에 대해 고민할 때 이와 같은 내용을 떠올려보고 적용해보도록 하자.

### 캡슐화: 정보 은닉
자바에서 정보 은닉이란 접근 제어자인 `private, default(생략), protected, public`이 생각날 것이다. 이들을 사용함으로써 객체의 멤버를 외부로부터 공개할 것인지 말것인지 결정할 수 있는 것이다.
자신의 멤버가 아닌 다른 객체의 멤버에 접근하려는 경우에는 다른 객체를 반드시 생성한 후 접근해야 한다.

#### 접근 제어자 protected에 대한 사실
접근 제어자 protected를 사용할 경우, 자신과 상속관계인 하위 클래스에서 접근이 가능하다. 그러나 이 뿐만 아니라 같은 패키지 내에 있는 다른 클래스들도 접근이 가능하다. 따라서 습관적으로 private이나 public만 사용하는
경우, 이 protected 제어자 사용을 한 번 생각해볼 필요가 있다. 또한 다른 jar 파일 내에서 같은 이름의 패키지를 가질 경우, 서로 다른 jar 파일 내 객체들끼리 접근 제한이 없어진다. 즉, 같은 패키지 이름을 갖게 될 경우
서로 접근할 수 있기 때문에 보안 관련 이슈가 생길 수 있다. protected 사용에 있어서 충분히 고려해볼만한 문제이다.

#### 정적 멤버변수에 접근할 때는 클래스명으로 접근할 것
static을 사용할 경우, 모든 인스턴스가 공통으로 갖는 멤버를 지정하게 된다. 따라서 객체를 생성하지 않고도 해당 정적 멤버에 접근이 가능하다. 이 때, 정적 멤버변수에 접근하기 위해서 `인스턴스명.멤버변수`가 아닌 `클래스명.멤버변수`
로 접근할 것을 권장한다. 사실 여기에는 메모리의 물리적 접근에 따른 이유도 존재한다.

![AnyConv com__039](https://github.com/whxogus215/JavaBookArchive/assets/70999462/549e887b-4697-4d51-9f2f-25a174564915)

#### 참조변수의 복사
기본 자료형의 경우 매개변수로 값이 넘어갈 때, 변수에 저장된 값이 넘어간다고 하였다. 하지만 객체를 저장하는 참조 변수를 매개변수로 넘기게 되면 그 값이 해당 객체의 주소가 된다. 따라서 그 주소가 넘어가기 때문에
일종의 포인터가 넘어가게 되는 것이므로 값을 수정하면 실제 메모리의 값이 변경된다. 즉, 이를 **Call By Reference(참조에 의한 호출)**이라고 칭한다. 하지만 Call By Value와 큰 차이가 없다. 어쨌든 둘 다 해당 인자로 넘어간
변수의 값이 복사되는 것이다. 다만 기본 자료형의 경우에는 변수에 들어가 있는 것이 주소가 아닌 리터럴 값인 거고, 객체 참조 변수 자료형의 경우 변수에 들어가 있는 것이 주소 값인 것 뿐이다.

결국 Call By Value와 Call By Reference를 다르다고 이해하기보다는 기본 자료형 변수는 저장하고 있는 값을 그 값 자체로 판단하고, 참조 변수는 저장하고 있는 값을 주소(포인터)로 판단한다고 이해하는 것이 더 쉽다.
**기본 자료형 변수를 복사할 때, 참조 자료형 변수를 복사할 때 일어나는 일은 같다. 즉, 가지고 있는 값을 그대로 복사해서 넘겨 준다.**


## 자바가 확장한 객체 지향

### abstract 키워드
**추상 메서드란 선언부만 있고, 구현부는 없는 메서드를 말한다.** 처음 자바의 정석으로 이 개념을 접했을 때는 이것이 과연 필요한지 몰랐다. 하지만 실제로 구현된 자바의 여러 클래스들이 해당 키워드를 사용하고 있으며,
이 책의 예제를 통해서도 그 필요성에 대해 알 수 있었다.

```java
class 동물 {
    void 울어보세요() {
        System.out.println("나는 동물! 어떻게 울어야 하나요?");
    }
}

class 병아리 extends 동물 {
    @Override
    void 울어보세요() {
        System.out.println("나는 병아리! 삐약! 삐약!");
    }
}

class 쥐 extends 동물 {
    @Override
    void 울어보세요() {
        System.out.println("나는 쥐! 찍! 찍!");
    }
}

public class Driver {
    public static void main(String[] args) {
        동물[] 동물들 = new 동물[3];

        동물들[0] = new 쥐();
        동물들[1] = new 병아리();
        동물들[2] = new 동물();

        for (int i = 0; i < 동물들.length; i++) {
            동물들[i].울어보세요();
        }
    }
}

///
나는 쥐! 찍! 찍!
나는 병아리! 삐약! 삐약!
나는 동물! 어떻게 울어야 하나요?
```
이와 같이 상위 클래스 타입으로 하나의 메서드를 호출했을 때, 다 다른 결과 값이 나온다. 그 이유는 오버라이딩한 메서드가 존재하기 때문이다. 각각 상속받은 클래스에서 구현한 메서드가 실행된다.
하지만 이때, 상위 클래스인 동물 클래스의 메서드에는 하나의 오류가 있다. 동물은 종마다 우는 방법이 다른데, 동물이라는 하나의 객체에 대해 울음을 정의할 수 없다. 따라서 동물 클래스의 `울어보세요()`는 실제로
사용되지는 않을 것이다. 하지만 상속받는 클래스들에서는 분명 쓰임새가 있기 때문에 상위 클래스에서도 분명 존재해야 하는 메서드이다. **바로 이럴 때, 추상 메서드로 사용이 되는 것이다.** 메서드 선언은 하되, 구현은 되어 있지
않기 때문에 실제로 사용이 되지는 않으며 단지 이에 대한 오버라이딩을 통해 구현되기 위한 하나의 몸통만을 제공할 뿐이다.

```java
abstract class 동물 {
    abstract void 울어보세요();
}

class 병아리 extends 동물 {
    @Override
    void 울어보세요() {
        System.out.println("나는 병아리! 삐약! 삐약!");
    }
}

class 쥐 extends 동물 {
    @Override
    void 울어보세요() {
        System.out.println("나는 쥐! 찍! 찍!");
    }
}

public class Driver {
    public static void main(String[] args) {
        동물[] 동물들 = new 동물[2];

        동물들[0] = new 쥐();
        동물들[1] = new 병아리();

        for (int i = 0; i < 동물들.length; i++) {
            동물들[i].울어보세요();
        }
    }
}

///
나는 쥐! 찍! 찍!
나는 병아리! 삐약! 삐약!
```
이처럼 추상 메서드를 사용하면 실제로 클래스를 만들 수도 없기 때문에 실행될 수 없다. 추상 메서드를 갖는 순간 해당 클래스는 반드시 추상 클래스여야 하기 때문이다. `abstract`를 사용할 경우 다음과 같은 이점이 있다.
- 상위 클래스 타입의 참조 변수로 하위 클래스들의 인스턴스를 참조할 수 있다.
- 각각의 하위 클래스에서 오버라이딩된 메서드를 하나의 메서드로 호출이 가능하다.
- 생성되어서는 안 될 클래스에 대한 걱정을 하지 않아도 된다. (추상 클래스는 인스턴스로 생성이 불가능하다.)

### 생성자
클래스의 인스턴스, 즉 객체를 만들 때마다 **new** 키워드를 사용한다. ex) `동물 뽀로로 = new 동물();`  
**이렇게 객체를 생서하는 생성자도 하나의 메서드이다.** 따라서 생성자 안에도 다양한 매개변수가 올 수 있는 것이다. 만약 클래스에서 생성자 메서드를 정의하지 않으면 기본 생성자를 자동으로 만들어준다.
**주의해야할 점은 클래스 내에서 생성자를 만든 상태에서 기본 생성자를 호출하면 오류가 발생한다. 기본 생성자가 생성되는 조건은 클래스에서 정의한 생성자 메서드가 없을 때이다.**

### 클래스 생성 시 실행되는 static 블록
static 블록을 사용하면 클래스가 스태틱 영역에 로딩될 때 실행된다.

```java
class 병아리 {
    static {
        System.out.println("병아리 클래스 레디 온!");
    }
}

public class Driver {
    public static void main(String[] args) {
        병아리 삐약 = new 병아리();
        System.out.println("main 메서드 시작");
    }
}

///
병아리 클래스 레디 온!
main 메서드 시작
```
병아리 객체가 생성이 되었기 때문에 스태틱 영역에 병아리 클래스가 로딩되면서 스태틱 블록이 실행되는 것이다. 하지만 병아리 클래스를 생성하지 않는 경우, 클래스에 정의된 스태틱 블록은 실행되지 않는다. 왜냐하면 클래스가 사용되지 않으면
스태틱 영역에 로딩되지 않기 때문에 당연히 정의된 스태틱 블록도 실행되지 않는 것이다. **앞서서 모든 클래스가 실행되기 전에 스태틱 영역에 로딩된다고 하였지만 실제로는 클래스가 처음으로 사용되는 시점에서 로딩되는 것이 맞다.**

만약 동일한 클래스를 여러번 생성하더라도 스태틱 블록은 **단 한번만 실행된다.**

```java
class 병아리 {

    static int age = 10;

    static {
        System.out.println("병아리 클래스 레디 온!");
    }
}

public class Driver {
    public static void main(String[] args) {
        System.out.println("main 메서드 시작");
        System.out.println(병아리.age);
    }
}

///
main 메서드 시작
병아리 클래스 레디 온!
10
```
이처럼 직접 클래스를 생성하지 않고, 클래스의 정적 멤버에 접근하는 경우에도 마찬가지로 스태틱 블록이 실행된다. 클래스의 정적 멤버에 접근한다는 것은 해당 클래스가 스태틱 영역에 로딩된 다음 사용이 되는 것이므로, 당연히 그 시점에서
스태틱 블록도 실행이 될 수 밖에 없는 것이다.

스태틱 영역에 데이터가 올라가게 되면 프로그램이 종료될 때까지 계속 메모리에 존재한다. 따라서 자바 입장에서는 메모리를 효율적으로 관리하기 위해 클래스가 사용되는 시점에 로딩을 하는 것이다. 사용이 되지 않는 클래스들을 전부 메모리에 로딩하면
분명 메모리 낭비가 될 것이다. 자바는 이러한 점을 고려하여 클래스가 생성되는 시점에 메모리에 로딩되도록 설계되어 있다.

### final
**final**은 클래스, 메서드, 변수에 붙을 수 있다.
- final 클래스 : 상속을 할 수 없다.
- final 메서드 : 오버라이딩을 할 수 없다.
- final 변수 : 초기에 값이 정해지면 그 뒤로 변경이 불가능하다.

### instanceof 연산자
`instanceof`의 반환 값은 true 혹은 false이다. 해당 객체가 어떤 타입인지를 확인하는 용도로 사용되는 것이다.

```java
class 쥐 extends 동물 {
    @Override
    void 울어보세요() {
        System.out.println("나는 쥐! 찍! 찍!");
    }
}

public static void main(String[] args) {
        쥐 미키 = new 쥐();
        
        System.out.println(미키 instanceof 쥐);
        System.out.println(미키 instanceof 동물);
    }

///
true
true
```
이처럼 상위 클래스에 대한 검증도 true로 반환된다.

```java
class 쥐 extends 동물 {
    @Override
    void 울어보세요() {
        System.out.println("나는 쥐! 찍! 찍!");
    }
}

public static void main(String[] args) {
        동물 미키 = new 쥐();
        
        System.out.println(미키 instanceof 쥐);
        System.out.println(미키 instanceof 동물);
    }

///
true
true
```
이처럼 상위 클래스 타입의 참조변수라 할지라도 동일한 값이 나온다. 그 이유는 **instanceof 연산자가 검증하는 것은 참조변수의 타입이 아닌 실제 인스턴스의 타입이기 때문이다.**

### package
package 키워드는 네임 스페이스(이름공간)를 만들어주는 역할을 한다. 네임 스페이스는 다른 언어에서도 많이 쓰인다. (ex. c++) 만약 동일한 이름을 갖는 클래스가 있을 경우,
이들을 구분하는 기능이 반드시 필요하다. 만약 겹치는 이름이 Customer라면, 패키지 이름에 따라 A.Customer와 B.Customer로 나뉠 수 있는 것이다. 이것이 네임 스페이스가 갖는 의미이다.
![002](https://github.com/whxogus215/JavaBookArchive/assets/70999462/9a8c1472-f29e-4471-ae94-98eb96a40219)

### 실제로 호출되는 것은 클래스의 메서드
```java
class 펭귄 {
  void test() {
    System.out.println("Test");
  }
}

public class Driver {
    public static void main(String[] args) {
        펭귄 뽀로로 = new 펭귄();

        뽀로로.test();
    }
}
```
![005](https://github.com/whxogus215/JavaBookArchive/assets/70999462/a8847753-669e-4a74-a2e1-2f4c722b623c)
이처럼 실제 스택에 쌓이는 것은 객체명.메서드()가 아닌 클래스명.메서드()이다. 만약, 동일한 종류의 인스턴스가 100개가 생성된다면 같은 인스턴스 메서드가
각각의 힙 영역에 할당되어 똑같은 메서드가 100개가 존재할 것이다. 이는 비효율적이다. 이들의 차이점은 메서드에서 다루는 변수밖에 없다.
따라서 다음 그림처럼 **인스턴스의 메서드는 스태틱 영역에 단 한 개만 올라가게 된다.** 그리고 **해당 메서드를 호출할 때 객체 자신을 나타내는 this 참조 변수를 넘긴다.**  
(`뽀로로.test() -> 펭귄.test(뽀로로)`)

![007](https://github.com/whxogus215/JavaBookArchive/assets/70999462/21bf4893-0f33-481c-9566-56463161e44b)


## 자바8 람다와 인터페이스 스펙 변화
하나의 CPU에 여러 코어가 장착되는 멀티 코어 프로세스가 등장하고 진화함에 따라 소프트웨어도 그 흐름에 따라갈 필요가 있었다. 따라서 자바 8에서는 병렬화를 위한 컬렉션(배열, List, Set, Map)을 강화했고,
이러한 컬렉션을 효율적으로 사용하기 위해 스트림을 강화했다. 또 스트림을 효율적으로 사용하기 위해 함수형 프로그래밍이, 다시 함수형 프로그래밍을 위해 람다가, 또 람다를 위해 인터페이스의 변화가 수반됐다.  

**람다를 지원하기 위한 인터페이스를 함수형 인터페이스라고 한다.**
![001](https://github.com/whxogus215/JavaBookArchive/assets/70999462/158b1c0a-e341-4b13-be52-69b88710fcd9)

### 람다란 무엇인가?
**람다란 한 마디로 코드 블록이다.**(`{ }`) 기존에 코드 블록을 사용하기 위해서는 반드시 메서드가 필요했다. 따라서 메스드를 사용하기 위해 익명 객체를 생성해야 하는 경우가 있었다.
하지만 자바8부터는 코드 블록을 만들기 위해 이러한 번거로움을 수반하지 않아도 된다. 코드 블록을 람다식으로 사용할 수 있고, 이는 **매개변수 또는 반환 값**으로도 활용될 수 있다.

```java
public class B001 {

    public static void main(String[] args) {
        Runnable r = new MyTest();
        r.run();
    }
}

class MyTest implements Runnable {
    @Override
    public void run() {
        System.out.println("Hello Lambda!!!");
    }
}
```
```java
public class B001 {

    public static void main(String[] args) {

        Runnable r = new Runnable() {
            @Override
            public void run() {
                System.out.println("Hello Lambda 2!!!");
            }
        };

        r.run();
    }
}
```
이처럼 코드 블록을 사용하기 위해서는 별도의 클래스를 생성하거나 익명 클래스를 활용하였다. 하지만 자바 8에서는 익명 클래스를 활용하지 않고 코드 블록을 사용하도록 지원한다.
```java
public class B001 {

    public static void main(String[] args) {

        Runnable r = () -> {
            System.out.println("Hello Lambda 3!!!");
        };

        r.run();
    }
}
```
메서드의 몸통에 해당하는 코드 블록은 그대로이되, 객체 생성자 및 메서드 이름이 존재하지 않는다.
1. 람다식에 사용되는 참조변수 r의 타입이 `Runnable`이기 때문에 컴파일러가 `new Runnable()`임을 알아낼 수 있다. 따라서 코드로 작성하지 않아도 된다.
2. `Runnable` 인터페이스가 갖고 있는 추상 메서드가 **단 하나**이기 때문에 `run()`을 `()`이라고만 써도 이게 어떤 메서드인지 알 수 있다.
> 람다식은 (인자 목록) -> {로직}으로 구성되는데, 로직이 단 한 줄일 경우, {}을 생략할 수 있다.

### 함수형 인터페이스 
```java
@FunctionalInterface
public interface Runnable {
    /**
     * When an object implementing interface <code>Runnable</code> is used
     * to create a thread, starting the thread causes the object's
     * <code>run</code> method to be called in that separately executing
     * thread.
     * <p>
     * The general contract of the method <code>run</code> is that it may
     * take any action whatsoever.
     *
     * @see     java.lang.Thread#run()
     */
    public abstract void run();
}
```
이처럼 `Runnable` 인터페이스는 추상 메서드를 **단 한개만** 갖고 있다. 이처럼 추상 메서드를 한 개만 갖는 인터페이스를 자바 8부터는 **함수형 인터페이스**라고 부른다.
따라서 이러한 함수형 인터페이스만 람다식으로 변경할 수 있다.

```java
public class B002 {
    public static void main(String[] args) {
        MyFunctionalInterface mfi = (int a) -> { return a * a; };

        int b = mfi.runSomething(9);

        System.out.println(b);
    }
}

@FunctionalInterface
interface MyFunctionalInterface {
    public abstract int runSomething(int count);
}

///
81
```
함수형 인터페이스를 직접 만들 경우, `@FunctionalInterface` 어노테이션을 붙여야 한다. 이 어노테이션을 통해 해당 인터페이스가 추상 메서드를 단 한개만 갖고 있는지 확인하게 된다.
여기서 인터페이스의 추상 메서드인 `runSomething`의 매개변수가 int 타입 1개만 존재한다는 걸 알 수 있다. 따라서 사용 시 int를 생략할 수 있다. 이를 **타입 추정 기능**이라고 한다.
- `(int a) -> { return a * a; };`
  - `(a) -> { return a * a; };`
    - `a -> { return a * a; };` (인자가 하나이고 자료형을 표기하지 않을 경우 소괄호 생략 가능)
      - `a -> a * a ;` (로직이 단 한줄 일경우, 중괄호도 생략가능하며 return도 같이 생략해야 한다.)
     
이처럼 **생략이 가능한 이유는 인터페이스에서 추상 메서드를 단 한개를 정의했으며 정의한 내용에 반환 타입과 매개변수가 명시되어 있기 때문이다. 따라서 컴파일러는 이를 근거로 생략된 정보들을
알아낼 수 있는 것이다.**

```java
public class B002 {
    public static void main(String[] args) {
        MyFunctionalInterface mfi = a -> a * a;

        int b = mfi.runSomething(9);

        System.out.println(b);
    }
}

@FunctionalInterface
interface MyFunctionalInterface {
    public abstract int runSomething(int count);
}

///
81
```
코드 블록 뒤에 있는 세미 콜론(;)은 생략할 수 없다. 이처럼 생략했을 때, 훨씬 간편하게 작성할 수 있다. **람다식을 보게 되면 이에 관련된 함수형 인터페이스가 무엇인지 확인하는 것이 중요하다.**

### 람다식을 변수에 담아서 사용하기
지금까지 람다식을 인터페이스 타입의 참조 변수에 담아서 사용하였다. 그렇다는 것은 해당 **람다식을 특정 메서드의 매개변수로 활용할 수 있다는 뜻이다.**
```java
public class B002 {
    public static void main(String[] args) {
        MyFunctionalInterface mfi = a -> a * a;
        doIt(mfi);
    }

    public static void doIt(MyFunctionalInterface mfi) {
        int b = mfi.runSomething(8);

        System.out.println(b);
    }
}

@FunctionalInterface
interface MyFunctionalInterface {
    public abstract int runSomething(int count);
}
```
람다식을 통해 `int runSomething(int count)` 메서드가 구현이 되었다. 따라서 해당 참조변수를 통해 람다식을 사용할 수 있는 것이다.  

**만약, 람다식을 한 번만 사용한다면 굳이 변수에 할당하지 않고 사용할 수 있다.**
```java
public class B002 {
    public static void main(String[] args) {
        doIt(a -> a * a);
    }

    public static void doIt(MyFunctionalInterface mfi) {
        int b = mfi.runSomething(8);

        System.out.println(b);
    }
}

@FunctionalInterface
interface MyFunctionalInterface {
    public abstract int runSomething(int count);
}
```
많은 것이 생략되어서 뭐가 뭔지 모를 수 있다. 람다식을 봤을 때는 **특정 인터페이스의 추상 메서드가 구현된 부분이라고 생각하고, 람다식을 메서드로 치환해서 생각해보자!**
```java
doIt(a -> a * a) -> doIt(runSomething(int a { return a * a; });)
```

### 람다식을 반환값으로 사용하기
```java
public class B002 {
    public static void main(String[] args) {
        MyFunctionalInterface mfi = todo();

        int result = mfi.runSomething(3);
        System.out.println(result);
    }

    public static MyFunctionalInterface todo() {
        return num -> num * num;
    }
}

@FunctionalInterface
interface MyFunctionalInterface {
    public abstract int runSomething(int count);
}
```
**주의해야할 점은 람다식을 구현했다고 해서 해당 메서드가 사용된 것은 아니다!** 람다식은 함수형 인터페이스의 추상 메서드를 구현한 것일 뿐이지 실제로 실행하는 것은 아니다.
따라서 위 예제들처럼 람다식이 구현한 실제 추상 메서드를 호출함으로써 기능이 수행되는 것이다!

### 자바8 API에서 제공하는 함수형 인터페이스
자바8에서는 사용자들이 많이 쓸 것이라 생각하는 인터페이스를 `java.util.function` 패키지 등에서 제공하고 있다. 개발자들이 많이 사용할 것으로 예상되는 함수형 인터페이스들을
정의하고 있다. 물론, 함수형 인터페이스이기 때문에 실제 추상 메서드를 구현하는 것은 사용하는 개발자의 몫이다. 이들이 제공하는 것은 인터페이스 이름, 반환형, 매개변수, 추상 메서드 뿐이다.
![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/2df13652-8c9b-41a0-8074-97b81ff261ac)(https://joomn11.tistory.com/22)

이처럼 각각의 인터페이스마다 제공하는 추상 메서드의 형태가 다 다르다. 따라서 개발자가 필요에 맞는 인터페이스를 사용하여 구현하기만 하면 된다.
```java
public class B003 {
    public static void main(String[] args) {
        /*
        Predicate<Integer> pre = new Predicate<Integer>() {
            @Override
            public boolean test(Integer integer) {
                return true;
            }
        };
        */
        Predicate<Integer> pre = num -> num > 10;

        System.out.println(pre.test(13));
        System.out.println(pre.test(3));
    }
}
```
이전 예제들처럼 직접 함수형 인터페이스를 생성하지 않고 기존에 있는 인터페이스를 사용함으로써 코드가 더욱 간편해졌다. 함수형 인터페이스는 주석부분처럼 익명 클래스로 작성이 가능하며, 자바 8 이후로부터는
람다식을 사용해서 구현할 수도 있다. 구현부에 해당하는 `num -> num > 10`은 추상 메서드인 `boolean test(Integer integer)`를 구현한 것이다. num은 integer와 매핑되며, `num > 10`은 비교 연산자를 통해 True 혹은
False가 반환된다. **주의해야할 점은 자바8의 함수형 인터페이스를 사용하는 경우, 해당 인터페이스에 정의된 추상 메서드의 형태에 맞춰서 람다식을 작성해야 한다.** 만약 위 예제에서 `num > 10`이 아닌 `num + 10`으로 하면
반환 타입의 오류가 발생한다. 정의된 test 메서드의 반환타입은 boolean인데 int 타입으로 반환하는 람다식을 작성했기 때문이다.






