# Contents of 컬렉션 프레임웍
- [컬렉션 프레임웍](#컬렉션-프레임웍)
 - [Collection 인터페이스](#Collection-인터페이스)
 - [List 인터페이스](#List-인터페이스)
 - [Set 인터페이스](#Set-인터페이스)
 - [Map 인터페이스](#Map-인터페이스)




출처 : [https://github.com/castello/javajungsuk_basic](https://github.com/castello/javajungsuk_basic)

## 컬렉션 프레임웍
컬렉션(Collection)이란, 여러 데이터가 모인 집합 그 자체를 의미한다. 이는 데이터 그룹이라고 할 수 있으며 이들에 관한 표준화된 프로그래밍 방식을 제공하는 것이
**컬렉션 프레임웍**이다. JDK 1.2부터 컬렉션 프레임웍이 등장하고 다양한 컬렉션 클래스들을 표준화된 방식으로 다룰 수 있도록 체계화되었다.

### 컬렉션 프레임웍의 장점
컬렉션 프레임웍은 컬렉션(다수의 데이터)을 효과적으로 다룰 수 있도록 돕는 다양한 클래스들을 제공하기 때문에 많은 데이터를 다루기 편하다. 또한 **인터페이스와
다형성을 이용한 객체지향적 설계를 통해 표준화**되어 있기 때문에 사용법을 익히기도 편하고 **재사용성이 높은 코드를 작성**할 수 있다.

컬렉션 프레임웍이 프레임웍인 이유는 단순히 모듈화된 기능들을 제공하는 라이브러리에 그치지 않고, 프로그래밍 방식 자체를 정형화하였기 때문이다. 이를 통해 개발자들은
개발의 생산성을 높이고 유지보수를 용이하게 할 수 있다.

### 핵심 인터페이스
컬렉션 프레임웍은 크게 3가지의 인터페이스로 나뉘어 있다. List, Set, Map이며 그중 List와 Set은 공통된 부분을 다시 뽑아 Collection 인터페이스로 정의하였다.
이처럼 공통되는 부분을 추상화하여 인터페이스로 설계하는 것은 자바의 객체지향이 주는 좋은 장점 중 하나이다.

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/bccc1078-a35f-4252-8eaa-e422c8827c2b)

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/55fd477a-e6ce-438d-ada6-0ab6c35d34e5)

이처럼 List와 Set과 달리 Map은 Key-Value 형태로 전혀 다른 형태로 컬렉션을 다루기 때문에 같은 Collection 인터페이스에 속하지 않는다. **컬렉션 프레임웍의 모든
클래스들은 List, Set, Map 중 하나를 구현하고 있다.**

### Collection 인터페이스
![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/0e1cf150-e01c-47eb-97d8-9518b708286f)

Collection 인터페이스는 컬렉션 클래스에 저장된 데이터를 읽고, 추가하거나 삭제하는 메서드들을 정의하고 있다. List와 Set 인터페이스는 Collection 인터페이스를 상속하고 있으므로,
해당 메서드들을 똑같이 정의하고 있다. 그리고 이들을 구현한 구현체 클래스들이 정의된 추상 메서드들을 직접 구현하게 된다.

### List 인터페이스
![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/5cd7dbf9-c959-4d2d-8a20-77b357dcd2e0)

List 인터페이스는 **중복을 허용하고, 저장순서가 유지되는 컬렉션을 구현하는데 사용된다.** JDK 11(자바11) 기준으로 List 인터페이스를 구현한 컬렉션 클래스는 AbstractList, RoleList 등등이 추가로 더 있다.

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/21d6fbde-aaaf-422c-97bf-12d34f2183b2)

Collection 인터페이스로부터 상속받은 메서드를 제외하고도 추가로 List 인터페이스가 자체적으로 제공하는 메서드들도 존재한다. 즉, List 인터페이스를 구현하는 ArrayList, Stack 등은 Collection 인터페이스의 메서드와
List 인터페이스의 메서드 둘 다 구현이 되어있다.

### Set 인터페이스
Set 인터페이스는 **중복을 허용하지 않고, 저장순서가 유지되지 않는 컬렉션 클래스를 구현하는데 사용된다.** List와 차이점이 바로 여기에 있다. 순서가 보장되지 않으며 중복을 허용하지 않는다. JDK 11 기준으로 AbstractSet, EnumSet 등등이
추가로 더 구현되어 있다.

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/64bc405f-29e8-41e7-865b-da279046e5d8)
![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/70c394ad-bada-46ff-b30e-5f42137a3abe)

위 메서드들은 Collection 인터페이스를 상속받은 것으로 Collection 인터페이스 메서드와 동일하다.

### Map 인터페이스
Map 인터페이스는 **Key와 Value를 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현하는데 사용된다.** 키는 중복될 수 없지만 **값은 중복을 허용한다.** 따라서 중복된 키 값이 들어올 경우, 기존에 저장된 값은 없어지고 새로운 값으로
덮어쓰게 된다. 

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/3e7fba4f-fe57-452a-a80c-8c863c50a409)

`values()`의 반환타입은 Collection이고, `KeySet()`의 반환타입은 Set임을 알 수 있다. **Map 인터페이스는 값에 대해서는 중복을 허용하기 때문에 Collection 타입(List)으로 반환하고, 키는 중복을 허용하지 않기 때문에 Set 타입으로 반환하는 것이다.**
JDK 11 기준으로 AbstractMap, EnumMap 등 여러 컬렉션 클래스들이 추가로 구현되어 있다. List, Set과는 비교가 안될 정도로 많은 구현 클래스들이 Map 인터페이스에 존재한다.
