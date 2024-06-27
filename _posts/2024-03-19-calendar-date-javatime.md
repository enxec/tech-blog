---
title: "[Java] Calendar와 Date 그리고 java.time 패키지"
description: 
author: Enxec
date: 2024-03-19
categories: [Language, Java]
tags: [java, 자바, Calendar, Date, java.time 패키지]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/java-logo.png
  lqip: 
  alt: 
---

Date는 날짜와 시간을 다룰 목적으로  JDK1.0부터 제공되어온 클래스이다. JDK1.0.이 제공하는 클래스의 수와 기능은 지금과 비교할 수 없을 정도로 빈약했다. Date클래스 역시 기능이 부족했기 때문에, 서둘러 Calendar라는 새로운 클래스를 그 다음 버전인 JDK1.1부터 제공하기 시작했다.

## Calendar와 GregorianCalendar
---
Calendar는 추상클래스이기 때문에 직접 객체를 생성할 수 없고, 메서드를 통해서 완전히 구현된 클래스의 인스턴스를 얻어야 한다.

```java
Calendar cal = new Calendar(); // 에러!!! 추상클래스는 인스턴스를 생성할 수 없다.

Calendar cal = Calendar.getInstance(); // Calendar 클래스를 구현한 클래스의 인스턴스를 반환
```

Calendar를 상속받아 완전히 구현한 클래스로는 GregorianCalendar와 BuddhistCalendar가 있는데 getIntance()는 시스템의 국가와 지역설정을 확인해서 태국인 경우에는 BuddhistCalendar의 인스턴스를 반환하고, 그 외에는 GregorianCalendar의 인스턴스를 반환한다.

GregorianCalendar는 Calendar를 상속받아 오늘날 전세계 공통으로 사용하고 있는 그레고리력에 맞게 구현한 것으로 태국을 제외한 나머지 국가에서는 GregorianCalendar를 사용하면 된다.
인스턴스를 직접 생성해서 사용하지 않고 이처럼 메서드를 통해서 인스턴스를 반환받게하는 이유는 최소한의 변경으로 프로그램이 동작할 수 있도록 하기 위한 것이다.

```java
class MyApplication {
    public static void main(String args[]) {
        Calendar cal = new GregorianCalendar(); // 경우에 따라 이 부분을 변경해야 한다.
    }
}
```

만일 위와 같이 특정 인스턴스를 생성하도록 프로그램이 작성되어 있다면, 다른 종류의 캘린더를 사용하는 국가에서 실행한다던가, 새로운 캘린더가 추가된다던가 하는 경우, 즉 다른 종류의 인스턴스를 필요로하는 경우에 MyApplication을 변경해야 하는데 비해 아래와 같이 메서드를 통해서 인스턴스를 얻어오도록 하면 MyApplication을 변경하지 않아도 된다.

class Application {
    public static void main(String args[]) {
        Calendar cal = Calendar.getInstance();
    }
}

<br>

## Date와 Calendar간의 변환
---
Calendar가 새로 추가되면서 Date는 대부분의 메서드가 'deprecated' 되었으므로 잘 사용되지 않는다. 그럼에도 불구하고 여전히 Date를 필요로 하는 메서드들이 있기 때문에 Calendar를 Date로 또는 그 반대로 변환할 일이 생긴다. 그럴 때는 다음과 같이 하자.

```java
// Calendar를 Date로 변환
Calendar cal = Calendar.getInstance();
...
Date d = new Date(cal.getTimeInMillis());

// Date를 Calendar로 변환
Date d = new Date();
...
Calendar cal = Calendar.getInstance();
cal.setTime(d);
```

<br>

## java.time패키지
---
Calendar도 Date보다는 훨씬 나았지만 몇 가지 단점들이 있었고, 늦은 감이 있지만 JDK1.8부터 'java.time패키지'로 기존의 단점들을 개선한 새로운 클래스들이 추가되었다.

```java
// LocalDate
LocalDate ld = LocalDate.now();  // 2024-01-14

// LocalTime
LocalTime lt = LocalTime.now();  // 17:05:31.531

// LocalDateTime
LocalDateTime ldt = LocalDateTime.now();  // 2024-01-14T17:07:04.709
```

java.time패키지는 이외에도 여러 편의 객체들을 가지고 있으니 문서를 참고바란다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  