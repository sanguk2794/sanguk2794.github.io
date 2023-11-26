---
layout: post
title: >
  호출 스택 (Call Stack)
tags: [Java]
---

# 호출 스택 (Call Stack)

---

## 1. 호출 스택
호출 스택은 프로그램이 메서드를 호출할 때 생성되는 메모리 구조로 메서드의 실행 정보를 저장하는 스레드 단위의 저장 공간이다. 
메서드의 실행 정보에는 메서드가 작업을 수행하는 동안의 지역 변수들과 연산의 중간 결과 등이 포함되며, 작업을 마쳤을때 할당된 메모리 공간은 반환되어 비어진다.

---

## 2. 호출 스택의 동작
호출 스택은 후입선출(LIFO, Last In First Out) 구조를 가진다. 가장 최근에 호출된 메소드가 가장 먼저 실행을 마치고 스택에서 제거된다.

### 1. 코드
아래는 호출 스택을 확인하기 위한 간단한 예제이다.
~~~java
public class CallStack {

    // main()
    public static void main(String[] args) {
        firstMethod();
    }
    
    // firstMethod()
    // main()
    static void firstMethod() {
        secondMethod();
    }
    
    // secondMethod()
    // firstMethod()
    // main()
    static void secondMethod() { 
        System.out.println("print!");
    }
}
~~~

### 2. 동작 확인
- `main()` 메서드가 실행되면 호출 스택에 `main()` 메서드의 프레임이 추가된다. 
- `main()` 메서드는 `firstMethod()` 메서드를 호출하고, 호출 스택에 `firstMethod()` 메서드의 프레임이 추가된다. 
- `secondMethod()` 메서드가 호출되면 호출 스택에 `secondMethod()` 메서드의 프레임이 추가된다.
- `println()` 메서드가 호출되면 호출 스택에 `println()` 메서드의 프레임이 추가된다. 해당 메서드는 표준입출력을 통한 출력을 수행한다.
- `println()` 메서드가 실행을 마치고 해당 프레임이 호출 스택에서 제거된다.
- `secondMethod()` 메서드가 실행을 마치고 해당 프레임이 호출 스택에서 제거된다.
- `firstMethod()` 메서드가 실행을 마치고 해당 프레임이 호출 스택에서 제거된다.
- `main()` 메서드가 실행을 마치고 해당 프레임이 호출 스택에서 제거된다.
- 남아 있는 작업이 없으므로 호출 스택이 메모리에서 할당 해제된다.

![Call Stack](https://drive.google.com/uc?export=view&id=1csiR7fODAzKQt6SEAUEfKqEAjRuHw9bW )

출처 : [[Java] OOP_호출 스택(Call Stack)](https://velog.io/@jeong11/Java-OOP-callstack)

---
#### ▶ Reference
- [[Java] OOP_호출 스택(Call Stack)](https://velog.io/@jeong11/Java-OOP-callstack)
- 남궁성, 『자바의 정석』, 도우출판

---