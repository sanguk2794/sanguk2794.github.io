---
layout: post
title: >
  가비지 컬렉션 (Garbage Collection)
tags: [Java]
---

# 가비지 컬렉션 (Garbage Collection)

---

## 1. C와 자바의 메모리 관리
가비지 컬렉션을 이해하기 위해서는 가비지 컬렉션을 제공하지 않는 언어(대표적으로 C)와 가비지 컬렉션을 제공하는 언어(대표적으로 자바)의 메모리 관리 방법을 이해해야 한다.

C를 통해 애플리케이션을 구현하는 경우, 코드를 통해 OS의 메모리에 직접 제어하기 때문에 `free()` 메서드를 호출하여 할당했던 메모리를 명시적으로 해제해야 한다.
그렇지 않으면 메모리에 로드한 인스턴스가 메모리에서 해제되지 않고 남아 메모리 누수가 발생하게 되고, 이 메모리 누수가 누적되어 메모리가 부족해지면 다른 프로그램에도 영향을 끼칠 수 있다.

반면, 자바는 OS의 메모리 영역에 직접적으로 접근하지 않고 JVM을 통해 간접적으로 접근하며, 이 JVM은 인스턴스가 도달 불가능해진 시점이 되면 자동으로 `Object.finalize()`를 수행하여 메모리를 확보한다.
또, JVM은 OS에 요청한 사이즈만큼의 메모리를 할당받아 실행되고 할당받은 이상의 메모리를 사용하게 되면 에러가 발생하므로 OS 레벨의 메모리 누수가 구조적으로 불가능하다.
즉, 해당 프로그램의 실패가 다른 프로그램에 영향을 주지 않는다.

--- 

## 2. 가비지 컬렉션
가비지 컬렉션이란 더 이상 사용할 수 없게 된 인스턴스들을 가비지 컬렉터가 자동으로 메모리에서 해제해주는 작업을 말한다.
메모리 관리라는 까다로운 부분을 JVM의 가비지 컬렉터가 대신 수행해주는 것이다.

#### 1. 가비지 컬렉션의 대상
가비지 컬렉터는 가비지 컬렉션에 해당 규칙을 적용한다.

> 힙 영역의 인스턴스 중 호출 스택에서 도달 불가능한 (Unreachable) 인스턴스들은 가비지 컬렉션의 대상이 된다.

이를 쉽게 말하면 해당 인스턴스를 가리키는 참조변수가 없는 인스턴스들을 가비지 컬렉션의 대상으로 지정한다는 의미이다.

#### 2. 가비지 컬렉션의 동작 방식
자바에서의 가비지 컬렉션은 힙 영역의 영 영역과 올드 영역에 따라 세부 동작이 다르나, 다음의 2가지 공통적인 단계를 따르게 된다.
~~~
- Stop The World
- Mark and Sweep
~~~

`Stop The World`는 가비지 컬렉션을 실행하기 위해 JVM이 애플리케이션의 실행을 멈추는 작업이다.
가비지 컬렉션을 실행할 때, 가비지 컬렉션을 실행하는 쓰레드를 제외한 모든 쓰레드들의 작업이 중단되고 애플리케이션이 중지된다. 그리고, 가비지 컬렉션이 완료되면 작업이 재개된다.

`Mark and Sweep`은 스택의 모든 변수 또는 도달 가능한 객체를 스캔하면서 각각이 어떤 객체를 참고하고 있는지를 탐색해 힙 영역 내의 도달 가능한 인스턴스를 식별한다.
이 과정을 `Mark`라고 한다. 이후에 `Mark`가 되지 않은 객체들을 메모리에서 할당 해제하는데, 이러한 과정을 `Sweep`라고 한다.

#### 3. Major GC, Minor GC
가비지 컬렉션을 수행할 때에는 모든 쓰레드가 동작을 멈추기 때문에 성능에 치명적인 영향을 미친다. 이를 개선하기 위해 자바에서는 가비지 컬렉션을 두 종류로 구분하고 있다.

- Minor GC : 영 영역만의 가비지 컬렉션을 수행한다. 상대적으로 `Stop The World`의 시간이 짧다.
- Major GC : 올드 영역과 영 영역 전체의 가비지 컬렉션을 수행한다. `Stop The World`의 시간이 길다.

가비지 컬렉션의 종류를 2가지로 나눔으로써, `Stop The World` 시간을 구분해 실행할 수 있어졌고, 이는 JVM의 성능 향상에 기여했다.

#### 4. 가비지 컬렉션 예제
~~~java
public class GarbageCollection {
    public static void main(String[] args) {
        String str = "안녕하세요";
        str = "안녕히 계세요";
    }
}
~~~

