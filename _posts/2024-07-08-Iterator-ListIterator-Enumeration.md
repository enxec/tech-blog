---
title: "[Java] Iterator, ListIterator, Enumeration"
description: 
author: 이원모
date: 2024-07-08
categories: [Language, Java]
tags: [Iterator, ListIterator, Enumeration, java, 자바, 컬렉션 프레임워크, Collections Framework]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/java-logo.png
  lqip: 
  alt: 
---

Iterator, ListIterator, Enumeration은 모두 컬렉션에 저장된 요소를 접근하는데 사용되는 인터페이스입니다. Enumeration은 Iterator의 구버전이며, ListIterator는 Iterator의 기능을 향상 시킨 것입니다.

## Iterator
---
컬렉션 프레임워크에서는 컬렉션에 저장된 요소들을 읽어오는 방법을 표준화하였습니다. 컬렉션에 저장된 각 요소에 접근하는 기능을 가진 Iterator 인터페이스를 정의하고, Collection 인터페이스에는 Iterator(Iterator를 구현한 클래스의 인스턴스)를 반환하는 iterator()를 정의하고 있습니다.

```java
public interface Iterator {
  boolean hasNext();
  Object next();
  void remove();
}

public interface Collection {
  ...
  public Iterator iterator();
  ...
}
```

iterator()는 Collection인터페이스에 정의된 메서드이므로 Collection 인터페이스의 자손인 List와 Set에도 포함되어 있습니다. 그래서 List나 Set 인터페이스를 구현하는 컬렉션은 iterator()가 각 컬렉션의 특징에 잘맞게 작성되어 있습니다. 컬렉션 클래스에 대해 iterator()를 호출하여 Iterator를 얻은 다음 반복문, 주로 while문을 사용해서 컬렉션 클래스의 요소들을 읽어 올 수 있습니다.

<table style="width:100%; border-collapse: collapse; table-layout: fixed;">
  <colgroup>
    <col style="width:40%;">
    <col style="width:60%;">
  </colgroup>
  <thead>
    <tr style="background-color:darkslategray;">
      <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">메서드</th>
      <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        boolean hasNext()
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        읽어 올 요소가 남아있는지 확인합니다. 있으면 true, 없으면 false를 반환합니다.
      </td>
    </tr>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        Object next()
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        다음 요소를 읽어 옵니다. next()를 호출하기 전에 hasNext()를 호출해서 읽어 올 요소가 있는지 확인하는 것이 안전합니다.
      </td>
    </tr>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        void remove()
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        next()로 읽어 온 요소를 삭제합니다. next()를 호출한 다음에 remove()를 호출해야합니다.(선택적 기능)
      </td>
    </tr>
  </tbody>
</table>

<p align="center">▲ Iterator 인터페이스의 메서드</p>

ArrayList에 저장된 요소들을 출력하기 위한 코드는 다음과 같이 작성할 수 있습니다.

```java
List list = new ArrayList(); // 다른 컬렉션으로 변경할 때는 이 부분만 고치면 됩니다.
Iterator it = list.iterator();

while(it.hasNext()) {
  System.out.println(it.next());
}
```

ArrayList 대신 List 인터페이스를 구현한 다른 컬렉션 클래스에 대해서도 이와 동일한 코드를 사용할 수 있습니다. 첫 줄에는 ArrayList대신 List 인터페이스를 구현한 다른 컬렉션 클래스의 객체를 생성하도록 변경하기만 하면 됩니다.

Iterator를 이용해서 컬렉션의 요소를 읽어오는 방법을 표준화했기 때문에 이처럼 코드의 재사용성을 높이는 것이 가능한 것입니다. 이처럼 공통 인터페이스를 정의해서 표준을 정의하고 구현하여 표준을 따르도록 함으로써 코드의 일관성을 유지하여 재사용성을 극대화하는 것이 객체지향 프로그래밍의 중요한 목적 중의 하나입니다.

