---
title: "[Java] 내부클래스"
description: 
author: Enxec
date: 2024-03-06
categories: [Language, Java]
tags: [java, 자바, Inner class, 내부클래스]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/java-logo.png
  lqip: 
  alt: 
---

내부 클래스는 클래스 내에 선언된다는 점을 제외하고는 일반적인 클래스와 다르지 않다. 다만 앞으로 배우게 될 내부 클래스의 몇 가지 특징만 잘 이해하면 실제로 활용하는데 어려움이 없을 것이다.
내부 클래스는 사용빈도가 높지 않으므로 내부 클래스의 기본 원리와 특징을 이해하는 정도까지만 학습해도 충분하다. 실제로는 발생하지 않을 경우까지 이론적으로 만들어 내서 고민하지말자.

<br>

## 내부 클래스란?
---
내부 클래스는 클래스 내에 선언된 클래스이다. 클래스에 다른 클래스를 선언하는 이유는 간단하다. 두 클래스가 서로 긴밀한 관계에 있기 때문이다. 한 클래스를 다른 클래스의 내부 클래스로 선언하면 두 클래스의 멤버들 간에 서로 쉽게 접근할 수 있다는 장점과 외부에는 불필요한 클래스를 감춤으로써 코드의 복잡성을 줄일 수 있다는 장점을 얻을 수 있다.

__내부 클래스의 장점__
- 내부 클래스에서 외부 클래스의 멤버들을 쉽게 접근할 수 있다.
- 코드의 복잡성을 줄일 수 있다(캡슐화).

아래 B는 A의 내부 클래스(inner class)가 되고 A는 B를 감싸고 있는 외부(outer class)가 된다.

```java
class A { // 외부 클래스
    ...
    class B { // 내부 클래스
        ...
    }
    ...
}
```

이 때 내부 클래스인 B는 외부 클래스인 A를 제외하고는 다른 클래스에 잘 사용되지 않는 것이어야 한다.

<br>

## 내부 클래스의 종류와 특징
---
내부 클래스의 종류는 변수의 선언위치에 따른 종류와 같다. 내부 클래스는 마치 변수를 선언하는 것과 같은 위치에 선언할 수 있으며, 변수의 선언위치에 따라 인스턴스 변수, 클래스변수(static변수), 지역변수로 구분되는 것과 같이 내부 클래스도 선언위치에 따라 다음과 같이 구분되어 진다.

<table border="1">
    <colgroup>
        <col style="width:20%">
        <col style="width:60%">
    </colgroup>
    <tr style="background-color:darkslategray;">
        <th style="text-align:center;">내부 클래스</th>
        <th style="text-align:center;">특징</th>
    </tr>
    <tr>
        <td style="text-align:center;">인스턴스 클래스(instance class)</td>
        <td style="text-align:left;">
            외부 클래스의 멤버변수 선언위치에 선언하며, 외부 클래스의 인스턴스멤버처럼 다루어진다. 주로 외부 클래스의 인스턴스멤버들과 관련된 작업에 사용될 목적으로 선언된다.
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">스태틱 클래스(static class)</td>
        <td style="text-align:left;">
            외부 클래스의 멤버변수 선언위치에 선언하며, 외부 클래스의 static멤버처럼 다루어진다. 주로 외부 클래스의 static멤버, 특히 static메서드에서 사용될 목적으로 선언된다.
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">지역 클래스(local class)</td>
        <td style="text-align:left;">외부 클래스의 메서드나 초기화블럭 안에 선언하며, 선언된 영역 내부에서만 사용될 수 있다.</td>
    </tr>
    <tr>
        <td style="text-align:center;">익명 클래스(anonymous class)</td>
        <td style="text-align:left;">
            클래스의 선언과 객체의 생성을 동시에 하는 이름없는 클래스(일회용)
        </td>
    </tr>
</table>

<br>

## 내부 클래스의 선언
---
아래의 코드는 외부 클래스(Outer)에 3개의 서로 다른 종류의 내부 클래스를 선언한 것이다. 내부 클래스의 선언위치가 변수의 선언위치와 동일함을 알 수 있다.