- "안녕하세요" 인스턴스를 생성하고, 해당 인스턴스를 가리키는 `str` 참조변수를 생성한다. 호출 스택에 저장되는 `str` 참조변수는 힙 영역의 "안녕하세요" 인스턴스를 가리킨다.

![Garbage Collection_1](https://drive.google.com/uc?export=view&id=15OP50EH2oWPZiiPn6T6XcotzQb26BeZ8 )

- "안녕히 계세요" 인스턴스를 생성하고, "안녕하세요" 인스턴스를 가리키던 `str` 참조변수의 값을 "안녕히 계세요" 인스턴스를 가리키도록 값을 변경한다.
이로 인해 "안녕하세요" 인스턴스는 콜 스택에서 더이상 도달이 불가능한 상태가 되었고, 가비지 컬렉터의 대상이 된다.

![Garbage Collection_2](https://drive.google.com/uc?export=view&id=1FL4wvXmpLajx3vIXpyOyGdf3bWL2qsfa )

- 가비지 컬렉션이 실행되고, "안녕하세요" 인스턴스는 메모리에서 해제된다.

![Garbage Collection_3](https://drive.google.com/uc?export=view&id=1uyZR8e6AhE2g8oZBwIzo6eHlvWP0Vu-m )

--- 

## 3. 가비지 컬렉션 실행
~~~java
public class GarbageCollection {
    public static void main(String[] args) {
        System.gc();
    }
}
~~~

`System.gc()`를 호출하여 명시적으로 가비지 컬렉션을 실행하도록 코드를 작성할 수 있지만, 모든 스레드가 중단되기 때문에 직접 호출하지 않는 것이 좋다.

--- 

## 4. Object.finalize()
~~~java
public class Object {
    @Deprecated(since="9")
    protected void finalize() throws Throwable { }
}
~~~ 

메모리 누수를 방지하기 위해 가비지 컬렉션이 수행될 때 더 이상 사용하지 않는 자원에 대한 정리 작업을 진행하기 위해 호출되는 종료자 메서드이다.
하지만, 종료자인 `finalizer()`는 예측할 수 없고, 상황에 따라 위험할 수 있어 일반적으로 불필요하며, 오동작, 낮은 성능, 이식성 문제의 원인이 되기도 하기 때문에 사용하지 않는 것이 권장된다. 실제로 자바 9에서 deprecated API로 지정되었다.

자바 9에서는 `Cleaner` 클래스를 그 대안으로 소개한다.

#### 1. java.lang.ref.Cleaner
자바 9에서 도입된 소멸자로 생성된 `Cleaner`가 더 이상 사용되지 않을 때 등록된 스레드에서 정의된 클린 작업을 수행한다.
~~~java
package java.lang.ref;

public final class Cleaner {
    // ...
}
~~~ 

명시적으로 `clean()`을 수행할 수도 있다. 보통 `AutoCloseable`을 구현해서 `try-with-resource`와 함께 사용한다.
~~~java
public class CleaningExample implements AutoCloseable {
    private final Resource resource;
    private final Cleaner cleaner;

    // Constructor
    public ResourceHandler() {
        this.resource = new Resource();
        this.cleaner = Cleaner.create(this, new CleanupTask(resource));
    }
    
    // AutoCloseable.close()
    @Override
    public void close() {
        cleaner.clean();
    }

    private static class Resource {
        void cleanup() {
            System.out.println("Cleaning up the resource");
        }
    }

    private static class CleanupTask implements Runnable {
        private final Resource resource;

        CleanupTask(Resource resource) {
            this.resource = resource;
        }

        @Override
        public void run() {
            resource.cleanup();
        }
    }
}
~~~ 

하지만, `Cleaner` 또한 `finalizer()`보다는 덜 위험하지만, 여전히 예측할 수 없고, 느리고, 일반적으로 불필요하다.

#### 2. 유효한 객체 할당 해제 방법
메모리 관리가 필요할 경우 객체를 사용하고 나서 쓸모가 없어졌을 때 `null`을 할당하도록 하자. 그러면 유휴시간에 가비지 컬렉터가 자동으로 메모리부터 할당을 해제해 줄 것이다.

---
#### ▶ Reference
- Joshua Bloch, 이복연 옮김,『EFFECTIVE JAVA 3/E』, 프로그래밍인사이트
- [finalize() Method in Java](https://www.scaler.com/topics/finalize-method-in-java/)
- [[Java] Garbage Collection(가비지 컬렉션)의 개념 및 동작 원리 (1/2)](https://mangkyu.tistory.com/118)
- [1. 가비지 컬렉션 - 기본 개념, 동작 원리, Java 8 버전 부터 달라진 점](https://velog.io/@bambi/1.-가비지-컬렉션-개념과-MinorGC-기본-동작-원리)
- [자바 메모리 관리 - 가비지 컬렉션](https://yaboong.github.io/java/2018/06/09/java-garbage-collection/)

---