---
layout: post
title: >
  메서드 디스패치 (Method Dispatch)
tags: [Java]
---

## 1. 메서드 디스패치 (Method Dispatch)
메서드 디스패치는 어떤 메서드를 호출할 것인지 결정하는 과정을 말한다.

자바는 기본적으로 런타임에 인스턴스를 생성하고 컴파일 시에는 생성할 객체 타입에 대한 정보만을 보유하기 때문에 컴파일 타임에 알 수 없는 메서드의 의존성을 런타임에 늦게 바인딩하는 경우가 많다.

정적 디스패치는 컴파일 타임에 호출될 메서드가 결정되며, 동적 디스패치는 런타임에 호출될 메서드가 결정된다.

---
## 2. 정적 디스패치 (Static Method Dispatch)
다형성을 이용하지 않는 코드를 작성할 경우 컴파일러는 컴파일 시점에 호출될 메서드를 명확하게 파악할 수 있다.

```java
public class StaticDispatch {
    public void run() {
        System.out.println("StaticDispatch.run()");
    }
    
    public void run(String msg) {
        System.out.println("StaticDispatch.run(" + msg + ")");
    }
}

public class Dispatch {
    public static void staticDispatch() {
        // 컴파일이 되는 시점에 컴파일러가 어느 메서드를 실행할 것인지 정확히 파악할 수 있다.
        new StaticDispatch().run();
        new StaticDispatch().run("dispatch");
    }
}
```

`Dispatch.staticDispatch()` 메서드를 역어셈블을 통해 확인해보면 아래와 같다.

```bash
  public static void staticDispatch();
    Code:
       0: new           #2                  // class StaticDispatch
       3: dup
       4: invokespecial #3                  // Method StaticDispatch."<init>":()V
       7: invokevirtual #4                  // Method StaticDispatch.run:()V
      10: new           #2                  // class StaticDispatch
      13: dup
      14: invokespecial #3                  // Method StaticDispatch."<init>":()V
      17: ldc           #5                  // String msg
      19: invokevirtual #6                  // Method StaticDispatch.run:(Ljava/lang/String;)V
      22: return
```

---
## 3. 동적 디스패치 (Dynamic Dispatch)
다형성을 이용하여 코드를 보다 유연하게 작성할 경우 컴파일러는 컴파일 시점에 어떤 메서드를 호출해야 할지 정확히 파악할 수 없다.
이러한 경우, 런타임에 객체의 실제 타입에 따라 호출될 메서드가 동적으로 선택되며, 이를 동적 디스패치라고 한다.

가장 대표적인 예시로 메서드 오버라이딩이 있으며, 동적 디스패치는 주로 상속과 인터페이스를 통해 구현된다.

```java
public abstract class DynamicDispatch {
    abstract void run();
}

public class DynamicDispatch1 extends DynamicDispatch {
    @Override
    void run() {
        System.out.println("DynamicDispatch1.run()");
    }
}

public class DynamicDispatch2 extends DynamicDispatch {
    @Override
    void run() {
        System.out.println("DynamicDispatch2.run()");
    }
}

public class Dispatch {
    public static void dynamicDispatch() {
        DynamicDispatch d1 = new DynamicDispatch1();
        DynamicDispatch d2 = new DynamicDispatch2();
        d1.run();
        d2.run();
    }
}
```

`Dispatch.dynamicDispatch()` 메서드를 역어셈블을 통해 확인해보면 아래와 같다.

```bash
  public static void dynamicDispatch();
    Code:
       0: new           #7                  // class DynamicDispatch1
       3: dup
       4: invokespecial #8                  // Method DynamicDispatch1."<init>":()V
       7: astore_0
       8: new           #9                  // class DynamicDispatch2
      11: dup
      12: invokespecial #10                 // Method DynamicDispatch2."<init>":()V
      15: astore_1
      16: aload_0
      17: invokevirtual #11                 // Method DynamicDispatch.run:()V
      20: aload_1
      21: invokevirtual #11                 // Method DynamicDispatch.run:()V
      24: return
```

---
## 4. 바이트 코드 확인
바이트코드를 확인하기 위해서는 일단 컴파일을 수행해야 한다.

```bash
$ javac -encoding utf-8 Dispatch.java
```

컴파일을 수행하면 아래의 클래스 코드를 얻을 수 있다.