```java
class Outer {
    class InstanceInner {}
    static class StaticInner {}
    
    voiud myMethod() {
        class LocalInner {}
    }
}
```

<br>

## 내부 클래스의 제어자와 접근성
---
아래 코드에서 인스턴스 클래스와 스태틱 클래스는 외부 클래스의 멤버변수와 같은 위치에 선언되며, 또한 멤버변수와 같은 성질을 갖는다. 따라서 내부 클래스가 외부 클래스의 멤버와 같이 간주되고, 인스턴스멤버와 static멤버 간의 규칙이 내부 클래스에도 똑같이 적용된다.

```java
class Outer {
    private class InstanceInner {}
    protected static class StaticInner {}
    
    void myMethod() {
        class LocalInner {}
    }
}
```

그리고 내부 클래스도 클래스이기 때문에 abstract나 final과 같은 제어자를 사용할 수 있을 뿐만 아니라, 멤버변수들처럼 private, protected과 접근제어자도 사용이 가능하다.

그리고 내부 클래스 중에서는 스태틱 클래스만 static멤버를 가질 수 있다. 드문 경우지만 내부 클래스에 static변수를 선언해야 한다면 스태틱 클래스로 선언해야 한다. 다만 final과 static이 동시에 붙은 변수는 상수이므로 모든 내부 클래스에서 정의가 가능하다.

```java
class InnerEx2 {
    class InstanceInner {}
    static class StaticInner {}
    
    // 인스턴스멤버 간에는 서로 직접 접근이 가능하다.
    IntanceInner iv = new InstanceInner();
    // static 멤버 간에는 서로 직접 접근이 가능하다.
    static StaticInner cv = new StaticInner();
    
    static void staticMethod() {
        // static멤버는 인스턴스멤버에 직접 접근할 수 없다.
        // InstanceInner obj1 = new InstanceInner();
        Static obj2 = new StaticInner();
        
        // 굳이 접근하려면 아래와 같이 객체를 생성해야 한다.
        // 인스턴스클래스는 외부 클래스를 먼저 생성해야만 생성할 수 있다.
        InnerEx2 outer = new InnerEx2();
        InstanceInner obj1 = outer.new InstanceInner();
    }
    
    void instanceMethod() {
        // 인스턴스메서드에서는 인스턴스 멤버와 static멤버 모두 접근 가능하다.
        InstanceInner obj1 = new InstanceInner();
        StaticInner obj2 = new StaticInner();
        // 메서드 내에 지역적으로 선언된 내부 클래스는 외부에서 접근할 수 없다.
        // LocalInner lv = new LocalInner();
    }
    
    void myMethod() {
        class LocalInner {}
        LocalInner lv = new LocalInner();
    }
}
```

인스턴스멤버는 같은 클래스에 있는 인스턴스 멤버와 static멤버 모두 직접 호출이 가능하지만, static멤버는 인스턴스멤버를 직접 호출할 수 없는 것처럼, 인스턴스클래스는 외부 클래스의 인스턴스멤버를 객체생성 없이 바로 사용할 수 있지만, 스태틱 클래스는 외부 클래스의 인스턴스 멤버를 객체생성 없이 사용할 수 없다.

마찬가지로 인스턴스 클래스는 스태틱 클래스의 멤버들을 객체생성 없이 사용할 수 있지만, 스태틱 클래스에서는 인스턴스 클래스의 멤버들을 객체생성 없이 사용할 수 없다.

