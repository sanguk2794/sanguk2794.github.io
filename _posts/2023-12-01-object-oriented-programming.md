---
layout: post
title: >
  객체 지향 프로그래밍 (OOP, Object-Oriented Programming)
tags: [Java]
---

# 객체 지향 프로그래밍 (OOP, Object-Oriented Programming)

---
## 1. 객체 지향 프로그래밍
프로그램 설계 방법론이자 개념의 일종이다.

실제 사물의 속성과 기능을 분석해 변수와 메서드로 정의함으로써, 실제 세계를 컴퓨터 속의 가상 세계로 재창조하는 프로그래밍 기법을 말한다.
객체 지향 이론은 상속, 캡슐화, 추상화 개념을 중심으로 구체적으로 발전했으며, 대표적인 객체지향 언어에는 Java, C++, C#등이 포함된다.

객체 지향 방법론을 따르는 프로그램들은 정의한 객체들을 필요에 따라 조합해 전체 프로그램을 완성해 나간다.
객체지향 프로그래밍시에는 다음의 키워드를 생각하며 코드를 작성하는 것이 좋다.

- 재사용성
- 유지보수
- 중복된 코드의 제거

자바는 성능을 위해 8개의 기본형 타입을 구현하고 있으므로 완벽한 객체지향 언어는 아니다. 기본형 타입은 힙 공간을 참조할 필요가 없으므로 속도가 보다 빠르다.

---
## 2. 객체지향 언어의 특징
객체지향 언어, 자바는 여러 특징이 있지만 가장 대표적인 특징은 다음과 같다.

- 캡슐화
- 은닉
- 상속
- 다형성

### 1. 캡슐화 (Encapsulation)
객체의 속성을 표현하는 변수와 객체의 행위를 표현하는 함수를 하나의 단위로 묶어 구현한다. 즉, 데이터의 번들링(bundling)이며, 
자바에서는 이를 클래스를 통해 구현한다. 해당 클래스의 인스턴스 생성을 통해 클래스 안에 포함된 멤버 변수와 메서드에 접근 가능하다.

`Man` 클래스는 사람을 정의한 클래스이다. 이 클래스는 이름과 나이를 속성으로 가지고 있으며, 일하다라는 행위를 정의하고 있다.

```java
public class Man {
    String name;
    int age;
    
    public void work() {
        System.out.println("Working.....");
    }
}
```

---
### 2. 은닉 (Information Hiding)
클래스 내부의 구조를 감춘다. 외부로의 노출을 최소화하여 모듈 간의 결합도를 떨어뜨려 유연함과 유지보수성을 높인다.

```java
public class Man implements Comparable<Man> {
    // sequence를 비교에만 사용하고자 할 때, 접근제어자를 통해 외부로의 노출을 최소화할 수 있다.
    // 이를 통해 모듈 간의 결합도를 떨어뜨려 유연함과 유지보수성을 높일 수 있다.
    private static int memberSequence = 0;
    private int sequence;

    public Man() {
        memberSequence++;
        sequence = memberSequence;        
    }

    public int getSequence() {
        return sequence;
    }

    @Override
    public int compareTo(Man other) {
        return Integer.compare(this.sequence, other.sequence);
    }
}
```

또, 객체에 대한 비정상적 접근을 제한할 수 있다. 자바에서는 이를 접근 제어자를 통해 제어할 수 있다.

```java
// 객체에 대한 비정상적인 접근을 제한할 수 있다. 예를 들어 사람의 나이를 -50살로 설정하는 것은 불가능해야 한다.
// 이를 age 변수에 대한 private 선언과 setAge() 메서드를 통한 값 제한을 통해 구현하고 있다.
public class Man {
    
    private String name;
    private int age;
    
    public void introduce() {
        System.out.println("My name is " + name + "\nI'm " + age + "years old");
    }
    
    public void setAge(int age) {
        if (age > 200 || age < 0) {
            throw new IllegalArgumentException();
        }

        this.age = age;
    }
}

public class Main {
    public static void setAge() {
        Man man = new Man();
        man.setAge(-50); // throw IllegalArgumentException
    }
}
```