![Compile Dispatch Class](https://drive.google.com/uc?export=view&id=1i3gveMSmt8X-0dE-1sjS6yZQunHGUPaG)

이후, 컴파일한 코드를 역어셈블러로 확인하면 다음과 같은 결과를 얻을 수 있다.

```bash
$ javap -c Dispatch

Compiled from "Dispatch.java"
public class Dispatch {
  public Dispatch();
    Code:
       0: aload_0
       1: invokespecial #1                  // Method java/lang/Object."<init>":()V
       4: return

  public static void staticDispatch();
    Code:
       0: new           #2                  // class StaticDispatch
       3: dup
       4: invokespecial #3                  // Method StaticDispatch."<init>":()V
       7: invokevirtual #4                  // Method StaticDispatch.run:()V
      10: new           #2                  // class StaticDispatch
      13: dup
      14: invokespecial #3                  // Method StaticDispatch."<init>":()V
      17: ldc           #5                  // String msg
      19: invokevirtual #6                  // Method StaticDispatch.run:(Ljava/lang/String;)V
      22: return

  public static void dynamicDispatch();
    Code:
       0: new           #7                  // class DynamicDispatch1
       3: dup
       4: invokespecial #8                  // Method DynamicDispatch1."<init>":()V
       7: astore_0
       8: new           #9                  // class DynamicDispatch2
      11: dup
      12: invokespecial #10                 // Method DynamicDispatch2."<init>":()V
      15: astore_1
      16: aload_0
      17: invokevirtual #11                 // Method DynamicDispatch.run:()V
      20: aload_1
      21: invokevirtual #11                 // Method DynamicDispatch.run:()V
      24: return

  public static void main(java.lang.String[]);
    Code:
       0: invokestatic  #12                 // Method staticDispatch:()V
       3: invokestatic  #13                 // Method dynamicDispatch:()V
       6: return
}
```

자바 11 기준으로 바이트코드 레벨에서 정적 디스패치와 동적 디스패치는 동일한 명령어를 사용하므로 바이트코드만으로는 명시적인 차이를 확인할 수 없다.

하지만 정적 디스패치는 컴파일 시간에 메서드 호출 대상을 결정한다는 말은 틀린 말이 아니다. 
이를 확인하기 위해서는 변수, 메서드 등에 대한 정보가 저장되어 있는 상수 풀 정보와 함께 확인할 필요가 있다.

```bash
$ javap -v Dispatch

Classfile /C:/Users/sangu/IdeaProjects/java-grammar/src/main/java/Dispatch.class
  Last modified 2023/12/11; size 642 bytes
  MD5 checksum 47ca1f63fcd4b3ce4fad7121645b4dde
  Compiled from "Dispatch.java"
public class Dispatch
  minor version: 0
  major version: 55
  flags: (0x0021) ACC_PUBLIC, ACC_SUPER
  this_class: #14                         // Dispatch
  super_class: #15                        // java/lang/Object
  interfaces: 0, fields: 0, methods: 4, attributes: 1
Constant pool:
   #1 = Methodref          #15.#26        // java/lang/Object."<init>":()V
   #2 = Class              #27            // StaticDispatch
   #3 = Methodref          #2.#26         // StaticDispatch."<init>":()V
   #4 = Methodref          #2.#28         // StaticDispatch.run:()V
   #5 = String             #29            // msg
   #6 = Methodref          #2.#30         // StaticDispatch.run:(Ljava/lang/String;)V
   #7 = Class              #31            // DynamicDispatch1
   #8 = Methodref          #7.#26         // DynamicDispatch1."<init>":()V
   #9 = Class              #32            // DynamicDispatch2
  #10 = Methodref          #9.#26         // DynamicDispatch2."<init>":()V
  #11 = Methodref          #33.#28        // DynamicDispatch.run:()V
  #12 = Methodref          #14.#34        // Dispatch.staticDispatch:()V
  #13 = Methodref          #14.#35        // Dispatch.dynamicDispatch:()V
  #14 = Class              #36            // Dispatch
  #15 = Class              #37            // java/lang/Object
  #16 = Utf8               <init>
  #17 = Utf8               ()V
  #18 = Utf8               Code
  #19 = Utf8               LineNumberTable
  #20 = Utf8               staticDispatch
  #21 = Utf8               dynamicDispatch
  #22 = Utf8               main
  #23 = Utf8               ([Ljava/lang/String;)V
  #24 = Utf8               SourceFile
  #25 = Utf8               Dispatch.java
  #26 = NameAndType        #16:#17        // "<init>":()V
  #27 = Utf8               StaticDispatch
  #28 = NameAndType        #38:#17        // run:()V
  #29 = Utf8               msg
  #30 = NameAndType        #38:#39        // run:(Ljava/lang/String;)V
  #31 = Utf8               DynamicDispatch1
  #32 = Utf8               DynamicDispatch2
  #33 = Class              #40            // DynamicDispatch
  #34 = NameAndType        #20:#17        // staticDispatch:()V
  #35 = NameAndType        #21:#17        // dynamicDispatch:()V
  #36 = Utf8               Dispatch
  #37 = Utf8               java/lang/Object
  #38 = Utf8               run
  #39 = Utf8               (Ljava/lang/String;)V
  #40 = Utf8               DynamicDispatch

```

`staticDispatch()` 메서드에서 `StaticDispatch.run()` 메서드 호출에 사용되는 `invokevirtual` 명령어를 확인해 보자. 
해당 메서드들은 인덱스 `#4`와 `#6`을 가리키고 있으며, 이 값을 상수 풀에서 확인하면 호출할 메서드가 이미 결정되어 있는 것을 확인할 수 있다.

반면에 동적 디스패치 메서드인 `dynamicDispatch()` 메서드에서 `DynamicDispatch.run()` 메서드 호출에 사용되는 `invokevirtual` 명령어를 확인해 보자.
해당 메서드들은 둘 다 인덱스 `#11`을 가리키고 있으며, 이 값을 상수 풀에서 확인하면 실제 호출되는 메서드인 `DynamicDispatch1.run()`와 `DynamicDispatch2.run()`를 확인할 수 없는 것을 알 수 있다.
이 때 실행되는 메서드는 JVM은 객체의 실제 타입을 확인하고 해당 타입에서 오버라이딩된 메서드를 찾아 호출한다.

---
## 5. 더블 디스패치 (Double Dispatch)
### 1. 더블 디스패치
동적 디스패치가 두 번 적용되는 기법을 더블 디스패치라고 한다.
다형성을 일차원적으로만 적용하는 때가 많으나 다형성을 이차원적으로 적용해야 할 경우 사용하는 것이 더블 디스패치 기법이다.

이 더블 디스패치 기법을 사용해 구현하는 디자인 패턴이 방문자 패턴이다. 방문자 패턴을 사용해야 하는 이유를 살펴보자.

---
## 6. 방문자 패턴 (Visitor Pattern)
의존코드는 어떤 객체가 왔는지 신경쓰지 않고 해당 구현체가 알아서 처리하도록 위임하는 패턴을 말한다. 이 패턴을 관통하는 철학이 바로 더블 디스패치이다.

해당 예제는 방문자 패턴을 통해 복수의 SNS에 텍스트 형태와 사진 형태의 포스트가 가능하도록 기능을 구현하는 것을 목적으로 한다.

### 1. Instanceof를 활용한 SNS, POST 구현
SNS를 공통화하는 인터페이스와 이를 구현하는 세부 클래스, 포스트를 공통화하는 인터페이스와 인터페이스를 구현하는 세부 클래스들을 작성했다.

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

// 사진 포스트 구현 클래스
class Picture implements Post {
    @Override
    public void postOn(SNS sns) {
        System.out.println("picture -> " + sns.getClass().getSimpleName());
    }
}

// 텍스트 포스트 구현 클래스
class Text implements Post {
    @Override
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

`Text` 클래스와 같은 코드는 많은 문제점을 안고 있다.

- 새로운 SNS 타입이 추가될 때마다 조건문을 추가해 주어야 한다.
- `Picture` 클래스와 `Text` 클래스에서 모두 추가적인 조건문이 필요하다. 그런데 SNS의 누락이 있더라도 컴파일이 된다!
- `instanceof` 키워드는 안티패턴으로 자주 언급되므로 왠만하면 사용하지 않는 것이 좋다.

---
### 2. Overloading을 활용한 SNS, POST 구현

```java
// SNS 인터페이스
interface SNS { }

// SNS 구현 클래스
class FaceBook implements SNS {}
class Twitter implements SNS {}
class Line implements SNS {}

// 포스트 인터페이스
interface Post {
    void postOn(FaceBook facebook);
    void postOn(Twitter twitter);
}

// 텍스트 포스트 구현 클래스
class Text implements Post {
    public void postOn(FaceBook facebook) {
        System.out.println("FACEBOOK UPLOADING");
        System.out.println("TEXT by facebook");
    }
    public void postOn(Twitter twitter) {
        System.out.println("TWITTER UPLOADING");
        System.out.println("TEXT by twitter");
    }
}


class Main {
    public void dispatch() {
        List<Post> posts = Arrays.asList(new Text(), new Picture());
        List<SNS> sns = Arrays.asList(new FaceBook(), new Twitter(), new Line());

        // postOn 메서드에 인자를 받는 부분에서 컴파일 에러가 발생한다.
        // Cannot resolve method 'postOn(project.study.grammar.generics.dispatch.DoubleDispatch2.SNS)'
        // posts.forEach(p -> sns.forEach(s -> p.postOn(s)));
    }
}
```

오버로딩을 사용해 다형성울 구현하도록 수정했다. 하지만, 이 또한 문제가 있는 코드이다.

#### 1. 개방 폐쇄 원칙 (Open Closed Principle)
이전 예제와 같이 수십개의 사용처가 있다면 그 수십 곳의 소스를 모두 수정해야 할 것이다.
이로 인해 객체의 확장이 어려워지며, 객체의 확장에는 열려있고, 변화에는 닫혀있도록 해야한다는 개방 폐쇄 원칙을 위배하게 된다.

#### 2. 공통화된 처리 불가
해당 설계를 통해 생성한 인스턴스를 공통 인터페이스로 묶어서 작업할 때 아래와 같이 에러가 발생한다.

```text
Cannot resolve method 'postOn(dispatch.DoubleDispatch2.SNS)'  
```

그 이유는, 메서드 오버로딩은 정적 메서드 디스패치를 수행하기 때문이다. 
오버로딩된 메서드는 컴파일 시점에서 정확히 타입 체크를 하고 어떤 메서드를 실행할지 알아야 하는데, 매개변수로 FaceBook이나 Twitter같은 특정 타입이 아니라 SNS 객체를 넘겨주고 있기 때문에 어떤 메서드를 실행할지 결정할 수 없는 것이다.

---
### 3. 방문자 패턴을 활용한 구현
`Post` 인터페이스를 상속받고 있는 `postOn()` 메서드는 그 기능의 구현을 구현체에게로 위임하고 있다.
이로 인해 새로운 `SNS` 구현체를 추가하더라도 기존의 의존코드에 직접적인 영향을 주지 않는다.
즉 수정에는 닫혀있고 확장에는 열려있는 개방 폐쇄 원칙을 지키고 있는 것이다.

```java
interface SNS {
    void post (Text text);
    void post (Picture picture);
}

class FaceBook implements SNS {
    @Override
    public void post(Text text) {
        System.out.println("text -> facebook" );
    }

    @Override
    public void post(Picture picture) {
        System.out.println("picture -> facebook" );

    }
}

class Twitter implements SNS {
    @Override
    public void post(Text text) {
        System.out.println("text -> Twitter" );
    }

    @Override
    public void post(Picture picture) {
        System.out.println("picture -> Twitter" );

    }
}

class Line implements SNS {
    @Override
    public void post(Text text) {
        System.out.println("text -> Line" );
    }

    @Override
    public void post(Picture picture) {
        System.out.println("picture -> Line" );
    }
}

interface Post {
    void postOn(SNS sns);
}

class Text implements Post {
    @Override
    public void postOn(SNS sns) {
        sns.post(this);
    }
}

class Picture implements Post {
    @Override
    public void postOn(SNS sns) {
        sns.post(this);
    }
}

class Main {
    public void dispatch() {
        List<Post> posts = Arrays.asList(new Text(), new Picture());
        List<SNS> sns = Arrays.asList(new FaceBook(), new Twitter(), new Line());
        
        // 해당 코드는 조건을 p.postOn() 메서드를 호출할 때 1번, postOn() 메서드 실행중 sns.post(this)를 호출할 때 1번, 총 두 번의 동적 디스패치가 이루어진다.
        posts.forEach(p -> sns.forEach(s -> p.postOn(s)));
    }
}
```

구현체별로 서로 다른 작업을 수행해야 할 때에는 의존코드가 아닌 구현체에 기능의 구현을 위임하도록 하자. 이를 통해 아래의 이점을 얻을 수 있다.

- 새로운 클래스가 추가되더라도, 의존 코드의 수정 없이 사용이 가능하다.
- 즉, 수정에는 닫혀있고 확장에는 열려있는 개방 폐쇄 원칙을 지킬 수 있다.

---
#### ▶ Reference
- [토비의 봄 TV 1회 - 재사용성과 다이나믹 디스패치, 더블 디스패치](https://www.youtube.com/watch?v=s-tXAHub6vg&t=2732s)
- [개방-폐쇄 원칙 (OCP: Open-Closed Principle)](https://yoongrammer.tistory.com/97)
- [[Java] 더블 디스패치(Double Dispatch)](https://wisdom-and-record.tistory.com/89)
- [instanceof의 사용을 지양하자](https://tecoble.techcourse.co.kr/post/2021-04-26-instanceof/)
- 남궁성, 자바의 정석, 도우출판

---