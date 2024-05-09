---
title: "[Java] ArrayList"
categories:
  - Java
tags:
  - Java
  - 자바
  - 컬렉션 프레임워크
  - Collections Framework
  - ArrayList
toc: true
toc_sticky: true
toc_label: "목차"
comments: true
---

# ArrayList란
---
ArrayList는 컬렉션 프레임워크에서 가장 많이 사용되는 컬렉션 클래스이며, List인터페이스를 구현하기 때문에 데이터의 저장순서가 유지되고 중복을 허용한다는 특징을 가진다.

ArrayList는 기존의 Vector를 개선한 것으로 Vector와 구현원리와 기능적인 측면에서 동일하다고 할 수 있다. 저번 포스팅에서 언급했듯이 Vector는 기존에 작성된 소스와의 호환성을 위해 계속 남겨두고 있을 뿐이기 때문에 가능하면 Vector보다는 ArrayList를 사용하는 것을 지향하도록 하자.

ArrayList는 <strong>Object배열</strong>을 이용해서 데이터를 <strong>순차적</strong>으로 저장한다. 예를 들면, 첫번째로 저장한 객체는 Object배열의 0번째 위치에 저장되고 그 다음에 저장하는 객체는 1번째 위치에 저장된다. 이런 식으로 계속 배열에 순서대로 저장되며, 배열에 더 이상 저장할 공간이 없으면 보다 큰 새로운 배열을 생성해서 기존의 배열에 저장된 내용을 새로운 배열로 복사한 다음에 저장된다.

```java
public class ArrayList extends AbstractList implements List, RandomAccess, Clonealbe, java.io.Serializable {
    ...
    transient Object[] elementData; // Object 배열
    ...
}
```

위의 코드는 ArrayList의 소스코드 일부인데 ArrayList는 elementData라는 이름의 Object배열을 멤버변수로 선언하고 있다는 것을 알 수 있다. 선언된 배열의 타입이 모든 객체의 최고조상인 Object이기 때문에 모든 종류의 객체를 담을 수 있다.

<br>

# ArrayList의 생성자와 메서드
---
ArrayList는 여러 생성자와 메서드를 가지고 있으며, 주요내용은 다음과 같다.

