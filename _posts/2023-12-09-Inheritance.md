---
layout: post
title: >
  상속 (Inheritance)
tags: [Java]
---

## 1. 상속 (Inheritance)
상속이란 기존의 클래스를 재사용해서 새로운 클래스를 작성하는 것을 말한다.
상속을 통해 코드를 작성하면 보다 적은 양의 코드로 새로운 클래스를 작성할 수 있고 중복되는 코드를 공통적으로 관리할 수 있기 때문에 코드의 수정 및 유지보수가 용이해진다.

상속을 구현할 때에는 클래스명 뒤에 `extends` 키워드와 함께 상속받고자 하는 클래스명을 써 주기만 하면 된다.

```text
클래스명 extends 클래스명 { }
```

실제 `extends`를 명시적으로 추가한 아래의 두 클래스를 표현할 때, 두 클래스가 서로 상속 관계에 있다고 표현한다.

```java
public class Parent { }
public class Child extends Parent { }
```

---
## 2. 조상 클래스와 자손 클래스
상속 관계에 있는 두 클래스 중 상속되는 클래스를 조상 클래스라고 하고 상속받는 클래스를 자손 클래스라고 한다.

- 조상 클래스 : 부모 (Parent) 클래스, 상위 (Super) 클래스 
- 자손 클래스 : 자식 (Child) 클래스, 하위 (Sub) 클래스

자손 클래스는 조상 클래스의 모든 멤버를 상속받기 때문에 항상 조상 클래스보다 같거나 많은 멤버를 갖는다.
이러한 특성으로 인해 상속에 상속을 거듭할수록 상속받는 클래스는 멤버 개수가 점점 늘어나게 된다.
이러한 이유로 상속의 키워드가 확장의 의미인 `extends`로 지정되었다.

단, 상속 시 생성자와 초기화 블럭은 상속되지 않는다. 이 부분에 대해서는 주의가 필요하다.

조상 클래스가 변경되면 자손 클래스는 자동으로 영향을 받게 되지만, 자손 클래스가 변경되는 것은 조상 클래스에 아무런 영향을 미치지 않는다.

```java
public class SuperClass {
    int superClassMember = 10;

    // 자손 클래스의 멤버를 사용하려 하면 조상 클래스에 선언되어 있지 않은 멤버이기 때문에 `Cannot resolve symbol` 컴파일 에러가 발생한다.
    @Override
    public String toString() {
        return "SuperClass{" +
                "superClassMember=" + superClassMember +
                // ", subClassMember=" + subClassMember +
                '}';
    }
}

public class SubClass extends SuperClass {
    int subClassMember = 20;

    // 조상 클래스의 멤버를 불러와 사용할 수 있다.
    @Override
    public String toString() {
        return "SubClass{" +
                "superClassMember=" + superClassMember +
                ", subClassMember=" + subClassMember +
                '}';
    }
}
```

---
## 3. 포함 관계
상속 관계를 설정하지 않더라도 다른 클래스를 재사용할 수 있다. 클래스의 멤버변수로 다른 클래스 타입의 참조변수를 선언하는 것이다. 이러한 관계를 포함 관계라고 한다.

```java
public class Inheritance {    
    // 포함 관계, 참조변수를 통해 다른 클래스를 재사용하고 있다.
    private static final SuperClass superClass = new SuperClass();

    public static void print() {
        System.out.println(superClass);
    }
}
```

---
## 4. 클래스간의 관계 설정
클래스를 작성하는데 있어서 상속 관계를 맺을것인지 포함 관계를 맺을것인지 결정해야 할 때, 다음과 같은 문장을 만들어 명확히 할 수 있다.

- 원은 점이다 : Circle is a Point (`is a` 관계)
- 원은 점을 가지고 있다 : Circle has a Point (`has a` 관계)

`is a` 관계일 경우 상속 관계를, `has a` 관계일 경우 포함 관계를 맺어주면 된다.

---
## 5. 단일 상속
자바에서는 하나의 클래스가 다른 하나의 클래스에서만 상속을 받을 수 있도록 하고 있다.

단일 상속만을 허용할 경우, 클래스가 하나의 조상 클래스만을 가질 수 있기 때문에 다중 상속에 비해 불편한 점도 있지만 클래스 간의 관계가 보다 명확해지고 코드를 더욱 신뢰할 수 있게 만들어 준다는 점에서는 다중 상속보다 유리하다.

```java
// 두 개의 클래스를 상속 관계로 선언할 경우 컴파일 에러가 발생한다.
// Class cannot extend multiple classes
public class SubClass extends SuperClass { //, SuperClass2
    // ...
}
```

---
## 6. 자바에서의 다중 상속
기본적으로 자바는 단일 상속만을 허용한다. 하지만 자바는 인터페이스를 통해 다중 상속을 구현할 수 있다.
인터페이스의 메서드는 어떤 로직도 가지고 있지 않기 때문에 다중 상속에서 발생하는 메서드 선택의 문제가 발생하지 않는다.

### 1. 다이아몬드 문제
다중 상속을 지원하게 되면 하나의 클래스가 여러 상위 클래스를 상속 받을 수 있으며 이런 특징 때문에 발생하게 되는 문제가 바로 다이아몬드 문제이다.
상속받는 두 클래스의 멤버 중 선언부가 같은 경우 어떤 메서드를 선택해야 할 것인지 알 수 없게 되는 것이다.

![diamond](https://drive.google.com/uc?export=view&id=1R6IO6AHnDYeiB9DmNcRfS7FzZu4VCkH8)

출처 : [자바는 왜 다중 상속을 지원하지 않을까? (다이아몬드 문제)](https://siyoon210.tistory.com/125)

이를 코드로 나타내면 다음과 같은 모습이 된다.

```java
class Father {
    @Override
    public String toString() {
        return "Father";
    }
}

class Mother {
    @Override
    public String toString() {
        return "Mother";
    }
}

class Son extends Father, Mother {
    @Override
    public String toString() {
        super.toString(); // Father를 출력해야 할까? Mother를 출력해야 할까?
    }
}
```

`Son` 클래스 입장에서는 `toString()` 메서드가 두개의 상위 클래스에 모두 정의되어 있기 때문에, 어떤 메서드를 실행해야 될지 알 수가 없다.

자바는 기본적으로 단일 상속만을 허용하기 때문에 해당 문제가 발생하지 않는다. 
인터페이스는 같은 메서드가 있더라도 다중 구현이 가능한데, 그 이유는 메서드의 구현이 없기 때문에 애초에 선택 문제가 발생하지 않기 때문에다.

```java
interface Father {
    void method();
}

class Mother {
    void method();
}

// Father.method(), Mother.method()는 애초에 구현이 존재하지 않는다. 그렇기 때문에 선택의 문제가 발생하지 않는다.
class Son implements Father, Mother {
    @Override
    void method() {
        System.out.println("Son");
    }
}
```

### 2. default method
사실 자바 1.8에서 디폴트 메서드가 추가되었기 때문에 인터페이스도 코드 구현이 가능하다. 이 경우에는 해당 클래스를 하위 클래스에서 재정의하지 않는 이상 컴파일 에러가 발생한다.

---
#### ▶ Reference
- [자바는 왜 다중 상속을 지원하지 않을까? (다이아몬드 문제)](https://siyoon210.tistory.com/125)
- 남궁성, 자바의 정석, 도우출판

---