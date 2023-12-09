---
layout: post
title: >
  재귀 호출 (Recursive call)
tags: [Java]
---

## 1. 재귀 호출 (Recursive call)
메서드 내부에서 메서드 자신을 다시 호출하는 것을 재귀 호출이라고 하며, 재귀 호출을 하는 메서드를 재귀 메서드라고 한다.
메서드 입장에서는 자신을 호출하는 것과 자신을 호출하는 것은 전혀 차이가 없다.

하지만, 무한히 자신을 호출하는 무한반복에 빠지게 되기 때문에 반드시 해당 메서드를 탈출 할 수 있는 조건문과 함께 사용되어야 한다.

---
## 2. 재귀 호출과 반복문
재귀 호출은 반복문과 유사한 점이 많으며 대부분의 재귀 호출은 대부분의 상황에서 반복문으로 변경해서 작성할 수 있다.

재귀 호출은 논리적으로 간결한 표현이 가능하다는 장점이 있으나, 매개변수 복사와 종료 후 복귀할 주소의 저장 등의 비용이 추가로 필요하기 때문에 반복문보다 수행 시간이 오래 걸리기 때문에,
재귀 호출의 간결함이 주는 이득이 호출에 드는 비용보다 큰 경우에만 재귀 호출을 사용하도록 하자.

아래 코드는 피보나치 수열을 재귀 호출을 통해 구현한 코드이다.

```java
public class Main {
    public void recursive() {
        System.out.println("fibonacci = " + fibonacci(5));
    }

    private int fibonacci(int n) {
        if (n <= 1)
            return n;
        return fibonacci(n-1) + fibonacci(n-2);
    }
}
```

이렇게 재귀적으로 풀면 같은 연산을 여러 번 수행하는 굉장히 비효율적인 코드가 된다. 
이 뿐만 아니라, 매개변수 복사와 종료 후 복귀할 주소의 저장 등의 비용 또한 추가로 필요하다.

![Pibonacci](https://drive.google.com/uc?export=view&id=1tdxdfzANHF_zPI_6_gk4VBn0sLE5Uy4F)

출처 : [Dynamic Programming(피보나치)](https://velog.io/@suseodd/Dynamic-Programming피보나치)

반복문이나 메모이제이션을 통해 피보나치 수열을 구현하면 위와 같은 재귀 호출의 비효율성을 해결할 수 있다. 
메모이제이션은 반복해서 계산하는 작업을 미리 저장해 두고 다시 그 계산이 필요할 때 미리 저장한 값을 불러오는 것으로 프로그램 속도를 높이는 방법이다.

```java
public class Main {
    public void iterative() {
        System.out.println("fibonacci = " + fibonacci(5));
    }
    
    // 반복문을 통해 피보나치 수열을 구현하고 있다.
    private int fibonacci(int n) {
        if (n <= 1) {
            return n;
        }

        int[] fib = new int[n + 1];
        fib[0] = 0;
        fib[1] = 1;

        for (int i = 2; i <= n; i++) {
            fib[i] = fib[i - 1] + fib[i - 2];
        }

        return fib[n];
    }

    // 메모이제이션을 통해 피보나치 수열을 구현하고 있다.
    public void memoized() {
        int n = 5;
        System.out.println("fibonacci = " + fibonacciMemoized(n, new int[n + 1]));
    }

    private int fibonacciMemoized(int n, int[] memo) {
        if (n <= 1) {
            return n;
        }

        // 이미 계산한 값이 메모에 있다면 해당 값을 반환
        if (memo[n] != 0) {
            return memo[n];
        }

        // 계산하지 않은 값일 경우 재귀적으로 계산하고 메모에 저장
        memo[n] = fibonacciMemoized(n - 1, memo) + fibonacciMemoized(n - 2, memo);

        return memo[n];
    }
}
```

배열에 값을 저장해두고, 그 값을 이용해 연산을 수행하기 때문에 여러 번의 메서드 호출이 필요하지 않다.

---
## 3. 재귀함수 사용시의 시간, 공간복잡도
재귀 함수의 시간복잡도 계산은 이전 호출의 입장에서 n번째 재귀 호출의 값을 알아내아 하므로 계산하는 것이 상대적으로 어렵다.
위의 피보나치 수열을 재귀로만 동작시켰을 경우의 시간 복잡도는 O(2<sup>n</sup>)으로 지수적으로 증가한다.

그리고 재귀의 공간복잡도는 `Call Stack`에 호출되어 쌓이는 메서드의 증가 비율이 된다. 자바는 꼬리 재귀 최적화를 제공하지 않는 언어이기에 보다 주의가 필요하다.

그러므로, 재귀를 사용할 때는 시간복잡도 뿐만 아니라 공간복잡도도 고려해야 한다.

참고로, 시간 복잡도와 공간 복잡도는 `n log n`까지 괜찮은 알고리즘으로 평가되며 n<sup>2</sup>를 넘어가면 데이터의 범위의 고려가 필요해진다.

### 1. 꼬리 재귀 최적화
꼬리 재귀 최적화란 재귀를 마지막에 호출하면 자신의 자원을 재사용하도록 최적화를 수행하는 것을 말한다. 
꼬리 재귀 최적화를 사용하기 위해서는 아래의 2가지 조건을 만족해야 한다.

- 재귀 함수를 꼬리 재귀 방식으로 구현할 것 
- 컴파일러가 꼬리 재귀 최적화를 지원할 것

아래 코드는 팩토리얼 함수를 두개로 분리했는데 요점은 재귀 호출 이후 추가적인 연산을 요구하지 않도록 구현한 것이다.
하지만 자바 컴파일러는 꼬리 재귀 최적화를 지원하지 않으므로 별로 의미는 없다.

```java
public class Main {
    // 꼬리 재귀 최적화를 고려하지 않은 코드, 재귀 이후 * n 연산이 존재한다.
    private int factorial(int n) {
        if (n <= 1) {
            return 1;

        } else {
            return factorial(n-1) * n;
        }
    }

    private int factorialTail(int n) {
        return factorialTail(n, 1);
    }

    // 꼬리 재귀 최적화를 고려한 코드, 재귀 이후 * n 연산이 존재한다.
    private int factorialTail(int n, int acc) {
        if (n == 1) {
            return acc;
        }

        // 재귀 호출 이후의 연산이 없는 것을 확인할 수 있다.
        return factorialTail(n-1, acc * n);
    }
}
```

참고로, `C++`, `C#`, `Kotlin`, `Swift` `JavaScript ES6`나 `JVM` 위에서 동작하는 `Scala` 등의 언어는 꼬리 재귀 최적화를 지원한다.
지원하는 언어와 지원하지 않는 컴파일러가 나뉘어 있으므로 꼬리 재귀를 사용하고자 할 경우는 반드시 컴파일러의 스펙을 확인하도록 하자.

---
#### ▶ Reference
- [재귀함수와 꼬리 재귀](https://velog.io/@dldhk97/재귀함수와-꼬리-재귀)
- [Dynamic Programming(피보나치)](https://velog.io/@suseodd/Dynamic-Programming피보나치)
- 남궁성, 자바의 정석, 도우출판

---