<table border="1">
    <colgroup>
        <col style="width:20%">
        <col style="width:60%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">메서드</th>
        <th style="text-align:center;">설명</th>
    </tr>
    <tr>
        <td style="text-align:left;">ArrayList()</td>
        <td style="text-align:left;">크기가 10인 ArrayList를 생성</td>
    </tr>
    <tr>
        <td style="text-align:left;">ArrayList(Collection c)</td>
        <td style="text-align:left;">주어진 컬렉션이 저장된 ArrayList를 생성</td>
    </tr>
    <tr>
        <td style="text-align:left;">ArrayList(int initialCapacity)</td>
        <td style="text-align:left;">지정된 초기용량을 갖는 ArrayList를 생성</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean add(Object o)</td>
        <td style="text-align:left;">ArrayList의 마지막에 객체를 추가. 성공하면 true</td>
    </tr>
    <tr>
        <td style="text-align:left;">void add(int index, Object element)</td>
        <td style="text-align:left;">지정된 위치(index)에 객체를 저장</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean addAll(Collection c)</td>
        <td style="text-align:left;">주어진 컬렉션의 모든 객체를 저장한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean addAll(int index, Collection c)</td>
        <td style="text-align:left;">지정된 위치부터 주어진 컬렉션의 모든 객체를 저장한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">void clear()</td>
        <td style="text-align:left;">ArrayList를 완전히 비운다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object clone()</td>
        <td style="text-align:left;">ArrayList를 복제한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean contains(Object o)</td>
        <td style="text-align:left;">지정된 객체(o)가 ArrayList에 포함되어 있는지 확인</td>
    </tr>
    <tr>
        <td style="text-align:left;">void ensureCapacity(int minCapacity)</td>
        <td style="text-align:left;">ArrayList의 용량이 최소한 minCapacity가 되도록 한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object get(int index)</td>
        <td style="text-align:left;">지정된 위치(index)에 저장된 객체를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">int indexOf(Object o)</td>
        <td style="text-align:left;">지정된 객체가 저장된 위치를 찾아 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean isEmpty()</td>
        <td style="text-align:left;">ArrayList가 비어있는지 확인한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Iterator iterator()</td>
        <td style="text-align:left;">ArrayList의 Iterator객체를 반환</td>
    </tr>
    <tr>
        <td style="text-align:left;">int lastIndexOf(Object o)</td>
        <td style="text-align:left;">객체(o)가 저장된 위치를 끝부터 역방향으로 검색해서 반환</td>
    </tr>
    <tr>
        <td style="text-align:left;">ListIterator listIterator()</td>
        <td style="text-align:left;">ArrayList의 ListIterator를 반환</td>
    </tr>
    <tr>
        <td style="text-align:left;">ListIterator listIterator(int index)</td>
        <td style="text-align:left;">ArrayList의 지정된 위치부터 시작하는 ListIterator를 반환</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object remove(int index)</td>
        <td style="text-align:left;">지정된 위치(index)에 있는 객체를 제거한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean remove(Object o)</td>
        <td style="text-align:left;">지정한 객체를 제거한다.(성공하면 true, 실패하면 false)</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean removeAll(Collection c)</td>
        <td style="text-align:left;">지정한 컬렉션에 저장된 것과 동일한 객체들을 ArrayList에서 제거한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">boolean retainAll(Collection c)</td>
        <td style="text-align:left;">ArrayList에 저장된 객체 중에서 주어진 컬렉션과 공통된 것들만을 남기고 나머지는 삭제한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object set(int index, Object element)</td>
        <td style="text-align:left;">주어진 객체(element)를 지정된 위치(index)에 저장한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">int size()</td>
        <td style="text-align:left;">ArrayList에 저장된 객체의 개수를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">void sort(Comparator c)</td>
        <td style="text-align:left;">지정된 정렬기준(c)으로 ArrayList를 정렬</td>
    </tr>
    <tr>
        <td style="text-align:left;">List subList(int fromIndex, int toIndex)</td>
        <td style="text-align:left;">fromIndex부터 toIndex사이에 저장된 객체를 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object[] toArray()</td>
        <td style="text-align:left;">ArrayList에 저장된 모든 객체들을 객체배열로 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">Object[] toArray(Object[] a)</td>
        <td style="text-align:left;">ArrayList에 저장된 모든 객체들을 객체배열 a에 담아 반환한다.</td>
    </tr>
    <tr>
        <td style="text-align:left;">void trimToSize()</td>
        <td style="text-align:left;">용량을 크기에 맞게 줄인다.(빈 공간을 없앤다.)</td>
    </tr>
</table>

<br>

# 예제1
---
아래 예제는 ArrayList의 기본적인 메서드를 이용해서 객체를 다루는 방법을 보여준다. ArrayList는 List인터페이스를 구현했기 때문에 저장된 순서를 유지한다는 것을 알 수 있다.

