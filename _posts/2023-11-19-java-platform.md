---
layout: post
title: >
  자바 플랫폼
tags: [Java]
---

# 자바 플랫폼
자바 플랫폼은 자바 언어로 작성된 애플리케이션을 개발하고 실행하기 위한 환경을 제공하는 소프트웨어 플랫폼이다. 자바 플랫폼은 가상 머신과 API로 구성된다.

### 1. 가상 머신 (Virtual Machine)
자바 가상 머신(JVM)은 자바 프로그램을 실행하기 위한 가상 컴퓨터이다. 자바는 javac 컴파일러를 통해 소스코드를 바이트코드로 변환하며, JVM은 소스코드의 컴파일 결과인 바이트 코드를 해당 운영 체제의 기계어로 변환해 실행한다.

### 2. API (Application Programming Interface)
자바 API는 자바 플랫폼에서 제공하는 라이브러리와 클래스들의 모음이다. 이 API를 통해 개발자는 자주 사용되는 기능을 구현할 때 필요한 여러 가지 도구와 클래스들을 활용할 수 있다. 
자바 API는 풍부한 기능을 제공하여 생산성을 높이고 코드의 재사용성을 촉진한다.

### 3. 자바 플랫폼의 종류
자바 플랫폼은 크게 세 가지 주요 에디션으로 나뉘어진다.

#### 1. Java SE (Standard Edition)
일반적인 데스크톱 및 서버 애플리케이션을 개발하기 위한 표준 에디션이다. 
이는 자바의 기본 플랫폼으로서, 자바 언어의 핵심 기능을 제공하며, Java Development Kit(JDK)과 Java Runtime Environment(JRE)를 포함한다.

#### 2. Java EE (Enterprise Edition)
기업 환경에서 대규모 및 분산형 애플리케이션을 개발하기 위한 플랫폼으로, 자바 SE를 기반으로 기업 애플리케이션 개발에 필요한 다양한 기술과 확장을 추가로 제공한다.
Java SE에 부가하여, 웹 애플리케이션 프로그래밍에서 사용하는 JSP, Servlet, JDBC, 장애 복구 등 기능이 추가적으로 제공된다.

#### 3. Java ME (Micro Edition)
자원이 제한된 환경에서 동작하는 모바일 기기 및 임베디드 시스템용으로 개발된 플랫폼이다. 스마트폰을 주로 사용하게 되면서 요즘에는 잘 사용되지 않는다.
CLDC(Connected Limited Device Configuration), MIDP(Mobile Information Device Profile) 등 모바일 및 임베디드 환경을 위한 구성 요소를 제공한다.

---
#### ▶ Reference
- [알아두면 쓸데없는 자바 플랫폼 종류](https://velog.io/@whitebear/알아두면-쓸데없는-자바-플랫폼)
- [[java] Java SE, Java EE, Java ME 차이](https://blog.naver.com/PostView.nhn?blogId=rorean&logNo=221636124268&categoryNo=16&parentCategoryNo=0&viewDate=&currentPage=1&postListTopCurrentPage=1&from=postView)
---