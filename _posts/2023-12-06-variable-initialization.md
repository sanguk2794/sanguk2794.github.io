---
layout: post
title: >
  변수의 초기화
tags: [Java]
---

# 변수의 초기화

---
## 1. 변수의 초기화
변수를 선언하고 처음으로 값을 저장하는 것을 변수의 초기화라고 한다. 변수는 선언과 동시에 적절한 값으로 초기화하는 것이 바람직하다.

변수의 초기화에는 명시적 초기화와 초기화 블럭, 생성자를 사용한다.

- 명시적 초기화
- 초기화 블럭
- 생성자

---
## 2. 명시적 초기화 (Explicit Initialization)
변수를 선언과 동시에 초기화하는 것을 명시적 초기화라고 한다. 가장 기본적이면서도 간단한 초기화 방법이므로 여러 초기화 방법 중에서 가장 우선적으로 고려되어야 한다.

```java
public class ExplicitInitialization {    
    private int serialNumber = 100; // 기본형 변수의 명시적 초기화
    private Point point = new Point(); // 참조형 변수의 명시적 초기화
    
    private static class Point {
        int x, y;
    }
}
```

---
## 3. 초기화 블럭 (Initialization Block)
초기화 블럭에는 클래스 초기화 블럭과 인스턴스 초기화 블럭이 포함된다.

### 1. 클래스 초기화 블럭 
클래스가 메모리에 처음 적재될 때 한 번만 실행된다.

```text
static {
    // 메모리에 처음 적재될 때 한 번만 실행된다.
}
```

### 2. 인스턴스 초기화 블럭 
인스턴스를 생성할 때마다 실행된다. 인스턴스 초기화 블럭은 생성자와 마찬가지로 초기화 작업을 수행하며, 여러 생성자에서 중복되는 코드를 작성하는 것을 피하기 위해 주로 사용된다.

```text
{
    // 인스턴스 생성시 실행된다.
}
```

```java
public class InitializationBlock {
    private static int sequence;
    private int serialNumber;
    private Point point;

    // 클래스 초기화 블럭 -> 메모리에 처음 적재될 때 한 번만 실행된다.
    static {
        sequence = 1;
    }
    
    // 인스턴스 초기화 블럭 -> 인스턴스 생성시 실행된다.
    {
        serialNumber = sequence++;
    }

    private static class Point {
        int x, y;
    }
}
```

---
## 4. 생성자 (Constructor)
인스턴스가 생성될 때 호출되는 인스턴스 초기화 메서드이다. 인스턴스의 초기화와 인스턴스 생성 시 실행되어야 할 작업을 처리하기 위해 사용된다.

클래스 내에 선언되며 메서드와 유사한 구조를 지닌다. 단, 생성자의 이름은 클래스 이름과 같아야 하며 반환 값이 없다는 것이 메서드와의 차이점이다.

```text
클래스명 () {
    // 인스턴스 생성시 실행된다.
}
```

```java
public class Constructor {
    private int serialNumber;
    private Point point;

    // 생성자 -> 인스턴스 생성시 실행된다.
    public Constructor(int serialNumber, Point point) {
        this.serialNumber = serialNumber;
        this.point = point;
    }
    
    private static class Point {
        int x, y;
    }
}
```

---
## 5. 초기화 적용 순서
### 1. 클래스 변수 초기화 적용 순서
클래스 변수의 초기화는 아래의 순서로 적용된다.

- 기본값
- 명시적 초기화
- 클래스 초기화 블럭

```java
public class Initialization {
    private static int count = 3;

    static {
        count = 5;
    }

    public void print() {
        System.out.println("static value = " + new ClassInitialization().count); // static value = 5
    }
}
```

`count` 클래스 변수의 초기화 과정을 정리하면 다음과 같다.

- 기본값으로 초기화된다. `count` 변수는 `int` 타입의 기본 값인 0으로 초기화된다.
- 명시적 초기화가 수행된다. `count` 변수의 값이 3으로 초기화된다.
- 클래스 초기화 블럭이 실행된다. `count` 변수의 값이 5으로 초기화된다.
- 결과적으로 `count` 변수의 값은 5가 된다.

### 2. 인스턴스 변수 초기화 적용 순서
인스턴스 변수의 초기화는 아래의 순서로 적용된다.

- 기본값
- 명시적 초기화
- 인스턴스 초기화 블럭
- 생성자

```java
public class ClassInitialization {
    private int count = 100;

    // 인스턴스 초기화 블럭 
    {
        count = 15;
    }

    // 생성자
    public ClassInitialization() {
        this.count = 35;
    }

    public void print() {
        System.out.println("static value = " + new ClassInitialization().count); // static value = 5
    }
}
```

`count` 인스턴스 변수의 초기화 과정을 정리하면 다음과 같다.

- 기본값으로 초기화된다. `count` 변수는 `int` 타입의 기본 값인 0으로 초기화된다.
- 명시적 초기화가 수행된다. `count` 변수의 값이 100으로 초기화된다.
- 인스턴스 초기화 블럭이 실행된다. `count` 변수의 값이 15로 초기화된다.
- 생성자가 실행된다. `count` 변수의 값이 35로 초기화된다.
- 결과적으로 `count` 변수의 값은 35가 된다.

---
#### ▶ Reference
- 남궁성, 자바의 정석, 도우출판

---