---
title: "[Java] 컬렉션 프레임워크 핵심 인터페이스"
categories:
  - Java
tags:
  - Java
  - 자바
  - 컬렉션 프레임워크
  - Collections Framework
  - 컬렉션 프레임워크 핵심 인터페이스
  - Collections Framework main point interface
toc: true
toc_sticky: true
toc_label: "목차"
comments: true
---

컬렉션 프레임워크란, '데이터 군을 저장하는 클래스들을 표준화한 설계'를 뜻한다. 컬렉션은 다수의 데이터, 즉 데이터 그룹을 프레임워크로 표준화한 프로그래밍 방식이다.

JDK1.2 이전까지는 Vector, Hashtable, Properties와 같은 컬렉션 클래스, 다수의 데이터를 저장할 수 있는 클래스들을 서로 다른 각자의 방식으로 처리해야 했으나, JDK1.2부터 컬렉션 프레임워크가 등장하면서 다양한 종류의 컬렉션 클래스가 추가되고 모든 컬렉션 클래스를 표준화된 방식으로 다룰 수 있도록 체계화되었다.

컬렉션 프레임워크는 컬렉션, 다수의 데이터를 다루는데 필요한 다양하고 풍부한 클래스들을 제공하기 때문에 프로그래머의 부담을 덜어주고, 또한 인터페이스와 다형성을 이용한 객체지향적 설계를 통해 표준화되어 있기 때문에 사용법을 익히기에도 편리하고 재사용성이 높은 코드를 작성할 수 있다는 장점이 있다.

# 컬렉션 프레임워크의 핵심 인터페이스
---
컬렉션 프레임워크에서는 컬렉션데이터 그룹을 크게 3가지 타입이 존재한다고 인식하고 각 컬렉션을 다루는데 필요한 기능을 가진 3개의 인터페이스를 정의하였다. 그리고 인터페이스 List와 Set의 공통된 부분을 다시 뽑아서 새로운 인터페이스인 Collection을 추가로 정의하였다.

![컬렉션 프레임워크의 핵심 인터페이스간의 상속계층도](/blog/assets/img/posts/20240502/collection-framework-Inheritance-hierarchy-between-core-interfaces.png "컬렉션 프레임워크의 핵심 인터페이스간의 상속계층도"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">컬렉션 프레임워크의 핵심 인터페이스간의 상속계층도</div>

인터페이스 List와 Set을 구현한 컬렉션 클래스들은 서로 많은 공통부분이 있어서, 공통된 부분을 다시 뽑아 Collection인터페이스를 정의할 수 있었지만 Map인터페이스는 이들과는 전혀 다른 형태로 컬렉션을 다루기 때문에 같은 상속계층도에 포함되지 못했다. 이러한 설계는 객체지향언어의 장점을 극명히 보여준다.

>💡 __참고__  
> JDK1.5부터 Iterable인터페이스가 추가되고 이를 Collection인터페이스가 상속받도록 변경되었으나 이것은 ㅁ단지 인터페이스들의 공통적인 메서드인 iterator()를 뽑아서 중복을 제거하기 위한 것에 불과하므로 상속계층도에서 별 의미가 없다.

<table border="1">
    <colgroup>
        <col style="width:20%">
        <col style="width:60%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">인터페이스</th>
        <th style="text-align:center;">특징</th>
    </tr>
    <tr>
        <td style="text-align:center;" rowspan="2">List</td>
        <td style="text-align:left;">
             순서가 있는 데이터의 집합. 데이터의 중복을 허용한다.<br>
             예) 대기자 명단
        </td>
    </tr>
    <tr>
        <td style="text-align:left;">구현클래스 : ArrayList, LinkedList, Stack, Vector 등</td>
    </tr>
    <tr>
        <td style="text-align:center;" rowspan="2">Set</td>
        <td style="text-align:left;">
            순서를 유지하지 않는 데이터의 집합, 데이터의 중복을 허용하지 않는다.<br>
            예) 양의 정수집합, 소수의 집합
        </td>
    </tr>
    <tr>
        <td style="text-align:left;">구현클래스 : HashSet, TreeSet 등</td>
    </tr>
    <tr>
        <td style="text-align:center;" rowspan="2">Map</td>
        <td style="text-align:left;">
            키(key)와 값(value)의 쌍으로 이루어진 데이터의 집합<br>
            순서는 유지되 않으며, 키는 중복을 허용하지 않고, 값은 중복을 허용한다.<br>
            예) 양의 정수집합, 소수의 집합
        </td>
    </tr>
    <tr>
        <td style="text-align:left;">구현클래스 : HashMap, TreeMap, Hashtable, Properties 등</td>
    </tr>
