---
layout: post
title: >
  예외 처리 (Exception Handling)
tags: [Java]
---

## 1. 에러 (Error)
프로그램 실행 중 어떤 원인에 의해서 프로그램이 오작동을 하거나 비정상적으로 종료될 수 있다. 이러한 결과를 초래하는 원인을 프로그램 에러 혹은 오류라고 한다.

### 1. 발생 시점에 따른 구분
자바에서는 에러를 발생 시점에 따라 컴파일 에러와 런타임 에러의 두 가지로 구분한다.

이와 별개로, 컴파일 에러나 런타임 에러는 발생하지 않으나 개발자의 의도와 다르게 동작하는 것을 논리적 에러라고 한다.

### 2. 예외 처리 가능 여부에 따른 구분
자바는 실행 시 발생할 수 있는 오류를 에러와 예외로 구분한다. 에러를 대표하는 클래스는 `Error`이며, 예외를 대표하는 클래스는 `Exception`이다.

에러는 메모리 부족이나 스택 오버플로우 같이 한 번 발생하면 프로그램을 복구할 수 없는 심각한 오류이다.
그러나 예외는 발생하더라도 수습할 수 있는 덜 심각한 오류이다. 예외는 발생하더라도 프로그래머가 이에 대한 적절한 코드를 미리 작성해 놓음으로써 프로그램의 비정상적인 종료를 막을 수 있다.

---
## 2. 예외 처리 (Exception Handling)
예외 처리란 프로그램 실행 시 발생할 수 있는 예기치 못한 컴파일, 런타임 예외의 발생에 대비한 코드를 작성하는 것이다.
예외 처리의 목적은 예외의 발생으로 인한 실행 중인 프로그램의 비정상 종료를 막고, 정상적인 실행 상태를 유지할 수 있도록 하는 것이다.

모든 예외 클래스는 `Throwable` 클래스를 상속하며 `Error`와 `Exception`으로 구분된다.

