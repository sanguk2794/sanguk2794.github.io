---
layout: post
title: >
  다형적 변수 (Polymorphic Variables)
tags: [Java]
---

## 1. 다형성 (Polymorphism)
다형성은 여러 가지 형태를 가질 수 있는 능력을 의미하며 상속으로 인해 발생하는 부가적 특성이다.

자바는 다형성을 구현하기 위한 수단으로 다형적 변수와 메서드 오버라이딩, 메서드 오버로딩, 함수형 인터페이스를 제공한다.

---
## 2. 다형적 변수 (Polymorphic Variables)
조상클래스 타입의 참조변수로 자손클래스의 인스턴스를 참조할 수 있음을 의미한다.
조상 클래스 또는 인터페이스를 통해 구현하며 서로 같은 성질을 가지기 때문에 서로 다른 클래스의 인스턴스를 같은 방식으로 처리할 수 있다.

같은 타입의 인스턴스라도 참조변수의 타입에 따라 사용할 수 있는 멤버의 개수가 달라진다.
다만, 참조변수가 사용할 수 있는 멤버의 개수는 인스턴스의 멤버 개수보다 같거나 적어야 한다.

```java
public class Main {
    public void polymorphism() {
        // 조상클래스 타입의 참조변수로 자손클래스의 인스턴스에 접근 가능하다.
        Food food = new Ramen();
        Ramen ramen = new Ramen();
        // 반대의 경우는 불가능하다. 자손클래스가 대부분 상황에서 사용 가능한 멤버의 수가 더 많기 때문이다.
        // Ramen ramen = new Food();
    }
    
    class Food { }
    class Ramen extends Food { }
}
```

조상클래스 타입의 참조변수로 자손클래스 타입의 인스턴스를 참조할 경우에는 조상클래스에 선언되어 있는 멤버에만 접근할 수 있다.
실제 같은 타입의 인스턴스라도 참조변수의 타입에 따라 사용할 수 있는 멤버가 달라진다는 것이다.

반대로 자손클래스 타입의 참조변수로 조상클래스의 인스턴스를 참조하는 것은 불가능하다.
대부분 상황에서 자손클래스의 멤버의 수가 조상클래스보다 더 많기 때문이다.

## 2. 참조변수에 따른 멤버 호출
메서드의 경우 조상 클래스의 메서드를 자손의 클래스에서 오버라이딩한 경우에도 참조변수 타입에 관계없이 항상 실제 인스턴스의 메서드가 호출된다.

반면, 멤버변수의 경우 참조변수의 타입에 따라 달라진다.

- 메서드 : 참조변수의 타입과 관계없이 항상 실제 인스턴스의 메서드가 호출됨
- 멤버변수 : 참조변수의 타입에 따라 호출되는 멤버변수가 달라짐

```java
public class Main {
    public void polymorphism() {
        Food food = new Ramen();
        Ramen ramen = new Ramen();

        // 메서드의 비교
        System.out.println("food.toString() = " + food); // food.toString() = price = 10, calorie = 50, thickness = 0
        System.out.println("ramen.toString() = " + ramen); // ramen.toString() = price = 10, calorie = 50, thickness = 0

        // 변수의 비교
        System.out.println("food.price = " + food.price); // food.price = 10
        System.out.println("ramen.price = " + ramen.price); // ramen.price = 20
    }

    static class Food {
        int price = 10;
        int calorie = 50;

        @Override
        public String toString() {
            return "price = " + price + ", calorie = " + calorie;
        }
    }

    @Data
    static class Ramen extends Food {
        int thickness;
        int price = 20;

        @Override
        public String toString() {
            return super.toString() + ", thickness = " + thickness;
        }
    }
}
```

---
## 3. 참조변수의 형변환
참조형 변수간의 형변환은 서로 상속관계에 있는 클래스 사이에서만 가능하다.

- 자손타입 → 조상타입 : 업캐스팅, 형변환 생략 가능
- 조상타입 → 자손타입 : 다운캐스팅, 형변환 생략 불가

형변환은 인스턴스의 실제 값에 아무런 영향도 주지 못한다. 사용 가능한 멤버의 개수가 변할 뿐이다.
```java
public class Main {
    public void casting() {
        Sushi sushi = new Sushi();
        // 자손타입 -> 조상타입으로의 형변환, 실제 인스턴스보다 참조변수가 사용할 수 있는 멤버가 적을 것이 확실하기에 형변환을 생략할 수 있다.
        Food food = sushi;

        // 조상타입 -> 자손타입으로의 형변환, 형변환을 명시적으로 지정해주어야 한다.
        // 이러한 경우 실제 인스턴스보다 참조변수가 사용할 수 있는 멤버가 더 많아질 수 있기에 문제가 생길 수 있다.
        Sushi sushi2 = (Sushi) food;
    }
}
```

---
## 4. instanceof
프로그래밍을 하다 보면 조상 타입의 참조변수로 다루어지는 자손 타입의 객체를 자손 타입의 참조변수로 다룰 수 있도록 다운캐스팅을 해야 하는 경우가 있다. 이 때 필요한 것이 `instanceof` 연산자이다.

`instanceof`는 참조변수가 검사한 타입으로 형변환이 가능할 경우 `true`, 형변환이 불가능할 경우 `false`를 반환한다.
따라서, 실제 인스턴스와 같은 타입 뿐만 아니라 해당 인스턴스 클래스의 슈퍼 클래스와의 `instanceof` 연산도 `true`를 반환한다.

해당 연산자를 조건문의 조건으로 활용하면 형변환이 가능한 경우에만 형변환을 수행할 수 있어 형변환 시의 타입 안정성을 높일 수 있다.

