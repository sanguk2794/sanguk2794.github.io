---
layout: post
title: >
  클래스 패스 (Classpath)
tags: [Java]
---

## 1. 클래스패스 (Classpath)
클래스패스는 JVM이 프로그램을 실행할 때 클래스 파일들을 찾기 위한 경로를 지정하는데 사용되는 개념이다.
클래스패스는 JVM이나 컴파일러에게 클래스 파일이나 라이브러리 파일의 위치를 알려주는 역할을 한다.

---
## 2. 클래스패스의 구성
클래스패스는 세미콜론 또는 콜론으로 구분된 디렉토리 및 파일들의 목록으로 표현되며, 
윈도우 계열의 운영체제에서는 구분자로 세미콜론을, 리눅스나 macOS 등의 유닉스 계열의 운영체제에서는 구분자로 콜론을 사용한다.

다음 세 가지 유형의 파일을 인수로 지정할 수 있다.
- 디렉토리
- zip 파일
- jar(Java Archive) 파일

--- 
## 3. 클래스패스 지정 방법
클래스패스를 지정하기 위해서는 `CLASSPATH` 환경변수를 추가하거나, 프로그램 실행시 인수로 `-classpath` 옵션을 추가해야 한다.

### 1. CLASSPATH 환경변수 설정
`CLASSPATH` 환경변수를 설정하여 클래스패스를 지정할 수 있다.
하지만, 이는 모든 프로젝트에 전역적으로 설정되므로 유연성이 부족하고, 다른 애플리케이션 간 충돌 가능성이 있어 권장되지 않는다.  

![Set CLASSPATH](https://drive.google.com/uc?export=view&id=18hrH8uO68EP76jkGxobyKgCZ8tTlSdxQ)

### 2. -classpath 옵션 설정
`java`, `javac` 명령어를 사용할 때 옵션을 사용하여 클래스패스를 직접 명시할 수 있다.
주의할 점이 있는데, 자바의 경로상에 공백이 존재한다면 해당 경로를 ""로 묶어줘야 한다는 것이다.

```bash
java -classpath /path/to/classes MyClass
java -cp /path/to/classes MyClass
```

--- 
## 4. 클래스패스 우선순위
클래스패스는 여러 경로로 구성될 수 있으며, 그렇기에 클래스패스에 포함된 순서는 중요하다. 
JVM은 클래스를 찾을 때 클래스패스의 앞쪽부터 차례로 검색하므로, 앞쪽에 위치한 클래스 또는 라이브러리가 먼저 사용된다.

---
#### ▶ Reference
- [[JAVA] Java 환경변수(path) 설정하기](https://blog.opid.kr/62)
- [자바 클래스패스(classpath)란?](https://effectivesquid.tistory.com/entry/자바-클래스패스classpath란)
- [Java 클래스 경로](https://www.ibm.com/docs/ko/i/7.3?topic=usage-java-classpath)

---