</table>

컬렉션 프레임워크의 모든 클래스들은 List, Set, Map 중의 하나를 구현하고 있으며, 구현한 인터페이스의 이름이 클래스의 이름에 포함되어있어 이름만으로도 클래스의 특징을 쉽게 알 수 있도록 되어있다. 그러나 Vector, Stack, Hashtable, Properties와 같은 클래스들은 컬렉션 프레임워크가 만들어지기 이전부터 존재하던 것이기 때문에 컬렉션 프레임워크의 명명법을 따르지 않는다.

Vector나 Hashtable과 같은 기존의 컬렉션 클래스들을 호환하기 위해, 설계를 변경해서 남겨두었지만 가능하면 사용하지 않는 것이 좋다. 대신 새로 추가된 ArrayList와 HashMap을 사용하자.,

![Vector클래스의 상속계층도 변화 - 왼쪽이 JDK1.2이전, 오른쪽이 이후](/blog/assets/img/posts/20240502/inheritance-hierarchy-of-the-Vector-class-also-changes.png "Vector클래스의 상속계층도 변화 - 왼쪽이 JDK1.2이전, 오른쪽이 이후"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">Vector클래스의 상속계층도 변화 - 왼쪽이 JDK1.2이전, 오른쪽이 이후</div>

## Collection 인터페이스
<table border="1">
    <colgroup>
        <col style="width:40%">
        <col style="width:60%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">메서드</th>
        <th style="text-align:center;">설명</th>
    </tr>
    <tr>
        <td style="text-align:left;">
            boolean add(Object o)<br>
            boolean addAll(Collection c)
        </td>
        <td style="text-align:left;">
             지정된 객체(o) 또는 Collection(c)의 객체들을 Collection에 추가한다.
        </td>
    </tr>
    <tr>
        <td style="text-align:left;">void clear()</td>
        <td style="text-align:left;">Collection의 모든 객체를 삭제한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">
            boolean contains(Object o)<br>
            boolean containsAll(Collection c)
        </td>
        <td style="text-align:left;">
            지정된 객체(o) 또는 Collection의 객체들이 Collection에 포함되어 있는지 확인한다.
        </td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean equals(Object o)</td>
        <td style="text-align:left;">동일한 Collection인지 비교한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">int hashCode()</td>
        <td style="text-align:left;">Collection의 hash code를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean isEmpty()</td>
        <td style="text-align:left;">Collection이 비어있는지 확인한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Iterator iterator()</td>
        <td style="text-align:left;">Collection의 Iterator를 얻어서 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean remove(Object o)</td>
        <td style="text-align:left;">지정된 객체를 삭제한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean removeAll(Collection c)</td>
        <td style="text-align:left;">지정된 Collection에 포함된 객체들을 삭제한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean retainAll(Collection c)</td>
        <td style="text-align:left;">
            지정된 Collection에 포함된 객체만을 남기고 다른 객체들은 Collection에서 삭제한다. 이 작업으로 인해 Collection에 변화가 있으면 true를 그렇지 않으면 false를 반환한다.
        </td>
    </tr>
    <tr>
        <td style="text-align:left;">int size()</td>
        <td style="text-align:left;">Collection에 저장된 객체의 개수를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object[] toArray()</td>
        <td style="text-align:left;">Collection에 저장된 객체를 객체배열(Object[])로 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object[] toArray(Object[] a)</td>
        <td style="text-align:left;">지정된 배열에 Collection의 객체를 저장해서 반환한다.</td>
    </tr>
</table>

>💡 __참고__  
> JDK1.8부터 추가된 parallelStream, removelf, stream, forEach 등은 향후 람다 스트림 포스팅에서 설명하겠다.

Collection인터페이스는 컬렉션 클래스에 저장된 데이터를 읽고, 추가하고 삭제하는 등 컬렉션을 다루는데 가장 기본적인 메서드들을 정의하고 있다.

그리고 Java API문서를 보면 위 표에서 사용된 'Object'가 아닌 'E'로 표기되어있는데, E는 특정 타입을 의미하는 것으로 JDK1.5부터 추가된 제네릭에 의한 표기이다.

## List 인터페이스
List 인터페이스는 중복을 허용하면서 저장순서가 유지되는 컬렉션을 구현하는데 사용된다.

![List의 상속계층도](/blog/assets/img/posts/20240502/list-Inheritance-hierarchy.png "List의 상속계층도"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">List의 상속계층도</div>

List인터페이스에 정의된 메서드는 다음과 같다. Collection인터페이스로부터 상속받은 것들은 제외하였다.

<table border="1">
    <colgroup>
        <col style="width:40%">
        <col style="width:60%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">메서드</th>
        <th style="text-align:center;">설명</th>
    </tr>
    <tr>
        <td style="text-align:left;">
            void add(int index, Object element)<br>
            boolean addAll(int index, Collection c)
        </td>
        <td style="text-align:left;">
            지정된 위치(index)에 객체(element) 또는 컬렉션에 포함된 객체들을 추가한다.
        </td>
    </tr>
    <tr>
        <td style="text-align:left;">Object get(int index)</td>
        <td style="text-align:left;">지정된 위치(index)에 있는 객체를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">int indexOf(Object o)</td>
        <td style="text-align:left;">
            지정된 객체의 위치(index)를 반환한다.<br>
            (List의 첫 번쨰 요소부터 역방향으로 찾는다.)
        </td>
    </tr>
    <tr>
        <td style="text-align:left;">int lastIndexOf(Object o)</td>
        <td style="text-align:left;">
            지정된 객체의 위치(index)를 반환한다.<br>
            (List의 마지막 요소부터 역방향으로 찾는다.)
        </td>
    </tr>
    <tr>
        <td style="text-align:left;">
            ListIterator listIterator()<br>
            ListIterator listIterator(int index)
        </td>
        <td style="text-align:left;">
            List의 객체에 접근할 수 있는 ListIterator를 반환한다.
        </td>
    </tr>
    <tr>
        <td style="text-align:left;">Object remove(int index)</td>
        <td style="text-align:left;">지정된 위치(index)에 있는 객체를 삭제하고 삭제된 객체를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object set(int index, Object element)</td>
        <td style="text-align:left;">지정된 위치(index)에 객체(element)를 저장한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">void sort(Comparator c)</td>
        <td style="text-align:left;">지정된 비교자(comparator)로 List를 정렬한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">List subList(int fromIndex, int toIndex)</td>
        <td style="text-align:left;">지정된 범위(fromIndex부터 toIndex)에 있는 객체를 반환한다.</td>
    </tr>
</table>

## Set인터페이스
Set인터페이스는 중복을 허용하지 않고 저장순서가 유지되지 않는 컬렉션 클래스를 구현하는데 사용된다. Set인터페이스를 구현한 클래스로는 HashSet, TreeSet 등이 있다.

![Set의 상속계층도](/blog/assets/img/posts/20240502/set-Inheritance-hierarchy.png "Set의 상속계층도"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">Set의 상속계층도</div>

## Map인터페이스
Map인터페이스는 키(key)와 값(value)을 하나의 쌍으로 묶어서 저장하는 컬렉션 클래스를 구현하는데 사용된다. 키는 중복될 수 없지만 값은 중복을 허용한다. 기존에 저장된 데이터와 중복된 키와 값을 저장하면 기존의 값은 없어지고 마지막에 저장된 값이 남게된다. Map인터페이스를 구현한 클래스로는 Hashable, HashMap, LinkedHashMap, SortedMap, TreeMap 등이 있다.

![Map의 상속계층도](/blog/assets/img/posts/20240502/map-Inheritance-hierarchy.png "Map의 상속계층도"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">Map의 상속계층도</div>

<table border="1">
    <colgroup>
        <col style="width:40%">
        <col style="width:60%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">메서드</th>
        <th style="text-align:center;">설명</th>
    </tr>
    <tr>
        <td style="text-align:left;">void clear()</td>
        <td style="text-align:left;">Map의 모든 객체를 삭제한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean containsKey(Object key)</td>
        <td style="text-align:left;">지정된 key객체와 일치하는 Map의 key객체가 있는지 확인한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean containsValue(Object value)</td>
        <td style="text-align:left;">지정된 value객체와 일치하는 Map의 value객체가 있는지 확인한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Set entrySet()</td>
        <td style="text-align:left;">Map에 저장되어 있는 key-value쌍을 Map.Entry타입의 객체로 저장한 Set으로 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean equals(Object o)</td>
        <td style="text-align:left;">동일한 Map인지 비교한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object get(Object key)</td>
        <td style="text-align:left;">지정한 key객체에 대응하는 value객체를 찾아서 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">int hashCode()</td>
        <td style="text-align:left;">해시코드를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean isEmpty()</td>
        <td style="text-align:left;">Map이 비어있는지 확인한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Set keySet()</td>
        <td style="text-align:left;">Map에 저장된 모든 key객체를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object put(Object key, Object value)</td>
        <td style="text-align:left;">Map에 value객체를 key객체에 연결(mapping)하여 저장한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">void putAll(Map t)</td>
        <td style="text-align:left;">지정된 Map의 모든 key-value쌍을 추가한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object remove(Object key)</td>
        <td style="text-align:left;">지정한 key객체와 일치하는 key-value객체를 삭제한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">int size()</td>
        <td style="text-align:left;">Map에 저장된 key-value쌍의 개수를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Collection values()</td>
        <td style="text-align:left;">Map에 저장된 모든 value객체를 반환한다.</td>
    </tr>
</table>

values()에서는 반환타입이 Collection이고, keySet()에서는 반환타입이 Set인 것에 주목하자. Map인터페이스에서 값(value)은 중복을 허용하기 때문에 Collection타입으로 반환하고, 키(key)는 중복을 허용하지 않기 때문에 Set타입으로 반환한다.

## Map.Entry인터페이스
Map.Entry인터페이스는 Map인터페이스의 내부 인터페이스이다. 내부 클래스와 같이 인터페이스도 인터페이스 안에 인터페이스를 정의하는 내부 인터페이스를 정의하는 것이 가능하다.

Map에 저장되는 key-value쌍을 다루기 위해 내부적으로 Entry인터페이스를 정의해 놓았다. 이것은 보다 객체지향적으로 설계하도록 유도하기 위한 것으로 Map인터페이스를 구현하는 클래스에서는 Map.Entry인터페이스도 함께 구현해야한다.

다음은 Map인터페이스의 소스코드의 일부이다.

```java
public interface Map {
    ...
    public static interface Entry {
        Object getKey();
        Object getValue();
        Object setValue(Object value);
        boolean equals(Object o);
        int hashCode();
        ...     // JDK8.0부터 추가된 메서드는 생략
    }
}
```

<table border="1">
    <colgroup>
        <col style="width:40%">
        <col style="width:60%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">메서드</th>
        <th style="text-align:center;">설명</th>
    </tr>
    <tr>
        <td style="text-align:left;">boolean equals(Object o)</td>
        <td style="text-align:left;">동일한 Entry인지 비교한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object getKey()</td>
        <td style="text-align:left;">Entry의 key객체를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object getValue()</td>
        <td style="text-align:left;">Entry의 value객체를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">int hashCode()</td>
        <td style="text-align:left;">Entry의 해시코드를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object setValue(Object value)</td>
        <td style="text-align:left;">Entry의 value객체를 지정된 객체로 바꾼다.</td>
    </tr>
</table>

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  