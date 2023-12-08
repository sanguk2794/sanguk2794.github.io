---
layout: post
title: >
  생성자 (Constructor)
tags: [Java]
---

## 1. 생성자 (Constructor)
인스턴스가 생성될 때 호출되는 인스턴스 초기화 메서드이다. 인스턴스의 초기화와 인스턴스 생성 시에 필요한 작업을 처리하기 위해 사용된다.

생성자는 클래스 내에 선언되며 메서드와 유사한 구조를 지닌다. 단, 생성자의 이름은 클래스 이름과 같아야 하며 반환값이 없다는 것이 메서드와의 차이점이다.

- 생성자의 이름은 클래스와 같아야 한다.
- 생성자는 반환값이 없다.

```java
public class Constructor {
    private final int x;
    private final int y;

    // 생성자의 이름은 클래스의 이름과 같으며, 반환값이 없다.
    public Constructor(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

---
## 2. 생성자의 오버로딩
생성자 또한 오버로딩이 가능해 서로 다른 파라미터를 가지는 생성자를 여러 개 정의할 수 있다.
생성자의 오버로딩을 통해 코드의 중복을 제거해 보다 유연한 코드를 작성할 수 있다.

```java
public class Constructor {
    private static final int createCount = 0;

    private final int x;
    private final int y;

    public Constructor() {
        this.x = 0;
        this.y = 0;
    }

    public Constructor(int x, int y) {
        createCount++;
        this.x = x;
        this.y = y;
    }
}
```

생성자의 오버로딩을 통해 다양한 생성자를 제공해 인스턴스 생성 후에 별도의 초기화 작업을 수행하지 않는 것이 바람직하다.

---
## 3. 기본 생성자
기본적으로 모든 클래스에는 반드시 하나 이상의 생성자가 정의되어야 한다. 
하지만, 생성자를 작성하지 않더라도 클래스의 생성이 가능한데, 그 이유는 클래스 내부에 아무런 생성자를 선언하지 않는 경우 컴파일러가 기본 생성자를 자동으로 생성해 주기 때문이다.

```
클래스명() { }
```

특별히 인스턴스 초기화 작업이 요구되지 않는다면 컴파일러가 제공하는 기본 생성자를 이용하는 것도 좋다.

하지만, 기본 생성자의 사용에는 주의가 필요하다.

- 생성자가 하나라도 선언되어 있을 경우 컴파일러는 기본생성자를 생성하지 않는다.
- 조상 클래스에 매개변수가 없는 생성자가 존재하지 않는 경우, 컴파일 에러가 발생한다. 
생성자에 조상 인스턴스의 초기화 메서드인 `super(파라미터)` 메서드를 명시적으로 기술하지 않으면 묵시적으로 `super()` 메서드를 호출하기 때문이다.

---
## 4. this
메서드와 마찬가지로, 생성자에서도 인스턴스 자신을 가리키는 참조변수인 `this`를 사용하는 것이 가능하다. 

`this`는 현재 인스턴스를 가리키는 특별한 참조변수이므로, `this` 참조변수를 통해 접근할 수 있는 멤버는 인스턴스 멤버로 제한된다.

사실 생성자를 포함한 모든 인스턴스 메서드에는 자신이 관련된 인스턴스를 가리키는 참조변수 `this`가 숨겨진 채로 존재하며, 이를 리시버 파라미터라고 한다.

```java
public class Constructor {
    private final int x;
    private final int y;

    public Constructor(int x, int y) {
        this.x = x;
        this.y = y;
    }
}
```

---
## 5. this()
같은 클래스의 멤버들 간에 서로 호출할 수 있는 것처럼 `this()`를 통해 생성자 간의 호출이 가능하다.

같은 클래스 내의 생성자들은 서로 관계가 깊은 경우가 많아서 서로 호출하도록 구현하면 코드가 더욱 명확해진다.
또, 중복되는 코드를 모아서 관리할 수 있어 수정과 유지보수가 쉬워진다.

단, 생성자 간 호출에는 다음의 두 조건을 만족시켜야 한다.

- 생성자의 이름으로 클래스명 대신 `this` 키워드를 사용한다.
- 한 생성자에서 다른 생성자를 호출할 때는 생성자 구현부의 첫 번째 줄에서만 호출할 수 있다.

생성자 내에서 초기화 작업 도중에 다른 생성자를 호출하면 다른 생성자를 호출하기 이전의 초기화 작업이 무의미해질 수 있다.
이러한 이유로, 한 생성자에서 다른 생성자를 호출할 때 생성자 구현부의 첫 번째 줄에서만 호출할 수 있도록 제한하고 있다. 

```java
public class Constructor {
    private static final int createCount = 0;
    private final int x;
    private final int y;

    public Constructor() {
        // this 키워드를 사용하면 클래스 내의 다른 생성자를 호출할 수 있다.
        this(0, 0);
    }

    public Constructor(int x) {
        // 주석을 풀면 컴파일 에러가 발생한다. 다른 생성자를 호출할 때에는 반드시 첫 줄에서만 호출이 가능하다.
        // createCount++;
        this(x, 0);
    }

    public Constructor(int x, int y) {
        createCount++;
        this.x = x;
        this.y = y;
    }
}
```

---
## 6. 생성자를 활용한 인스턴스 복사
현재 사용하고 있는 인스턴스와 같은 상태를 가지는 인스턴스를 하나 더 생성하고자 할 때 생성자를 이용할 수 있다.
이를 통해 인스턴스의 상태를 모르더라도 똑같은 상태의 인스턴스를 추가로 생성할 수 있다.

```java
public class Constructor {
    private final int x;
    private final int y;

    public Constructor(int x, int y) {
        createCount++;
        this.x = x;
        this.y = y;
    }

    // 생성자를 통해 같은 타입의 인스턴스를 파라미터로 넘겨받아, 해당 인스턴스의 복사를 수행할 수 있다.
    public Constructor(Constructor constructor) {
        this(constructor.x, constructor.y);
    }
}
```

아래와 같이 `Copyable` 인터페이스를 추가하고, `copy()` 메서드 내에서 복사 생성자를 호출하면 보다 유연한 처리가 가능하다.
```java
public interface Copyable<T> {
    T copy();
}

public class Constructor implements Copyable<Constructor> {
    // ...

    @Override
    public Constructor copy() {
        return new Constructor(this);
    }
}
```

### 1. Cloneable
자바는 복제해도 되는 클래스임을 명시하는 용도로 `Cloneable` 믹스인 인터페이스를 별도로 제공한다. 하지만, 이 인터페이스는 여러 문제점이 존재하므로, 사용이 권장되지 않는다.

가장 큰 문제는 `clone()` 메서드가 선언된 곳이 `Cloneable`이 아닌 `Object` 클래스이고 접근 제어자마저 `protected`라는 점이다.
이외에도 해당 인터페이스를 구현한 클래스의 `clone()` 메서드 구현이 보장되지 않는다는 점, 깊은 복사가 이루어지도록 재정의하지 않는 이상 얕은 복사만이 이루어진다는 점 등 여러 문제가 존재한다.

---
#### ▶ Reference
- [[Java] new 연산자란](https://yoo11052.tistory.com/52)
- [Why can we use 'this' as an instance method parameter?](https://stackoverflow.com/questions/24291091/why-can-we-use-this-as-an-instance-method-parameter)
- 남궁성, 자바의 정석, 도우출판
- Joshua Bloch, 이복연 옮김, EFFECTIVE JAVA 3/E, 프로그래밍인사이트

---