![Error Class Hierarchy](https://drive.google.com/uc?export=view&id=1_x0FxebYSDDDFdXW2x-tRWXXVjRatBua)

출처 : [예외처리](https://velog.io/@bluesky7017/예외처리)

그 중 개발자가 처리할 수 있는 예외는 크게 `RuntimeException`과 그 외의 `Exception`으로 구분할 수 있다.
`RuntimeException`은 예외 처리를 강제하지 않으며, 그 외의 `Exception`은 예외 처리를 강제한다.

![Error Class Hierarchy](https://drive.google.com/uc?export=view&id=1SjNBhBOIsUe--mtgdTJDe59SU0QgsiYl)

출처 : [예외처리](https://velog.io/@bluesky7017/예외처리)

---
## 3. 체크 예외 (Checked exception), 언체크 예외 (Unchecked exception)
`Exception` 클래스의 자손들을 체크 예외, `RuntimeException` 클래스의 자손들을 언체크 예외로 구분하기도 한다.

체크 예외로 구분되는 `Exception` 클래스의 자손들은 예외처리가 강제된다. 
입출력, 네트워크 등 외부 환경과 관계가 깊어 제대로 작성된 코드라도 문제가 발생할 소지가 있기 때문이다.

하지만, 언체크 예외로 구분되는 `RuntimeException`은 예외처리가 강제되지 않는다.
`RuntimeException` 클래스를 상속받는 언체크 예외들은 주로 프로그래머의 실수에 의해 발생하는 예외이기에 예외 처리를 강제하지 않는 것이다.

---
## 4. try-catch
발생한 예외를 처리해 주지 않으면 프로그램은 비정상적으로 종료되며, 처리되지 못한 예외는 JVM의 예외 처리기인 `UncaughtExceptionHandler`에서 예외의 원인을 화면에 출력한다.
프로그램이 멈추지 않도록 예외를 제어하기 위해 `try-catch` 구문을 이용한다. 

```java
try {
    // ...
} catch (Exception e) {
    // ...
}
```

해당 예제는 0부터 1사이의 값을 랜덤으로 호출하여 0.5 이상일 경우 `RuntimeException`을 발생시키며, 
예외가 발생했을 경우 그 당시의 스택 트레이스를 출력한 후 메서드를 종료한다.

```java
public class Main {
    public void tryCatch() {
        try {
            //  0부터 1사이의 값을 랜덤으로 호출하여 0.5 이상일 경우 RuntimeException을 던진다.
            if (Math.random() > 0.5) {
                throw new RuntimeException("so sad ㅠㅠ");
            }

        } catch (RuntimeException e) {
            e.printStackTrace();
        }
    }
}
```

---
### 1. try-catch의 흐름
`try-catch`는 예외가 발생한 경우와 발생하지 않았을 경우의 흐름이 달라진다.

`try` 블럭 내부에서 예외가 발생한 경우에는 발생한 예외와 일치하는 `catch` 블럭이 있는지 확인한다.
만약 일치하는 `catch` 블럭을 찾게 되면 해당 `catch` 블럭 내의 문장들을 수행하고 `try-catch` 구문을 빠져나가서 다음 문장들을 계속해서 수행한다.
만약 일치하는 `catch` 블럭을 찾지 못하면 예외는 처리되지 못하고 프로그램은 종료된다.

`try` 블럭 내에서 예외가 발생하지 않은 경우에는 `catch` 블럭을 거치지 않고 `try-catch` 구문을 빠져나가 다음 문장들을 계속해서 수행한다.

`catch` 블럭의 참조변수로 `Exception` 클래스를 선언하면 모든 예외를 처리할 수 있다.
해당 클래스는 모든 예외 계층도의 최상위에 위치하고 있기 때문이다.

---
### 2. 멀티 캐치 (Multi Catch)
자바 7부터 여러 `catch` 블럭을 `|` 기호를 이용해 하나의 `catch` 블럭으로 합칠 수 있게 되어 중복 코드를 줄일 수 있게 되었다.

`|` 기호를 이용해 연결 가능한 예외 클래스의 개수에는 제한이 없으나, 멀티 캐치 블럭의 예외 클래스들이 자손 관계라면 컴파일 에러가 발생한다는 제약이 존재한다.
왜냐하면 조상과 자손 클래스를 둘 다 선언하는 것은 조상 클래스 하나만 선언하는 것과 같은 의미를 가지기 때문에 의미가 없기 때문이다.

```java
public class Main {
    public void multiCatchBlock() {
        try {
            throw new IllegalArgumentException("車が来る");

        } catch (ArithmeticException | IllegalArgumentException e) {
            e.printStackTrace();
        }
    }
}
```

---
### 3. finally
`finally`는 예외의 발생 여부에 상관없이 실행되어야 할 코드를 포함시킬 목적으로 사용되며, 실제로 예외의 발생 여부에 상관없이 반드시 실행된다. 

심지어 `try-catch` 블럭을 `try` 블럭 혹은 `catch` 블럭에서 `return` 예약어를 호출하더라도 `finally` 블럭의 문장들이 먼저 실행된 후에 현재 실행 중인 메서드가 종료된다.

```java
public class Main {
    public void finallyBlock() {
        try {
            System.out.println("try");
            return;

        } catch (Exception e) {
            System.out.println("catch");

        } finally {
            System.out.println("finally");
        }

        System.out.println("unreachable code");
    }
}
```

---
## 5. 사용자 정의 예외
예외 클래스를 상속하는 새로운 클래스를 작성하는 것으로 사용자 정의 예외 클래스를 정의할 수 있다.

기존의 예외 클래스는 주로 `Exception`을 상속받아 체크 예외 예외로 작성하는 경우가 많았으나 최근에는 `RuntimeException`을 상속받아 작성하는 경우가 많아지고 있다.
체크 예외인 `Exception`으로 선언할 경우 예외처리가 불필요한 경우에도 예외처리를 강제하므로 코드의 복잡성이 증가하기 때문이다.

```java
// 체크 예외
static class MyException extends Exception {
    private int errorCode = 0;

    MyException() {
        super();
    }

    MyException(String msg) {
        super(msg);
    }

    MyException(String msg, int errorCode) {
        super(msg);
        this.errorCode = errorCode;
    }
}
```

---
### 1. 주의점
사용자 예외를 만들 때 몇 가지 주의해야 할 점이 있다.

- 자바가 제공하는 표준 예외 중, 적당한 예외가 없는 경우에만 작성하는 것이 좋다. 
- 자바의 네이밍규칙을 따르는 것이 좋다 `××Exception.java`
- 커스텀한 예외이므로 만들고 난 후, 어떤 용도인지 설명하는 자바독의 작성이 필요하다. 
- 원인 예외를 담는 생성자를 반드시 같이 선언하는 것이 좋다.

자바의 표준 예외를 사용할 경우 다른 개발자들이 어떤 의도로 예외를 던지는지 쉽게 이해할 수 있다.
하지만, 사용자 예외를 생성해 사용한다면 특별한 목적이 없는 이상 이해하기 힘든 코드가 될 뿐이다.

또, 원인 예외를 포함하는 생성자를 반드시 같이 선언하는 것이 좋은데, 이 예외 생성자를 선언하지 않을 경우 해당 사용자 예외를 발생시킨 원인이 되는 예외의 정보를 유지할 수 없기 때문이다. 
그러므로 반드시 해당 생성자는 선언해주도록 하자.

```java
// 위의 사용자 정의 예외 클래스의 원인 예외를 포함하는 생성자를 추가했다.
static class MyException extends Exception {
    private int errorCode = 0;

    MyException() {
        super();
    }

    MyException(String msg) {
        super(msg);
    }
    
    // 원인 예외를 포함하는 생성자
    MyException(String msg, int errorCode, Exception cause) {
        super(msg, cause);
        this.errorCode = errorCode;
    }
    
    MyException(String msg, int errorCode) {
        super(msg);
        this.errorCode = errorCode;
    }
}
```

---
## 6. 예외 던지기
예약어 `throw`를 사용하면 프로그래머가 고의적으로 예외를 발생시킬 수 있다.

- 연산자 `new`를 이용해 발생시키고자 하는 예외 클래스의 인스턴스를 생성한다.
- 키워드 `throw`를 통해 예외를 발생시킨다.

```java
public class Main {
    public void throwException() {
            // 발생시키고자 하는 예외 클래스의 인스턴스를 생성한 후, 해당 인스턴스를 통해 예외 발생시키고 있다.
            throw new RuntimeException();
    }
}
```

---
## 7. 메서드에 예외 선언
`try-catch`가 아닌 다른 방법으로도 예외를 처리할 수 있다. 예외를 메서드에 선언하는 것이다.

메서드에 예외를 선언하려면 메서드의 선언부에 `throws` 키워드를 사용해서 메서드 내에서 발생할 수 있는 에러를 적어주어야 한다.
이 때, 발생 가능한 예외가 여러개일 경우에는 쉼표로 구분한다.

메서드에 예외를 선언하는 것은 사실 호출한 메서드에게 예외를 전달하여 예외처리를 떠맡기는 것이다.

메서드의 선언부에 예외를 선언함으로써 메서드를 사용하려는 사람이 메서드의 선언부를 보았을 때 이 메서드를 사용하기 위해서 어떤 예외들을 처리해야 하는지 쉽게 파악할 수 있게 해 준다.
또, 이 메서드를 사용하는 경우 예외에 대한 처리를 강요하기 때문에 보다 견고한 프로그램 코드의 작성이 가능하다.

메서드에 예외를 선언할 때 일반적으로 `RuntimeException` 클래스들은 적지 않는다. 반드시 처리되어야 하는 예외가 아니기 때문이다.

```java
public class Main {
    public void throwsException (int n) throws IOException {
        if (n < 1) {
            throw new IllegalArgumentException();
        }
    
        for (int i = 0; i < n; i++) {
            // 입력 처리
            BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
            String str = reader.readLine();
    
            // 출력 처리
            BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(System.out));//선언
            writer.write(str); // 출력
            writer.newLine(); // 줄바꿈
            writer.flush(); // 스트림을 플러쉬함, 스트림을 플러시하면 버퍼링되었을 수 있는 데이터 지우기를 포함하여 해당 스트림에 기록된 모든 데이터가 출력됨
            writer.close(); // 스트림을 닫음
        }
    }
}
```

---
## 8. 예외 되던지기
한 메서드에서 발생할 수 있는 예외가 여러 개인 경우, 몇 개는 메서드 내에서 자체적으로 처리하고 나머지는 선언부에 지정하여 호출한 메서드에서 처리하도록 할 수 있다.
심지어 하나의 예외에 대해서도 예외가 발생한 메서드와 호출한 메서드 양 쪽에서 처리하도록 할 수도 있다.

이것은 예외를 처리한 후 인위적으로 다시 발생시키는 방법을 통해 가능하며 이를 예외 되던지기라고 한다.

```java
public class Main {
    public void exceptionReThrowing() throws Exception {
        try {
            throw new Exception();

        } catch (Exception e) {
            // 예외를 되던진다.
            throw new Exception();
        }
    }
}
```

---
## 9. try-with-resource 구문
`try-catch` 구문의 변형이다. 자동으로 사용한 자원을 반환할 수 있다는 장점이 있다.

`try-with-resource` 구문을 사용하면 `try` 블럭을 벗어나는 순간 자동적으로 `close()`가 호출된다.
대신, `try-with-resource`에 의해 자동으로 `close()`가 호출될 수 있는 객체는 `AutoCloseable` 인터페이스를 구현한 객체들로 제한된다.

```java
public class Main {
    public static void tryWithResource() {
        // try-with-resource 구문 사용
        try (BufferedReader reader = new BufferedReader(new InputStreamReader(System.in));
             BufferedWriter writer = new BufferedWriter(new OutputStreamWriter(System.out))) {

            String str = reader.readLine();
            writer.write(str); // 출력
            writer.newLine(); // 줄바꿈
            writer.flush(); // 플러쉬

        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

---
### 1. try-catch의 문제점
위의 코드를 이전 방식으로 `finally` 블럭에서 자원을 반환하도록 구현하면 아래와 같다.

```java
public class Main {
    public void notTryWithResource() {
        // try-catch 구문 사용
        BufferedReader reader = null;
        BufferedWriter writer = null;

        try {
            reader = new BufferedReader(new InputStreamReader(System.in));
            writer = new BufferedWriter(new OutputStreamWriter(System.out));
            String str = reader.readLine();
            writer.write(str); // 출력
            writer.newLine(); // 줄바꿈
            writer.flush(); // 플러쉬

        } catch (IOException e) {
            e.printStackTrace();

        } finally {
            if (!Objects.isNull(reader)) {
                reader.close();
            }
            if (!Objects.isNull(writer)) {
                writer.close();
            }
        }
    }
}
```

하지만, 해당 코드는 컴파일 에러가 발생한다. `close()` 메서드가 `IOException`을 던질 수 있으나 해당 예외가 처리되지 않기 때문이다.
이를 처리하기 위해 `finally` 구문에서 `try-catch` 구문을 다시 호출해야 한다.

```java
public class Main {
    public void notTryWithResource() {
        BufferedReader reader = null;
        BufferedWriter writer = null;

        try {
            reader = new BufferedReader(new InputStreamReader(System.in));
            writer = new BufferedWriter(new OutputStreamWriter(System.out));
            String str = reader.readLine();
            writer.write(str); // 출력
            writer.newLine(); // 줄바꿈
            writer.flush(); // 스트림을 플러시

        } catch (IOException e) {
            e.printStackTrace();

        } finally {
            try {
                if (!Objects.isNull(reader)) {
                    reader.close();
                }
                if (!Objects.isNull(writer)) {
                    writer.close();
                }

            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

`close()` 메서드의 예외처리를 위해 `try-catch` 처리를 한 번 더 추가했다. 하지만, 이렇게 수정하더라도 여러 문제가 남아 있다. 

- 핵심 로직과 관계없는 예외 처리 코드로 인해 코드의 가독성이 떨어진다. 
- `finally` 블럭에서 예외가 발생할 경우 `try` 블럭의 예외가 무시된다. 
- `reader.close()` 중 에러가 발생할 경우 `writer.close()` 메서드가 호출되지 않으므로 메모리가 누수된다. 이를 처리하기 위해서는 `try-catch`를 또 한 번 사용해야 한다.

이 것이 바로 우리가 `try-with-resource` 구문을 사용해야 하는 이유이다.

---
## 10. 원인 예외 (Chained Exception)
한 예외가 다른 예외를 발생시킬 수도 있다. 이렇게 다른 예외의 원인이 된 예외를 원인 예외라고 한다.
원인 예외의 등록에는 `initCause()` 메서드를 사용한다. 

원인 예외를 등록해 두면 여러가지 예외를 하나의 큰 분류의 예외로 묶어서 사용할 수 있다는 장점이 있다.

```java
public class Main {
    public void install() {
        try {
            startInstall();
            copyFiles();

        } catch (SpaceException | MemoryException e) {
            InstallException installException = new InstallException();
            installException.initCause(e);
            throw installException;
        }
    }
}
```

---
## 11. 스택 트레이스 (Stack Trace)
스택 트레이스는 예외 발생 당시 호출 스택의 메서드 호출 목록이다.
`printStackTrace()` 메서드를 호출하면 스택 트레이스와 함께 예외 메세지를 확인할 수 있다.

```text
java.lang.reflect.InvocationTargetException
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at project.study.common.extend.CommonExtend.run(CommonExtend.java:26)
	at project.study.grammar.exceptionhandling.ThrowException.main(ThrowException.java:95)
Caused by: java.lang.Exception
	at project.study.grammar.exceptionhandling.ThrowException.exceptionReThrowing(ThrowException.java:81)
	... 6 more
```

위는 스택 트레이스의 예시이다. 해당 스택 트레이스의 내용을 분석해보면 다음과 같다.

- 리플렉션을 통해 `method.invoke()`를 실행하는 중에 `InvocationTargetException`이 발생했다.
- `InvocationTargetException`의 원인 예외는 `ThrowException.exceptionReThrowing` 메서드의 81번째 줄에서 발생한 `Exception` 예외이다.

### 1. InvocationTargetException
`InvocationTargetException`은 주로 리플렉션을 통해 호출한 메서드의 내부에서 예외가 발생했을 경우의 래핑 클래스이다.

그러므로, `InvocationTargetException` 발생의 근본 원인은 해당 예외가 아니라 원인 예외이며, `InvocationTargetException`을 해결하기 위해서는 원인 예외를 해결해야 한다.

---
#### ▶ Reference
- [9주차 과제: 예외 처리.](https://yadon079.github.io/2021/java%20study%20halle/week-09)
- [예외처리](https://velog.io/@bluesky7017/예외처리)
- [예외처리(exception handling)](https://catsbi.oopy.io/92cfa202-b357-4d47-8de2-b9b3968dfb2e)
- [Java에서 java.lang.reflect.InvocationTargetException 오류 이해](https://www.delftstack.com/ko/howto/java/java-invocationtargetexception/)
- 남궁성, 자바의 정석, 도우출판
- 
---