---
title: "[Java] 오버라이딩"
description: 
author: Enxec
date: 2024-02-02
categories: [Language, Java]
tags: [java, 자바, Overriding, 오버라이딩]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/java-logo.png
  lqip: 
  alt: 
---

## 오버라이딩이란?
---
상속받은 메서드를 그대로 사용하기도 하지만, 자손 클래스 자신에 맞게 변경해야하는 경우가 많다. 이럴 때 조상 클래스로부터 상속받은 메서드의 내용을 변경하는 것을 오버라이딩이라고 한다.

<br>

## 오버라이딩 구현 방법
---
2차원 좌표계의 한점을 표현하기 위한 Point클래스가 있을 때, 이를 조상으로 하는 Point3D클래스, 3차원 좌표계의 한 점을 표현하기 위한 클래스를 다음과 같이 새로 작성하였다고하자.

```java
class Point {
    int x;
    int y;
    
    String getLocation() {
    	return "x :" + x + ", y :"+ y;
    }
}

class Point3D extends Point {
    int z;
    
    String getLocation() {
    	return "x :" + x + ", y :"+ y + ", z :" + z;
    }
}
```

이 두 클래스는 서로 상속관계에 있으므로 Point3D클래스는 Point클래스로부터 getLocation()을 상속받지만, Point3D클래스는 3차원 좌표계의 한 점을 표현하기 위한 것이므로 조상인 Point클래스로부터 상속받은 getLocation()은 Point3D클래스에 맞지 않는다. 따라서, 이 메서드를 Point3D클래스 자신에 맞게 z축의 자표값도 포함하여 반환하도록 오버라이딩 한 것이다.

Point클래스를 사용하던 사람들은 새로 작성된 Point3D클래스가 Point클래스의 자손이므로 Point3D클래스의 인스턴스에 대해서 getLocation()을 호출하면 Point클래스의 getLocation()이 그랬듯이 좌표를 문자열로 얻을 수 있을 것이라고 기대할 것이다. 그렇기 때문에 새로운 매서드를 제공하는 것보다 오버라이딩을 하는 것이 바른 선택이다.

<br>

## 오버라이딩의 조건
---
오버라이딩은 메서드의 내용만을 새로 작성하는 것이므로 메서드의 선언부는 조상의 것과 완전히 일치해야한다. 따라서 오버라이딩이 성립하기 성립하기 위해서는 다음과 같은 조건을 만족하여야 한다.

> - 메서드명 동일
> - 매개변수 동일
> - 반환타입 동일
> - 접근 제어자는 조상 클래스의 메서드보다 좁은 범위로 변경 X
> - 조상 클래스의 메서드보다 많은 수의 예외를 선언 X
> - 인스턴스 메서드를 static메서드로 또는 그 반대로 변경 할 수 없다.

__알아두기__ 
JDK1.5부터 "공변 반환타입"이 추가되어, 반환타입을 자손 클래스의 타입으로 변경하는 것이 가능토록 조건이 완화되었다.

한마디로 요약하면 선언부가 서로 일치해야 하고, 접근 제어자와 예외는 제한된 조건 하에서만 다르게 변경할 수 있으며, 멤버 메서드의 static 유무를 컨트롤 할 수 없다. 추가적인 설명은 다음과 같다.

__1) 접근 제어자는 조상 클래스의 메서드보다 좁은 범위로 변경할 수 없다.__

>만일 조상 클래스에 정의된 메서드의 접근 제어자가 protected라면, 이를 오버라이딩하는 자손 클래스의 메서드는 접근 제어자가 protected나 public이어야 한다. 대부분의 경우 같은 범위의 접근 제어자를 사용한다. 접근 제어자의 접근범위를 넓은 것에서 좁은 것 순으로 나열하면 public, protected, (default), private이다.

__2) 조상 클래스의 메서드보다 많은 수의 예외를 선언할 수 없다.__

>아래의 코드를 보면 Child클래스의 parentMethod()에 선언된 예외의 개수가 조상인 Parent클래스의 parentMethod()에 선언된 예외의 개수보다 적으므로 바르게 오버라이딩 되었다.

```java
class Parent {
    void parentMethod() throws IOException, SQLException {
    
    }
}

class Child extends Parent {
    void parentMethod() throws IOException {
    
    }
}
```

여기서 주의해야할 점은 단순히 선언된 예외의 개수가 문제가 아니라는 것이다.

```java
class Child extends Parent {
    void parentMethod() throws Exception {
    
    }
}
```

만일 위와 같이 오버라이딩을 하였다면, 분명히 조상클래스에 정의된 메서드보다 적은 개수의 예외를 선언한 것처럼 보이지만 Exception은 모든 예외의 최고 조상이므로 가장 많은 개수의 예외를 던질 수 있도록 선언한 것이다.

따라서 예외의 개수는 적거나 같아야 한다는 조건을 만족시키지 못하는 잘못된 오버라이딩인 것이다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  