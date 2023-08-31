# Contents of 컬렉션 프레임웍
- [컬렉션 프레임웍](#컬렉션-프레임웍)
  - [Collection 인터페이스](#Collection-인터페이스)
  - [List 인터페이스](#List-인터페이스)
    - [ArrayList](#ArrayList)
    - [LinkedList](#LinkedList)
  - [Set 인터페이스](#Set-인터페이스)
  - [Map 인터페이스](#Map-인터페이스)
  - [스택과 큐](#스택과-큐)
- [Iterator](#Iterator)
  - [Map과 Iterator](#Map과-Iterator)
- [Arrays](#Arrays)
- [Comparator와 Comparable](#Comparator와-Comparable)
- [HashSet](#HashSet)
  - [equals()와 hashCode()](#equals--와-hashCode--)



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

## Iterator
`Iterator, ListIterator, Enumeration`은 모두 **컬렉션에 저장된 요소들에 접근하기 위해 사용되는 인터페이스이다.** Enumeration은 Iterator의 구버전이며, List를 구현한 클래스의 경우에만 ListIterator 호출이 가능하다.
컬렉션 프레임웍에서는 컬렉션에 저장된 요소들을 읽어오는 방법을 **표준화**하였다. 따라서 요소를 읽어오는 기능들을 정의한 인터페이스인 Iterator가 등장하게 되었고, Collection 인터페이스에는 Iterator를 구현한 클래스의
인스턴스를 반환하는 `iterator()`를 정의하고 있다.

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/2ca6ea7a-4490-4675-aba6-e16c00592676)
> Collection 인터페이스에 정의된 iterator()

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/84dc8263-0501-4afe-9a7e-497c4e0262ee)
> ArrayList 클래스에 구현된 iterator()

이처럼 컬렉션 클래스에는 iterator를 직접 구현하였고, 반환되는 클래스 또한 Iterator 인터페이스를 구현한 클래스이다. 따라서 `Iterator<E> iterator()`라는 메서드 정의만으로 다양한 종류의 클래스들을 반환할 수 있는 것이다.
(이것이 바로 객체지향의 장점 중 하나이다.)

그렇다면 왜 굳이 ArrayList 내에 있는 요소를 for문 혹은 Enhanced for문을 통해 데이터를 탐색할 수 있음에도 불구하고 왜 다음과 같은 Iterator라는 인터페이스를 사용하여 표준화한 것일까?
```java
List list = new ArrayList();
Iterator it = list.iterator(); // ArrayList 클래스에서 구현한 iterator()가 호출된다.

while(it.hasNext()){
  System.out.println(it.next()); // iterator 인스턴스가 지칭한 객체를 반환한다.
}
```
다음 코드에서 ArrayList가 아닌 다른 List 인터페이스의 구현 클래스를 사용해야 한다면 우리는 `new ArrayList()` 부분만 바꿔주면 된다. 각각의 구현 클래스마다 요소에 접근하는 방법이 다를지라도
저 Iterator를 통해 접근하는 방법 만큼은 **표준화되어 있기 때문이다.** 따라서 이와 같이 **인터페이스를 통해 표준화시킴으로써 코드의 일관성을 유지하고 재사용성을 극대화할 수 있는 것이다.**

### Map과 Iterator
Map 인터페이스를 구현한 클래스는 `iterator()`를 직접 호출할 수 없다. Map은 Key-Value 쌍 형태이기 때문에 이 엔트리를 저장한 `keySet()`,`entrySet()`을 통해 key와 value에 대한 각각의
Set을 얻은 뒤, iterator()를 호출해야 한다.
```java
Map map = new HashMap();
Iterator it = map.entrySet().iterator();
```
EntrySet과 KeySet 클래스는 내부에 Iterator를 갖고 있다. 따라서 Map에 대한 Iterator를 호출하기 위해서는 Set이 있어야 한다.

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/e8503218-47f7-43e1-a936-c75012398c66)

```java
public class Ex11_5 {
    public static void main(String[] args) {
        Map map = new HashMap();

        map.put(1, "11");
        map.put(2, "22");

        Iterator iterator = map.entrySet().iterator();

        while (iterator.hasNext()) {
            Object next = iterator.next();
            System.out.println(next);

            ///
            1=11
            2=22
        }

        Iterator iterator = map.keySet().iterator();

        while (iterator.hasNext()) {
            Object next = iterator.next();
            System.out.println(next);

            ///
            1
            2
        }
    }
}
```

## Arrays
Arrays 클래스에는 배열을 다루는데 유용한 메서드가 정의되어 있다. `toString()`의 경우에도, 매개변수가 boolean 타입부터 Object 타입까지 여러 메서드가 오버로딩되어 있으며,
**Arrays에 정의된 메서드는 모두 static 메서드이다. 따라서 Arrays 클래스를 생성하지 않고도 Arrays의 메서드들을 바로 사용할 수 있는 것이다.**

1. copyOf(), copyOfRange() : 배열 전체 혹은 배열의 일부를 복사해서 새로운 배열을 만들어서 반환하는 메서드
2. fill(), setAll() : 지정된 값으로 배열의 요소를 채우거나, 람다식을 활용하여 원하는 값으로 채울 수 있다.

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/3e2458c7-a745-4608-80a6-b98e84b7fc28)
> IntFunction의 apply()를 람다식을 통해 구현할 수 있으며, 구현된 내용을 토대로 배열의 값을 할당한다.

3. sort(), binarySearch() : 배열의 요소를 정렬하거나, 찾고자 하는 요소의 인덱스를 탐색할 수 있다.(이진탐색 : 정렬된 배열만 가능)
4. equals(), toString() : 배열의 요소가 같은지 비교하거나, 배열의 요소를 문자열로 출력한다.
   - 2차원 배열의 경우, deppEquals()를 사용한다. 2차원 배열을 equals()로 비교할 경우, 배열이 아닌 배열에 저장된 주소 값을 비교하기 때문이다.
5. asList() : List를 구현한 클래스를 List 타입으로 변환한다.

## Comparator와 Comparable
Comparator와 Comparable은 모두 **인터페이스이다.** 인터페이스는 저마다 특정한 기능을 수행하는 메서드들을 정의하고 있다. 이들은 **컬렉션을 정렬하기 위해
필요한 메서드를 정의하고 있다.**

![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/89d64321-541f-41ec-82b3-dda116a67830)
![image](https://github.com/whxogus215/JavaBookArchive/assets/70999462/1fc02714-dc78-48d2-a7de-268770ab0270)

`Comparable`은 `java.lang`에 있으며, `Comparator`는 `java.util`에 있다. `Comparable`과 달리 `Comparator`는 함수형 인터페이스이며, 다양한 메서드들이 정의되어 있다.
즉, Comparator를 사용하면 람다식을 통해 보다 간결화된 구현이 가능하다는 뜻이다. 어쨌든 두 인터페이스의 비교 메서드는 두 객체를 비교하여 정수 값을 반환하는 것을 알 수 있다.

- Comparable : 기본 정렬기준을 구현하는데 사용
- Comparator : 기본 정렬기준 외에 다른 기준으로 정렬하고자 할 때 사용(Comparable이 구현되어 있는 클래스에서 추가적으로 정렬 기준을 만들고 싶을 때 사용)

### 정리
`Comparable`과 `Comparator`는 **정렬에 대한 비교 기준을 제시하는 방법이다.** 이들이 구현된 클래스들은 일반적으로 **오름차순을 기준으로 되어 있다.**
따라서 특정 클래스가 `Comparable` 혹은 `Comparator`를 구현한 클래스는 **정렬이 가능하다는 뜻이다.**

```java
public class Ex11_7 {
    public static void main(String[] args) {
        String[] strArr = {"cat", "Dog", "lion", "tiger"};

        Arrays.sort(strArr);
        System.out.println("strArr=" + Arrays.toString(strArr));

        Arrays.sort(strArr,String.CASE_INSENSITIVE_ORDER);
        System.out.println("strArr=" + Arrays.toString(strArr));

        Arrays.sort(strArr,new Descending());
        System.out.println("strArr=" + Arrays.toString(strArr));

    }

}

class Descending implements Comparator {

    @Override
    public int compare(Object o1, Object o2) {
        if (o1 instanceof Comparable && o2 instanceof Comparable) {
            Comparable c1 = (Comparable) o1;
            Comparable c2 = (Comparable) o2;
            return c1.compareTo(c2) * -1; // String 클래스가 구현한 Comparable의 compareTo 메서드 : 오름차순
        }
        return -1;
    }
}
```
이와 같이 기존에 구현된 `Integer` 클래스의 `compareTo()`는 오름차순을 기준으로 구현되었다. 따라서 이를 내림차순으로 하고 싶으면 * -1을 하는 것이다. 정렬함수는
비교함수(`Comparable`과 `Comparator`에서 구현해야 하는 메서드)를 호출함으로써 반환 값을 토대로 정렬을 수행한다. **이미 정렬에 관련된 메서드들은 자바에서 다 구현이 되어있다.
따라서 구현된 내용의 스펙에 따라 반환 값을 설정만 해주면 된다.(비교 기준 제시) 얼마나 간편한가!**

## HashSet
**HashSet은 Set 인터페이스를 구현한 가장 대표적인 컬렉션이며, Set 인터페이스의 특징을 이어받아 중복된 요소를 저장하지 않는다.** HashSet은 저장순서를 유지하지 않으므로 저장순서를
유지하길 원한다면 **LinkedHashSet을 사용해야 한다.**

```java
public class Ex11_10 {
    public static void main(String[] args) {
        Set set = new HashSet();
        for (int i = 0; set.size() < 6; i++) {
            int num = (int)(Math.random() * 45) + 1;
            set.add(new Integer(num));
        }
        List list = new LinkedList<>(set);
        Collections.sort(list, new DescComp2());
        System.out.println(list);
    }
}

class DescComp2 implements Comparator {
    @Override
    public int compare(Object o1, Object o2) {
        if(!(o1 instanceof Integer && o2 instanceof Integer))
            return -1;

        Integer i = (Integer) o1;
        Integer i2 = (Integer) o2;
        return i.compareTo(i2) * -1;
    }
}
```
이처럼 정렬을 하기 위해서는 List 타입에 담아야 한다. 따라서 컬렉션을 매개변수로 한 생성자를 사용하여 `LinkedList`를 만들었고, List는 Collection에 속하기 때문에 `Collections`의 sort 함수를 사용한다.
이 때 마찬가지로 Comparator를 통해 새로운 정렬 기준을 제공하는 클래스를 생성하여 매개변수로 전달할 수도 있다. 위 코드의 경우, `Integer`는 기존에 Comparable을 구현한게 있었기 때문에 추가로 비교 기준을
만들기 위해 Comparator를 구현하였다.

### equals()와 hashCode()

```java
public class Ex11_11 {
    public static void main(String[] args) {
        Set set = new HashSet();
        set.add("abc");
        set.add("abc");
        set.add(new Person("David", 10));
        set.add(new Person("David", 10));

        System.out.println(set);
    }
}

class Person {
    String name;
    int age;

    public Person(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return name + ":" + age;
    }

    @Override
    public boolean equals(Object o) {
        if(!(o instanceof Person)) return false;
        Person p = (Person) o;
        return name.equals(p.name) && age==p.age;
    }

    @Override
    public int hashCode() {
        return Objects.hash(name, age);
    }
}
```
이는 HashSet에 여러 객체를 저장하였고, 중복되는 값들을 추가하지 않도록 하였다. 이 때 "abc"의 경우 문자열 리터럴이기에 문자열 내용이 같으면 같은 값으로 판단하므로 중복으로 추가되지 않는다.
다만 Person 클래스의 경우 사용자가 만든 클래스이기에 인스턴스를 비교하기 위한 기준이 마련되어 있지 않다. HashSet의 add 메서드는 **새로운 요소를 추가하기 전에 기존의 요소와
같은 것인지 판단하기 위해 `equals()`와 `hashcode()`를 호출한다.** 따라서 이 두 메서드에 대한 오버라이딩을 통해 인스턴스가 어떠한 조건에서 같은지 정의해야 한다.

이처럼 `equals()`와 `hashCode`는 Object 메서드로 얼마든지 오버 라이딩이 가능하다. 이 둘은 인스턴스를 비교하기 위해 하나의 묶음으로 존재하며, String이나 Integer 등등 자바 API 클래스들은
저 두 메서드가 알맞게 구현되어 있다.

```java
  Iterator it = setB.iterator();
  while (it.hasNext()) {
      Object tmp = it.next();
      if (setA.contains(tmp)) {
          setKyo.add(tmp);
      }
  }
```
**Set 인터페이스는 Iterator를 사용하여 해당 요소를 순회할 수 있다. 이는 컬렉션에서 요소를 탐색하는 방법을 표준화한 것이다.**




