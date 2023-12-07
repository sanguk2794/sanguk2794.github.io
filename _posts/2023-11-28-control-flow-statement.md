---
layout: post
title: >
  제어문 (Control Flow Statement)
tags: [Java]
---

# 제어문 (Control Flow Statement)

--- 
## 1. 제어문
프로그램의 흐름을 바꾸는 문장들을 제어문이라고 한다. 제어문에는 조건문과 반복문이 포함된다.
- 조건문 : if, switch
- 반복문 : for, while

---
## 2. 조건문 (Decision-Making Statements)
조건문은 조건식과 문장을 포함하는 블럭으로 구성되며, 조건식의 연산결과에 따라 실행할 문장을 제어할 수 있다. `if`와 `switch`가 조건문에 포함된다.

---
### 1. if
`if`는 조건식과 블럭만으로 이루어지는 조건문이다. 조건식의 결과에 따라서 블럭의 실행 여부가 결정된다.

```text
if (조건식) {
    // 조건식이 참을 반환할 경우 실행
}
```

조건식의 결과값은 `boolean`이다. `true`일 경우 블럭을 실행하며, `false`일 경우 실행하지 않는다.

```java
public class Main {
    public void conditionalStatements() {
        // 조건식
        if (Integer.MAX_VALUE > 150) {
            // 조건식이 true일 경우 실행
            System.out.println("Integer.MAX_VALUE > 150");
        }
    }
}
```

---
### 2. if-else
조건식의 결과값은 `boolean`이다. `true`일 경우 `if` 블럭을 실행하며, `false`일 경우 `else` 블럭을 실행한다.

```text
if (조건식) {
    // 조건식이 true를 반환할 경우 실행
        
} else {
    // 조건식이 false를 반환할 경우 실행
}
```

```java
public class Main {
    public void conditionalStatements() {
        // 조건식
        if (Integer.MAX_VALUE > 150) {
            // 조건식이 true일 경우 실행
            System.out.println("Integer.MAX_VALUE > 150");
            
        } else {
            // 조건식이 true일 경우 실행
            System.out.println("Integer.MAX_VALUE <= 150");
        }
    }
}
```

---
### 3. else if
조건식의 결과가 `false`일 경우 다음 `else if` 블럭의 조건을 확인한다. 
이 블럭 조건식의 확인은 `true`를 반환하는 `if-else` 구문이 나오거나 `else` 구문과 만나거나 조건식에서 벗어날 때까지 계속된다.

```text
if (조건식) {
    // 조건식이 true를 반환할 경우 실행
        
} else if (조건식2) {
    // 조건식2가 true를 반환할 경우 실행
        
} else {
    // 조건식1, 조건식2가 false를 반환할 경우 실행
}
```

모든 `else if` 구문의 결과값이 `false`일 경우 `else` 구문의 블럭을 수행한다. 만약 `else` 구문이 없다면 아무것도 수행하지 않고 다음 로직으로 넘어간다. 

```java
public class Main {
    /**
     * 학점 반환
     * @param score 점수
     * @return 학점
     * */
    private static String getScore(int score) throws IllegalArgumentException {
        if (score > 100 || score < 0) {
            throw new IllegalArgumentException("Score must be between 0 and 100");
        }

        Score c;
        if (score >= Score.A.getScore()) {
            c = Score.A;
            
        } else if (score >= Score.B.getScore()) {
            c = Score.B;
            
        } else if (score >= Score.C.getScore()) {
            c = Score.C;
            
        } else if (score >= Score.D.getScore()) {
            c = Score.D;
            
        } else {
            c = Score.F;
        }

        return c.getGrade();
    }
}
```

---
### 4. 중첩 if
`if` 블록 내부에 또 다른 `if`를 사용하는 것을 말한다. 중첩의 횟수에는 거의 제한이 없다.

