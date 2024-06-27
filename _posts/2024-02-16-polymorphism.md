---
title: "[Java] 다형성"
description: 
author: Enxec
date: 2024-02-16
categories: [Language, Java]
tags: [java, 자바, Polymorphism, 다형성]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/java-logo.png
  lqip: 
  alt: 
---

## 다형성이란?
---
상속과 함께 객체지향개념의 중요한 특징 중의 하나인 다형성에 대해서 배워 보도록 하자. 다형성은 상속과 깊은 관계가 있으므로 학습하기에 앞서 상속에 대해 충분히 알고 있어야 한다.

객체지향 개념에서 다형성이란 '여러 가지 형태를 가질 수 있는 능력'을 의미하며, 자바에서는 한 타입의 참조변수로 여러 타입의 객체를 참조할 수 있도록 함으로써 다형성을 프로그램적으로 구현하였다.

이를 좀 더 구체적으로 말하자면, 조상클래스 타입의 참조변수로 자손클래스의 인스턴스를 참조할 수 있도록 하였다는 것이다.

아래 코드를 보자.

```java
class Tv {
    boolean power;    // 전원상태(on/off)
    int channel;      // 채널
    
    void power() {
        power != power;
    }
    
    void channelUp() {
        ++channel;
    }
    
    void channelDown() {
        --channel;
    }
}

class CaptionTv extends Tv {
    String text;    // 캡션을 보여주기 위한 문자열
    void caption() {
        ...
    }
}
```

Tv클래스와 CaptionTv클래스가 이와 같이 정의되어 있을 때, 두 클래스는 서로 상속관계에 있으며, 이 두 클래스의 인스턴스를 생성하고 사용하기 위해서는 다음과 같이 할 수 있다.

```java
Tv t = new Tv();
CaptionTv c = new CaptionTv();
```

지금까지 우리는 생성된 인스턴스를 다루기 위해, 인스턴스의 타입과 일치하는 타입의 참조변수만을 사용하였다. 즉, Tv인스턴스를 다루기 위해서는 Tv타입의 참조변수를 사용하고, CaptionTv인스턴스를 다루기 위해서는 CaptionTv타입의 참조변수를 사용했다.

이처럼 인스턴스의 타입과 참조변수의 타입이 일치하는 것이 보통이지만, Tv와 CaptionTv클래스가 서로 상속관계에 있을 경우, 다음과 같이 조상 클래스 타입의 참조변수로 자손 클래스의 인스턴스를 참조하도록 하는 것도 가능하다.

```java
// 조상 타입의 참조변수로 자손 인스턴스를 참조
Tv t = new CaptionTv(); 
```

그렇다면  인스턴스를 같은 타입의 참조변수로 참조하는 것과 조상타입의 참조변수로 참조하는 것은 어떤 차이가 있을까?

```java
CaptionTv c = new CaptionTv();
Tv t = new CaptionTv();
```

위의 코드는 CaptionTv 인스턴스를 2개 생성하고, 참조변수 c와 t가 생성된 인스턴스를 하나씩 참조하도록 한것이다. 이 경우 실제 인스턴스가 CaptionTv타입이라 할지라도, 참조변수 t로는 CaptionTv인스턴스의 모든 멤버를 사용할 수 없다.

Tv타입의 참조변수로는 CaptionTv인스턴스 중에서 Tv클래스의 멤버들(상속받은 멤버 포함)만 사용할 수 있다. 따라서, 생성된 CaptionTv인스턴스의 멤버 중에서 Tv클래스에 정의 되지 않은 멤버, text와 caption()은 참조변수 t로 사용이 불가능하다. 즉, t.text 또는 t.caption()와 같이 할 수 없다는 것이다. 둘 다 같은 타입의 인스턴스지만 참조변수의 타입에 따라 사용할 수 있는 멤버의 개수가 다르기 때문이다. 

반대로 아래와 같이 자손타입의 참조변수로 조상타입의 인스턴스를 참조하는 것은 가능할까?

```java
CaptionTv c = new Tv();
```

불가능하다. 위의 코드를 컴파일하면 에러가 발생하며, 실제 인스턴스인 Tv의 멤버 개수가보다 참조변수 c가 사용할 수 있는 멤버 개수가 더 많기 때문이다.

