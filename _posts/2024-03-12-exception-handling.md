---
title: "[Java] 예외처리"
description: 
author: Enxec
date: 2024-03-12
categories: [Language, Java]
tags: [java, 자바, Exception handling, 예외처리]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/java-logo.png
  lqip: 
  alt: 
---

## 프로그램 오류
---
프로그램이 실행 중 어떤 원인에 의해서 오작동을 하거나 비정상적으로 종료되는 경우가 있다. 이러한 결과를 초래하는 원인을 프로그램 에러 또는 오류라고 한다. 이를 발생시점에 따라 '컴파일 에러'와 '런타임 에러'로 나눌 수 있는데, 글자 그대로 '컴파일 에러'는 컴파일 할 때 발생하는 에러이고 프로그램의 실행도중에 발생하는 에러를 '런타임 에러' 라고 한다. 이 외에도 '논리적 에러'가 있는데, 컴파일도 잘되고 실행도 잘되지만 의도한 것과 다르게 동작하는 것을 말한다. 예를 들어, 창고의 재고가 음수가 된다던가, 게임 프로그램에서 비행기가 총알을 맞아도 죽지 않는 경우가 이에 해당된다.

>- 컴파일에러 : 컴파일 시에 발생하는 에러
>- 런타임에러 : 실행 시에 발생하는 에러
>- 논리적에러 : 실행은 되지만, 의도와 다르게 동작하는 것

기본적인 검사를 수행하여 오류가 있는지를 알려 준다. 컴파일러가 알려 준 에러들을 모두 수정해서 컴파일을 성공적으로 마치고 나면, 클래스 파일(*.class)이 생성되고, 생성된 클래스 파일을 실행할 수 있게 되는 것이다.

하지만 컴파일을 에러없이 성공적으로 마쳤다고 해서 프로그램의 실행 시에도 에러가 발생하지 않는 것은 아니다. 컴파일러가 소스코드의 기본적인 사항은 컴파일 시에 모두 걸러 줄 수는 있지만, 실행도중에 발생할 수 있는 잠재적인 오류까지 검사할 수 없기 때문에 컴파일은 잘되었어도 실행 중에 에러에 의해서 잘못된 결과를 얻거나 프로그램이 비정상적으로 종료될 수 있다. 여러분은 이미 실행도중에 발생하는 런타임 에러를 여러 번 경험했을 것이다. 예를 들면 프로그램이 실행 중 동작을 멈춘 상태로 오랜 시간 지속되거나, 갑자기 프로그램이 실행을 멈추고 종료되는 경우 등이 이에 해당한다.

런타임 에러를 방지하기 위해서는 프로그램의 실행도중 발생할 수 있는 모든 경우의 수를 고려하여 이에 대한 대비를 하는 것이 필요하다. 자바에서는 실행 시(runtime) 발생할 수 있는 프로그램 오류를 '에러'와 '예외' 두 가지로 구분하였다.

에러는 메모리 부족이나 스택오버플로우와 같이 일단 발생하면 복구할 수 없는 심각한 오류이고, 예외는 발생하더라도 수습될 수 있는 비교적 덜 심각한 것이다. 에러가 발생하면, 프로그램의 비정상적인 종료를 막을 길이 없지만, 예외는 발생하더라도 프로그래머가 이에 대한 적절한 코드를 미리 작성해 놓음으로써 프로그램의 비정상적인 종료를 막을 수 있다. 

>- 에러 : 프로그램 코드에 의해서 수습될 수 없는 심각한 오류
>- 예외 : 프로그램 코드에 의해서 수습될 수 있는 다소 미약한 오류

<br>

## 예외 클래스의 계층구조
---
자바에서는 실행 시 발생할 수 있는 오류(Exception과 Error)를 클래스로 정의하였다. 앞서 배운 것처럼 모든 클래스의 조상은 Object클래스이므로 Exception과 Error클래스 역시 Object클래스의 자손들이다.