```java
public class Main {
    public void conditionalStatements() {
        // 중첩 if
        int value = 150;
        if (Integer.MIN_VALUE > value) { // false
            if (Integer.MAX_VALUE > value) { // true
                System.out.println("between Integer.MIN_VALUE and Integer.MAX_VALUE");
            }
        }
    }
}
```

---
### 5. switch
`switch`는 조건식과 `case`, `default`로 이루어지는 조건문이다. 조건식의 결과에 따라서 실행 블럭이 결정된다.

```text
switch (조건식) {
    case 결과값1:
        // 조건식의 결과값이 결과값1일 경우 실행

    case 결과값2:
        // 조건식의 결과값이 결과값2일 경우 실행

    default :
        // 조건식이 case의 결과값 중 어떤 값에도 포함되지 않을 경우 실행
}
```

`if`의 경우 조건식의 결과과 `true`와 `false`뿐이기 때문에 경우의 수가 추가될 때마다 `else if` 조건이 추가되어 복잡해지고 처리시간이 길어진다.
하지만, `switch`는 단 하나의 조건식으로 많은 경우의 수를 처리할 수 있으므로 보다 빠른 성능을 보여준다.

```java
public class Main {
    /**
     * 자기 소개 반환
     * @param name 이름
     * @param age 나이
     * @return 자기 소개
     * */
    public static String getIntroduction(String name, int age) throws IllegalArgumentException {
        if (age < 0) {
            throw new IllegalArgumentException("age must more than 0");
        }

        StringBuilder result = new StringBuilder();
        result.append("I am ").append(name).append(", ");

        int input = age / 10;
        switch (input) {
            case 0:
                result.append("I am a child.");
                break;
            case 1:
                result.append("I'm a teenager.");
                break;
            default:
                result.append("I am in my ").append(input).append("0s.\n");
                result.append("I'm an adult.");
        }

        return result.toString();
    }
}
```

#### 1. break
`break`는 현재 위치를 감싸는 반복문을 탈출할 때 사용한다.
`break` 키워드를 생략할 경우 다음 `break`를 만나거나 `switch`가 끝날 때까지의 모든 문장을 수행하므로 경우에 따라서는 고의적으로 `break`를 생략하기도 한다.

#### 2. 제약 조건
`switch`는 제약조건이 존재하기 때문에 어쩔 수 없이 `if`로 작성해야 하는 경우도 있다.

- 조건식의 결과가 정수, 문자열, 열거형 중 하나를 반환해야 한다.
- `case` 조건의 결과값은 정수, 문자열, 상수, 열거형만 가능하며 중복되지 않아야 한다.

#### 3. switch의 중첩
`if`와 같이 `switch` 또한 중첩이 가능하다. 다만 `break` 사용시 스코프가 햇갈릴 수 있으므로 주의가 필요하다.

---
## 3. 반복문 (Loop Statements)
반복문은 어떤 작업을 반복적으로 수행하도록 제어할 때 사용하며, `for`, `while`, `while`의 변형인 `do-while`이 포함된다.

`for`과 `while`은 어느 경우에나 서로 변환이 가능하기 때문에 코드를 작성할 때 어느 쪽을 선택해도 좋다.
하지만 `for` 구문은 주로 반복 횟수가 정해져 있을 때, `while` 구문은 반복 횟수가 정해져 있지 않을 때 사용한다는 암묵적인 룰은 존재한다.

--- 
### 1. for
`for`은 초기화, 조건식, 증감식을 기준으로 블럭을 여러 번 반복해서 실행할 때 사용한다.

```text
for (초기식; 조건식; 증감식) {
    // 조건식이 true일경우 반복해서 실행
}
```

`for`는 반복 횟수가 정해져 있을 때 주로 사용된다.

```java
public class Main {
    private void forLoop() {
        for (int i = 0; i < 5; i++) {
            System.out.print("*");
        }
    }
}
```

#### 1. 초기화
반복문에 사용될 변수를 초기화하는 부분이며 처음 한 번만 수행된다. 블럭 내에서 사용해야 할 변수가 여러 개일 경우에는 여러 개의 변수를 선언할 수 있다.