그래서, 자손타입의 참조변수로 조상타입의 인스턴스를 참조하는 것은 존재하지 않는 멤버를 사용하고자 할 가능성이 있으므로 허용하지 않는 것이다. 참조변수가 사용할 수 있는 멤버의 개수는 인스턴스의 멤버 개수보다 같거나 적어야 한다.

💡 __참고__  
- 클래스는 상속을 통해서 확장될 수는 있어도 축소될수는 없기 때문에, 조상인스턴스의 멤버 개수는 자손 인스턴스의 멤버 개수보다 항상 적거나 같아야 한다.
- 모든 참조변수는 null 또는 4byte 주소값이 저장되며, 참조변수의 타입은 참조할 수 있는 객체의 종류와 사용할 수 있는 멤버의 수를 결정한다.

📝 __요약__
>- 조상타입의 참조변수로 자손타입의 인스턴스를 참조할 수 있다.
>- 반대로 자손타입의 참조변수로 조상타입의 인스턴스를 참조할 수는 없다.

<br>

## 참조변수의 형변환
---
기본형 변수와 같이 참조변수도 형변환이 가능하다. 단, 서로 상속관계에 있는 클래스사이에서만 가능하기 때문에 자손타입의 참조변수를 조상타입의 참조변수로, 조상타입의 참조변수를 자손타입의 참조변수로의 형변환만 가능하다.

💡 __참고__  
바로 윗 조상이나 자손이 아닌, 조상의 조상으로도 형변환이 가능하다. 따라서 모든 참조변수는 모든 클래스의 조상인 Object클래스 타입으로 형변환이 가능하다.

기본형 변수의 형변환에서 작은 자료형에서 큰 자료형의 형변환은 생략이 가능하듯이, 참조형 변수의 형변환에서는 자손타입의 참조변수를 조상타입으로 형변환하는 경우에는 형변환을 생략할 수 있다.

- 자손타입 -> 조상타입(Up-casting) : 형변환 생략가능
- 자손타입 <- 조상타입(Down-casting) : 형변환 생략불가

참조변수간의 형변환 역시 캐스트연산자를 사용하며, 괄호()안에 변환하고자 하는 타입의 이름(클래스명)을 적어주면 된다.

예시를 통해 좀더 자세히 살펴보자.

```java
class Car {
    String color;
    int door;
    void drive() {    // 운전하는 기능
        System.out.println("drive, Brrrr");
    }
    
    void stop() {    // 멈추는 기능
        System.out.println("stop");
    }
}

class FireEngine extends Car {    // 소방차
     void water() {               // 물 뿌리는 기능
         System.out.println("water");
     }
}

class Ambulance extends Car {
    void siren() {
        System.out.println("siren");
    }
}
```

위와 같이 세 클래스, Car, FireEngine, Ambulance가 정의되어 있다. 잠깐 하나만 먼저 짚고가면, Car 클래스는 FireEngine클래스와 Ambulance클래스의 조상이다. 그렇다고 해서 FireEngine클래스와 Ambulance클래스가 형제관계는 아니다. 자바에서는 조상과 자식관계만 존재하기 때문에 FireEngine클래스와 Ambulance클래스는 서로 아무런 관계가 없다.

따라서 Car타입의 참조변수와 FireEngine타입의 참조변수 그리고 Car타입의 참조변수와 Ambulance타입의 참조변수 간에는 서로 형변환이 가능하지만, FireEngine타입의 참조변수와 Ambulance타입의 참조변수 간에는 서로 형변환이 가능하지 않다.

```java
FireEngine f;
Ambulance a;
a = (Ambulance)f;      // 에러. 상속관계가 아닌 클래스간의 형변환 불가
f = (FireEngine)a;     // 에러. 상속관계가 아닌 클래스간의 형변환 불가
```

이제 조상과 자손의 관계에서의 형변환을 보자.

```java
Car car = null;
FireEngine fe = new FireEngine();
FireEngine fe2 = null;

car = fe;                  // car = (Car)fe; 에서 형변환 생략됨. 업캐스팅
fe2 = (FireEngine)car;     // 형변환을 생략불가. 다운캐스팅
```

