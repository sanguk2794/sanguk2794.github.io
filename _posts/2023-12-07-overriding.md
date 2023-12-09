---
layout: post
title: >
  메서드 오버라이딩 (Overriding)
tags: [Java]
---

## 1. 메서드 오버라이딩 (Overriding)
조상 클래스로부터 상속받은 메서드를 그대로 사용하기도 하지만 자손 클래스 자신에 맞게 변경해야 하는 경우도 많다.
이러한 경우에 조상 클래스로부터 상속받은 메서드를 재정의해서 사용할 수 있는데, 이를 메서드 오버라이딩이라고 한다.

```java
public class Man {
    private final String name;
    private final int age;

    Man(String name, int age) {
        this.name = name;
        this.age = age;
    }

    @Override
    public String toString() {
        return String.format("name = %s, age = %d", name, age);
    }
}

public class Engineer { 
    private final String language;

    Engineer(String name, int age, String language) {
        super(name, age);
        this.language = language;
    }

    @Override
    public String toString() {
        return super.toString() + String.format("language = %s", language);
    }
}
```

메서드를 재정의 할 때 조상 클래스의 메서드가 변경될 때 변경 내용이 자손 클래스의 메서드에 자동으로 반영되도록 조상 클래스의 코드를 포함시켜 구현하는 것이 좋다.

---
## 2. 오버라이딩의 조건
오버라이딩은 아래 조건들을 만족해야 한다.

- 메서드의 이름이 같아야 한다.
- 매개변수가 같아야 한다.
- 반환타입이 같거나, 공변 반환 타이핑의 조건을 만족시켜야 한다.
- 접근제어자는 조상 클래스의 메서드보다 좁은 범위로 변경할 수 없다.
- 조상 클래스의 메서드보다 넓은 범위의 예외를 선언할 수 없다.

### 1. 공변 반환 타이핑 (Covariant Return Typing)
공변 반환 타이핑은 자바 5부터 도입되었으며, 하위 클래스에서 재정의한 메서드가 상위 클래스의 메서드가 정의한 반환 타입이 아닌 더 구체적인(하위 타입) 타입을 반환할 수 있도록 하는 기능이다.

공변 반환 타이핑은 주로 제네릭과 함께 사용되며, 오버라이딩된 메서드가 상위 클래스의 메서드보다 더 구체적인 타입을 반환할 수 있도록 허용함으로써 코드의 유연성을 높여준다.

```java
class Parent {
    protected Parent instanceOf() {
        return new Parent();
    }
}

class Child extends Parent {
    // 부모 클래스로부터 재정의하였으나 반환형을 자식 클래스로 변경할 수 있다.
    @Override 
    public Child instanceOf() {
        return new Child();
    }
}
```

---
## 3. 오버라이딩과 오버로딩의 차이
이름이 비슷해 혼동하기 쉬우나 그 차이는 명확하다. 오버로딩은 기존에 없는 새로운 메서드를 추가하는 것이고, 오버라이딩은 조상으로부터 상속받은 메서드의 내용을 변경하는 것이다.

---
## 4. super, super()
`super`는 자손 클래스에서 조상 클래스로부터 상속받은 멤버를 참조하는데 사용되는 참조변수이다.
조상 클래스의 멤버와 자손 클래스에 정의된 멤버 중 같은 이름의 멤버가 있을 경우에 `super` 키워드를 통해 구분할 수 있다.

클래스 메서드는 인스턴스와 관련이 없으므로 `super`의 사용이 불가능하다.

또, `super()` 메서드를 통해 조상 클래스의 생성자를 호출할 수 있다.
자손 클래스의 인스턴스를 생성하면 자손의 멤버와 조상의 멤버가 모두 합쳐진 하나의 인스턴스가 생성되기에 조상 클래스의 생성자 또한 호출이 필요하며, 
조상 클래스의 멤버변수는 조상의 생성자에 의해 초기화되도록 구현하는 것이 바람직하다.

모든 클래스의 생성자는 첫줄에 반드시 자신의 다른 생성자 또는 조상의 생성자를 호출해야 한다. 
명시적으로 호출하지 않으면 컴파일러는 생성자의 첫 줄에 자동으로 `super()`를 추가한다.

```java
public class Engineer { 
    private final String language;

    // super() 호출을 통해 조상클래스에 생성자를 호출할 수 있다.
    Engineer(String name, int age, String language) {
        super(name, age);
        this.language = language;
    }
    
    // super 참조변수를 통해 조상 메서드를 호출할 수 있다.
    @Override
    public String toString() {
        return super.toString() + String.format("language = %s", language);
    }
}
```

## 5. @Override
해당 메서드가 재정의한 메서드라는 것을 `@Override` 어노테이션을 통해 명시적으로 표현할 수 있다.
해당 어노테이션을 이용하면 컴파일러에게 해당 메서드가 실제로 상위 클래스의 메서드를 오버라이드하는 의도임을 알려주어 버그를 방지할 수 있다.

재정의중이 아닌 메서드에 해당 어노테이션을 붙일 경우 컴파일 에러가 발생한다.

```text
Method does not override method from its superclass
```

```java
public class Engineer {
    private final String language;

    Engineer(String name, int age, String language) {
        super(name, age);
        this.language = language;
    }

    // 메서드 시그니처가 다르기 때문에 메서드 재정의에 포함되지 않는다. 컴파일 에러가 발생한다.
    // Method does not override method from its superclass
    @Override
    public String toString(String c) {
        return super.toString();
    }
}
```

`@Override` 어노테이션의 사용은 필수적이지 않으므로 묵시적인 재정의 메서드가 있을 수 있다.
조상 클래스의 메서드를 재정의하고 있는 메서드인지 햇갈리는 메서드가 등장할 경우 어노테이션을 붙이는 것으로 컴파일 타임에 확실한 체크가 가능하다.

---
#### ▶ Reference
- [[Java]오버로딩 & 오버라이딩(Overloading & Overriding)](https://hyoje420.tistory.com/14)
- [[Java] 공변 반환 타입(covariant return type)](https://ingnoh.tistory.com/153)
- 남궁성, 자바의 정석, 도우출판
- Joshua Bloch, 이복연 옮김, EFFECTIVE JAVA 3/E, 프로그래밍인사이트

---