Map 인터페이스를 구현한 컬렉션 클래스는 키와 값을 쌍으로 저장하고 있기 때문에 iterator()를 직접 호출할 수 없고, 그 대신 keySet()이나 entrySet()과 같은 메서드를 통해서 키와 값을 각각 따로 Set의 형태로 얻어 온 후에 다시 iterator()를 호출해야 Iterator를 얻을 수 있습니다.

```java
Map map = new HashMap();
...
Iterator it = map.entrySet().iterator();
```

Iterator it = map.entrySet().iterator(); 는 아래의 두 문장을 하나로 합친 것이라고 이해하면 됩니다.

```java
Set eSet = map.entrySet();
Iterator it = eSet.iterator();
```

이 문장들의 실행순서를 그림으로 그려보면 다음과 같습니다.

![실행순서](/posts/20240708/Screenshot_1.png "실행순서"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;"></div>

__① map.entrySet()의 실행결과가 Set이므로__  
  Iterator it = map.entrySet().iterator(); -> Iterator it = Set인스턴스.iterator();  

__② map.entrySet()를 통해 얻은 Set인스턴스의 iterator()를 호출해서 Iterator인스턴스를 얻습니다.__  
  Iterator it = Set인스턴스.iterator(); -> Iterator it = Iterator인스턴스;  

__③ 마지막으로 Iterator인스턴스의 참조가 it에 저장됩니다.__  

<br>
StringBuffer를 사용할 때 이와 유사한 코드를 많이 보았을 것입니다.

```java
StringBuffer sb = new StringBuffer();
sb.append("A");
sb.append("B");
sb.append("C");
```

위와 같은 코드를 아래와 같이 간단히 쓸 수 있는데 그 이유는 바로 append메서드가 수행결과로 StringBuffer를 리턴하기 때문입니다. 만일 void append(String str)과 같이 void를 리턴하도록 선언되어 있다면 위의 코드를 아래와 같이 쓸 수 없었을 것입니다. append메서드의 호출결과가 void이기 때문에 또 다시 void에 append메서드를 호출 할 수 없기 때문입니다.

```java
StringBuffer sb = new StringBuffer();
sb.append("A").append("B").append("C"); // StringBuffer append(String str)
```

List 클래스들은 저장순서를 유지하기 때문에 Iterator를 이용해서 읽어 온 결과 역시 저장순서와 동일하지만 Set클래스들은 각 요소간의 순서가 유지 되지 않기 때문에 Iterator를 이용해서 저장된 요소들을 읽어 와도 저장된 순서와 같지 않습니다.

```java
import java.util.*;

class IteratorEx1 {
  public static void main(String[] args) {
    ArrayList list = new ArrayList();
    list.add("1");
    list.add("2");
    list.add("3");
    list.add("4");
    list.add("5");

    Iterator it = list.iterator();

    while(it.hasNext()) {
      Object obj = it.next();
      System.out.println(obj);
    }
  }
}
```

<br>

## ListIterator와 Enumeration
---
Enumeration은 컬렉션 프레임워크가 만들어지기 이전에 사용하던 것으로 Iterator의 구전이라고 생각하면 됩니다. 이전 버전으로 작성된 소스와의 호환을 위해서 남겨 두고 있을 뿐이므로 가능하면 Enumeration대신 Iterator를 사용하는 것이 좋습니다.

ListIterator는 Iterator를 상속받아서 기능을 추가한 것으로, 컬렉션의 요소에 접근할 때 Iterator는 단방향으로만 이동할 수 있는 데 반해 ListIterator는 양방향으로의 이동이 가능합니다. 다만 ArrayList나 LinkedList와 같이 List인터페이스를 구현한 컬렉션에서만 사용할 수 있습니다.

> __Enumeration__ Iterator의 구버전  
> __ListIterator__ Iterator에 양방향 조회기능추가(List를 구현한 경우만 사용가능)

다음은 Enumeration, Iterator, ListIterator의 메서드에 대한 설명입니다. Enumeration과 Iterator는 메서드이름만 다를 뿐 기능은 같고, ListIterator는 Iterator에 이전방향으로의 접근기능을 추가한 것일 뿐이라는 것을 알 수 있습니다.

<table style="width:100%; border-collapse: collapse; table-layout: fixed;">
  <colgroup>
    <col style="width:40%;">
    <col style="width:60%;">
  </colgroup>
  <thead>
    <tr style="background-color:darkslategray;">
      <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">메서드</th>
      <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        boolean hasMoreElements()
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        읽어 올 요소가 남아있는지 확인한다. 있으면 true, 없으면 false를 반환합니다. Iterator의 hasNext()와 같습니다.
      </td>
    </tr>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        Object nextElement();
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        다음 요소를 읽어 온다. nextElement()를 호출하기 전에 hasMoreElements()를 호출해서 읽어올 요소가 남아있는지 확인하는 것이 안전합니다. Iterator의 next()와 같습니다.
      </td>
    </tr>
  </tbody>
</table>

<p align="center">▲ Enumeration 인터페이스의 메서드</p>

<table style="width:100%; border-collapse: collapse; table-layout: fixed;">
  <colgroup>
    <col style="width:40%;">
    <col style="width:60%;">
  </colgroup>
  <thead>
    <tr style="background-color:darkslategray;">
      <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">메서드</th>
      <th style="text-align:center; border: 1px solid black; word-wrap: break-word;">설명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        void add(Object o)
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        컬렉션에 새로운 객체(o)를 추가합니다. (선택적 기능)
      </td>
    </tr>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        boolean hasNext()
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        읽어 올 다음 요소가 남아있는지 확인합니다. 있으면 true, 없으면 false를 반환
      </td>
    </tr>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        boolean hasPrevious()
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        읽어 올 이전 요소가 남아있는지 확인합니다. 있으면 true, 없으면 false를 반환
      </td>
    </tr>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        Object next()
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        다음 요소를 읽어 옵니다. next()를 호출하기 전에 hasNext()를 호출해서 읽어올 요소가 있는지 확인하는 것이 안전합니다.
      </td>
    </tr>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        Object previous()
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        이전 요소를 읽어 온다. previous()를 호출하기 전에 hasPrevious()를 호출해서 읽어 올 요소가 있는지 확인하는 것이 안전합니다.
      </td>
    </tr>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
         int nextIndex()
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        다음 요소의 index를 반환합니다.
      </td>
    </tr>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        int previousIndex()
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        이전 요소의 index를 반환합니다.
      </td>
    </tr>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        void remove()
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        next() 또는 previous()로 읽어 온 요소를 삭제합니다. 반드시 next()나 previous()를 먼저 호출한 다음에 이 메서드를 호출해야합니다. (선택적 기능)
      </td>
    </tr>
    <tr>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        void set(Object o)
      </td>
      <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
        next() 또는 previous()로 읽어 온 요소를 지정된 객체(o)로 변경합니다. 반드시 next()나 previous()를 먼저 호출한 다음에 이 메서드를 호출해야합니다. (선택적 기능)
      </td>
    </tr>
  </tbody>
</table>

<p align="center">▲ ListIterator의 메서드</p>

```java
import java.util.*;

class ListIteratorEx1 {
  public static void main(String[] args) {
    ArrayList list = new ArrayList();
    list.add("1");
    list.add("2");
    list.add("3");
    list.add("4");
    list.add("5");

    ListIterator it = list.listIterator();

    while(it.hasNext()) {
      System.out.print(it.next()); // 순방향으로 진행하면서 읽어옵니다.
    }
    System.out.println();

    while(it.hasPrevious()) {
      System.out.print(it.previous()); // 역방향으로 진행하면서 읽어옵니다.
    }
    System.out.println();
  }
}

/** 결과

12345
54321

 */
```

ListIterator의 사용방법을 보여주는 간단한 예제입니다. Iterator는 단방향으로만 이동하기 때문에 컬렉션의 마지막 요소에 다다르면 더 이상 사용할 수 없지만, ListIterator는 양방향으로 이동하기 때문에 각 요소간의 이동이 자유롭습니다. 다만 이동하기 전에 반드시 hasNext()나 hasPrevious()를 호출해서 이동할 수 있는지 확인해야 합니다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성