참조변수 car와 fe의 타입이 서로 다르기 때문에, 대입연산자가 수행되기 전에 형변환을 수행하여 두 변수간의 타입을 맞추주어야 한다.

그러나, 자손타입의 참조변수를 조상타입의 참조변수에 할당할 경우 형변환을 생략할 수 있어서 'car = fe;' 와 같이 하였다. 원칙적으로는 'car = (Car)fe;' 와 같이 해야 한다.

반대로 조상타입의 참조변수를 자손타입의 참조변수에 저장할 경우 형변환을 생략할 수 없으므로, 'fe2 = (FireEngine)car;' 와 같이 명시적으로 형변환을 해주어야 한다.

그런데 상속관계에서의 형변환 시 주의해야 할 점이 있다. 아래 예제를 보자.

```java
Car car = new Car();
Car car2 = null;
FireEngine fe = null;

car.drive();
fe = (FireEngine)car;    // 컴파일은 정상. 실행시 에러
fe.drive();
car2 = fe;
car2.drive();
```

위 예제는 컴파일은 성공하지만, 실행 시 형변환의 오류로 에러가 발생한다. 왜일까? 캐스트 연산자를 이용해서 조상타입의 참조변수를 자손타입의 참조변수로 형변환한 것이기 때문에 문제가 없어 보이지만, 문제는 참조변수 car가 참조하고 있는 인스턴스가 Car타입의 인스턴스라는데 있다. 전에 언급했던 것처럼 조상타입의 인스턴스를 자손타입의 참조변수로 참조하는 것은 허용되지 않는다.

📝 __요약__
서로 상속관계에 있는 타입간의 형변환은 양방향으로 자유롭게 수행될 수 있으나, 참조변수가 가리키는 인스턴스의 
자손타입으로 형변환은 허용되지 않는다. 따라서 참조변수가 가리키는 인스턴스의 타입이 무엇인지 확인하는 것이 중요하다.

<br>

## instanceof 연산자
---
참조변수가 참조하고 있는 인스턴스의 실제 타입을 알아보기 위해 instanceof 연산자를 사용한다. 주로 조건문에 사용되며, instanceof의 왼쪽에는 참조변수를, 오른쪽에는 타입(클래스명)이 피연산자로 위치한다. 그리고 연산의 결과로 boolean값인 true와 false 중의 하나를 반환한다. instanceof를 이용한 연산결과로 true를 얻었다는 것은 참조변수가 검사한 타입으로 형변환이 가능하다는 것을 뜻한다.

💡 __참고__  
값이 null인 참조변수에 대해 instanceof연산을 수행하면 false를 결과로 얻는다.

```java
void doWork(Car c) {
    if(c instanceof FireEngine) {
        FireEngine fe = (FireEngine)c;
        fe.water();
        ...
    } else if(c instanceof Ambulance) {
        Ambulance a = (Ambulance)c;
        a.siren();
        ...
    }
    ...
}
```

위 예제는 Car타입의 참조변수 c를 매개변수로 하는 메서드다. 이 메서드가 호출될 때, 매개변수로 Car클래스 또는 그 자손 클래스의 인스턴스를 넘겨받겠지만 메서드 내에서는 정확히 어떤 인스턴스인지 알 길이 없다. 때문에 instanceof연산자를 이용해서 참조변수 c가 가리키고 있는 인스턴스의 타입을 체크하고, 적절히 형변환한 다음에 작업을 해야한다.

📝 __요약__  
어떤 타입에 대한 instanceof연산의 결과가 true라는 것은 검사한 타입으로 형변환이 가능하다는 것을 뜻한다.

<br>

## 참조변수와 인스턴스의 연결
---
조상 타입의 참조변수와 자손 타입의 참조변수의 차이점이 사용할 수 있는 멤버의 개수에 있다고 배웠다. 여기서 한 가지 더 알아두어야 할 내용이 있다. 조상 클래스에 선언된 멤버변수와 같은 이름의 인스턴스변수를 자손 클래스에 중복으로 정의했을 때, 조상타입 참조변수로 자손 인스턴스를 참조하는 경우와 자손타입의 참조변수로 자손 인스턴스를 참조하는 경우는 서로 다른 결과를 얻는다.

