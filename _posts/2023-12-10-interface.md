---
layout: post
title: >
  인터페이스 (Interface)
tags: [Java]
---

## 1. 인터페이스 (Interface)
인터페이스는 일종의 추상클래스이며, 추상클래스보다 추상화의 정도가 더 높아 추상 메서드와 상수만을 멤버로 가질 수 있다.

인터페이스는 주로 `~을 할 수 있는`의 의미하는 `able`로 끝나는 것들이 많다.
그 이유는 어떠한 기능 또는 행위로 하는데 필요한 메서드를 제공한다는 의미를 강조하기 위해서이다. 
물론 모든 인터페이스들이 `able`로 끝나는 것은 아니다.

---
## 2. 인터페이스의 작성
인터페이스를 작성하는 것은 클래스를 작성하는 것과 같다. 다만 `class`대신 `interface` 예약어를 사용한다.

```text
interface 인터페이스명 { 
    // ...
}
```

`interface`에도 접근 제어자를 사용할 수 있다.

```java
private interface SuperInterface {
    String SUPER_MEMBER = "superMember";
    
    void method();
}
```

---
## 3. 인터페이스의 구성
인터페이스에는 기본적으로 상수와 추상 메서드, 클래스 메서드를 선언 가능하다.

### 1. 상수
제어자가 `public static final`인 상수를 선언 가능하다. 이는 인터페이스에 정의된 모든 멤버에 예외없이 적용되는 사항이기에 생략할 수 있으며, 생략된 제어자는 컴파일시에 컴파일러가 자동으로 추가해 준다.

```java
public interface SuperInterface {
    // public static final이 생략되어 있다. 
    String SUPER_MEMBER = "superMember";
}
```

---
### 2. 추상 메서드
기본적으로 인터페이스의 모든 메서드는 `public abstract`이어야 한다. 따라서 이 또한 생략할 수 있으며, 생략된 제어자는 컴파일시에 컴파일러가 자동으로 추가해 준다.

```java
public interface SuperInterface {
    // public abstract가 생략되어 있다.
    void method();
}
```

---
### 3. 클래스 메서드
클래스 메서드는 인스턴스와 관련 없는 독립적인 메서드이기 때문에 인스턴스화가 불가능한 인터페이스라고 하더라도 전혀 문제가 되지 않는다.

해당 메서드는 인터페이스에 종속되므로 자손 클래스 혹은 인터페이스에서 재정의가 불가능하기 때문에 인터페이스 내에서 구현해주어야 한다.

```java
public interface Flyable {
   static void staticMethod() {
      System.out.println("static method");
   }
}
```

---
### 4. 디폴트 메서드 (Default method)
자바 8 버전부터 추가된 디폴트 메서드는 추상 메서드의 기본적인 구현을 제공해 준다.

추상 메서드가 아니기 때문에 수정 시 해당 인터페이스를 구현한 클래스를 변경하지 않아도 된다는 장점이 생긴다.
또, 필요하지 않은 메서드에 대해서는 구현을 하고 싶지 않은 경우가 있는데 디폴트 메서드가 없으면 항상 모든 메서드를 재정의할 것이 강제된다.

디폴트 메서드는 메서드 앞에 예약어 `default`를 붙이며, 접근제어자는 역시 `public`이다.

```java
public interface Flyable {
   default void defaultMethod() {
      System.out.println("default method");
   }
}


```

인터페이스를 다중 구현할 때, 두 인터페이스에 이름이 같은 추상 메서드가 있더라도 추상 메서드는 내부 구현이 없으므로 선택의 문제가 발생하지 않았다.
하지만 디폴트 메서드를 사용하면 내부 구현이 존재하므로, 어떤 클래스의 메서드를 상속받을 것인지 알 수 없게 된다.

이러한 문제가 발생하면 구현하는 메서드에서 같은 이름의 메서드를 재정의해주어야 한다.