---
### 3. 상속 (Inheritance)
기존의 클래스를 재사용하여 새로운 클래스를 작성하는 것을 말한다. 자바에서는 조상 클래스에 멤버를 추가할 경우 모든 자손 클래스가 그 멤버를 상속받는다.
상속을 위한 키워드는 `extends`이며 조상 클래스를 확장한다는 의미를 담고 있다.

조상 클래스에 멤버를 추가할 경우 모든 자손 클래스가 그 멤버를 상속받기 때문에 비효율적인 코드 중복을 제거할 수 있다.
또, 조상 클래스를 한번만 수정하면 자손 클래스 전체가 수정되므로 때문에 유지보수 또한 편리해진다.

```java
// 아이언맨과 슈퍼맨은 모두 자기소개를 할 수 있으며, 자바에서는 이를 공통으로 관리할 수 있다.
public class Man {
    private String name;
    private int age;
    
    public void introduce() {
        System.out.println("My name is " + name + "\nI'm " + age + "years old");
    }
}

// 슈퍼맨은 인간이다. 그러나 날 수 있다.
public class SuperMan extends Man {
    public void fly() {
        System.out.println("Flying.........");
    }
}

// 아이언맨은 수트를 가지고 있다.
public class IronMan extends Man {
    private Suit suit;
}

// 수트를 통해 날 수 있다...
public class Suit {
    public void fly() {
        System.out.println("Flying.........");
    }
}
```

---
### 4. 다형성
여러 가지 형태를 가질 수 있는 능력으로 상속으로 인해 발생하는 부가적 특성이다. 
다형성 구현을 위한 수단으로 다형적 변수, 메서드 오버라이딩, 메서드 오버로딩이 있다.

#### 1. 다형적 변수
자식 인스턴스를 부모의 참조변수로 가리킬 수 있다. 조상클래스, 인터페이스를 통해 구현하며 서로 같은 성질을 가지기 때문에 같은 방식으로 처리가 가능해진다.

같은 타입의 인스턴스라도 참조변수의 타입에 따라 사용할 수 있는 멤버의 개수가 달라진다.
다만, 참조변수가 사용할 수 있는 멤버의 개수는 인스턴스의 멤버 개수보다 같거나 적어야 한다.

```java
public class Man { }
public class SuperMan extends Man { }
public class IronMan extends Man {}

public class Main {
    public void man() {
        Man man = new Man();
        // 자식 인스턴스를 부모의 참조변수로 가리킬 수 있다.
        Man superMan = new SuperMan();
        Man ironMan = new IronMan();
    }
}
```

#### 2. 메서드 오버로딩
같은 클래스 안에서 같은 이름의 메서드를 여러 개 정의하는 것을 말한다.
같은 이름의 메서드가 있더라도 매개변수의 개수 또는 타입이 다르면 같은 이름을 사용해 메서드를 정의할 수 있다.

```java
// 같은 add 연산이지만, 2개 값을 더할 수 있고, 3개 값을 더할 수 있다.
public class Calculator {
    private int add (int a, int b) { return a + b; }
    private int add (int a, int b, int c) { return a + b + c; }
}
```

#### 3. 메서드 오버라이딩
조상클래스로부터 상속받는 메서드를 자손 클래스에 맞게 재정의해 사용하는 것이다.

오버라이딩 메서드의 선언부는 조상의 것과 완전히 일치해야 한다. 
또, 반환 값은 조상클래스에서 자손 클래스 타입으로 변경 하는 것이 허용되나, 일반적으로 반환 타입이 같아야 한다.

```java
// 아이언맨과 슈퍼맨은 자신만의 특별한 자기 소개가 가능하다.
public class Man {
    private String name;
    private int age;
    
    public void introduce() {
        System.out.println("My name is " + name + "\nI'm " + age + "years old");
    }
}

public class SuperMan extends Man {
    @Override
    public void introduce() {
        System.out.println(super.introduce() + "\nI can fly");
    }
}

public class IronMan extends Man {
    @Override
    public void introduce() {
        System.out.println(super.introduce() + "\nI have a suit");
    }
}
```

---
#### ▶ Reference
- [객체 지향 프로그래밍](https://namu.wiki/w/객체%20지향%20프로그래밍)
- [OOP의 네가지 특징(추상화/캡슐화/상속/다형성)](https://velog.io/@0sunset0/OOP의-네가지-특징추상화캡슐화상속다형성)
- 남궁성, 자바의 정석, 도우출판

---