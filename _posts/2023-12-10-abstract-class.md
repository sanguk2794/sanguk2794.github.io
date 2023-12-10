---
layout: post
title: >
  추상 클래스 (Abstract Class)
tags: [Java]
---

## 1. 추상 클래스 (Abstract Class)
추상 클래스란 구체적이지 않은 클래스를 의미한다. 이 클래스는 구체적이지 않기 때문에 인스턴스를 생성할 수 없다.
추상 클래스는 자체로 클래스로의 역할을 온전하게 수행하지 않지만 새로운 클래스를 작성하는데 있어서 바탕이 되는 조상 클래스로써 중요한 의미를 가진다.

---
## 2. 추상 클래스의 구조
추상 클래스의 선언에는 예약어 `abstract`를 사용한다.
클래스 선언부에 `abstract`가 붙어 있다면 상속을 통해서 구현해주어야 하는 클래스라는 것을 쉽게 파악할 수 있다.

```java
public abstract class Game {
    // Game.play()는 구체적인 동작이 정의되지 않았다. 그러므로 해당 클래스를 상속받는 구체적인 클래스들은 반드시 해당 메서드를 구현해야 한다.
    abstract void play();
}

public class TFT extends Game {
    void play() {
        System.out.println("TFT....");
    }
}
```

추상 클래스는 추상 메서드를 포함할 수 있다는 것을 제외하고는 일반 클래스와 전혀 다르지 않다.
추상 클래스에도 생성자가 있으며, 멤버변수와 메서드도 가질 수 있다.

```java
public abstract class Game {
    // 변수
    private final BigDecimal price;
    
    // 생성자
    Game (BigDecimal price) {
        this.price = price;
    }

    // 추상 메서드
    abstract void play();

    // 클래스 메서드, 추상 클래스 또한 자신만의 클래스 메서드를 가질 수 있다.
    static void relieveStress() {
        System.out.println("Relieve stress!!!");
    }
}
```

---
## 3. 추상 메서드
메서드의 선언부만 작성하고 구현부는 작성하지 않은 채로 남겨둔 메서드를 추상 메서드라고 한다.
메서드를 이와 같이 미완성으로 남겨두는 이유는 메서드의 내용이 상속받는 클래스에 따라 달라질 수 있기 때문이다.

조상 클래스에서는 선언부만을 작성하고 주석을 덧붙여 어떤 기능을 수행할 목적으로 작성되었는지 알려주고 실제 내용은 상속받는 클래스에서 구현하도록 비워 두는 것이다.

추상 메서드 역시 예약어 `abstract`를 붙여 선언하며, 구현부가 없으므로 괄호 대신 문장의 끝을 알리는 `;`을 적어준다.


```java
abstract void abstractMethod();
```

추상 클래스를 상속받는 자손 클래스들은 오버라이딩을 통해 조상 추상 클래스의 추상 메서드를 모두 구현해주어야 한다.
만약 조상으로부터 상속받은 추상 메서드 중 하나라도 구현하지 않았다면 자손 클래스 역시 추상 클래스로 지정해주어야 한다.

```java
public abstract class Game {
    abstract void play();
}

public class DJMax extends Game {
    // 해당 메서드를 구현하지 않으면 컴파일 에러가 발생한다.
    @Override
    void play() {
        System.out.println("DJMax.....");
    }
}
```

---
## 4. 추상 클래스와 인터페이스
추상 클래스는 아래와 같은 경우에 주로 사용된다.

- 관련성이 높은 클래스 간에 코드를 공유하고 싶을 때
- 추상 클래스를 상속 받을 클래스들이 공통으로 가지는 메서드와 필드가 많거나 `public` 이외의 접근자 선언이 필요할 때
- non-static, non-final 필드 선언이 필요할 때 (각 인스턴스에서 상태 변경을 위한 메소드가 필요한 경우)

그리고 인터페이스는 아래와 같은 경우에 주로 사용된다.

- 서로 관련성이 없는 클래스들이 인터페이스를 구현할 때 : 직렬화를 위한 `Serializable`, 비교를 위한 `Comparable`, 복사를 위한 `Cloneable`등 믹스인 인터페이스
- 특정 데이터 타입의 행동을 명시하고 싶은데, 어디서 그 행동이 구현되는지는 신경쓰지 않을 때
- 다중상속을 허용하고 싶을 때

---
#### ▶ Reference
- [추상클래스](https://school.programmers.co.kr/learn/courses/5/lessons/188)
- [[Java] 추상 클래스와 인터페이스의 차이](https://velog.io/@new_wisdom/Java-추상-클래스와-인터페이스의-차이)
- 남궁성, 자바의 정석, 도우출판

---