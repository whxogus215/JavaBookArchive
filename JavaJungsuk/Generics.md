# Contents of 지네릭스
- [지네릭스](#지네릭스)
  - [지네릭스의 장점](#지네릭스의-장점)
  - [타입 안정성](#타입-안정성)
- [타입 변수](#타입-변수)
- [지네릭스 용어 정리](#지네릭스-용어-정리)
- [지네릭 타입 일치와 다형성](#지네릭-타입-일치와-다형성)
- [Iterator<E>](#Iterator<E>)
- [Hashmap<K,V>](#Hashmap<K,V>)
- [지네릭 클래스를 제한하는 방법 : T extends ](#지네릭-클래스를-제한하는-방법)
  



출처 : [https://github.com/castello/javajungsuk_basic](https://github.com/castello/javajungsuk_basic)

## 지네릭스
지네릭스는 JDK 1.5부터 도입된 기능이다. 따라서 자바 8 혹은 그 이상의 문서들에도 이 지네릭 기능이 도입되어 있다. 따라서 지네릭의 기능들을 모르면
공식문서의 클래스나 메서드들을 읽을 때 어려움을 겪을 수 있을 것이다. **지네릭스는 다양한 타입의 객체들을 다루는 메서드나 컬렉션 클래스에 컴파일 시 타입 체크를 해주는 기능이다.**
객체에 대한 타입은 **컴파일 시에 체크하기 때문에** 타입의 안정성을 높이고, 형변환의 번거로움을 줄인다.

```java
  ArrayList<Tv> tvList = new ArrayList<Tv>();

  tvList.add(new Tv()); // 가능
  tvList.add(new Audio()); // tvList는 Tv 클래스만 허용하기 때문에 컴파일 에러 발생!
```
```java
ArrayList tvList = new ArrayList();
tvList.add(new Tv());
Tv t = (Tv)tvList.get(0);  // 지네릭을 도입하기 전에는 컬렉션에 들어오는 타입이 정해져 있지 않아 반환 타입이 다 Object였다. 따라서 따로 형변환을 해야 했다.

ArrayList<Tv> tvList2 = new ArrayList<Tv>();
tvList2.add(new Tv());
Tv t = tvList2.get(0); // 지네릭을 도입함으로써 컬렉션에 들어오는 타입이 Tv로 정해져 있으므로 반환 타입이 자동으로 Tv 타입이 된다. 따라서 형변환을 할 필요가 없다.
```
### 지네릭스의 장점
1. 타입 안정성을 제공한다 : 지정한 타입 외에 들어오는 경우 컴파일 에러가 발생함
2. 타입체크와 형변환을 생략할 수 있다 : 코드가 간결해짐

#### 타입 안정성
타입 안정성이 높다는 것은 **의도하지 않은 타입의 객체가 저장되는 것을 막으며, 저장된 객체를 꺼내올 때 원래의 타입과 다른 타입으로 형변환 될 수 있는 오류를 줄여준다**는 뜻이다.

## 타입변수
<img width="614" alt="image" src="https://github.com/whxogus215/JavaBookArchive/assets/70999462/374ed0b1-71f0-46c5-9d2f-e2a3be1fcdad">

이와 같이 ArrayList에 `<>`가 있으며, 안에 있는 E를 **타입 변수(type variable)** 라고 한다. 보통 T를 많이 사용하지만 T 말고도 다른 기호를 사용해도 된다. 주로 사용되는 타입 변수의 의미는 다음과 같다.
1. T : **T**ype
2. E : **E**lement
3. K, V : **K**ey, **V**alue (Map에서 Key와 Value를 표현할 때 주로 사용)

기존에는 모든 타입을 받을 수 있도록 **Object 타입**을 많이 사용했다. 따라서 반환타입도 Object이므로 형변환이 필요했다. 하지만 지네릭을 통해 타입 매개변수로 값을 받음으로써 지정된 타입을
반환할 수 있게 되었다.

```java
public class ArrayList<E> extends AbstractList<E> {
  private transient E[] elementData;
  public boolean add(E o) {...};
  public E get(int index) {...};
}
```

컬렉션 클래스 같은 **지네릭 클래스를 생성할 때는 찹조변수와 생성자의 타입 변수를 실제 타입으로 지정하고 둘 다 동일해야 한다.** `ArrayList<Tv> tvList = new ArrayList<Tv>();`에서 Tv는 실제
타입 변수로 대입된 타입이다. 따라서 이를 **parameterized type, 대입된 타입**이라고 한다. 이렇게 타입을 대입하게 되면 위에서 작성했던 E 부분이 대입된 타입으로 변경되는 것이다.

## 지네릭스 용어 정리
<img width="421" alt="image" src="https://github.com/whxogus215/JavaBookArchive/assets/70999462/45032ecf-983a-4bee-90ee-bcc800d35d3e">

`Box<String> b = new Box<String>();`라고 했을 때, 실제 T에 대입되는 String은 **Parameterized Type**이다. Box<String>과 Box<Integer>는 지네릭 클래스 Box<T>에 서로 다른 매개변수 타입이 대입된 것일 뿐이지
별개의 클래스를 의미하는 것이 아니다. **중요한 건 컴파일이 되면 대입된 타입은 사라지고 원시 타입인 Box만 남는다. 즉, 지네릭 타입이 제거된다.** Box<String>과 Box<Integer>는 컴파일이 되면 둘 다
Box 타입으로 바뀌는 것이다.

## 지네릭 타입 일치와 다형성
지네릭 클래스의 객체(인스턴스)를 생성할 때, **참조변수에 지정한 지네릭 타입과 생성자에 지정한 지네릭 타입이 반드시 일치해야 한다.** 서로 상속관계라고 할지라도 생성 당시에는 타입이 서로 일치해야 한다.
```java
ArrayList<Tv> list = new ArrayList<Tv>(); // 일치
ArrayList<Product> list = new ArrayList<Tv>(); // 불일치, 컴파일 에러

class Product {}
class Tv extends Product {}
```
물론 클래스 간의 다형성은 여전히 허용된다. 다만 이 때도 지네릭 타입은 일치해야 한다. `List<Tv> list = new ArrayList<Tv>();`
만약, 지네릭 클래스 타입의 자손이 있다면 해당 자손도 추가될 수 있다. **대신 이 때는 조상의 타입으로 객체를 생성해야 한다.**

```java
ArrayList<Product> list = new ArrayList<Product>();
list.add(new Product());
list.add(new Tv());
list.add(new Audio());

// 대신에 객체를 꺼낼 때는 형변환이 필요하다.
Product p = list.get(0);
Tv t = (Tv)list.get(1);
// 매개변수 타입이 Product이므로 반환 타입도 Product이다. 따라서 조상에서 자손 타입으로 바꿀 시에는 명시적으로 형변환을 해야 한다.
```
```java
public class Practice1 {
    public static void main(String[] args) {
        ArrayList<Product> productList = new ArrayList<>();
        ArrayList<Tv> tvList = new ArrayList<>();
        // ArrayList<Product> tvList2 = new ArrayList<Tv>(); 서로 타입 안 맞아서 에러
        List<Tv> tvList2 = new ArrayList<Tv>(); // 다형성 가능
        
        // Product 타입만 올 수 있으며, 자손 타입들의 경우 자동으로 형변환이 되어 대입됨 - 다형성
        productList.add(new Audio());
        productList.add(new Tv());

        // Tv 타입만 가능
        tvList.add(new Tv());
        tvList.add(new Tv());

        printAll(productList); // ArrayList<Product>이므로 가능
        // printAll(tvList); // ArrayList<Tv>이므로 불가능
    }
    public static void printAll(ArrayList<Product> list) {
        for (Product p : list) {
            System.out.println(p);
        }
    }
}

class Product {}
class Tv extends Product {}
class Audio extends Product {}
```

### Iterator<E>
Iterator<E>는 기존의 이터레이터 인터페이스에 지네릭이 적용된 형태이다. 기존의 이터레이터는 Object로 되어 있었으며, `it.next()`를 통해 접근할 때도 Object가 반환되기 때문에 매번
형변환을 했어야 했다. 하지만 지네릭이 적용됨으로써 지정한 매개변수 타입이 반환된다.

```java
public class Ex12_2 {
    public static void main(String[] args) {
        ArrayList<Student> list = new ArrayList<Student>();
        list.add(new Student("자바왕", 1, 1));
        list.add(new Student("자바짱", 1, 2));
        list.add(new Student("홍길동", 2, 1));

        Iterator<Student> it = list.iterator();
        while (it.hasNext()) {
            // Student s = (Student)it.next(); // 지네릭을 사용하지 않았을 때는 형변환을 해야 했다.
            Student s = it.next();
            System.out.println(s.name);
        }
    }
}

class Student {
    String name ="";
    int ban;
    int no;

    public Student(String name, int ban, int no) {
        this.name = name;
        this.ban = ban;
        this.no = no;
    }
}
```

### Hashmap<K,V>
HashMap과 같이 Key-Value 형태로 데이터를 저장하는 컬렉션의 경우, 지정해야 할 매개변수 타입이 2개가 된다. 이 때는 콤마를 통해 구분하여 두 가지 매개변수를 지정할 수 있다. 두 개가 아닌 세 개 이상일 때도 마찬가지이다.
```java
public class HashMap<K, V> extends AbstractMap<K, V> {
  public V get(Object key) {}
  public V put(K key, V value) {}
  public V remove(Object key) {}
}
```
이처럼 지네릭을 사용함으로써 HashMap의 경우, 값을 꺼내오는 `get()`이나 `keySet()`,`values()`를 사용할 때도 마찬가지로 형변환을 하지 않아도 된다.

### 지네릭 클래스를 제한하는 방법
지네릭을 통해 타입을 지정하면 한 종류의 타입만 저장할 수 있도록 제한할 수 있다. 하지만 어쨌든 한 가지가 들어오지만 그 한 가지는 어떤 타입이든 될 수 있는 것이다. 따라서 타입 매개변수에 올 수 있는
타입의 종류를 **제한할 수 있다면 좀 더 안정적인 클래스 작성이 가능할 것이다.**

```java
FruitBox<Toy> fruitBox = new FruitBox<Toy>();
fruitBox.add(new Toy()); // 문제 없음 - 과일 상자에 장난감을 담을 수 있다.
```
이처럼 지네릭을 통해 모든 타입을 지정할 수 있다는 특성으로 인해 **논리적 오류가 발생할 수 있다.** 클래스를 작성할 때 `class FruitBox<T> {}`라고 해버리면 위처럼
논리적으로 맞지 않은 타입이 들어오더라도 코드 상으로는 문제가 없으며 정상적으로 실행이 된다.

```java
class FruitBox<T extends Fruit> { // Fruit을 상속한 클래스만 들어올 수 있도록 제한! (Fruit 클래스도 포함)
  ArrayList<T> list = new ArrayList<T>();
}
```
이렇게하면 역시 한 종류의 타입만 받을 수 있게 된다. 하지만 이전과 같이 과일 상자에 장난감을 담는 논리적 오류를 피할 수 있다. 만약, 인터페이스를 구현한 클래스로 제한한다면
이 때도 마찬가지로 **extends를 사용한다. 인터페이스라고 해서 implements를 사용하지 않는다.**

```java
interface Eatable {]
class FruitBox<T extends Eatable> {...}

// 상속과 인터페이스 구현으로 제한할 경우 : 둘 다 extends로 제한하기 때문에 &로 묶을 수 있다.
class FruitBox<T extends Fruit & Eatable> {...}
```
```java
class Fruit implements Eatable {
    public String toString() {return "Fruit";}
}

class Apple extends Fruit { public String toString() { return "Apple";}}
class Grape extends Fruit { public String toString() { return "Grape";}}

class PineApple extends Apple {}
class Toy { public String toString() { return "Toy";}}

interface Eatable {}

public class Ex12_3 {
    public static void main(String[] args) {
        // Fruit을 상속한 클래스들만 들어올 수 있으며, 마찬가지로 한 가지의 종류만 들어올 수 있다.
        FruitBox<Fruit> fruitBox = new FruitBox<Fruit>();
        FruitBox<Apple> appleBox = new FruitBox<Apple>();
        FruitBox<Grape> grapeBOx = new FruitBox<Grape>();
        // FruitBox<Toy> toyBox = new FruitBox<Toy>(); // 에러 발생 : Toy는 Fruit을 상속받지 않는다.

        // FruitBox<Fruit>은 Fruit을 구현한 클래스들이 들어올 수 있다.
        fruitBox.add(new Fruit());
        fruitBox.add(new Apple());
        fruitBox.add(new Grape());

        appleBox.add(new Apple());
        // appleBox.add(new Grape()); // FruitBox<Apple>은 Apple 클래스 혹은 Apple을 구현한 클래스만 가능하다.
        appleBox.add(new PineApple()); // Apple을 상속받은 파인애플은 가능하다.

    }
}

class FruitBox<T extends Fruit & Eatable> extends Box<T> {}

class Box<T> {
    ArrayList<T> list = new ArrayList<T>();
    void add(T item) { list.add(item);}
    T get(int i) { return list.get(i);}
    int size() { return list.size();}
    public String toString() { return list.toString();}
}
```