#### 2. 조건식
조건식의 결과가 `true`일 경우 블럭 내의 작업을 반복한다. `false`일 경우 반복을 중단하고 `for` 구문을 벗어난다.
조건식은 생략이 가능한데, 생략했을 때에는 블럭 내의 코드를 무한 반복하게 되므로 특정 조건을 만족하면 `for` 구문을 빠져나올 수 있도록 제어가 필요하다.

#### 3. 증감식
반복문을 제어하는 변수의 값을 증가 또는 감소시킨다.

--- 
### 2. 중첩 for
`for` 안에 또 다른 `for`를 포함시키는 것이 가능하다. 그리고 이 중첩의 횟수는 거의 제한이 없다.

```text
for (초기식; 조건식; 증감식) {
    // 조건식이 true일경우 반복해서 실행

    for (초기식2; 조건식2; 증감식2) {
        // 조건식2가 true일경우 반복해서 실행
    }
}
```

하지만, `for`를 여러 번 중첩해서 사용할 때에는 시간 복잡도와 성능을 고려해야 한다.

```java
public class Main {
    private void forLoop() {
        for (int i = 0; i < 3; i++) {
            // 중첩 for
            for (int j = 0; j < 10; j++) {
                System.out.print("*");
            }
            
            System.out.println();
        }
    }
}
```

--- 
### 3. foreach
JDK 1.5부터 배열과 컬렉션에 저장된 요소에 접근할 때 보다 편리한 방법으로 처리할 수 있도록 `for`의 새로운 용법이 추가되었다. 그것이 바로 `foreach`이다. 

```text
for (E element : 배열 또는 컬렉션) {
    // element 사용 가능
}
```

일반적인 `for`와 달리 배열이나 컬렉션에 저장된 요소들을 읽어오는 용도로만 사용할 수 있다는 제약이 있다.

```java
public class Main {
    private void forLoop() {
        List<String> strings = List.of("리", "스", "트");
        for (String s : strings) {
            System.out.print(s + " ");
        }
    }
}
```

--- 

### 4. while
조건식이 `true`이면 블럭 내의 문장을 실행하고, `false`이면 해당 반복문을 벗어난다. 

```text
while (조건문) {
    // 해당 구현을 반복
}
```

`while`의 조건식은 `for`와 달리 생략이 불가능하다.

```java
public class Main {
    private void whileLoop() {
        int i = 0;
        while (i < 5) {
            System.out.print("*");
            i++;
        }
    }
}
```

---
### 5. do-while
`while`의 변형으로 기본적인 구조는 `while`과 같으나 조건식과 블럭의 순서를 바꿔놓았다.

```text
do {
    // while의 결과값이 false더라도 최소 한 번은 실행될 것이 보장된다.
} while (false);
```

`while`과 구분되는 점은 조건식을 고려하지 않고 최소한 한 번의 반복을 수행한다는 것이다.

```java
public class Main {
    private void doWhileLoop() {
        do {
            System.out.println("바보");
        } while (false);
    }
}
```

---
### 6. break, continue 
반복문의 블럭 내부에서 조건문과 함께 사용된다.
- break : 가장 가까운 반복문을 벗어날 수 있다.
- continue : 반복문의 끝으로 이동하여 해당 반복을 끝내고 다음 번 반복으로 넘어간다.

---
### 7. 이름 붙인 반복문
반복문에 이름을 붙이는 것이 가능하다. 반복문에 붙인 이름을 이용해 지정한 반복문을 벗어날 수 있다.

```java
public class Main {
    public static void namedFor() {
        Loop1 : for(;;) {
            // 일반적인 break 구문을 사용할 경우 Loop2 반복문에서 break
            // 이름 붙인 반복문을 통해 Loop1 반복에서 탈출이 가능
            Loop2 : for (;;) {
                System.out.println("break Loop1");
                break Loop1;
            }
        }
    }
}
```

---
#### ▶ Reference
- 남궁성, 자바의 정석, 도우출판

---