![예외클래스 계층도](/posts/20240312/exceptionclass-hierarchy.png "예외클래스 계층도"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">예외클래스 계층도</div>

모든 예외의 최고 조상은 Exception클래스이며, 상속계층도를 Exception클래스부터 도식화하면 다음과 같다.

![Exception클래스와 RuntimeException클래스 중심의 상속계층도](/posts/20240312/exceptionclass-hierarchy2.png "Exception클래스와 RuntimeException클래스 중심의 상속계층도"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">Exception클래스와 RuntimeException클래스 중심의 상속계층도</div>

위 그림에서 볼 수 있듯이 예외 클래스들은 다음과 같이 두 그룹으로 나눠질 수 있다.

>1. Exception클래스와 그 자손들
>2. RuntimeException클래스와 그 자손들

RuntimeException클래스들은 주로 프로그래머의 실수에 의해서 발생될 수 있는 예외들로 자바의 프로그래밍 요소들과 관계가 깊다. 예를 들면, 배열의 범위를 벗어난다던가, 값이 null인 참조변수의 멤버를 호출하려 했다던가, 클래스간의 형변환을 잘못했다던가, 정수를 0으로 나누려고하는 경우에 발생한다.

Exception클래스들은 주로 외부의 영향으로 발생할 수 있는 것들로서, 프로그램의 사용자들의 동작에 의해서 발생하는 경우가 많다. 예를 들면, 존재하지 않는 파일의 이름을 입력했다던가, 실수로 클래스의 이름을 잘못 적었다던가, 또는 입력한 데이터 형식의 잘못된 경우에 발생한다.

>- Exception클래스 : 사용자의 실수와 같은 외적인 요인에 의해 발생하는 예외
>- RuntimeException클래스 : 프로그래머의 실수로 발생하는 예외

<br>

## 예외처리하기 (try - catch문)
---
프로그램의 실행도중에 발생하는 에러는 어쩔 수 없지만, 예외는 프로그래머가 이에 대한 처리를 미리 해주어야 한다.

예외처리란, 프로그램 실행 시 발생할 수 있는 예기치 못한 예외의 발생에 대비한 코드를 작성하는 것이며, 예외처리의 목적은 예외의 발생으로 인한 실행 중인 프로그램의 갑작스런 비정상 종료를 막고, 정상적인 실행상태를 유지할 수 있도록 하는 것이다.

> 예외처리의 정의와 목적
>- 정의 : 프로그램 실행 시 발생할 수 있는 예외에 대비한 코드를 작성하는 것
>- 목적 : 프로그램의 비정상 종료를 막고, 정상적인 실행상태를 유지하는 것

발생한 예외를 처리하지 못하면, 프로그램은 비정상적으로 종료되며, 처리되지 못한 예외는 JVM '예외처리기'가 받아서 예외의 원인을 화면에 출력한다.
예외를 처리하기 위해서는 try-catch문을 사용하며, 그 구조는 다음과 같다.

```java
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch (Exception1 e1) {
    // Exception1이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch (Exception e2) {
    // Exception2가 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
} catch (ExceptionN eN) {
    // ExceptionN이 발생했을 경우, 이를 처리하기 위한 문장을 적는다.
}
```

하나의 try블럭 다음에는 여러 종류의 예외를 처리할 수 있도록 하나 이상의 catch블럭이 올 수 있으며, 이 중 발생한 예외의 종류와 일치하는 단 한 개의 catch블럭만 수행된다. 발생한 예외의 종류와 일치하는 catch블럭이 없으면 예외는 처리되지 않는다.

💡 __참고__  
if문과 달리 try블럭이나 catch블럭 내에 포함된 문장이 하나뿐이어도 괄호{}를 생략할 수 없다.

```java
class ExceptionEx1 {
    public static void main(String[] args) {
        try {
            try {
            
            } catch (Exception e1) {
                
            }
        } catch (Exception e) {
            try {
            
            } catch (Exception e) { // 에러. 변수 e가 중복 선언되었다.
                
            }
        }
    }
}
```

위의 예제는 아무 일도 하지 않는다. 단순히 try-catch문의 사용 예를 보여 주기 위해서 작성한 코드이다. 이처럼 하나의 메서드 내에 여러 개의 try-catch문이 사용될 수 있으며,  try블럭 또는 catch블럭에 또 다른 try-catch문이 포함될 수 있다. catch블럭 내의 코드에서도 예외가 발생할 수 있기 때문이다. catch블럭의 괄호 내에 선언된 변수는 catch블럭 내에서만 유효하기 때문에, 위의 모든 catch블럭에 참조변수 'e'하나만을 사용해도 된다.

그러나 catch블럭 내에 또하나의 try-catch문이 포함된 경우, 같은 이름의 참조변수를 사용해서는 안된다. 각 catch블럭에 선언된 두 참조변수의 영역이 서로 겹치므로 다른 이름을 사용해야만 서로 구별되기 때문이다. 따라서 위의 예제에서 catch블럭 내의 try-catch문에 선언되어 있는 참조변수의 이름을 'e'가 아닌 다른 것으로 바꾸어야 한다.

```java
class ExceptionEx2 {
    public static void main(String args[]) {
        int number = 100;
        int result = 0;
        
        for(int i = 0; i < 10; i++) {
            result = number / (int)(Math.random() * 10);
            System.out.println(result);
        }
    }
}
```

위의 예제는 변수 number에 저장되어 있는 값 100을 0 ~ 9 사이의 임의의 정수로 나눈 결과를 출력하는 일을 10번 반복한다. random()을 사용했기 떄문에 매번 실행할 때마다 결과가 다르겠지만, 대부분의 경우 10번이 출력되기 이전에 예외가 발생하여 프로그램이 비정상적으로 종료될 것이다. 결과에 나타난 메시지를 보면 예외의 발생원인과 위치를 알 수 있다. 이 메시지를 보면, 0으로 나누려 했기 때문에 'ArithmeticException'이 발생했고, 발생위치는 ExceptionEx2클래스의 main메서드(ExceptionEx2.java의 7번째 라인)라는 것을 알 수 있다.

ArithmeticException은 산술연산과정에서 오류가 있을 때 발생하는 예외이며, 정수는 0으로 나누는 것이 금지되어있기 때문에 발생했다. 하지만, 실수를 0으로 나누는 것은 금지되어 있지 않으며 예외가 발생하지 않는다.

위의 코드에 예외처리를 적용하면 다음과 같이 변경면 된다.

```java
class ExceptionEx2 {
    public static void main(String args[]) {
        int number = 100;
        int result = 0;
        
        for(int i = 0; i < 10; i++) {
            try {
                result = number / (int)(Math.random() * 10);
                System.out.println(result);
            } catch(ArithmeticException e) {
                System.out.println("0");
            }
        }
    }
}
```

<br>

## try-catch문에서의 흐름
---
try-catch문에서, 예외가 발생한 경우와 발생하지 않았을 때의 흐름(문장의 실행순서)이 달라지는데, 아래에 이 두가지 경우에 따른 문장 실행순서를 정리하였다.

- try블럭 내에서 예외가 발생한 경우
1. 발생한 예외와 일치하는 catch블럭이 있는지 확인한다.
2. 일치하는 catch블럭을 찾게 되면, 그 catch블럭 내의 문장들을 수행하고 전체 try-catch문을 빠져나가서 그 다음 
   문장을 계속해서 수행한다. 만일 일치하는 catch블럭을 찾지 못하면, 예외는 처리되지 못한다.

- try블럭 내에서 예외가 발생하지 않은 경우
1. catch블럭을 거치지 않고 전체 try-catch문을 빠져나가서 수행을 계속한다.

<br>

## 예외의 발생과 catch블럭
---
catch블럭은 괄호()와 블럭{} 두 부분으로 나눠져 있는데, 괄호()내에는 처리하고자 하는 예외와 같은 타입의 참조변수 하나를 선언해야한다. 예외가 발생하면, 발생한 예외에 해당하는 클래스의 인스턴스가 만들어 진다. 예제8-5에서는 AruthmeticException이 발생했으므로 ArithmeticException인스턴스가 생성된다. 예외가 발생한 문장이 try블럭에 포함되어 있다면, 이 예외를 처리할 수 있는 catch블럭이 있는지 찾게 된다.

첫 번째 catch블럭부터 차례로 내려가면서 catch블럭의 괄호()내에 선언된 참조변수의 종류와 생성된 예외클래스의 인스턴스에 instanceof연산자를 이용해서 검사하게 되는데 검사결과가 true인 catch블럭을 만날 때까지 검사는 계속된다.

검사결과가 true인 catch블럭을 찾게 되면 블럭에 있는 문장들을 모두 수행한 후에 try-catch문을 빠져나가고 예외는 처리되지만, 검사결과가 true인 catch블럭이 하나도 없으면 예외는 처리되지 않는다.

모든 예외 클래스는 Exception클래스의 자손이므로, catch블럭의 괄호()에 Exception클래스 타입의 참조변수를 선언해 놓으면 어떤 종류의 예외가 발생하더라도 이 catch블럭에 의해서 처리된다.

### printStackTrace()와 getMessage()

예외가 발생했을 때 생성되는 예외 클래스의 인스턴스에는 발생한 예외에 대한 정보가 담겨 있으며, getMessage()와 printStackTrace()를 통해서 이 정보들을 얻을 수 있다. catch블럭의 괄호()에 선언된 참조변수를 통해 이 인스턴스에 접근할 수 있다. 이 참조변수는 선언된 catch블럭 내에서만 사용 가능하며, 자주 사용되는 메서드는 다음과 같다.

>- printStackTrace()  
>예외발생 당시의 호출스택(Call Stack)에 있었던 메서드의 정보와 예외 메시지를 화면에 출력한다.
>- getMessage()  
>발생한 예외클래스의 인스턴스에 저장된 메시지를 얻을 수 있다.

💡 __참고__  
printStackTrace(PrintStream s) 또는 printStackTrace(PrintWriter s)를 사용하면 발생한 예외에 대한 정보를 파일에 저장할 수도 있다.

### 멀티 catch블럭

JDK1.7부터 여러 catch블럭을 '\|' 기호를 이용해서, 하나의 catch블럭으로 합칠 수 있게 되었으며, 이를 '멀티 catch블럭'이라 한다. 아래의 코드에서 알 수 있듯이 '멀티 catch블럭'을 이용하면 중복된 코드를 줄일 수 있다. 그리고 '\|' 기호로 연결할 수 있는 예외 클래스의 개수에는 제한이 없다.

```java
try {
    ...
} catch (ExceptionA e) {
    e.printstackTrace();
} catch (ExceptionB e2) {
    e2.printStackTrace();
}

▼

try {
    ...
} catch (ExceptionA | ExceptionB e) {
    e.printStackTrace();
}
```

만일 멀티 catch블럭의 '\|' 기호로 연결된 예외 클래스가 조상과 자손의 관계에 있다면 컴파일 에러가 발생한다.

```java
try {
    ...
} catch (ParentException | ChildException e) { // 에러!
    e.printStackTrace();
}
```

이유는 조상클래스만 작성하는 것과 같기 때문이며, 불필요한 코드는 제거하라는 의미에서 에러가 발생하는 것이다.

그리고 멀티 catch는 하나의 catch블럭으로 여러 예외를 처리하는 것이기 때문에, 발생한 예외를 멀티 catch블럭으로 처리하게 되었을 때, 멀티 catch블럭 내에서는 실제로 어떤 예외가 발생한 것인지 알 수 없다. 그래서 참조변수 e로 멀티 catch블럭에 '|' 기호로 연결된 예외 클래스들의 공통 분모인 조상 예외 클래스에 선언된 멤버만 사용할 수 있다.

필요하다면, 위와 같이 instanceof로 어떤 예외가 발생한 것인지 확인하고 개별적으로 처리할 수는 있다. 그러나 이렇게까지 해가면서 catch블럭을 합칠 일은 거의 없을 것이다.

마지막으로 멀티 catch블럭에 선언된 참조변수 e는 상수이므로 값을 변경할 수 없다는 제약이 있는데, 이것은 여러 catch블럭이 하나의 참조변수를 공유하기 때문에 생기는 제약으로 실제로 참조변수의 값을 바꿀 일은 없을 것이다.

<br>

## 예외 발생시키기
---
키워드 throw를 사용해서 프로그래머가 고의로 예외를 발생시킬 수 있으며, 방법은 아래의 순서를 따르면 된다.

1. 먼저, 연산자 new를 이용해서 발생시키려는 예외 클래스의 객체를 만든 다음
   Exception e = new Exception("고의로 발생시켰음");

2. 키워드 throw를 이용해서 예외를 발생시킨다.
   throw e;

```java
class ExceptionEx9 {
    public static void main(String args[]) {
        try {
            Exception e = new Exception("고의로 발생시켰음");
            throw e;    // 예외를 발생시킴
            // 위의 두줄을 아래와 같이 한 줄로 줄여 쓸 수 있음
            // throw new Exception("고의로 발생시켰음."); 
        } catch(Exception e) {
            System.out.println("에러 메시지 : " + e.getMessage());
            e.printStackTarce();
        }
        System.out.println("프로그램이 정상 종료되었음.");
    }
}
```

Exception인스턴스를 생성할 때, 생성자에 String을 넣어주면, 이 String이 Exception 인스턴스에 메시지로 저장된다. 이 메시지는 getMessage()를 이용해서 얻을 수 있다.

```java
class ExceptionEx10 {
    public static void main(String[] args) {
        throw new Exception();    // Exception을 고의로 발생시킨다.
    }
}
```

위 예제는 컴파일 시 에러가 발생하며 컴파일이 완료되지 않을 것이다. 예외처리가 되어야 할 부분에 예외처리가 되어 있지 않다는 에러이다. Exception 발생 가능성이 있는 문장들에 대해 예외처리를 해주지 않으면 컴파일 조차 되지 않는다.

```java
class ExceptionEx11 {
    public static void main(String[] args) {
        throw new RuntimeException();     // RuntimeException을 고의로 발생시킨다.
    }
}
```

위 예제는 예외처리를 하지 않았음에도 불구하고 이전의 예제와는 달리 성공적으로 컴파일된다. 그러나 실행하면, 위의 실행결과처럼 RuntimeException이 발생하여 비정상적으로 종료될 것이다.

이 예제가 명백히 RuntimeException을 발생시키는 코드를 가지고 있고, 이에 대한 예외처리를 하지 않았음에도 불구하고 성공적으로 컴파일 되었다. 앞서 말했듯이 RuntimeException은 프로그래머 실수로 인해 발생하는 것들이기 때문에 예외처리를 강제하지 않는 것이다. 만일 RuntimeException클래스들에 속하는 예외가 발생할 가능성이 있는 코드에도 예외처리를 필수로 해야 한다면, 아래와 같이 참조 변수와 배열이 사용되는 모든 곳에 예외처리를 해주어야 한다.

```java
try {
    int[] arr = new int[10];
    
    System.out.println(arr[0]);
} catch(ArrayIndexOutOfBoundsException ae) {
    ...
} catch(NullPointerException ne) {
    ...
}
```
컴파일러가 예외처리를 확인하지 않는 RuntimeException클래스들은 'unchecked예외'라고 부르며, 예외처리를 확인하는 Exception클래스들은 'checked예외'라고 부른다.

## 메서드에 예외 선언하기
---
예외를 처리하는 방법에는 지금까지 배워 온 try-catch문을 사용하는 것 외에, 예외를 메서드에 선언하는 방법이 있다. 메서드에 예외를 선언하려면, 메서드의 선언부에 키워드 throws를 사용해서 메서드 내에서 발생할 수 있는 예외를 적어주기만 하면 된다. 그리고 예외가 여러 개일 경우에는 쉼표(,)로 구분한다.

```java
void method() throws Exception1, Exception2, ... ExceptionN {
    // 메서드의 내용
}
```

만일 아래와 같이 모든 예외의 최고조상인 Exception클래스를 메서드에 선언하면, 이 메서드는 모든 종류의 예외가 발생할 가능성이 있다는 뜻이다.

```java
void method() throws Exception {
    // 메서드의 내용
}
```

이렇게 예외를 선언하면, 이 예외뿐만 아니라 그 자손타입의 예외까지도 발생할 수 있다는 점에 주의하자. 앞서 오버라이딩에서 살펴본 것과 같이, 오버라이딩할 때는 단순히 선언된 예외의 개수가 아니라 상속관계까지 고려해야 한다.

메서드의 선언부에 예외를 선언함으로써 메서드를 사용하려는 사람이 메서드의 선언부를 보았을 때, 이 메서드를 사용하기 위해서는 어떠한 예외들이 처리되어져야 하는지 쉽게 알 수 있다. 기존의 많은 언어들에서는 메서드에 예외선언을 하지 않기 때문에, 경험 많은 프로그래머가 아니고서는 어떤 상황에 어떤 종류의 예외가 발생할 가능성이 있는지 충분히 예측하기 힘들기 때문에 그에 대한 대비를 하는 것이 어려웠다.

그러나 자바에서는 메서드를 작성할 때 메서드 내에서 발생할 가능성이 있는 예외를 메서드의 선언부에 명시하여 이 메서드를 사용하는 쪽에서는 이에 대한 처리를 하도록 강요하기 때문에, 프로그래머들의 짐을 덜어 주는 것은 물론이고 보다 견고한 프로그램 코드를 작성할 수 있도록 도와준다.

<br>

## finally블럭
---
finally블럭은 예외의 발생여부에 상관없이 실행되어야할 코드를 포함시킬 목적으로 사용된다. try-catch문의 끝에 선택적으로 덧붙여 사용할 수 있으며, try-catch-finally의 순서로 구성된다.

```java
try {
    // 예외가 발생할 가능성이 있는 문장들을 넣는다.
} catch(Exception1 e1) {
    // 예외처리를 위한 문장을 적는다.
} finally {
    // 예외의 발생여부에 관계없이 항상 수행되어야하는 문장들을 넣는다.
    // finally블럭은 try-catch문의 맨 마지막에 위치해야한다.
}
```

예외가 발생한 경우에는 'try -> catch -> finally'의 순으로 실행되고, 예외가 발생하지 않은 경우에는 'try -> finally'의 순으로 실행된다.

💡 __참고__  
try블럭에 return이 존재할 경우 finally문장까지 실행한 뒤 함수를 종료한다.

## 자동 자원 반환 - try - with - resources문
---
JDK1.7부터 try-with-resources문이라는 try-catch문의 변형이 새로 추가되었다. 이 구문은 주로 입출력과 관련된 클래스를 사용할 때 유용한데, 주로 입출력에 사용되는 클래스 중에서는 사용한 후에 꼭 닫아 줘야 하는 것들이 있다. 그래야 사용했던 자원이 반환되기 때문이다.

```java
try {
    fis = new FileInputStream("score.dat");
    dis = new DataInputStream(fis);
         ...
} catch(IOException ie) {
    ie.printStackTrace();
} finally {
    dis.close();  // 작업 중에 예외가 발생하더라도, dis가 닫히도록 finally블럭에 넣음
}
```

위의 코드는 DataInputStream을 사용해서 파일로부터 데이터를 읽는 코드인데, 데이터를 읽는 도중에 예외가 발생하더라도 DataInputStream이 닫히도록 finally블럭 안에 close()를 넣었다. 여기까지는 별 문제가 없어 보이는데, 진짜 문제는 close()가 예외를 발생시킬 수 있다는데 있다. 그래서 위의 코드는 아래와 같이 해야 올바른 것이 된다.

```java
try {
    fis = new FileInputStream("score.dat");
    dis = new DataInputStream(fis);
} catch(IOException ie) {
    ie.printStackTrace();
} finally {
    try {
        if(dis != null) {
            dis.close();
        }
    } catch(IOException ie) {
        ie.printStackTrace();
    }
}
```

finally블럭 안에 try-catch문을 추가해서 close()에서 발생할 수 있는 예외를 처리하도록 변경했는데, 코드가 복잡해져서 별로 보기에 좋지 않다. 더 나쁜 것은 try블럭과 finally블럭에서 모두 예외가 발생하면, try블럭의 예외는 무시된다는 것이다. 이러한 점을 개선하기 위해서 try-with-resources문이 추가된 것이다. 위의 코드를 try-with-resources문으로 바꾸면 다음과 같다.

```java
// 괄호() 안에 두 문장 이상 넣을 경우 ';'로 구분한다.
try(FileInputStream fis = new FileInputStream("score.dat");
    DataInputStream dis = new DataInputStream(fis)) {
   while(true) {
       score = dis.readInt();
       System.out.println(score);
       sum += score;
   }
} catch(EOFException e) {
    System.out.println("점수의 총합은 " + sum + "입니다.");
} catch(IOException ie) {
    ie.printStackTrace();
}
```

try-with-resources문의 괄호()안에 객체를 생성하는 문장을 넣으면, 이 객체는 따로 close()를 호출하지 않아도 try블럭을 벗아나는 순간 자동적으로 close()가 호출된다. 그 다음에 catch블럭 또는 finally블럭이 수행된다.

💡 __참고__  
try블럭의 괄호()안에 변수를 선언하는 것도 가능하며, 선언된 변수는 try블럭 내에서만 사용할 수 있다.

이처럼 try-with-resources문에 의해 자동으로 객체의 close()가 호출될 수 있으려면, 클래스가 AutoCloseable이라는 인터페이스를 구현한 것이어야만 한다.

```java
public interface AutoCloseable {
    void close() throws Exception;
}
```

위의 인터페이스는 각 클래스에서 적절히 자원 반환작업을 하도록 구현되어 있다. 그런데, 위의 코드를 잘 보면 close()도 Exception을 발생시킬 수 있다. 만일 자동 호출된 close()에서 예외가 발생하면 어떻게 될까? close 하나만 예외가 발생한다면 일반적인 예외가 발생하였을 때와 같은형태로 에러가 출력되지만 close예외 발생 전 어떤 예외가 발생한 후 close가 발생한다면 suppressed라는 머리말과 함께 에러가 출력된다. 이때 close는 억제된 예외라고 부른다.

Throwable에는 억제된 예외와 관련된 다음과 같은 메서드가 정의되어 있다.

```java
void addSuppressed(Throwable exception) // 억제된 예외를 추가
Throwable[] getSuppressed() // 억제된 예외(배열)를 반환
```

만일 기존의 try-catch문을 사용했다면, 먼저 발생한 예외는 무시되고, 마지막으로 발생한 예외에 관한 내용만 출력이 될 것이다.

<br>

## 사용자정의 예외 만들기
---
기존의 정의된 예외 클래스 외에 필요에 따라 프로그래머가 새로운 예외 클래스를 정의하여 사용할 수 있다. 보통 Exception클래스 또는 RuntimeException클래스로부터 상속받아 클래스를 만들지만, 필요에 따라서 알맞은 예외 클래스를 선택할 수 있다.

💡 __참고__  
가능한 새로운 예외 클래스를 만들기보다 기존의 예외클래스를 활용하자.

```java
class MyException extends Exception {
    MyException(String msg) {    // 문자열을 매개변수로 받는 생성자
        super(msg);  // 조상인 Exception클래스의 생성자를 호출한다.
    }
}
```

Exception클래스로부터 상속받아서 MyException클래스를 만들었다. 필요하다면, 멤버변수나 메서드를 추가할 수 있다. Exception클래스는 생성 시에 String값을 받아서 메시지로 저장할 수 있다. 여러분이 만든 사용자정의 예외 클래스도 메시지를 저장할 수 있으려면, 위에서 보는 것과 같이 String을 매개변수로 받는 생성자를 추가해주어야 한다.

```java
class MyException extends Exception {
    // 에러 코드 값을 저장하기 위한 필드를 추가 했다.
    private final int ERR_CODE; // 생성자를 통해 초기화 한다.
    
    MyException(String msg, int errCode) {  // 생성자
        super(msg);
        ERR_CODE = errCode;
    }
    
    MyException(String msg) {  // 생성자
        this(msg, 100);        // ERR_CODE를 100(기본값)으로 초기화한다.
    }
    
    public int getErrCode() {  // 에러 코드를 얻을 수 있는 메서드도 추가했다.
        return ERR_CODE;  // 이 메서드는 주로 getMessage()와 함께 사용될 것이다.
    }
}
```

이전의 코드를 좀더 개선하여 메시지뿐만 아니라 에러코드 값도 저장할 수 있도록 ERR_CODE와 getErrCode()를 MyException클래스의 멤버로 추가했다. 이렇게 함으로써 MyException이 발생했을 때, catch블럭에서 getMessage()와 getErrCode()를 사용해서 에러코드와 메시지를 모두 얻을 수 있을 것이다.

기존의 예외 클래스는 주로 Exception을 상속받아서 'checked예외'로 작성하는 경우가 많았지만, 요즘은 예외처리를 선택적으로 할 수 있도록 RuntimeException을 상속받아서 작성하는 쪽으로 바뀌어가고 있다. 'checked예외' 는 반드시 예외처리를 해주어야 하기 때문에 예외처리가 불필요한 경우에도 try-catch문을 넣어서 코드가 복잡해지기 때문이다.

예외처리를 강제하도록 한 이유는 프로그래밍경험이 적은 사람들도 보다 견고한 프로그램을 작성할 수 있게 유도하기 위한 것이었는데, 요즘은 자바가 탄생하던 20년 전과 달리 프로그래밍 환경이 많이 달라졌다. 그 때 자바를 설계하던 사람들은 자바가 주로 소형 가전기기나, 데스크탑에서 실행될 것이라고 생각했지만 현재 자바는 모바일이나 웹 프로그래밍에서 주로 쓰이고 있다. 이처럼 프로그래밍 환경이 달라진 만큼 필수적으로 처리해야만 할 것 같았던 예외들이 선택적으로 처리해도 되는 상황으로 바뀌는 경우가 종종발생하고 있다. 그래서 필요에 따라 예외처리의 여부를 선택할 수 있는 'unchecked예외'가 강제적인 'checked예외' 보다 더 환영받고 있다.

<br>

## 예외 되던지기(exception re-throwing)
---
한 메서드에서 발생할 수 있는 예외가 여럿인 경우, 몇 개는 try-catch문을 통해서 메서드 내에서 자체적으로 처리하고, 그 나머지는 선언부에 지정하여 호출한 메서드에서 처리하도록 함으로써, 양쪽에서 나눠서 처리되도록 할 수 있다.

그리고 심지어는 단 하나의 예외에 대해서도 예외가 발생한 메서드와 호출한 메서드, 양쪽에서 처리하도록 할 수 있다. 이것은 예외를 처리한 후에 인위적으로 다시 발생시키는 방법을 통해서 가능한데, 이것을 '예외 되던지기(exception re-throwing)' 라고 한다.

먼저 예외가 발생할 가능성이 있는 메서드에서 try-catch문을 사용해서 예외를 처리해주고 catch문에서 필요한 작업을 행한 후에 throw문을 사용해서 예외를 다시 발생시킨다. 다시 발생한 예외는 이 메서드를 호출한 메서드에게 전달되고 호출한 메서드의 try-catch문에서 예외를 또 다시 처리한다. 이 방법은 하나의 예외에 대해서 예외가 발생한 메서드와 이를 호출한 메서드 양쪽 모두에서 처리해줘야 할 작업이 있을 때 사용된다. 이 때 주의할 점은 예외가 발생할 메서드에서는 try-catch문을 사용해서 예외처리를 해줌과 동시에 메서드의 선언부에 발생할 예외를 throws에 지정해줘야 한다는 것이다.

```java
class ExceptionEx17 {
    public static void main(String[] args) {
        try {
            method1();
        } catch(Exception e) {
            System.out.println("main메서드에서 예외가 처리되었습니다.");
        }
    } // main메서드 끝
    
    static void method1() throws Exception {
        try {
            throw new Exception();
        } catch(Exception e) {
            System.out.println("method1메서드에서 예외가 처리되었습니다.");
            throw e;  // 다시 예외를 발생시킨다.
        }
    } // method1메서드 끝
}
```

<br>

## 연결된 예외(chained exception)
---
한 예외가 다른 예외를 발생시킬 수도 있다. 예를 들어 예외 A가 예외 B를 발생시켰다면, A를 B의 '원인 예외(cause exception)'라고 한다. 

```java
try {
    startInstall();  // spaceException 발생
    copyFiles();
} catch(SpaceException e) {
    InstallException ie = new InstallException("설치중 예외발생"); // 예외 생성
    ie.initCause(e);  // InstallException의 원인 예외를 SpaceException으로 지정
    throw ie; // InstallException을 발생시킨다.
} catch(MemoryException me) {
    ...
}
```

먼저 InstallException을 생성한 후에, initCause()로 SpaceException을 Install Exception의 원인 예외로 등록한다. 그리고 'throw'로 이 예외를 던진다.

initCause()는 Exception클래스의 조상인 Throwable클래스에 정의되어 있기 때문에 모든 예외에서 사용가능하다.

```java
Throwable initCause(Throwable cause) // 지정한 예외를 원인 예외로 등록
Throwable getCause(); // 원인 예외를 반환
```

발생한 예외를 그냥 처리하면 될 텐데, 원인 예외로 등록해서 다시 예외를 발생시키는 이유는 여러가지 예외를 큰 분류의 예외로 묶어서 다루기 위해서이다. 그렇다고 아래와 같이 InstallException을 SpaceException과 MemoryException의 조상으로 해서 catch블럭을 작성하면, 실제로 발생한 예외가 어떤 것인지 알 수 없다는 문제가 생긴다. 그리고 SpaceException과 MemoryException의 상속관계를 변경해야 한다는 것도 부담이다.

또 다른 이유는 checked예외를 unchecked예외로 바꿀 수 있도록 하기 위해서이다. 앞서 언급하였듯이 checked예외로 예외처리를 강제한 이유는 프로그래밍 경험이 적은 사람도 보다 견고한 프로그램을 작성할 수 있도록 유도하기 위해서인데, 그래서 checked예외가 발생해도 예외를 처리할 수 없는 상황이 하나둘 발생하기 시작했다. 이럴 때 할 수 있는건 의미없는 try-catch문을 추가하는 것뿐인데, checked예외를 unchecked예외로 바꾸면 예외처리가 선택적이 되므로 억지로 예외처리를 하지 않아도 된다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  