```java
public interface Flyable {
   default void defaultMethod() {
      System.out.println("Flyable default method");
   }
}

public interface Copyable {
    default void defaultMethod() {
        System.out.println("Copyable default method");
    }
}

public class Main implements Flyable, Copyable {
    // 재정의해주지 않으면 Flyable.defaultMethod()를 선택해야 할지, Copyable.defaultMethod()를 선택해야 할지 알 수 없다.
    void defaultMethod() {
        System.out.println("overriding...");
    }
}
```

---
### 5. 디폴트 메서드의 하위 호환성
기존에 존재하던 인터페이스를 구현한 의존 코드가 있다고 가정하자.
해당 인터페이스를 보완하는 과정에서 추가적으로 구현해야 할, 혹은 필수적으로 존재해야 할 메소드가 추가되면 이미 이 인터페이스를 구현한 클래스는 해당 클래스를 재정의하도록 수정되어야 하며, 하위 호환성이 떨어지게 된다.

이 때 추상 메서드 대신 디폴트 메소드를 추가하게 된다면 하위 호환성은 유지되고 인터페이스의 보완을 진행 할 수 있다.

---
### 6. 프라이빗 메서드 (Private Method)
자바 9 버전부터 인터페이스에도 프라이빗 메서드를 선언하는 것이 가능해졌다.
프라이빗 메서드는 인터페이스 내부에서만 사용되며, 디폴트 메서드나 정적 메서드에서 코드 중복을 피하기 위해 사용된다.

프라이빗 메서드는 접근 제어자가 `private`이므로, 인터페이스를 구현하는 클래스에서는 재정의하거나 직접 호출할 수 없고, 디폴트 메서드나 정적 메서드를 통해 간접적으로 호출된다.

아래 `MethodsInvokable` 인터페이스의 `invokeMethods()` 메서드는 `Invokable` 어노테이션이 존재하는 메서드를 찾아 실행해 준다.
이 동작을 프라이빗 메서드를 사용함으로써 보다 간결하게 작성할 수 있었다.

```java
public interface MethodsInvokable {
    // private 메서드를 사용하면 코드를 보다 간결하게 작성 가능하다.
    default void invokeMethods() {
        getTargetMethods().forEach(this::invokeMethod);
    }

    private void invokeMethod(Method method) {
        // ...
    }

    private List<Method> getTargetMethods() {
        // ...
    }
}

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.RUNTIME)
public @interface Invokable {

    int INVOKABLE_DEFAULT_PRIORITY = 999;

    boolean value() default true;

    int priority() default INVOKABLE_DEFAULT_PRIORITY;
}

public class Main implements NoArgsMethodInvokable {
    @Invokable
    public void method() {
        System.out.println("method");
    }

    // method()를 찾아서 실행
    public static void invokeMethod() {
        new Main().invokeNoArgsMethods();
    }
}
```

---
## 4. 인터페이스의 상속과 구현
인터페이스는 인터페이스로부터만 상속을 받을 수 있으며, 클래스와는 달리 다중 상속을 허용한다.
클래스의 상속과 마찬가지로 자손 인터페이스는 조상 인터페이스에 정의된 모든 멤버를 상속받는다.

```java
interface Interface extends Serializable, Runnable {}
```

인터페이스를 클래스에서 구현하기 위한 예약어로 구현의 의미를 가진 `implements`를 사용한다.
인터페이스를 구현하는 클래스는 인터페이스에 선언된 추상 메서드들의 구현부를 반드시 작성해야 한다.

```java
// Comparable 구현시 반드시 구현해야 할 compareTo() 메서드를 재정의하고 있다.
public class Fruit implements Comparable<Fruit> {
    @Override
    public int compareTo(Fruit o) {
        return 0;
    }
}
```

---
## 5. 인터페이스의 장점
일반적으로 알려진 인터페이스의 장점은 다음과 같다.


