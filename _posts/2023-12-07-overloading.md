---
layout: post
title: >
  메서드 오버로딩 (Overloading)
tags: [Java]
---

## 1. 메서드 오버로딩 (Overloading)
클래스 내에 같은 이름의 메서드를 여러 개 정의하는 것을 메서드 오버로딩이라고 한다. 
자바는 메서드를 메서드 시그니처로 구분하며, 메서드 시그니처는 메서드명과 더불어 파라미터로 이루어지기 때문에 메서드명이 같더라도 파라미터가 다르면 다른 메서드로 간주된다.

이를 통해 오버로딩의 조건을 정리하자면 다음과 같다.

- 메서드의 이름이 같아야 한다.
- 매개변수의 개수 또는 타입이 달라야 한다.
- 반환 타입은 메서드 시그니처의 구성 요소가 아니므로, 메서드 오버로딩에 아무런 영향을 미치지 못한다.

```java
public class Overloading {
    int add (int a, int b) { return a + b; }

    // 오버로딩 불가능 예시 1
    // 매개변수의 개수와 타입이 같기에 두 메서드를 구분하는 것이 불가능하다.
    // int add (int x, int y) { return x + y; }

    // 오버로딩 불가능 예시 2
    // 반환타입은 오버로딩을 구현하는데 아무런 영향을 주지 못 한다.
    // long add (int x, int y) { return x + y; }
}
```

---
## 2. println
메서드 오버로딩의 가장 대표적인 예시는 `System.out.println()` 메서드이다.
표준 출력용 스트림인 `PrintStream` 클래스는 어떤 종류의 파라미터를 대입해도 그 값을 출력할 수 있도록 여러 개의 `println()` 메서드를 정의하고 있다.

```text
void	println()           Terminates the current line by writing the line separator string.
void	println(boolean x)  Prints a boolean and then terminate the line.
void	println(char x)     Prints a character and then terminate the line.
void	println(char[] x)   Prints an array of characters and then terminate the line.
void	println(double x)   Prints a double and then terminate the line.
void	println(float x)    Prints a float and then terminate the line.
void	println(int x)      Prints an integer and then terminate the line.
void	println(long x)     Prints a long and then terminate the line.
void	println(Object x)   Prints an Object and then terminate the line.
void	println(String x)   Prints a String and then terminate the line.
```

---
## 3. 오버로딩의 장점
오버로딩의 장점은 다음과 같다.

- 근본적으로 같은 기능을 하는 메서드들을 사용하는 쪽에서 구분하지 않아도 괜찮다. 오버로딩이 불가능하다면 파라미터에 따라 다른 메서드의 이름을 모두 외워야 할 것이다.
- 메서드를 작성하는 쪽에서도 이득이다. 메서드에 사용되는 이름을 절약할 수 있기 때문이다.
오버로딩이 불가능하다면 같은 기능을 하는 메서드들에 대해 전부 다른 이름을 지정해 줘야 한다는 부담이 생긴다.

---
## 4. 가변인자의 오버로딩
JDK 1.5에서 추가된 가변인자는 파라미터의 개수를 동적으로 지정해 줄 수 있도록 하기 위해서 고안되었다.
`타입... 변수 이름` 의 형태로 선언하며, 실제 실행시에는 내부적으로 배열을 생성하여 가변인자를 다룬다.

### 1. 내부적으로 배열을 생성
성능에 민감한 상황이라면 가변인자가 걸림돌이 될 수 있다. 가변인자 메서드는 호출될 때마다 배열을 새로 하나 할당하고 초기화하기 때문이다.

```java
public class Overloading {
    private int add(int... arr) {
        // 내부적으로 int[] 배열이 생성된다.
        return Arrays.stream(arr).reduce(0, Integer::sum);
    }
}
```

가변인자의 유연성이 필요하지만, 성능에 대한 비용을 감당할 수 없는 상황이라면 다중정의를 통해 해결할 수 있다.

```text
public void foo() {}
public void foo(int a1) {}
public void foo(int a1, int a2) {}
public void foo(int a1, int a2, int a3) {}
public void foo(int a1, int a2, int a4, int... rest) {}
```

### 2. 가변인자 파라미터는 가장 마지막
가변인자를 매개변수로 선언할 때, 가변인자 이외의 매개변수가 존재한다면 가변인자를 매개변수 중 제일 마지막에 선언해야 한다.
그렇지 않으면 들어온 파라미터가 가변인자인지 아닌지 구분할 수 있는 방법이 없기 때문에 컴파일 에러가 발생한다.

```text
Vararg parameter must be the last in the list
```

```java
public class Overloading {
    // 컴파일 에러 발생
    // Vararg parameter must be the last in the list
    private int add(int... arr, String r) {
        return Arrays.stream(arr).reduce(Integer::sum).orElse(0);
    }
}
```

### 3. 애매한 메서드 호출
또, 가변인자를 사용해 메서드를 작성하면 어떤 메서드를 호출하는 것인지 컴파일러가 정확히 파악하는 것이 불가능할 수 있다.
예를 들면 아래와 같이 메서드의 파라미터를 가변인자와 같은 타입으로 지정하는 메서드를 생성하는 경우이다.

```java
public class Overloading {
    private int add(int identity, int... arr) {
        return Arrays.stream(arr).reduce(identity, Integer::sum);
    }

    private int add(int... arr) {
        return Arrays.stream(arr).reduce(0, Integer::sum);
    }
}
```

이 때, 컴파일러는 두 메서드의 시그니처가 다르기 때문에 선언만으로는 컴파일 에러를 발생시키지 않는다.
하지만, 실제 이 애매한 메서드를 호출할 경우 어떤 메서드를 사용할 것인지 컴파일러는 파악할 수 없고 이로 인해 컴파일 에러가 발생한다.

```text
Ambiguous method call
```

```java
public class Overloading {
    void overloading() {
        // 컴파일 에러 발생
        // Ambiguous method call
        new Overloading().add(13);
    }

}
```

---
#### ▶ Reference
- [[Java]오버로딩 & 오버라이딩(Overloading & Overriding)](https://hyoje420.tistory.com/14)
- [메소드 오버로딩](http://www.tcpschool.com/java/java_usingMethod_overloading)
- [Class PrintStream](https://docs.oracle.com/en/java/javase/11/docs/api/java.base/java/io/PrintStream.html)
- 남궁성, 자바의 정석, 도우출판
- Joshua Bloch, 이복연 옮김, EFFECTIVE JAVA 3/E, 프로그래밍인사이트

---