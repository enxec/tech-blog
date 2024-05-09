---
title: "[Java] java.lang 패키지"
categories:
  - Java
tags:
  - Java
  - 자바
  - java.lang package
  - java.lang 패키지
toc: true
toc_sticky: true
toc_label: "목차"
comments: true
---

java.lang패키지는 자바프로그래밍에 가장 기본이 되는 클래스들을 포함하고 있다. 그렇기 때문에 java.lang패키지의 클래스들은 import문 없이도 사용할 수 있게 되어 있다. 그 동안 String클래스나 System클래스를 import문 없이 사용할 수 있었던 이유가 바로 java.lang패키지에 속한 클래스들이기 때문이었던 것이다. 우선 java.lang패키지의 여러 클래스들 중에서도 자주 사용되는 클래스 몇 가지만을 골라서 살펴보자.

# Object 클래스
---
클래스의 상속을 학습할 때 Object클래스에 대해서 이미 배웠지만, 여기서는 보다 자세히 알아보자. Object클래스는 모든 클래스의 최고 조상이기 때문에 Object클래스의 멤버들은 모든 클래스에서 바로 사용 가능하다.

<table border="1">
    <colgroup>
        <col style="width:40%">
        <col style="width:60%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">Object클래스의 메서드</th>
        <th style="text-align:center;">설 명</th>
    </tr>
    <tr>
        <td style="text-align:center;">protected Object clone()</td>
        <td style="text-align:left;">
            객체 자신의 복사본을 반환한다.
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">public boolean equals(Object obj)</td>
        <td style="text-align:left;">
            객체 자신과 객체 obj가 같은 객체인지 알려준다.(같으면 true)
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">protected void finalize()</td>
        <td style="text-align:left;">객체가 소멸될 때 가비지 컬렉터에 의해 자동적으로 호출된다. 이 때 수행되어야하는 코드가 있을 때 오버라이딩한다.(거의 사용안함)</td>
    </tr>
    <tr>
        <td style="text-align:center;">public Class getClass()</td>
        <td style="text-align:left;">
            객체 자신의 클래스 정보를 담고 있는 Class인스턴스를 반환한다.
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">public int hashCode()</td>
        <td style="text-align:left;">
            객체 자신의 해시코드를 반환한다.
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">public String toString()</td>
        <td style="text-align:left;">
            객체 자신의 정보를 문자열로 반환한다.
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">public void notify()</td>
        <td style="text-align:left;">
            객체 자신을 사용하려고 기다리는 쓰레드를 하나만 깨운다.
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">public void notifyAll()</td>
        <td style="text-align:left;">
            객체 자신을 사용하려고 기다리는 모든 쓰레드를 깨운다.
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">
            public void wait()<br>
            public void wait(long timeout)<br>
            public void wait(long timeout, int nanos)
        </td>
        <td style="text-align:left;">
            다른 쓰레드가 notify()나 notifyAll()을 호출할 때까지 현재 쓰레드를 무한히 또는 지정된 시간(timeout, nanos)동안 기다리게 한다.(timeout은 천 분의 1초, nanos(10의 9승 분의 1초)
        </td>
    </tr>
</table>

Object클래스는 멤버변수는 없고 오직 11개의 메서드만 가지고 있으며, 이 메서드들은 모든 인스턴스가 가져야 할 기본적인 것들이다.

<br>

# String 클래스
---
기존의 다른 언어에서는 문자열을 char형의 배열로 다루었으나 자바에서는 문자열을 위한 클래스를 제공한다. 그것이 바로 String 클래스인데, String클래스는 문자열을 저장하고 이를 다루는데 필요한 메서드를 함께 제공한다.

## 변경 불가능한 클래스
String 클래스에는 문자열을 저장하기 위해서 문자형 배열 참조변수(char[]) value를 인스턴스 변수로 정의해놓고 있다. 인스턴스 생성 시 생성자의 매개변수로 입력받는 문자열은 이 인스턴스변수(value)에 문자형 배열(char[])로 저장되는 것이다.

>💡 __참고__  
>String 클래스는 앞에 final이 붙어 있으므로 다른 클래스의 조상이 될 수 없다.

한번 생성된 String인스턴스가 갖고 있는 문자열은 읽어 올 수만 있고, 변경할 수는 없다. 예를 들어 아래의 코드와 같이 '+'연산자를 이용해서 문자열을 결합하는 경우 인스턴스 내의 문자열이 바뀌는 것이 아니라 새로운 문자열("ab")이 담긴 String인스턴스가 생성되는 것이다.

따라서 덧셈연산자'+'를 사용해서 문자열을 결합하는 것은 매 연산 시 마다 새로운 문자열을 가진 String인스턴스가 생성되어 메모리공간을 차지하게 되므로 가능한 한 결합횟수를 줄이는 것이 좋다.

문자열간의 결합이나 추출 등 문자열을 다루는 작업이 많이 필요한 경우에는 String클래스 대신 StringBuffer클래스를 사용하는 것이 좋다. StringBuffer인스턴스에 저장된 문자열은 변경이 가능하므로 하나의 StringBuffer인스턴스만으로도 문자열을 다루는 것이 가능하다.

## 문자열의 비교

문자열을 만들 때는 두 가지 방법, 문자열 리터럴을 지정하는 방법과 String클래스의 생성자를 사용해서 만드는 방법이 있다.

```java
String str1 = "abc";  // 문자열 리터럴 "abc"의 주소가 str1에 저장됨
String str2 = "abc";  // 문자열 리터럴 "abc"의 주소가 str2에 저장됨
String str3 = new String("abc");  // 새로운 String인스턴스를 생성
String str4 = new String("abc");  // 새로운 String인스턴스를 생성
```

String클래스의 생성자를 이용한 경우에는 new연산자에 의해서 메모리할당이 이루어지기 때문에 항상 새로운 String인스턴스가 생성된다. 그러나 문자열 리터럴은 이미 존재하는 것을 재사용하는 것이다.

>💡 __참고__  
>문자열 리터럴은 클래스가 메모리에 로드될 때 자동적으로 미리 생성된다.

equals()를 사용했을 때는 두 문자열의 내용("abc")을 비교하기 때문에 두 경우 모두 true를 결과로 얻는다. 하지만, 각 String인스턴스의 주소를 등가비교연산자 '=='로 비교했을 때는 결과가 다르다.

## 문자열 리터럴

자바 소스파일에 포함된 모든 문자열 리터럴은 컴파일 시에 클래스 파일에 저장된다. 이 때 같은 내용의 문자열 리터럴은 한번만 저장된다. 문자열 리터럴도 String인스턴스이고, 한번 생성하면 내용을 변경할 수 없으니 하나의 인스턴스를 공유하면 되기 때문이다.

클래스 파일에는 소스파일에 포함된 모든 리터럴의 목록이 있다. 해당 클래스 파일이 클래스 로더에 의해 메모리에 올라갈 때, 이 리터럴의 목록에 있는 리터럴들이 JVM내에 있는 '상수 저장소'에 저장된다.

## 빈 문자열

char형 배열도 길이가 0인 배열을 생성할 수 있고, 이 배열을 내부적으로 가지고 있는 문자열이 바로 빈 문자열이다. 'String s = "";과 같은 문장이 있을 때, 참조변수 s가 참조하고 있는 String인스턴스는 내부에 'new char[0]'과 같이 길이가 0인 char형 배열을 저장하고 있는 것이다.

```java
char[] chArr = new char[0];  // 길이가 0인 char배열
int[] iArr = {};  // 길이가 0인 int배열
```

길이가 0이기 때문에 아무런 문자도 저장할 수 없는 배열이라 무의미하게 느껴지겠지만 어쨌든 이러한 표현이 가능하다. 그러나 'String s = "";과 같은 표현이 가능하다고 해서 'char c ='';' 와 같은 표현도 가능한 것은 아니다. char형 변수에는 반드시 하나의 문자를 지정해야 한다.

## join()과 StringJoiner

join()은 여러 문자열 사이에 구분자를 넣어서 결합한다. 구분자로 문자열을 자르는 split()과 반대의 작업을 한다고 생각하면 이해하기 쉽다.

```java
String animals = "dog,cat,bear";
String[] arr = animals.split(",");  // 문자열을 ','를 구분자로 나눠서 배열에 저장
String str = String.join("-", arr);  // 배열의 문자열을 '-'로 구분해서 결합
System.out.println(str);  // dog-cat-bear
```

## 유니코드의 보충문자

String 클래스 메서드 중 매개변수의 타입이 char인 것들이 있고, int인 것들도 있다. 문자를 다루는 메서드라서 매개변수의 타입이 char일 것 같은데 왜 int도 있는 것일까? 그것은 확장된 유니코드를 다루기 위해서이다. 유니코드는 원래 2byte, 즉 16비트 문자체계인데, 이걸로도 모자라서 20비트로 확장하게 되었다. 그래서 하나의 문자를 char타입으로 다루지 못하고, int타입으로 다룰 수밖에 없다. 확장에 의해 새로 추가된 문자들을 '보충 문자' 라고 하는데, String클래스의 메서드 중에서는 보충 문자를 지원하는 것이 있고 지원하지 않는 것도 있다. 이들을 구별하는 방법은 쉽다. 매개변수가 'int ch'인 것들은 보충문자를 지원하는 것이고, 'char ch'인 것들은 지원하지 않는 것들이다. 보충 문자를 사용할 일은 거의 없기 때문에 이정도만 알아두자.

## 문자 인코딩 변환

getBytes(String charsetName)를 사용하면, 문자열의 문자 인코딩을 다른 인코딩으로 변경할 수 있다. 자바가 UTF-16을 사용하지만, 문자열 리터럴에 포함되는 문자들은 OS의 인코딩을 사용한다. 한글 윈도우즈의 경우 문자 인코딩으로 CP949(MS949)를 사용하며, UTF-8로 변경하려면, 아래와 같이 한다.

```java
byte[] utf8_str = "가".getBytes("UTF-8"); // 문자열을 UTF-8로 변환
String str = new String(utf8_str, "UTF-8"); // byte배열을 문자열로 변환
```

>💡 __참고__  
>사용가능한 문자 인코딩의 목록은 'System.out.println(java.nio.charset.Charset.availableCharsets());'로 모두 출력할 수 있다.

## String.format()

format()은 형식화된 문자열을 만들어내는 간단한 방법이다. printf()하고 사용법이 완전이 똑같으므로 사용하는데 별 어려움은 없을 것이다.

```java
String str = String.format("%d 더하기 %d는 %d입니다.", 3, 5, 3+5);
System.out.println(str); // 3 더하기 5는 8입니다.
```

## 기본형 값을 String으로 변환

숫자로 이루어진 문자열을 숫자로, 또는 그 반대로 변환하는 경우가 자주 있다. 이미 배운 것과 같이 기본형을 문자열로 변경하는 방법은 간단하다. 숫자에 빈 문자열 ""을 더해주기만 하면 된다. 이 외에도 valueOf()를 사용하는 방법도 있다. 성능은 valueOf()가 더 좋지만, 빈 문자열을 더하는 방법이 간단하고 편하기 때문에 성능향상이 필요한 경우에만 valueOf()를 쓰자.

```java
int i = 100;
String str1 = i + "";  // 100을 "100"으로 변환하는 방법1
String str2 = String.valueOf(i); // 100을 "100"으로 변환하는 방법 2
```

## String을 기본형 값으로 변환

반대로 String을 기본형으로 변환하는 방법도 간단하다. valueOf()를 쓰거나 앞서 배운 parseInt()를 사용하면 된다.

```java
int i = Integer.parseInf("100");  // "100"을 100으로 변환하는 방법1
int i2 = Integer.valueOf("100");  // "100"을 100으로 변환하는 방법2
```

원래 valueOf()의 반환 타입은 int가 아니라 Integer인데,  오토박싱(auto-boxing)에 의해 Integer가 int로 자동 변환된다.

<br>

# StringBuffer클래스와 StringBuilder클래스
---
String클래스는 인스턴스를 생성할 때 지정된 문자열을 변경할 수 없지만 StringBuffer클래스는 변경이 가능하다. 내부적으로 문자열 편집을 위한 버퍼(buffer)를 가지고 있으며, StringBuffer인스턴스를 생성할 때 그 크기를 지정할 수 있다. 이 때, 편집할 문자열의 길이를 고려하여 버퍼의 길이를 충분히 잡아주는 것이 좋다. 편집 중인 문자열이 버퍼의 길이를 넘어서게 되면 버퍼의 길이를 늘려주는 작업이 추가로 수행되어야하기 때문에 작업효율이 떨어진다.

StringBuffer클래스는 String클래스와 유사한 점이 많다. 아래의 코드에서 알 수 있듯이, StringBuffer클래스는 String클래스와 같이 문자열을 저장하기 위한 char형 배열의 참조변수를 인스턴스변수로 선언해 놓고 있다. StringBuffer인스턴스가 생성될 때, char형 배열이 생성되며 이 때 생성된 char형 배열을 인스턴스변수 value가 참조하게 된다.

```java
public final class StringBuffer implements java.io.Serializable {
    private char[] value;
    ...
}
```

## StringBuffer의 생성자

StringBuffer클래스의 인스턴스를 생성할 때, 적절한 길이의 char형 배열이 생성되고, 이 배열은 문자열을 저장하고 편집하기 위한 공간(buffer)으로 사용된다. StringBuffer인스턴스를 생성할 때는 생성자 StringBuffer(int length)를 사용해서 StringBuffer인스턴스에 저장될 문자열의 길이를 고려하여 충분히 여유있는 크기로 지정하는 것이 좋다. StringBuffer인스턴스를 생성할 때, 버퍼의 크기를 지정해주지 않으면 16개의 문자를 저장할 수 있는 크기의 버퍼를 생성한다.

```java
public StringBuffer(int length) {
    value = new char[length];
    shared = false;
}

public StringBuffer() {
    this(16); // 버퍼의 크기를 지정하지 않으면 버퍼의 크기는 16이 된다.
}

public StringBuffer(String str) {
    this(str.length() + 16); // 지정한 문자열의 길이보다 16이 더 크게 버퍼를 생성한다.
    append(str);
}
```

아래의 코드는 StringBuffer클래스의 일부인데, 버퍼의 크기를 변경하는 내용의 코드다. StringBuffer인스턴스로 문자열을 다루는 작업을 할 때, 버퍼의 크기가 작업하려는 문자열의 길이보다 작을 때는 내부적으로 버퍼의 크기를 증가시키는 작업이 수행된다. 배열의 길이는 변경될 수 없으므로 새로운 길이의 배열을 생성한 후에 이전 배열의 값을 복사해야 한다.

```java
// 새로운 길이(newCapacity)의 배열을 생성한다. newCapacity는 정수값이다.
char newValue[] = new char[newCapacity];

// 배열 value의 내용을 배열 newValue로 복사한다.
System.arraycopy(value, 0, newValue, 0, count); // count는 문자열의 길이
value = newValue; // 새로 생성된 배열의 주소를 참조변수 value에 저장.
```

이렇게 함으로써 StringBuffer클래스의 인스턴스변수 value는 길이가 증가된 새로운 배열을 참조하게 된다.

## StringBuilder란?

StringBuffer는 멀티쓰레드에 안전(thread safe)하도록 동기화되어 있다. 동기화는 StringBuffer의 성능을 떨어뜨리며, 멀티쓰레드로 작성된 프로그램이 아닌 경우, StringBuffer의 동기화는 불필요하게 성능만 떨어뜨리게 된다. 때문에 StringBuffer에서 쓰레드의 동기화만 뺀 StringBuilder가 새로 추가되었다. StringBuilder는 StringBuffer와 완전히 똑같은 기능으로 작성되어 있어서, 소스코드에서 StringBuffer대신 StringBuilder를 사용하도록 바꾸기만 하면 된다. StringBuffer도 충분히 성능이 좋기 때문에 성능향상이 반드시 필요한 경우를 제외하고는 기존에 작성한 코드에서 StringBuffer를 StringBuilder로 굳이 바꿀 필요는 없다.

<br>

# StrictMath 클래스
---
Math클래스는 최대한의 성능을 얻기 위해 JVM이 설치된 OS의 메서드를 호출해서 사용한다. 즉, OS에 의존적인 계산을 하고 있는 것이다. 예를 들어 부동소수점 계산의 경우, 반올림의 처리방법 설정이 OS마다 다를 수 있기 때문에 자바로 작성된 프로그램임에도 불구하고 컴퓨터마다 결과가 다를 수 있다. 이러한 차이를 업애기 위해 성능은 다소 포기하는 대신, 어떤 OS에서 실행되어도 항상 같은 결과를 얻도록 Math클래스를 새로 작성한 것이 StrictMath클래스이다.

<br>

# 래퍼 클래스
---
객체지향 개념에서 모든 것은 객체로 다루어져야 한다. 그러나 자바에서는 8개의 기본형을 객체로 다루지 않는데 이것이 바로 자바가 완전한 객체지향 언어가 아니라는 얘기를 듣는 이유이다. 그 대신 보다 높은 성능을 얻을 수 있었다. 

때로는 기본형 변수도 어쩔 수 없이 객체로 다뤄야 하는 경우가 있다. 예를 들면, 매개변수로 객체를 요구할 때, 기본형 값이 아닌 객체로 저장해야할 때, 객체간의 비교가 필요할 때 등등의 경우에는 기본형 값들을 객체로 변환하여 작업을 수행해야한다.

이 때 사용되는 것이 래퍼 클래스이다. 8개의 기본형을 대표하는 8개의 래퍼클래스가 있는데, 이 클래스들을 이용하면 기본형 값을 객체로 다룰 수 있다. char형과 int형을 제외한 나머지는 자료형 이름의 첫 글자를 대문자로 한 것이 각 래퍼 클래스의 이름이다.

래퍼 클래스의 생성자는 매개변수로 문자열이나 각 자료형의 값을을 인자로 받는다. 이 때 주의해야할 것은 생성자의 매개변수로 문자열을 제공할 때, 각 자료형에 알맞는 문자열을 사용해야한다는 것이다. 예를 들어 'new Integer("1.0");'과 같이 하면 Number FormatException이 발생한다. 아래의 코드는 int형의 래퍼 클래스인 Integer클래스의 실제코드이다.

```java
public final class Integer extends Number implements Comparable {
    ...
    private int value;
    ...
}
```

이처럼 래퍼 클래스들은 객체생성 시에 생성자의 인자로 주어진 각 자료형에 알맞은 값을 내부적으로 저장하고 있으며, 이에 관련된 여러 메서드가 정의되어있다.

래퍼 클래스들은 모두 equals()가 오버라이딩되어 있어서 주소값이 아닌 객체가 가지고 있는 값을 비교한다. 실행결과를 보면 equals()를 이용한 두 Integer객체의 비교 결과가 true라는 것을 알 수 있다. 오토박싱이 된다고 해도 Integer객체에 비교연산자를 사용할 수 없다. 대신 compareTo()를 제공한다.

그리고 toString()도 오버라이딩되어 있어서 객체가 가지고 있는 값을 문자열로 변환하여 반환한다. 이 외에도 래퍼 클래스들은 MAX_VALUE, MIN_VALUE, SIZE, BYTES, TYPE 등의 static상수를 공통적으로 가지고 있다.

<br>

# Number클래스
---
Number클래스는 추상클래스로 내부적으로 숫자를 멤버변수로 갖는 래퍼 클래스들의 조상이다. 아래는 래퍼 클래스의 상속계층도이다.

![래퍼클래스 상속계층도](/assets/img/posts/20240314/wrapper-class-Inheritance-hierarchy.png "래퍼클래스 상속계층도"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">래퍼클래스 상속계층도</div>

BigInteger은 long으로도 다룰 수 없는 큰 범위의 정수를, BigDecimal은 double로도 다룰 수 없는 큰 범위의 부동 소수점수를 처리하기 위한 것으로 연산자의 역할을 대신하는 다양한 메서드를 제공한다.

<br>

# 문자열을 숫자로 변환하기
---
문자열을 숫자로 변환할 때는 아래의 방법 중 하나를 선택하여 사용하면 된다.

```java
int i = new Integer("100").intValue();

int i2 = Integer.parseInt("100");    // 주로 문자열을 기본형으로 바꿀 때 사용

Integer i3 = Integer.valueOf("100"); // 주로 문자열을 래퍼 클래스로 바꿀 때 사용
```

JDK1.5부터 도입된 오토박싱 기능이 있는데, 반환값이 기본형일 때와 래퍼 클래스일 때의 차이가 없어져 이제는 구분없이 valueOf()를 사용해도 된다. 단 성능은 valueOf()가 조금 더 느리다.

<br>

# 오토박싱과 언박싱
---
JDK1.5 이전에는 기본형과 참조형 간의 연산이 불가능했기 때문에, 래퍼 클래스로 기본형을 객체로 만들어서 연산해야 했다. 그러나 이제는 기본형과 참조형 간의 덧셈이 가능하다. 자바 언어의 규칙이 바뀐 것은 아니고, 컴파일러가 자동으로 변환하는 코드를 넣어주기 때문이다. 

```java
// 컴파일 전의 코드
int i = 5;
Integer iObj = new Integer(7);

int sum = i + iObj;

// 컴파일 후의 코드
int i = 5;
Integer iObj = new Integer(7);

int sum = i + iObj.intValue();
```

이 외에도 내부적으로 객체 배열을 가지고 있는 Vector클래스나 ArrayList클래스에 기본형 값을 저장해야할 때나 형변환이 필요할 때도 컴파일러가 자동적으로 코드를 추가해 준다. 이렇게 기본형 값을 래퍼 클래스의 객체로 자동 변환해주는 것을 오토박싱이라고하고 반대로 현한하는 것을 언박싱이라고 한다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  