```java
class InnerEx3 {
    private int outerIv = 0;
    static int outerCv = 0;
    
    class InstanceInner {
        int iiv = outerIv;  // 외부 클래스의 private멤버도 접근가능하다.
        int iiv2 = outerCv;
    }
    
    static class StaticInner {
        // 스태틱 클래스는 외부 클래스의 인스턴스 멤버에 접근할 수 없다.
        // int siv = outerIv;
        static int scv = outerCv;
    }
    
    void myMethod() {
        int lv = 0;
        final int LV = 0; // JDK1.8부터 final 생략 가능
        
        class LocalInner {
            int liv = outerIv;
            int liv2 = outerCv;
            // 외부 클래스의 지역변수는 final이 붙은 변수(상수)만 접근가능하다.
            int liv3 = lv;  // 에러(JDK1.8부터 에러아님)
            int liv4 = LV;  // OK
        }
    }
}
```

내부 클래스에서 외부 클래스의 변수들에 대한 접근성을 보여 주는 예제이다. 인스턴스 클래스는 외부 클래스의 인스턴스 멤버이기 때문에 인스턴스변수 outerIv와 static변수 outerCv를 모두 사용할 수 있다. 심지어는 outerIv의 접근 제어자가 private일지라도 사용가능하다.

스태틱 클래스는 외부 클래스의 static멤버이기 때문에 외부 클래스의 인스턴스멤버인 outerIv와 InstaceInner를 사용할 수 없다. 단지 static멤버인 outerCv만을 사용할 수 있다.

지역 클래스는 외부 클래스의 인스턴스멤버와 static멤버를 모두 사용할 수 있으며, 지역 클래스가 포함된 메서드에 정의된 지역변수도 사용할 수 있다. 단, final이 붙은 지역변수만 접근가능한데 그 이유는 메서드가 수행을 마쳐서 지역변수가 소멸된 시점에도, 지역 클래스의 인스턴스가 소멸된 지역변수를 참조하려는 경우가 발생할 수 있기 때문이다. 

JDK1.8부터 지역 클래스에서 접근하는 지역 변수 앞에 final을 생략할 수 있게 바뀌었다. 대신 컴파일러가 자동으로 붙여준다. 즉, 편의상 final을 생략할 수 있게 한 것일 뿐, 해당 변수의 값이 바뀌는 문장이 있으면 컴파일 에러가 발생한다.

```java
class Outer {
    class InstanceInner {
        int iv = 100;
    }
    
    static class StaticInner {
        int iv = 200;
        static int cv = 300;
    }
    
    void myMethod() {
        class LocalInner {
            int iv = 400;
        }
    }
}

class InnerEx4 {
    public static void man(String[] args) {
        // 인스턴스 클래스의 인스턴스를 생성하려면
        // 외부 클래스의 인스턴스를 먼저 생성해야 한다.
        Outer oc = new Outer();
        Outer.InstanceInner ii = oc.new InstanceInner();
        
        System.out.println("ii.iv : " + ii.iv);
        System.out.println("Outer.StaticInner.cv : " + Outer.StaticInner.cv);
        
        // 스태틱 내부 클래스의 인스턴스는 외부 클래스를 먼저 생성하지 않아도 된다.
        Outer.StaticInner si = new Outer.StaticInner();
        Syetem.out.println("si.iv : " + si.iv);
    }
}
```

실제로 위와 같은 경우가 발생했다는 것은 내부 클래스로 선언해서는 안 되는 클래스를 내부 클래스로 선언했다는 의미이다.

<br>

## 익명 클래스
---
익명 클래스는 특이하게도 다른 내부 클래스들과는 달리 이름이 없다. 클래스의 선언과 객체의 생성을 동시에 하기 때문에 단 한번만 사용될 수 있고 오직 하나의 객체만을 생성할 수 있는 일회용 클래스이다.

```java
new 조상클래스이름() {
    // 멤버 선언
}

또는

new 구현인터페이스이름() {
    // 멤버 선언
}
```

이름이 없기 때문에 생성자도 가질 수 없으며, 조상클래스의 이름이나 구현하고자 하는 인터페이스의 이름을 사용해서 정의하기 때문에 하나의 클래스로 상속받는 동시에 인터페이스를 구현하거나 둘 이상의 인터페이스를 구현할 수 없다. 오로지 단 하나의 클래스를 상속받거나 단 하나의 인터페이스만을 구현할 수 있다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  