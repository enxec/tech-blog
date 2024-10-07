---
title: "[Java] Annotation"
description: 
author: 이원모
date: 2024-10-07
categories: [Language, Java]
tags: [annotation, 애너테이션, java, 자바]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/java-logo.png
  lqip: 
  alt: 
---

## Annotation이란?
---
자바를 개발한 사람들은 소스코드에 대한 문서를 따로 만들기보다 소스코드와 문서를 하나의 파일로 관리하는 것이 낫다고 생각했습니다. 그래서 소스코드의 주석 '/** ~ */' 에 소스코드에 대한 정보를 저장하고, 소스코드의 주석으로부터 HTML문서를 생성해내는 프로그램(javadoc.exe)을 만들어서 사용했습니다. 다음은 모든 Annotation의 조상인 Annotation 인터페이스의 소스코드 일부입니다.

```java
/**
 * ...
 * @author Josh Bloch
 * @since 1.5
 */
public interface Annotation {
    ...
}
```

/**로 시작하는 주석 안에 소스코드에 대한 설명들이 있고, 그 안에 '@'이 붙은 태그들이 있습니다. 미리 정의된 태그들을 이용해서 주석 안에 정보를 저장하고, javadoc.exe라는 프로그램이 이 정보를 읽어서 문서를 작성하는데 사용합니다. 이 기능을 응용하여, 프로그램의 소스코드 안에 다른 프로그램을 위한 정보를 미리 약속된 형식으로 포함시킨 것이 바로 애너테이션입니다. 애너테이션은 주석처럼 프로그래밍 언어에 영향을 미치지 않으면서도 다른 프로그램에게 유용한 정보를 제공할 수 있다는 장점이 있습니다.

예를 들어, 자신이 작성한 소스코드 중에서 특정 메서드만 테스트하기를 원한다면, 다음과 같이 '@Test'라는 애너테이션을 메서드 앞에 붙인다. '@Test'는 '이 메서드를 테스트해야 한다'는 것을 테스트 프로그램에게 알리는 역할을 할 뿐, 메서드가 포함된 프로그램 자체에는 아무런 영향을 미치지 않습니다. 주석처럼 존재하지 않는 것이나 다름없는 것입니다.

```java
@Test
public void method() {
    ...
}
```

애너테이션은 JDK에서 기본적으로 제공하는 것과 다른 프로그램에서 제공하는 것들이 있는데, 어느 것이든 그저 약속된 형식으로 정보를 제공하기만 하면 될 뿐입니다. JDK에서 제공하는 표준 애너테이션은 주로 컴파일러를 위한 것으로 컴파일러에게 유용한 정보를 제공합니다. 그리고 새로운 에너테이션을 정의할 때 사용하는 메타 애너테이션을 제공합니다.

<br>

## 표준 Annotation
---
자바에서 기본적으로 제공하는 애너테이션들은 몇개 없습니다. 그나마 이들의 일부는 '메타 애너테이션'으로 애너테이션을 정의하는데 사용되는 애너테이션의 애너테이션입니다.

<table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
        <col style="width:40%;">
        <col style="width:60%;">
    </colgroup>
    <thead>
        <tr style="background-color:darkslategray;">
            <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">Annotation</th>
            <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                @Override
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                컴파일러에게 오버라이딩하는 메서드라는 것을 알립니다.
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                @Deprecated
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                앞으로 사용하지 않을 것을 권장하는 대상에 붙입니다.
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                @SuppressWarnings
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                컴파일러의 특정 경고메시지가 나타나지 않게 해줍니다.
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                @SafeVarargs
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                제네릭 타입의 가변인자에 사용합니다. (JDK1.7)
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                @FunctionalInterface
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                함수형 인터페이스라는 것을 알립니다. (JDK1.8)
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                @Native
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                native메서드에서 참조되는 상수 앞에 붙인다.(JDK1.8)
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                @Target*
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                애너테이션이 적용가능한 대상을 지정하는데 사용합니다.
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                @Documented*
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                애너테이션 정보가 javadoc으로 작성된 문서에 포함되게 합니다.
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                @Inherited*
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                애너테이션이 자손 클래스에 상속되도록 합니다.
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                @Retention*
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                애너테이션이 유지되는 범위를 지정하는데 사용합니다.
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                @Repeatable*
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                애너테이션을 반복해서 적용할 수 있게 합니다.(JDK1.8)
            </td>
        </tr>
    </tbody>
</table>

<p align="center">▲ 자바에서 기본적으로 제공하는 표준 애너테이션(*가 붙은 것은 메타 애너테이션)</p>

#### @Override

메서드 앞에만 붙일 수 있는 애너테이션으로, 조상의 메서드를 오버라이딩하는 것이라는걸 컴파일러에게 알려주는 역할을 합니다. 아래의 코드에서와 같이 오버라이딩할 때 조상 메서드의 이름을 잘못 써도 컴파일러는 이것이 잘못된 것인지 알지 못합니다.

```java
class Parent {
    void parentMethod() {

    }
}

class Child extends Parend {
    // 오버라이딩하려 했으나 잘못된 함수명 작성
    void parentmethod() {

    }
}
```

오버라이딩할 때는 이처럼 메서드의 이름을 잘못 적는 경우가 많은데, 컴파일러는 그저 새로운 이름의 메서드가 추가된 것으로 인식할 뿐입니다. 게다가 실행 시에도 오류가 발생하지 않고 조상의 메서드가 호출되므로 어디서 잘못되었는지 알아내기 어렵습니다.

```java
class Child extends Parent {
    void parentmethod() {

    }
}

            ⬇️

class Child extends Parent {
    @Override
    void parentmethod() {

    }
}
```

그러나 아래의 코드와 같이 메서드 앞에 '@Overrid'라고 애너테이션을 붙이면, 컴파일러가 같은 이름의 메서드가 조상에 있는지 확인하고 없으면, 에러메시지를 출력합니다. 오버라이딩할 때 메서드 앞에 '@Override'를 붙이는 것이 필수는 아니지만, 알아내기 어려운 실수를 미연에 방지해주므로 반드시 붙이는 것이 좋습니다.

#### @Deprecated

새로운 버전의 JDK가 소개될 때, 새로운 기능이 추가될 뿐만 아니라 기존의 부족했던 기능들을 개선하기도 합니다. 이 과정에서 기존의 기능을 대체할 것들이 추가되어도, 이미 여러 곳에서 사용되고 있을지 모르는 기존의 것들을 함부로 삭제할 수 없습니다.

그래서 생각해낸 방법이 더 이상 사용되지 않는 필드나 메서드에 '@Deprecated'를 붙이는 것입니다. 이 애너테이션이 붙은 대상은 다른 것으로 대체되었으니 더 이상 사용하지 않을 것을 권한다는 의미입니다. 예를 들어 java.util.Date클래스의 대부분의 메서드에는 '@Deprecated'가 붙어있는데, Java API에서 Date클래스의 getDate()를 보면 아래와 같이 적혀있습니다.

```java
int getDate()
    Deprecated.
    As of JDK version 1.1, replaced by Calendar.get(Calendar.DAY_OF_MONTH).
```

이 메서드 대신에 JDK1.1부터 추가된 Calendar클래스의 get()을 사용하라는 뜻입니다. 기존 메서드 대신 새로 추가된 개선된 기능을 사용하도록 유도하는 것입니다.

```text
Note: AnnotationEx2.java uses or overrides a deprecated API.
Note: Recompile with -Xlint:deprecation for details.
```

해당 소스파일이 'deprecated'된 대상을 사용하고 있으며, '-Xlint:deprecation'옵션을 붙여서 다시 컴파일하면 자세한 내용을 알 수 있다는 뜻입니다.

```java
class NewClass {
    int newField;

    int getNewField() {
        return newField;
    }

    @Deprecated
    int oldField;

    @Deprecated
    int getOldField() {
        return oldField;
    }
}

class AnnotationEx2 {
    public static void main(String args[]) {
        NewClass nc = new NewClass();

        nc.oldField = 10;                       // @Deprecated가 붙은 대상을 사용
        System.out.println(nc.getOldField());   // @Deprecated가 붙은 대상을 사용
    }
}
```

이 예제는 '@Deprecated'가 붙은 대상을 사용해서 고의적으로 위의 에러메시지가 나타나도록 했지만, 컴파일도 실행도 잘됩니다. '@Deprecated'가 붙은 대상을 사용하지 않도록 권할 뿐 강제성은 없기 때문입니다.

#### @FunctionalInterface

'함수형 인터페이스'를 선언할 때, 이 애너테이션을 붙이면 컴파일러가 '함수형 인터페이스'를 올바르게 선언했는지 확인하고, 잘못된 경우 에러를 발생시킵니다. 필수는 아니지만, 붙이면 실수를 방지할 수 있으므로 '함수형 인터페이스'를 선언할 때는 이 애너테이션을 반드시 붙이는 것이 좋습니다.

>💡 __참고__  
> 함수형 인터페이스는 추상 메서드가 하나뿐이어야한다는 제약이 있습니다.

```java
@FunctionalInterface
public interface Runnable {
    public abstract void run(); // 추상 메서드
}
```

#### @SuppressWarnings

컴파일러가 보여주는 경고메세지가 나타나지 않게 억제해줍니다. 이전 예제에서처럼 컴파일러의 경고메세지는 무시하고 넘어갈 수도 있지만, 모두 확인하고 해결해서 컴파일 후에 어떠한 메세지도 나타나지 않게 해야합니다. 그러나 경우에 따라서는 경고가 발생할 것을 알면서도 묵인해야 할 때가 있는데, 이 경고를 그대로 놔두면 컴파일할 때마다 메세지가 나타납니다. 이전 예제에서 확인한 것과 같이 '-Xlint' 옵션을 붙이지 않으면 컴파일러는 경고의 자세한 내용은 보여주지 않으므로 다른 경고들을 놓치기 쉽습니다. 이 때는 묵인해야하는 경고가 발생하는 대상에 반드시 '@SuppressWarnings'를 붙여서 컴파일 후에 어떤 경고 메세지도 나타나지 않게 해야합니다. '@SuppressWarnings'로 억제할 수 있는 경고 메세지의 종류는 여러 가지가 있는데, JDK의 버전이 올라가면서 앞으로도 계속 추가될 것입니다. 이 중에서 주로 사용되는 것은 "deprecation", "unchecked", 
"rawtypes", "varargs" 정도입니다.

"desprecation"은 앞서 살펴본것과 같이 '@Deprecated'가 붙은 대상을 사용해서 발생하는 경고를, "unchecked"는 제네릭으로 타입을 지정하지 않았을 때 발생하는 경고를, "rawtypes"는 제네릭을 사용하지 않아서 발생하는 경고를, "varargs"는 가변인자의 타입이 제네릭타입일 때 발생하는 경고를 억제할 때 사용합니다. 억제하려는 경고 메세지를 애너테이션의 뒤에 괄호()안에 문자열로 지정하면 됩니다.

```java
@SuppressWarnings("unchecked")      // 제네릭과 관련된 경고를 억제
ArrayList list = new ArrayList();   // 제네릭타입을 지정하지 않았음
list.add(obj);                      // 여기서 경고가 발생
```

만일 둘 이상의 경고를 동시에 억제하려면 다음과 같이 해야합니다. 배열에서처럼 괄호{}를 추가로 사용해야한다는 것에 주의합니다.

```java
@SuppressWarnings({"deprecation", "unchecked", "varargs"})
```

'@SuppressWarnings'로 억제할 수 있는 경고 메세지의 종류는 JDK의 버전이 올라가면서 계속 추가될 것이기 때문에, 이전 버전에서는 발생하지 않던 경고가 새로운 버전에서는 발생할 수 있습니다. 새로 추가된 경고 메세지를 억제하려면, 경고 메세지의 종류를 알아야하는데, -Xlint옵션으로 컴파일해서 나타나는 경고의 내용 중에서 대괄호[]안에 있는 것이 바로 메세지의 종류입니다.

#### @SafeVarargs

메서드에 선언된 가변인자의 타입이 non-reifiable타입일 경우, 해당 메서드를 선언하는 부분과 호출하는 부분에서 "unchecked"경고가 발생합니다. 해당 코드에 문제가 없다면 이 경고를 억제하기 위해 '@SafeVarargs'를 사용해야 합니다.

이 애너테이션은 static이나 final이 붙은 메서드와 생성자에만 붙일 수 있습니다. 즉, 오버라이드될 수 있는 메서드에는 사용할 수 없다는 뜻입니다. 제네릭에서 살펴본 것과 같이 어떤 타입들은 컴파일 이후에 제거됩니다. 컴파일 후에도 제거되지 않는 타입을 reifiable타입이라고 하고, 제거되는 타입을 non-reifiable타입이라고 합니다. 제네릭 타입들은 대부분 컴파일 시에 제거되므로 non-reifiable타입입니다.

예를 들어, java.util.Arrays의 asList()는 다음과 같이 정의되어 있으며, 이 메서드는 매개변수로 넘겨받은 값들로 배열을 만들어 새로운 ArrayList객체를 만들어서 반환하는데 이 과정에서 경고가 발생합니다.

```java
public static <T> List<T> asList(T... a) {
    return new ArrayList<T>(a); // ArrayList(E[] array)를 호출. 경고발생
}
```

asList()의 매개변수가 가변인자인 동시에 제네릭 타입입니다. 메서드에 선언된 타입 T는 컴파일 과정에서 Object로 바뀝니다. 즉, Object[]가 되는 것입니다. Object[]에는 모든 타입의 객체가 들어있을 수 있으므로, 이 배열로 ArrayList<T>를 생성하는 것은 위험하다고 경고하는 것입니다. 그러나 asList()가 호출되는 부분을 컴파일러가 체크해서 타입 T가 아닌 다른 타입이 들어가지 못하게 할 것이므로 위의 코드는 아무런 문제가 없습니다.  
이럴 때는 메서드 앞에 '@SafeVarargs'를 붙여서 '이 메서드의 가변인자는 타입 안정성이 있다.'고 컴파일러에게 알려 경고가 발생하지 않도록 해야합니다.

메서드를 선언할 때 @SafeVarargs를 붙이면, 이 메서드를 호출하는 곳에서 발생하는 경고도 억제됩니다. 반면에 @SafeVarargs대신, @SuppressWarnings("unchecked")로 경고를 억제하려면, 메서드 선언뿐만 아니라 메서드가 호출되는 곳에도 애너테이션을 붙여야합니다. 그리고 @SafeVarargs로 'unchecked'경고는 억제할 수 있지만, 'varargs'경고는 억제할 수 없기 때문에 습관적으로 @SafeVarargs와 @SuppressWarnings("varargs")를 같이 붙입니다.

```java
@SafeVarargs                    // 'unchecked'경고를 억제한다.
@SuppressWarnings("varargs")    // 'varargs'경고를 억제한다.
public static <T> List<T> asList(T... a) {
    return new ArrayList<>(a);
}
```

@SuppressWarnings("varargs")를  붙이지 않아도 경고 없이 컴파일 됩니다. 그러나 -Xlint옵션을 붙여서 컴파일 해보면, 'varargs'경고가 발생한 것을 확인할 수 있습니다. 그래서 가능하면 이 두 애너테이션을 항상 같이 사용하는 것이 좋습니다.

<br>

## 메타 애너테이션
---
앞서 설명한 것과 같이 메타 애너테이션은 '애너테이션을 위한 애너테이션', 즉 애너테이션에 붙이는 애너테이션으로 애너테이션을 정의할 때 애너테이션의 적용대상(target)이나 유지기간등을 지정하는데 사용됩니다.

#### @Target

애너테이션이 적용가능한 대상을 지정하는데 사용됩니다. 아래는 '@SuppressWarnings'를 정의한 것인데, 이 애너테이션에 적용할 수 있는 대상을 '@Target'으로 지정하였습니다. 앞서 언급한 것과 같이 여러 개의 값을 지정할 때는 배열에서처럼 괄호{}를 사용해야합니다.

```java
@Target({TYPE, FIELD, METHOD, PARAMETER, CONSTRUCTOR, LOCAL_VARIABLE})
@Retention(RetentionPolicy.SOURCE)
public @interface SuppressWarnings {
    String[] value();
}
```

'@Target'으로 지정할 수 있는 애너테이션 적용대상의 종류는 아래와 같습니다.

<table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
        <col style="width:40%;">
        <col style="width:60%;">
    </colgroup>
    <thead>
        <tr style="background-color:darkslategray;">
            <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">대상 타입</th>
            <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">의미</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                ANNOTATION_TYPE
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                애너테이션
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                CONSTRUCTOR
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                생성자
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                FIELD
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                필드(멤버변수, enum상수)
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                LOCAL_VARIABLE
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                지역변수
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                METHOD
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                메서드
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                PACKAGE
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                패키지
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                PARAMETER
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                매개변수
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                TYPE
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                타입(클래스, 인터페이스, enum)
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                TYPE_PARAMETER
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                타입 매개변수(JDK1.8)
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                TYPE_USE
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                타입이 사용되는 모든 곳(JDK1.8)
            </td>
        </tr>
    </tbody>
</table>

<p align="center">▲ 애너테이션 적용대상의 종류</p>

'TYPE'은 타입을 선언할 때, 애너테이션을 붙일 수 있다는 뜻이고 'TYPE_USE'는 해당 타입의 변수를 선언할 때 붙일 수 있다는 뜻입니다. 위 값들은 'java.lang.annotation.ElementType'이라는 열거형에 정의되어 있으며, 아래와 같이 static import문을 쓰면 'ElementType.TYPE'을 'TYPE'과 같이 간단히 할 수 있습니다.

```java
import static java.lang.annotation.ElementType.*;

@Target({FIELD, TYPE, TYPE_USE})    // 적용대상이 FIELD, TYPE, TYPE_USE
public @interface MyAnnotation {

}

@MyAnnotation       // 적용대상이 TYPE인 경우
class MyClass {
    @MyAnnotation   // 적용대상이 FIELD인 경우
    int i;

    @MyAnnotation   // 적용대상이 TYPE_USE인 경우
    MyClass mc;
}
```

'FIELD'는 기본형에, 그리고 'TYPE_USE'는 참조형에 사용된다는 점에 주의해야합니다.

#### @Retention

애너테이션이 유지되는 기간을 지정하는데 사용됩니다. 애너테이션의 유지 정책의 종류는 다음과 같습니다.

<table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
        <col style="width:40%;">
        <col style="width:60%;">
    </colgroup>
    <thead>
        <tr style="background-color:darkslategray;">
            <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">유지 정책</th>
            <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">의미</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                SOURCE
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                소스 파일에만 존재. 클래스파일에는 존재하지 않음
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                CLASS
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                클래스 파일에 존재. 실행시에 사용불가. 기본값
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                RUNTIME
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                클래스 파일에 존재. 실행시에 사용가능.
            </td>
        </tr>
    </tbody>
</table>

<p align="center">▲ 애너테이션 유지정책의 종류</p>

'@Override'나 '@SuppressWarnings'처럼 컴파일러가 사용하는 애너테이션은 유지 정책이 'SOURCE'입니다. 컴파일러를 직접 작성할 것이 아니면, 이 유지정책은 필요없습니다.

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {

}
```

유지 정책을 'RUNTIME'으로 하면, 실행 시에 '리플렉션'을 통해 클래스 파일에 저장된 애너테이션의 정보를 읽어서 처리할 수 있다. '@FunctionalInterface'는 '@Override'처럼 컴파일러가 체크해주는 애너테이션이지만, 실행 시에도 사용되므로 유지 정책이 'RUNTIME'으로 되어 있습니다.

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface FunctionalInterface {

}
```

유지 정책 'CLASS'는 컴파일러가 애너테이션의 정보를 클래스 파일에 저장할 수 있게는 하지만, 클래스 파일이 JVM에 로딩될 때는 애너테이션의 정보가 무시되어 실행 시에 애너테이션에 대한 정보를 얻을 수 없습니다. 이것이 'CLASS'가 유지정책의 기본값임에도 불구하고 잘 사용되지 않는 이유입니다.

#### @Documented

애너테이션에 대한 정보가 javadoc으로 작성한 문서에 포함되도록 합니다. 자바에서 제공하는 기본 애너테이션 중에 '@Override'와 '@SuppressWarnings'를 제외하고는 모두 '@Documented' 메타 애너테이션이 붙어 있습니다.

```java
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
public @interface FunctionalInterface {

}
```

#### @Inherited

애너테이션이 자손 클래스에 상속되도록 합니다. '@Inherited'가 붙은 애너테이션을 조상 클래스에 붙이면, 자손 클래스도 이 애너테이션이 붙은 것과 같이 인식됩니다.

```java
@Inherited                      // @SupperAnno가 자손까지 영향 미치게
@interface SupperAnno {

}

@SuperAnno
class Parent {

}

class Child extends Parent {    // Child에 애너테이션이 붙은 것으로 인식

}
```

위의 코드에서 Child클래스는 애너테이션이 붙지 않았지만, 조상인 Parent클래스에 붙은 '@SuperAnno'가 상속되어 Child클래스에도 '@SuperAnno'가 붙은 것처럼 인식됩니다.

#### @Repeatable

보통은 하나의 대상에 한 종류의 애너테이션을 붙이는데, '@Repetable'이 붙은 애너테이션은 여러 번 붙일 수 있습니다.

```java
@Repeatable(Todos.class)    // ToDo애너테이션을 여러 번 반복해서 쓸 수 있게 한다.
@interface ToDo {
    String value();
}
```

예를 들어 '@ToDo'라는 애너테이션이 위와 같이 정의되어 있을 때, 다음과 같이 MyClass클래스에 '@ToDo'를 여러 번 붙이는 것이 가능합니다.

```java
@ToDo("delete test codes.")
@ToDo("override inherited methods")
class MyClass {
    ...
}
```

일반적인 애너테이션과 달리 같은 이름의 애너테이션이 여러 개가 하나의 대상에 적용될 수 있기 때문에, 이 애너테이션들을 하나로 묶어서 다룰 수 있는 애너테이션도 추가로 정의해야 합니다.

```java
@interface ToDos {  // 여러개의 ToDo애너테이션을 담을 컨테이너 애너테이션 ToDos
    ToDo[] value(); // ToDo애너테이션 배열타입의 요소를 선언. 이름이 반드시 value이어야 함
}

@Repeatable(ToDos.class)    // 괄호 안에 컨테이너 애너테이션을 지정해 줘야한다.
@interface ToDo {
    String value();
}
```

#### @Native

네이티브 메서드(native method)에 의해 참조되는 '상수 필드'에 붙이는 애너테이션이다. 아래는 java.lang.Long클래스에 정의된 상수입니다.

```java
@Native public static final long MIN_VALUE = 0x8000000000000000L;
```

네이티브 메서드는 JVM이 설치된 OS의 메서드를 말합니다. 네이티브 메서드는 보통 C언어로 작성되어 있는데, 자바에서는 메서드의 선언부만 정의하고 구현은 하지 않습니다. 그래서 추상 메서드처럼 선언부만 있고 몸통이 없습니다.

```java
public class Object {
    private static native void registerNatives();   // 네이티브 메서드
    
    static {
        registerNatives();  // 네이티브 메서드를 호출
    }

    protected native Object clone() throws CloneNotSupportedException;
    public final native Class<?> getClass();
    public final native void notify();
    public final native void notifyAll();
    public final native void wait(long timeout) throws InterruptedException;
    public native int hashCode();
    ...
}
```

이처럼 모든 클래스의 조상인 Object클래스의 메서드들은 대부분 네이티브 메서드입니다. 네이티브 메서드는 자바로 정의되어 있기 때문에 호출하는 방법은 자바의 일반 메서드와 다르지 않지만 실제로 호출되는 것은 OS의 메서드입니다.
그냥 아무런 내용도 없는 네이티브 메서드를 선언해 놓고 호출한다고 되는 것은 아니고, 자바에 정의된 네이티브 메서드와 OS의 메서드를 연결해주는 작업이 추가로 필요합니다. (JNI가 수행)

<br>

## 애너테이션 타입 정의하기
---
새로운 애너테이션을 정의하는 방법은 아래와 같습니다. '@'기호를 붙이는 것을 제외하면 인터페이스를 정의하는 것과 동일합니다.

```java
@interface 애너테이션이름 {
    타입 요소이름();    // 애너테이션의 요소를 선언한다.
    ...
}
```

엄밀히 말해서 '@Override'는 애너테이션이고 'Override'는 애너테이션의 타입'입니다.

#### 애너테이션의 요소

애너테이션 내에 선언된 메서드를 '애너테이션의 요소'라고 하며, 아래에 선언된 TestInfo애너테이션은 다섯개의 요소를 가집니다.

```java
@interface TestInfo {
    int count();
    String testBy();
    String[] testTools();
    TestType testType();    // enum TestType {FIRST, FINAL}
    DateTime testDate();    // 자신이 아닌 다른 애너테이션 {@DateTime}을 포함할 수 있다.
}

@interface DateTime {
    String yymmdd();
    String hhmmss();
}
```

애너테이션의 요소는 반환값이 있고 매개변수는 없는 추상 메서드의 형태를 가지며, 상속을 통해 구현하지 않아도 됩니다. 다만, 애너테이션을 적용할 때 이 요소들의 값을 빠짐없이 지정해주어야합니다. 요소의 이름도 같이 적어주므로 순서는 상관없습니다.

```java
@TestInfo (
    count = 3, testedBy = "KIM",
    testTools = {"JUnit", "AutoTester"},
    testType = TestType.FIRST,
    testDate = @DateTime(yymmdd = "160101", hhmmss = "235959")
)
public class NewClass {
    ...
}
```

애너테이션의 각 요소는 기본값을 가질 수 있으며, 기본값이 있는 요소는 애너테이션을 적용할 때 값을 지정하지 않으면 기본값이 사용됩니다.

```java
@interface TestInfo {
    int count() default 1;  // 기본값을 1로 지정
}

@TestInfo   // @TestInfo(count = 1)과 동일
public class NewClass {
    ...
}
```

애너테이션 요소가 오직 하나뿐이고 이름이 value인 경우, 애너테이션을 적용할 때 요소의 이름을 생략하고 값만 작성해도 됩니다.

```java
@interface TestInfo {
    String value();
}

@TestInfo("passed") // @TestInfo(value = "passed")와 동일
class NewClass {
    ...
}
```

요소의 타입이 배열인 경우, 괄호 {}를 사용하여 여러 개의 값을 지정할 수 있습니다.

```java
@interface TestInfo {
    String[] testTools();
}

@Test(testTools = {"JUnit", "AutoTester"})  // 값이 여러개인 경우
@Test(testTools = "JUnit")                  // 값이 하나일 때는 괄호{} 생략가능
@Test(testTools = {})                       // 값이 없을 때는 괄호{}가 반드시 필요
```

기본값을 지정할 떄도 마찬가지로 괄호{}를 사용할 수 있습니다.

```java
@interface TestInfo {
    String[] info() default {"aaa", "bbb"}; // 기본값이 여러개인 경우. 괄호{}사용
    String[] info2() default "ccc";         // 기본값이 하나인 경우. 괄호 생략가능
}

@TestInfo               // @TestInfo(info = {"aaa", "bbb"}, info2 = "ccc")
@TestInfo(info2 = {})   // @TestInfo(info = {"aaa", "bbb"}, info2 = {})
class NewClass {
    ...
}
```

요소의 타입이 배열일 때도 요소의 이름이 value이면, 요소의 이름을 생략할 수 있습니다. 예를 들어, '@SuppressWarnings'의 경우, 요소의 타입이 String배열이고 이름이 value입니다.

```java
@interface SuppressWarnings {
    String[] value();
}
```

그래서 애너테이션을 적용할 때 요소의 이름을 생략할 수 있는 것입니다.

```java
// @SuppressWarnings(value = {"deprecation", "unchecked"})
   @SuppressWarnings({"deprecation", "unchecked"})
   class NewClass {
    ...
   }
```

#### java.lang.annotation.Annotation

모든 애너테이션의 조상은 Annotation입니다. 그러나 애너테이션은 상속이 허용되지 않으므로 아래와 같이 명시적으로 Annotation을 조상으로 지정할 수 없습니다.

```java
@interface TestInfo extends Annotation {    // 에러. 허용되지 않는 표현
    int count();
    String testedBy();
    ...
}
```

게다가 아래의 소스에서 볼 수 있듯이 Annotation은 애너테이션이 아니라 일반적인 인터페이스로 정의되어 있습니다.

```java
package java.lang.annotation;

public interface Annotation {   // Annotation자신은 인터페이스이다.
    boolean equals(Object obj);
    int hashCode();
    String toString();

    Class<? extends Annotation> annotationType();   // 애너테이션의 타입을 반환
}
```

모든 애너테이션의 조상인 Annotation인터페이스가 위와 같이 정의되어 있기 때문에, 모든 애너테이션 객체에 대해 equals(), hashCode(), toString()과 같은 메서드를 호출하는 것이 가능합니다.

```java
Class<AnnotationTest> cls = AnnotationTest.class;
Annotation[] annoArr = AnnotationTest.class.getAnnotations();

for(Annotation a : annoArr) {
    System.out.println("toString() : " + a.toString());
    System.out.println("hashCode() : " + a.hashCode());
    System.out.println("equals() : " + a.equals(a));
    System.out.println("annotationType() : " + a.annotationType());
}
```

위의 코드는 AnnotationTest클래스에 적용된 모든 애너테이션에 대해 toString(), hashCode(), equals()를 호출합니다.

#### 마커 애너테이션

값을 지정할 필요가 없는 경우, 애너테이션의 요소를 하나도 정의하지 않을 수 있습니다. Serializable이나 Cloneable인터페이스처럼, 요소가 하나도 정의되지 않은 애너테이션을 마커 애너테이션이라고 합니다.

```java
@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Override {    // 마커 애너테이션. 정의된 요소가 하나도 없다.

}

@Target(ElementType.METHOD)
@Retention(RetentionPolicy.SOURCE)
public @interface Test {    // 마커 애너테이션. 정의된 요소가 하나도 없다.

}
```

#### 애너테이션 요소의 규칙

애너테이션의 요소를 선언할 때 반드시 지켜야 하는 규칙은 다음과 같습니다.

> - 요소의 타입은 기본형, String, enum, 애너테이션, Class만 허용된다.
> - ()안에 매개변수를 선언할 수 없다.
> - 예외를 선언할 수 없다.
> - 요소를 타입 매개변수로 정의할 수 없다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  
ChatGPT - OpenAI