- 개발시간을 단축시킬 수 있다 : 메서드를 호출해 사용하는 쪽에서는 인터페이스가 있다면 내부 구현이 끝날때까지 기다리지 않고 동시에 개발을 진행할 수 있다. 
- 표준화가 가능하다 : 개발자들에게 인터페이스를 구현하여 프로그램을 작성하도록 함으로써 보다 일관되고 정형화된 프로그램 개발이 가능하다. 
- 서로 관계가 없는 클래스들에게 관계를 맺어 줄 수 있다 : 공통적으로 인터페이스를 구현하도록 하면 동일한 형질을 가지게 되므로 묶어서 처리가 가능해진다.
- 독립적인 프로그래밍이 가능하다 : 클래스의 선언과 구현을 분리시킬 수 있기 때문에 실제 구현에 독립적인 프로그램 작성이 가능하다. (JDBC)

독립적인 프로그래밍이 가능한 이유는 아래의 사진을 통해 이해할 수 있다.

![Interface](https://drive.google.com/uc?export=view&id=1HYO-jXO6Nk5Lw6KDi2mKG7DAMuwmEAIZ)

출처 : [인터페이스](https://www.notion.so/4b0cf3f6ff7549adb2951e27519fc0e6)

의존 코드를 표준화된 인터페이스를 통해 구현할 경우 다른 서비스 제공자의 구현체를 사용하도록 코드를 변경해야 할 경우 의존 코드의 수정이 불필요하다.

가장 대표적인 예시가 JDBC이다.
JDBC의 드라이버를 변경하는 것으로 Oracle, Mysql 등 어떤 DB에도 접속이 가능하며, DB의 변경이 있더라도 의존 코드의 수정은 불필요하다.

---
## 6. 인터페이스를 이용한 다형성
인터페이스 역시 이를 구현한 클래스의 조상이라고 할 수 있으므로 해당 인터페이스 타입의 참조변수로 이를 구현한 클래스의 인스턴스를 참조할 수 있으며, 인터페이스 타입으로의 형변환도 가능하다.

파라미터, 혹은 반환 타입이 인터페이스라는 것은 메서드가 해당 인터페이스를 구현한 클래스의 인스턴스를 파라미터, 혹은 반환 타입으로 한다는 것을 의미한다.

```java
public interface Flyable {
    static void staticMethod() {
        System.out.println("check static method");
    }

    default void fly() {
        System.out.println("fly : " + this.getClass().getSimpleName());
    }
}

public interface Walkable {
    default void walk() {
        System.out.println("walk : " + this.getClass().getSimpleName());
    }
}

public class Bird implements Flyable, Walkable {}
public class Dog implements Walkable {}
public class Cat implements Walkable {}

public class Main {
    public void interfacePolymorphism() {
        // 인터페이스로 공통화하는 것으로 같은 방식으로 다룰 수 있다.
        List<Walkable> walkableList = List.of(new Bird(), new Dog(), new Cat());
        walkableList.forEach(Walkable::walk); 
        // walk : Bird
        // walk : Dog
        // walk : Cat
    }
}
```

---
## 7. 중첩 인터페이스
인터페이스도 클래스 내부에 선언할 수 있으며, 이를 중첩 인터페이스라고 한다. 해당 클래스와 긴밀한 관계를 맺는 구현 인터페이스를 만들기 위해서 사용한다.

중첩 인터페이스는 묵시적으로 `static`이다.

```java
public interface OuterInterface {
    // static이 생략되어 있다.
    interface InnerInterface {
        
    }
}
```

---
#### ▶ Reference
- [인터페이스](https://www.notion.so/4b0cf3f6ff7549adb2951e27519fc0e6)
- [온라인 자바 스터디 #8 - 인터페이스](https://dev-coco.tistory.com/13)
- [Why would a static nested interface be used in Java?](https://stackoverflow.com/questions/71625/why-would-a-static-nested-interface-be-used-in-java)
- 남궁성, 자바의 정석, 도우출판

---