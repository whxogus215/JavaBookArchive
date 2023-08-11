# Contents of 컬렉션 프레임웍
- [컬렉션 프레임웍](#컬렉션-프레임웍)
  - [Collection 인터페이스](#Collection-인터페이스)
  - [List 인터페이스](#List-인터페이스)
    - [ArrayList](#ArrayList)
    - [LinkedList](#LinkedList)
  - [Set 인터페이스](#Set-인터페이스)
  - [Map 인터페이스](#Map-인터페이스)
  - [스택과 큐](#스택과-큐)




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

### ArrayList
ArrayList는 컬렉션 프레임웍 중 가장 많이 사용되는 **컬랙션 클래스**이다. List 인터페이스를 구현하기 때문에 순서가 유지되고, 중복을 허용한다. ArrayList는 기존의 Vector 클래스를 개선하였기 때문에 웬만하면 Vector보다는
ArrayList 사용을 권장한다. (Vector는 구버전 코드의 호환성을 위해 남겨져있다.)

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/552be761-a6b9-4e13-98f8-676839a804ac)

이처럼 ArrayList는 내부에 Object형 배열을 갖고 있기 때문에 모든 객체를 담을 수 있으며, 데이터의 순서는 0번부터 차례대로 저장된다.

ArrayList의 경우, 추가와 삭제가 기존의 배열 자료구조의 특징을 따른다. 즉, 순차적으로 데이터를 추가하거나 맨 끝에서부터 데이터를 삭제할 때는 O(1)이지만, 중간에 데이터를 삽입하거나 삭제하는 경우, O(N)의 시간복잡도가 소요된다.

### LinkedList
앞서 ArrayList의 경우, 배열의 특성을 따르기 때문에 **데이터의 중간 부분의 추가 및 삭제에 시간이 많이 걸린다는 단점이 있다. 하지만 연결 리스트, LinkedList를 사용하면 그 문제가 해결된다.** 또한 배열과 달리 유기적으로 데이터를 추가할 수 있다. 새로운 데이터를 맨 끝에 이어붙이기만 하면 되기 때문이다.**(배열의 경우, 새로운 크기의 배열을 생성하여 복사해야 한다.)** 

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/577c8fcf-420e-4736-b519-d25106558499)

이처럼 연결 리스트는 배열과 다릴 물리적으로 데이터가 차례대로 저장되어 있지 않기 때문에 자유롭게 수정이 가능하다. 하지만 데이터를 순회하는 경우에는 배열보다 시간이 더 많이 걸린다는 단점이 있다.

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/d7580aab-1d35-4dd2-bcbb-e9a82bc91bd6)

이처럼 배열의 경우, 특정 데이터의 주소를 찾는 방법은 매우 간편하다. 하지만 연결 리스트의 경우, 처음부터 n번째 데이터까지 차례대로 따라가야지 원하는 값을 얻을 수 있다.

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/175c586a-2f7d-405a-b5c1-58f7939eb17f)

따라서 순차적으로 데이터를 저장하고 삭제하는 **Stack 자료구조의 경우, ArrayList로 구현하는 것이 적합하다.** 들어오는 곳과 나가는 곳이 동일하기 때문이다. 하지만 **Queue의 경우,** 뒤에서 삽입되어 앞으로 나가기 때문에 ArrayList보다는
추가,삭제가 쉬운 **LinkedList로 구현하는 것이 더 적합하다.**

### 스택과 큐
자바에서는 스택을 하나의 클래스로 만들어 두었다. Vector 클래스를 상속받았기에, 해당 타입의 참조변수로 바로 생성이 가능하다. 하지만 큐는 인터페이스 타입으로 존재하며 이를 구현한 다양한 큐 클래스가 존재한다.

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/68a4c5b8-8730-4094-9ed9-9441134e1140)

```java
public class Ex11_2 {
    public static void main(String[] args) {
        Stack st = new Stack();
        Queue q = new LinkedList(); // Queue 인터페이스의 구현 클래스 중 하나인 LinkedList 사용

        st.push("0");
        st.push("1");
        st.push("2");

        q.offer("0");
        q.offer("1");
        q.offer("2");

        System.out.println("= Stack =");
        while (!st.empty()) {
            System.out.println(st.pop());
        }

        System.out.println("= Queue =");
        while (!q.isEmpty()) {
            System.out.println(q.poll());
        }
    }
}
```
이처럼 큐를 사용할 경우, 해당 인터페이스를 구현한 구현 클래스를 생성자로 사용해야 한다. (인터페이스 타입의 참조변수를 사용하면 다형성을 통해 얼마든지 다른 구현체로 손쉽게 변경이 가능)











