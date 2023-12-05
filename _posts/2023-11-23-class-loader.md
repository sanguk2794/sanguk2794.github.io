---
layout: post
title: >
  클래스 로더 (ClassLoader)
tags: [Java]
---

# 클래스 로더

---

## 1. 클래스 로더 (ClassLoader)
클래스 로더는 JRE의 일부로써 런타임에 컴파일 된 자바의 클래스 파일을 동적으로 로드하는 역할을 담당한다.
자바 클래스들은 시작 시 한번에 로드되지 않고 애플리케이션에서 필요할 때 로드된다.

![ClassLoader](https://drive.google.com/uc?export=view&id=1KejKkdjaIe_4Slo0NqsbOHRmu0cTasL9)

참조 : [[1주차] JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가.](https://catsbi.oopy.io/df0df290-9188-45c1-b056-b8fe032d88ca)

---

## 2. Java 8 이전 구조
자바 클래스 로더는 모듈 시스템이 도입되기 이전인 자바 8 이전 버전과 자바 9 이후 버전의 구조가 다르다. 자바 8 이하 버전에서의 클래스 로더는 다음의 요소들로 구성된다.
- 부트스트랩 클래스 로더
- 확장 클래스 로더
- 애플리케이션 클래스 로더

---

### 1. 부트스트랩 클래스 로더 (Bootstrap ClassLoader)
부트스트랩 클래스 로더는 최상위 클래스 로더이며, 원시 클래스 로더(Primordial classloader)라고도 불린다.

이 클래스 로더는 JVM 시작 시 가장 최초로 실행되어 JVM 실행을 위한 자바 클래스와 `${JAVA_HOME}/jre/lib`에 위치한 자바 런타임 코어 클래스 등을 로드한다.
해당 코어 클래스 목록은 `sun.boot.class.path`라는 자바 내부적으로 사용되는 환경 변수를 통해 정의된다.

#### 1. 코드
```java
public class Main {
    public static void main(String[] args) {
        System.out.println("Boot Class Path: " + System.getProperty("sun.boot.class.path")); // In JDK 11 this returns null 
    }
}
```

#### 2. 결과
이 결과는 JDK 버전 및 환경에 따라 다르다.

```bash
# In JDK 8
Boot Class Path: /Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home/jre/lib/resources.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home/jre/lib/rt.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home/jre/lib/sunrsasign.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home/jre/lib/jsse.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home/jre/lib/jce.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home/jre/lib/charsets.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home/jre/lib/jfr.jar:/Library/Java/JavaVirtualMachines/jdk1.8.0_291.jdk/Contents/Home/jre/classes
```

#### 3. rt.jar
특히, 해당 리스트 안의 `jre/lib/rt.jar`는 자바를 실행하는데 기본이 되는 `java.lang` 패키지, `java.util` 등의 필수 패키지들이 포함된다.

```bash
jar -tf rt.jar
```

![TIOBE Index for November 2023](https://drive.google.com/uc?export=view&id=19ayAbyPbsjgOS_6SFrpYQCNZS9C5QFDW )

#### 4. Java 9 버전 이후
```bash
# In JDK 11
Boot Class Path: null
```

하지만, 같은 코드를 자바 9 이상의 버전에서 실행하면 `null`을 반환할 뿐이다. 그 이유는 자바 플랫폼의 개선 및 보안 강화와 관련이 있다.

환경변수를 통해 부트 클래스 패스에 접근 가능하기에 부트 클래스 패스를 조작할 수 있다. 이는 특정 클래스들을 변경하거나 재정의하는 데 사용될 수 있어 보안과 호환성 문제를 발생시킬 여지가 존재한다.

그렇기에, 이러한 옵션을 제거하고 `--patch-module` 옵션을 도입함으로써 모듈 시스템을 통해 더욱 안전하고 유연한 클래스 패스 구성이 가능하도록 구조를 변경한 것이다.

> The boot class path has been mostly removed in this release. 
> The java -Xbootclasspath and -Xbootclasspath/p options have been removed. 
> The javac -bootclaspath option can only be used when compiling to JDK 8 or older. 
> The system property sun.boot.class.path has been removed. 
> Deployments that rely on overriding platform classes for testing purposes with -Xbootclasspath/p will need to changed to use the --patch-module option that is documented in JEP 261. 
> The -Xbootclasspath/a option is unchanged.

참조 : [sun.boot.class.path and java.ext.dirs were removed in JDK 9](https://github.com/tobias/clojure-java-9/issues/9)

---

### 2. 확장 클래스 로더 (Extension ClassLoader)
확장 클래스 로더는 부트스트랩 클래스 로더를 조상으로 가지는 클래스 로더로써 확장 자바 클래스들을 로드한다. 
부트스트랩 클래스 로더가 Native C로 구현되어 있는 것과 달리 자바 코드로 구현되어 있다.
`java.ext.dirs` 환경변수에 설정된 디렉토리의 클래스 파일을 로드하고, 이 값이 설정되어 있지 않은 경우 `${JAVA_HOME}/jre/lib/ext`에 있는 클래스 파일을 로드한다.

```java
// sun.misc.Launcher.ExtClassLoader
static class ExtClassLoader extends URLClassLoader {
    // ...
}
```

---

### 3. 애플리케이션 클래스 로더 (Application Classloader)
애플리케이션 클래스 로더는 `-classpath`나 JAR 파일 안에 있는 Manifest 파일의 classpath 속성 값으로 지정된 폴더에 있는 클래스를 로딩한다.
확장 클래스 로더와 마찬가지로 자바로 구현되어 있다. 개발자가 애플리케이션 구동을 위해 직접 작성한 대부분의 클래스는 이 애플리케이션 클래스 로더에 의해 로딩된다.

```java
// sun.misc.Launcher.AppClassLoader
static class AppClassLoader extends URLClassLoader {
    // ...
}
```

---

### 4. 확인
클래스 인스턴스를 통해 호출할 수 있는 `getClassLoader()` 메서드를 통해 해당 클래스를 로드한 클래스 로더 정보를 취득할 수 있다.

#### 1. 코드
```java
public class Main {
    public static void main(String[] args) {
        // 부트스트랩 클래스 로더
        System.out.println("Object's classloader: " + Object.class.getClassLoader());

        // 확장 클래스 로더
        System.out.println("BinaryNode's classloader: " + BinaryNode.class.getClassLoader());

        // 애플리케이션 클래스 로더
        System.out.println("Classloader's classloader: " + Classloader.class.getClassLoader());
    }
}
```

#### 2. 결과
```bash
Object's classloader: null
BinaryNode's classloader: sun.misc.Launcher$ExtClassLoader@58372a00
Classloader's classloader: sun.misc.Launcher$AppClassLoader@73d16e93
```
- `Object` 클래스가 `null`을 반환하는데, 이는 부트스트랩 클래스 로더를 통해 로딩된 클래스임을 나타낸다. 
- 부트스트랩 클래스 로더는 네이티브 코드로 작성되어 있어서 자바 언어 레벨에서 직접 접근할 수 없기 때문에, Java 언어 레벨에서의 직접적인 호출은 불가능하다.
즉, `getClassLoader()` 메서드가 `null`을 반환하는 것은 해당 클래스가 부트스트랩 클래스 로더에 의해 로딩되었음을 나타내는 관례이다.
- `BinaryNode` 클래스는 JAVA8에서 제공하는 확장 기능인 `${JAVA_HOME}\jre\lib\ext\Nashorn.jar` 라이브러리에 포함되어 있으며, 확장 클래스 로더를 통해 로드된다.
- 직접 구현한 클래스인 `Classloader` 클래스는 애플리케이션 클래스 로더를 통해 로드된 것을 확인할 수 있다.

---

## 3. Java 9 이후 구조
자바 9 이상의 버전의 클래스 로더는 다음의 요소들로 구성된다.
- 부트스트랩 클래스 로더
- 플랫폼 클래스 로더
- 시스템 클래스 로더

---

### 1. 부트스트랩 클래스 로더 (Bootstrap ClassLoader)
기존에는 모든 Java SE 클래스들을 로드하는 역할을 담당했으나, `rt.jar`가 모듈화 되어 작은 단위로 나뉘면서 `java.lang.Object`, `java.io.Serializable`, `java.lang.ClassLoader` 등 JVM 동작에 필수적인 클래스들만을 로드하도록 역할이 축소되었다.

```java
// jdk.internal.loader.ClassLoaders
/**
 * The class loader that is used to find resources in modules defined to
 * the boot class loader. It is not used for class loading.
 */
private static class BootClassLoader extends BuiltinClassLoader {
    // ...
}
```

---

### 2. 플랫폼 클래스 로더 (Platform ClassLoader)
기존에는 `URLClassLoader`를 상속하고 있었으나, `BuiltinClassLoader`를 상속하는 내부 클래스로 변경되었다.
더이상 `jre/lib/ext`, `java.ext.dirs`를 지원하지 않지만 `Java SE`의 모든 클래스 및 `Java SE`에는 없지만 JCP(Java Community Process)에 의해 표준화된 모듈 내의 클래스를 로드함으로써, `Java 8`에 비해 로드할 수 있는 범위가 확장되었다.

```java
// jdk.internal.loader.ClassLoaders
/**
 * The platform class loader, a unique type to make it easier to distinguish
 * from the application class loader.
 */
private static class PlatformClassLoader extends BuiltinClassLoader {
    // ...
}
```

---

### 3. 시스템 클래스 로더 (System ClassLoader)
시스템 클래스 로더 또한 기존에는 `URLClassLoader`를 상속하고 있었으나, `BuiltinClassLoader`를 상속하는 내부 클래스로 변경되었다.

`Java SE`나 `JDK` 모듈이 아닌 모듈들을 로딩하는 역할을 수행한다.

```java
// jdk.internal.loader.ClassLoaders
/**
 * The application class loader that is a {@code BuiltinClassLoader} with
 * customizations to be compatible with long standing behavior.
 */
private static class AppClassLoader extends BuiltinClassLoader {
    // ...
}
```

---

### 4. 확인
클래스 인스턴스를 통해 호출할 수 있는 `getClassLoader()` 메서드를 통해 해당 클래스를 로드한 클래스 로더 정보를 취득할 수 있다.

#### 1. 코드
```java
public class Main {
    public static void main(String[] args) {
        // 부트스트랩 클래스 로더
        System.out.println("Object's classloader: " + Object.class.getClassLoader());

        // 플랫폼 클래스 로더
        System.out.println("Scanner's classloader: " + Scanner.class.getClassLoader());

        // 시스템 클래스 로더
        System.out.println("Classloader's classloader: " + Classloader.class.getClassLoader());
    }
}
```

#### 2. 결과
```bash
Object's classloader: null
Scanner's classloader: null
Classloader's classloader: jdk.internal.loader.ClassLoaders$AppClassLoader@14514713
```

- 부트스트랩 클래스 로더에 의해 로드되는 `Object`뿐만 아니라 플랫폼 클래스 로더에 의해 로드되는 `Scanner` 클래스 또한 `null`을 반환하는 것을 확인할 수 있었다.
- 직접 구현한 `Classloader` 클래스는 시스템 클래스 로더를 통해 로드된 것을 확인할 수 있다.

---

## 4. 클래스 로더의 동작 방식
계층 구조를 바탕으로 클래스 로더끼리 로드를 위임하는 구조로 동작한다. 클래스를 로드할 때 먼저 상위 클래스 로더를 확인하여 상위 클래스 로더에 있다면 해당 클래스를 사용하고 없다면 로드를 요청받은 클래스로더가 클래스를 로드한다.

- JVM의 메서드 영역에 클래스가 로드되어 있는지 확인한다. 만일 로드되어 있는 경우 해당 클래스를 사용한다.
- 메서드 영역에 클래스가 로드되어 있지 않을 경우, 시스템 클래스 로더에 클래스 로드를 요청한다.
- 시스템 클래스 로더는 확장 클래스 로더에 요청을 위임한다.
- 확장 클래스 로더는 부트스트랩 클래스 로더에 요청을 위임한다.
- 부트스트랩 클래스 로더는 부트스트랩 클래스패스에 해당 클래스가 있는지 확인한다. 클래스가 존재하지 않는 경우 확장 클래스 로더에게 요청을 넘긴다.
- 확장 클래스 로더는 확장 클래스패스에 해당 클래스가 있는지 확인한다. 클래스가 존재하지 않을 경우 애플리케이션 클래스 로더에게 요청을 넘긴다.
- 애플리케이션 클래스 로더는 시스템 클래스패스에 해당 클래스가 있는지 확인한다. 클래스가 존재하지 않는 경우 `ClassNotFoundException`을 발생시킨다.

---
#### ▶ Reference
- [[1주차] JVM은 무엇이며 자바 코드는 어떻게 실행하는 것인가.](https://catsbi.oopy.io/df0df290-9188-45c1-b056-b8fe032d88ca)
- [자바 클래스패스(classpath)란?](https://effectivesquid.tistory.com/entry/자바-클래스패스classpath란)
- [자바의 클래스 로더 알아보기](https://leeyh0216.github.io/posts/java_class_loader/)
- [[JAVA] Java의 클래스 로딩](https://co-no.tistory.com/103)
- [[Java] JVM의 클래스 로더란?](https://steady-coding.tistory.com/593)
- [Why do we use rt.jar in a java project?](https://stackoverflow.com/questions/3091040/why-do-we-use-rt-jar-in-a-java-project)
- [Stock JDK classes and the "null" ClassLoader?](https://stackoverflow.com/questions/8450624/stock-jdk-classes-and-the-null-classloader)
- [sun.boot.class.path and java.ext.dirs were removed in JDK 9](https://github.com/tobias/clojure-java-9/issues/9)

---