```java
import java.util.*;

class ArrayListEx1 {
    public static void main(String[] args) {
        ArrayList list1 = new ArrayList(10);
        list1.add(new Integer(5));
        list1.add(new Integer(4));
        list1.add(new Integer(2));
        list1.add(new Integer(0));
        list1.add(new Integer(1));
        list1.add(new Integer(3));

        ArrayList list2 = new ArrayList(list1.subList(1, 4));
        print(list1, list2);

        Collections.sort(list1); // list1과 list2를 정렬한다.
        Collections.sort(list2); // Collections.sort(List 1)
        print(list1, list2);

        System.out.println("list1.containsAll(list2):" + list1.containsAll(list2));

        list2.add("B");
        list2.add("C");
        list2.add("A");
        print(list1, list2);

        list2.set(3, "AA");
        print(list1, list2);

        // list1에서 list2와 겹치는 부분만 남기고 나머지는 삭제한다.
        System.out.println("list1.retainAll(list2):" + list1.retainAll(list2));
        print(list1, list2);

        // list2에서 list1에 포함된 객체들을 삭제한다.
        for(int i = list2.size() - 1; i >= 0; i--) {
            if(list1.contains(list2.get(i))) list2.remove(i);
        }
        print(list1, list2);
    } // main end

    static void print(ArrayList list1,  ArrayList list2) {
        System.out.println("list1:" + list1);
        System.out.println("list2:" + list2);
        System.out.println();
    }
} // class end
```

여기서 하나 짚고 넘어가면 아래코드는 위코드의 일부분인데, list2에서 list1과 공통되는 요소들을 찾아서 삭제하는 부분이다.

```java
for(int i = list2.size() - 1; i >= 0; i--) {
    if(list1.contains(list2.get(i))) list2.remove(i);
}
```

여기서 list2의 각 요소를 접근하기 위해 get(int index)메서드와 for문을 사용하였는데, for문의 변수 i를 0부터 증가시킨 것이 아니라, 'list2.size() - 1' 부터 감소시키면서 거꾸로 반복시켰다.

만일 변수 i를 증가시켜가면서 삭제하면, 한 요소가 삭제될 때마다 빈 공간을 채우기 위해 나머지 요소들이 자리이동을 하기 때문에 올바른 결과를 얻을 수 없다. 그래서 제어변수를 감소시켜가면서 삭제를 해야 자리이동이 발생해도 영향을 받지 않고 작업이 가능하다.

<br>

# 예제2
---
이번 예제는 긴 문자열 데이터를 원하는 길이로 잘라 ArrayList에 담은 다음 출력하는 예제이다. 단순히 문자열을 특정크기로 잘라 출력할 것이라면, charAt(int i)와 for문을 이용하면 되겠지만 ArrayList에 잘라서 담아놓음으로써 ArrayList의 기능을 이용해서 다양한 작업을 간단하게 처리할 수 있다.

```java
import java.util.*;

class ArrayListEx2 {
    public static void main(String[] args) {
        final int LIMIT = 10; // 자르고자 하는 글자의 개수를 지정한다.
        String source = "0123456789abcdefghijABCDEFGHIJ!@#$%^&*()ZZZ";
        int length = source.length();

        List list = new ArrayList(length/LIMIT + 10); // 크기를 약간 여유 있게 잡는다.

        for(int i = 0; i < length; i += LIMIT) {
            if(i + LIMIT < length) {
                list.add(source.substring(i, i + LIMIT));
            } else {
                list.add(source.substring(i));
            }
        }

        for(int i = 0; i < list.size(); i++) {
            System.out.println(list.get(i));
        }
    } // main()
}
```

아래코드는 위 예제의 일부인데, 이렇게 하는 이유는 ArrayList를 생성할 때, 저장할 요소의 개수를 고려해서 실제 저장할 개수보다 약간 여유있는 크기로 하는 것이 좋다. 생성할 때 지정한 크기보다 더 많은 객체를 저장하면 자동적으로 크기가 늘어나기는 하지만 이 과정에서 처리시간이 많이 소요되기 때문이다.

```java
List list = new ArrayList(length/LIMIT + 10); // 크기를 약간 여유 있게 잡는다.
```

<br>