메서드의 경우 조상 클래스의 메서드를 자손의 클래스에서 오버라이딩한 경우에도 참조변수의 타입에 관계없이 항상 실제 인스턴스의 메서드(오버라이딩된 메서드)가 호출되지만, __멤버변수의 경우 참조변수의 타입에 따라 달라진다.__

💡 __참고__  
static메서드는 static변수처럼 참조변수의 타입에 영향을 받는다. 참조변수의 타입에 영향을 받지 않는 것은 인스턴스메서드 뿐이다. 그래서 static메서드는 반드시 참조변수가 아닌 '클래스이름.메서드()' 로 호출해야 한다.

결론부터 말하자면, 멤버변수가 조상 클래스와 자손 클래스에 중복으로 정의된 경우, 조상타입의 참조변수를 사용했을 때는 조상 클래스에 선언된 멤버변수가 사용되고, 자손타입의 참조변수를 사용했을 때는 자손 클래스에 선언된 멤버변수가 사용된다.

하지만 중복 정의되지 않은 경우에는 조상타입의 참조변수를 사용하였을 때와 자손타입의 참조변수를 사용했을 때의 차이는 없다. 중복된 경우는 참조변수의 타입에 따라 달리지지만 중복되지 않는 경우 하나뿐이므로 선택의 여지가 없기 때문이다.

```java
class BindingTest {
    public static void main(String[] args) {
        Parent p = new Child();
        Child c = new Child();
        
        System.out.println("p.x = " + p.x);
        p.method();
        
        System.out.println("c.x = " + c.x);
        c.method();
    }
}

class Parent {
    int x = 100;
    
    void mothod() {
        System.out.println("Parent Method");
    }
}

class Child extends Parent {
    int x = 200;
    
    void method() {
        System.out.println("Child Method");
    }
}
```

```java
// 실행 결과
p.x = 100
Child Method
c.x = 200
Child Method
```

타입은 다르지만, 참조변수 p와 c모두 Child인스턴스를  참조하고 있다. 그리고 Parent클래스와 Child클래스는 서로 같은 멤버들을 정의하고 있다. 이 때  조상타입의 참조변수 p로 Child인스턴스의 멤버들을 사용하는 것과 자손타입의 참조변수 c로 Child인스턴스의 멤버들을 사용하는 것의 차이를 알 수 있다.

메서드인 method()의 경우 참조변수의 타입에 관계없이 항상 실제 인스턴스의 타입인 Child클래스에 정의된 메서드가 호출되지만, 인스턴스변수인 x는 참조변수의 타입에 따라서 달라진다.

```java
class BindingTest {
    public static void main(String[] args) {
        Parent p = new Child();
        Child c = new Child();
        
        System.out.println("p.x = " + p.x);
        p.method();
        
        System.out.println("c.x = " + c.x);
        c.method();
    }
}

class Parent {
    int x = 100;
    
    void mothod() {
        System.out.println("Parent Method");
    }
}

class Child extends Parent {
    
}
```

```java
// 실행 결과
p.x = 100
Parent Method
c.x = 100
Parent Method
```

이전의 예제와는 달리 Child클래스에는 아무런 멤버도 정의되어 있지 않고 단순히 조상으로부터 멤버들을 상속받는다. 그렇기 때문에 참조변수의 타입에 관계없이 조상의 멤버들을 사용하게 된다.

이처럼 자손 클래스에서 조상 클래스의 멤버를 중복으로 정의하지 않았을 때는 참조변수의 타입에 따른 변화는 없다. 어느 클래스의 멤버가 호출되어야 할지, 즉 조상의 멤버가 호출되어야할 지, 자손의 멤버가 호출되어야 할지에 대해 선택의 여지가 없기 때문이다. 참조변수의 타입에 따라 결과가 달라지는 경우는 조상 클래스의 멤버변수와 같은 이름의 멤버변수를 자손 클래스에 중복해서 정의한 경우뿐이다.

<br>

## 매개변수의 다형성
---
참조변수의 다형적인 특징은 메서드의 매개변수에도 적용된다. 아래와 같이 Product, Tv, Computer, Audio, Buyer클래스가 정의되어 있다고 가정하자.