```java
public class Main {
    public void instanceOf() {
        List<Food> foods = Arrays.asList(new Ramen(), new Food());

        foods.forEach(food -> {
            if (food instanceof Ramen) {
                System.out.println("casting!");

                // 조상타입 -> 자손타입으로의 형변환, 실제 인스턴스보다 참조변수가 사용할 수 있는 멤버가 더 많아질 수 있기에 문제가 생길 수 있다.
                // 따라서 instanceOf 연산자를 활용해 실제 인스턴스의 타입을 확인하는 것이 안전하다.
                Ramen ramen = (Ramen) food;
                System.out.println("ramen.toString() = " + ramen.toString());
            }
        });
    }
}
```

### 1. instanceof는 안티 패턴?
`instanceof`를 사용하면 간편하게 클래스의 타입을 확인할 수 있음에도 불구하고 보다는 다형성을 이용할 것이 권장된다.

#### 1. 캡슐화
`instanceof`를 사용하는 경우, 객체의 실제 타입을 외부에서 확인할 수 있게 된다. 이는 객체의 내부 구현을 외부로 노출시키고, 캡슐화 원칙을 위배될 수 있다.
캡슐화를 통해 각 객체가 가진 책임과 역할을 분리하는 것으로 유지보수, 확장에 있어 편리함을 얻을 수 있으나, 캡슐화가 보장되지 않기에 그 의미가 사라진다.

#### 2. 개방 폐쇄 원칙 (Open Closed Principle)
해당 예제는 복수의 SNS에 텍스트 형태와 사진 형태의 포스트가 가능하도록 기능을 구현하는 것을 목적으로 하는 예제이다.
이 때, 새로운 SNS 구현 클래스를 추가한다고 가정하자. 개발자는 새로운 SNS 타입이 추가될 때마다 이에 대응하기 위해 조건문을 추가해 주어야 할 것이다.

지금은 `Text.postOn()` 메서드 한 군데에서 사용되고 있지만, 수십개의 사용처가 있다면 그 수십 곳의 소스를 모두 수정해야 할 것이다.
이로 인해 객체의 확장이 어려워지며, 객체의 확장에는 열려있고, 변화에는 닫혀있도록 해야한다는 개방-폐쇄 원칙을 위배하게 된다.

```java
// SNS 인터페이스
interface SNS { }

// SNS 구현 클래스
class FaceBook implements SNS {}
class Twitter implements SNS {}
class Line implements SNS {}

// 포스트 인터페이스
interface Post {
    void postOn(SNS sns);
}

class Text implements Post {
    public void postOn(SNS sns) {
        if (sns instanceof FaceBook) {
            System.out.println("text -> FaceBook");
            
        } else if (sns instanceof Twitter) {
            System.out.println("text -> Twitter");
            
        } else if (sns instanceof Line) {
            System.out.println("text -> Line");
            
        } else {
            // SNS 클래스를 상속하는 새로운 클래스 Slack 클래스를 추가한다면 에러를 던지게 된다.
            throw new IllegalArgumentException();
        }
    }
}
```

#### 3. 단일 책임 원칙 (Single Responsibility Principle)
`instanceof`가 사용되는 이유는 특정 타입임을 알아내고 특정 코드를 실행하기 위해서이다. 
이것은 `instanceof를` 사용하는 `postOn()` 메서드가 FaceBook, Twitter, Line의 포스트 구현을 모두 알고 있어야 하는 책임이 부가되는 것이다. 각 타입에게 책임을 부여하면 되는 일이 하나의 메서드에게 모든 책임이 가중되는 일이 발생한 것이다. 

이로 인해, 한 클래스는 하나의 책임만 가져야 한다는 객체지향프로그래밍의 원칙 중 하나인 단일 책임 원칙을 위배하게 된다.

#### 4. 대안
좀 더 객체지향적인 방법은 다형성을 활용하여 각 클래스가 자신의 책임을 수행하도록 하는 것이다. 

```java
// SNS 인터페이스에 동작을 추가해 각 클래스가 자신의 책임을 수행하도록 변경했다.
interface SNS {
    void post();
}

// SNS 구현 클래스
class FaceBook implements SNS {
    @Override
    void post() {
        System.out.println("FaceBook Post");
    }
}

class Twitter implements SNS {
    @Override
    void post() {
        System.out.println("Twitter Post");
    }
}

class Line implements SNS {
    @Override
    void post() {
        System.out.println("Line Post");
    }
}

interface Post {
    void postOn(SNS sns);
}

// 수정을 통해 SNS 구현 클래스가 추가되더라도 추가 대응을 할 필요가 없게 되었다.
class Text implements Post {
    public void postOn(SNS sns) {
        sns.post();
    }
}
```

---
## 5. 다형적 변수를 활용한 공통 메서드의 구현
상위 클래스의 참조변수를 매개변수로 선언함으로써 하위 클래스의 모든 인스턴스를 같은 메서드로 처리할 수 있다.
이를 활용하면 슈퍼 클래스를 상속하는 다른 서브클래스를 선언해도 로직의 변경 없이 해당 메서드의 파라미터로 사용할 수 있다.

```java
public class Main {
    public void cook(Food food) {
        System.out.println(food.toString());
    }

    public void cooking() {
        cook(new Food());
        cook(new Ramen());
        cook(new Sushi());
    }
}
```

---
#### ▶ Reference
- [2020.8.11일자 수업 : 상속과 다형적 변수](https://eungeun506.tistory.com/19)
- [다형성(Polymorphism)이란?](https://tecoble.techcourse.co.kr/post/2020-10-27-polymorphism/)
- [instanceof의 사용을 지양하자](https://tecoble.techcourse.co.kr/post/2021-04-26-instanceof/)
- 남궁성, 자바의 정석, 도우출판

---