# 예제3
---
```java
import java.util.*;

class VectorEx1 {
    public static void main(String[] args) {
        Vector v = new Vector(5); // 용량(capacity)이 5인 Vector를 생성한다.
        v.add("1");
        v.add("2");
        v.add("3");
        print(v);

        v.trimToSize(); // 빈 공간을 없앤다. (용량과 크기가 같아진다.)
        System.out.prinln("=== After trimToSize() ===");
        print(v);

        v.ensureCapacity(6);
        System.out.println("=== After ensureCapacity(6) ===");
        print(v);

        v.setSize(7);
        System.out.println("=== After setSize(7) ===");
        print(v);

        v.clear();
        System.out.println("=== After clear() ===");
        print(v);
    }

    public static void print(Vector v) {
        System.out.println(v);
        System.out.println("size :" + v.size());
        System.out.println("capacity :" + v.capacity());
    }
}
```

이 예제는 Vector의 용량(capacity)과 크기(size)에 관한 것인데, 각 실행과정을 그림과 함께 단계별로 살펴보자.

1. 다음 capacity가 5인 Vector인스턴스 v를 생성하고, 3개의 객체를 저장한 후의 상태를 그림으로 나타낸 것이다.

    ![Screenshot_1](/assets/img/posts/20240508/Screenshot_1.png "Screenshot_1"){: width="100%"}

2. v.trimToSize()를 호출하면 v의 빈 공간을 없애서 size와 capacity를 같게 한다. 배열은 크기를 변경할 수 없기 때문에 새로운 배열을 생성해서 그 주소값을 변수 v에 할당한다. 기존의 Vector인스턴스는 더 이상 사용할 수 없으며, 후에 가비지컬렉터에 의해서 메모리에서 제거된다.

    ![Screenshot_2](/assets/img/posts/20240508/Screenshot_2.png "Screenshot_2"){: width="100%"}

3. v.ensureCapacity(6)는 v의 capacity가 최소한 6이 되도록 한다. 만일 v의 capacity가 6이상이라면 아무 일도 일어나지 않는다. 현재는 v의 capacity가 3이므로 크기가 6인 배열을 생성해서 v의 내용을 복사했다. 기존의 인스턴스를 다시 사용하는 것이 아니라 새로운 인스턴스를 생성하였음에 주의하자.

    ![Screenshot_3](/assets/img/posts/20240508/Screenshot_3.png "Screenshot_3"){: width="100%"}

4. v.setSize(7)는 v의 size가 7이 되도록 한다. 만일 v의 capacity가 충분하면 새로 인스턴스를 생성하지 않아도 되지만 지금은 capacity가 6이므로 새로운 인스턴스를 생성해야한다. Vector는 capacity가 부족할 경우 자동적으로 기존의 크기보다 2배의 크기로 증가된다. 그래서 v의 capacity는 12가 된다.

    >💡 __참고__  
    > 생성자 Vector(int initialCapacity, int capacityIncrement)를 사용해서 인스턴스를 생성한 경우에는 지정해준 capacityIncrement만큼 증가하게 된다.

    ![Screenshot_4](/assets/img/posts/20240508/Screenshot_4.png "Screenshot_4"){: width="100%"}

5. v.clear();는 v의 모든 요소를 삭제한다. 아래의 왼쪽그림의 상태에서 오른쪽 그림과 같은 상태가 되는 것이다.

    ![Screenshot_5](/assets/img/posts/20240508/Screenshot_5.png "Screenshot_5"){: width="100%"}

    >💡 __참고__  
    > Vector는 Object배열이기 때문에 실제로는 마지막 그림처럼 주소가 저장되어야 더 정확한 것이지만, 편의상 이전의 그림들은 간략히 표현한 것이다.

ArrayList나 Vector 같이 배열을 이용한 자료구조는 데이터를 읽어오고 저장하는 데는 효율이 좋지만, 용량을 변경해야할 때는 새로운 배열을 생성한 후 기존의 배열로부터 새로 생성된 배열로 데이터를 복사해야하기 때문에 상당히 효율이 떨어진다는 단점을 가지고 있다. 그래서 처음에 인스턴스를 생성할 때, 저장할 데이터의 개수를 잘 고려하여 충분한 용량의 인스턴스를 생성하는 것이 좋다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  