```java
class Product {
    int price;        // 제품의 가격
    int bonusPoint;   // 제품구매 시 제공하는 보너스 점수
}

class Tv extends Product {}
class Computer extends Product {}
class Audio extends Product {}

class Buyer {               // 고객, 물건을 사는 사람
    int money = 1000;       // 소유금액
    int bonusPoint = 0;     // 보너스점수
}
```

Product클래스는 Tv, Audio, Computer클래스의 조상이며, Buyer클래스는 제품(Product)을 구입하는 사람을 클래스로 표현한 것이다.

Buyer클래스에 물건을 구입하는 기능의 메서드를 추가해보자. 구입할 대상이 필요하므로 매개변수로 구입할 제품을 넘겨받아야 한다. Tv를 살 수 있도록 매개변수를 Tv타입으로 하였다.

```java
void buy(Tv t) {
    // Buyer가 가진 돈(money)에서 제품의 가격(t.price)만큼 뺀다.
    money = meney - t.price;
    
    // Buyer의 보너스점수(bonusPoint)에 제품의 보너스점수(t.bonusPoint)를 더한다.
    bonusPoint = bonusPoint + t.bonusPoint;
}
```

buy(Tv t)는 제품을 구입하면 제품을 구입한 사람이 가진 돈에서 제품의 가격을 빼고, 보너스점수는 추가하는 작업을 하도록 작성되었다. 그런데 buy(Tv t)로는 Tv밖에 살 수 없기 때문에 아래와 같이 다른 제품들도 구입할 수 있는 메서드가 추가로 필요하다.

```java
void buy(Computer c) {
    money = money - c.price;
    bonusPoint = bonusPoint + c.bonusPoint;
}

void buy(Audio a) {
    money = money - a.price;
    bonusPoint = bonusPoint + a.bonusPoint;
}
```

이렇게 되면, 제품의 종류가 늘어날 때마다 Buyer클래스에는 새로운 buy메서드를 추가해주어야 할 것이다.

그러나 메서드의 매개변수에 다형성을 적용하면 아래와 같이 하나의 메서드로 간단히 처리할 수 있다.

```java
void buy(Product p) {
    money = money - p.price;
    bonusPoint = bonusPoint + p.bonusPoint;
}
```

매개변수가 Product타입의 참조변수라는 것은, 메서드의 매개변수로 Product클래스의 자손타입의 참조변수면 어느 것이나 매개변수로 받아들일 수 있다는 뜻이다. 그리고 Product클래스에 price와 bonusPoint가 선언되어 있기 때문에 참조변수 p로 인스턴스의 price와 bonusPoint를 사용할 수 있기에 이와 같이 할 수 있다.

앞으로 다른 제품 클래스를 추가할 때 Product클래스를 상속받기만 하면, buy(Product p) 메서드의 매개변수로 받아들여질 수 있다.

```java
Buyer b = new Buyer();
Tv t = new Tv();
Computer c = new Computer();
b.buy(t);
b.buy(c);
```

Tv클래스와 Computer클래스는 Product클래스의 자손이므로 위의 코드와 같이 buy(Product p)메서드에 매개변수로 Tv인스턴스와 Computer인스턴스를 제공하는 것이 가능하다.

<br>

## 여러 종류의 객체를 배열로 다루기
---
조상타입의 참조변수로 자손타입의 객체를 참조하는 것이 가능하므로, Product클래스가 Tv, Computer, Audio클래스의 조상일 때, 다음과 같이 할 수 있는 것을 이미 배웠다.

```java
Product p1 = new Tv();
Product p2 = new Computer();
Product p3 = new Audio();
```

위의 코드를 Product타입의 참조변수 배열로 처리하면 아래와 같다.

```java
Product p[] = new Product[3];
p[0] = new Tv();
p[1] = new Computer();
p[2] = new Audio();
```

이처럼 조상타입의 참조변수 배열을 사용하면, 공통의 조상을 가진 서로 다른 종류의 객체를 배열로 묶어서 다룰 수 있다. 또는 묶어서 다루고 싶은 객체들의 상속관계를 따져서 가장 가까운 공통조상 클래스 타입의 참조변수 배열을 생성해서 객체들을 저장하면 된다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  