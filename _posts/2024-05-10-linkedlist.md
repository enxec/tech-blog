---
title: "[Java] LinkedList"
description: 
author: Enxec
date: 2024-05-10
categories: [Language, Java]
tags: [java, 자바, 컬렉션 프레임워크, Collections Framework, LinkedList]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/java-logo.png
  lqip: 
  alt: 
---

## LinkedList란
---
배열은 가장 기본적인 형태의 자료구조로 구조가 간단하며 사용하기 쉽고 데이터를 읽어오는데 걸리는 시간(접근시간, access time)이 가장 빠르다는 장점을 가지고 있지만 다음과 같은 단점도 가지고 있다.

> 1. 크기를 변경할 수 없다.
>   - 크기를 변경할 수 없으므로 새로운 배열을 생성해서 데이터를 복사해야한다.
>   - 실행속도를 향상시키기 위해서는 충분히 큰 크기의 배열을 생성해야 하므로 메모리가 낭비된다.
>
> 2. 비순차적인 데이터의 추가 또는 삭제에 시간이 많이 걸린다.
>   - 차례대로 데이터를 추가하고 마지막에서부터 데이터를 삭제하는 것은 빠르지만, 배열의 중간에 데이터를 추가하려면 빈자리를 만들기 위해 다른 데이터들을 복사해서 이동해야 한다.

이러한 배열의 단점을 보완하기 위해서 LinkedList 자료구조가 고안되었다. 배열은 모든 데이터가 연속적으로 존재하지만 링크드 리스트는 불연속적으로 존재하는 데이터를 서로 연결한 형태로 구성되어 있다.
    
![배열과 링크드 리스트](/posts/20240510/Screenshot_1.png "배열과 링크드 리스트"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">배열과 링크드 리스트</div>

위의 그림에서 알 수 있듯이 링크드 리스트의 각 요소(node)들은 자신과 연결된 다음 요소에 대한 참조와 데이터로 구성되어 있다.

```java
class Node {
  Node next;   // 다음 요소의 주소를 저장
  Object obj;  // 데이터를 저장
}
```

링크드 리스트에서의 데이터 삭제는 간단하다. 삭제하고자하는 요소의 이전요소가 삭제하고자 하는 요소의 다음 요소를 참조하도록 변경하기만 하면 된다. 단 하나의 참조만 변경하면 삭제가 이루어지는 것이다. 배열처럼 데이터를 이동하기 위해 복사하는 과정이 없기 때문에 처리속도가 매우 빠르다.

![링크드 리스트에서의 데이터 삭제](/posts/20240510/Screenshot_2.png "링크드 리스트에서의 데이터 삭제"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">링크드 리스트에서의 데이터 삭제</div>

새로운 데이터를 추가할 때는 새로운 요소를 생성한 다음 추가하고자 하는 위치의 이전 요소의 참조를 새로운 요소에 대한 참조로 변경해주고, 새로운 요소가 그 다음 요소를 참조하도록 변경하기만 하면 되므로 처리속도가 매우 빠르다.

![링크드 리스트에서의 데이터 추가](/posts/20240510/Screenshot_3.png "링크드 리스트에서의 데이터 추가"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">링크드 리스트에서의 데이터 추가</div>

링크드 리스트는 이동방향이 단방향이기 때문에 다음 요소에 대한 접근은 쉽지만 이전요소에 대한 접근은 어렵다. 이 점을 보완한 것이 더블 링크드 리스트이다.
더블 링크드 리스트는 단순히 링크드 리스트에 참조변수를 하나 더 추가하여 다음 요소에 대한 참조뿐 아니라 요소에 대한 참조가 가능하도록 했을 뿐, 그 외에도 링크드 리스트와 같다.

더블 링크드 리스트는 링크드 리스트보다 각 요소에 대한 접근과 이동이 쉽기 때문에 링크드 리스트보다 더 많이 사용된다.

```java
class Node {
  Node next;      // 다음 요소의 주소를 저장
  Node previous;  // 이전 요소의 주소를 저장
  Object obj;     // 데이터를 저장
}
```

![링크드 리스트와 더블 링크드 리스트](/posts/20240510/Screenshot_4.png "링크드 리스트와 더블 링크드 리스트"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">링크드 리스트와 더블 링크드 리스트</div>

더블 링크드 리스트의 접근성을 보다 향상시킨 것이 '더블 써큘러 링크드 리스트'인데, 단순히 더블 링크드 리스트의 첫 번째 요소와 마지막 요소를 서로 연결시킨 것이다. 이렇게 하면, 마지막 요소의 다음요소가 첫번째 요소가 되고, 첫번째 요소의 이전 요소가 마지막 요소가 된다. 마치 TV의 마지막 채널에서 채널을 증가시키면 첫번째 채널로 이동하고 첫번째 채널에서 채널을 감소시키면 마지막 채널로 이동하는 것과 같다.

![더블 써큘러 링크드리스트](/posts/20240510/Screenshot_5.png "더블 써큘러 링크드리스트"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">더블 써큘러 링크드리스트</div>

실제로 LinkedList클래스는 이름과 달리 '링크드 리스트'가 아닌 '더블 링크드 리스트'로 구현되어 있는데, 이는 링크드 리스트의 단점인 낮은 접근성을 높이기 위한 것이다. 

지금까지 링크드 리스트의 기본원리에 대해 살펴보았고, 이제 예제를 통해 ArrayList와 LinkedList를 비교분석해보자.

<br>

## ArrayList와 LinkedList 비교
---
LinkedList 역시 List인터페이스를 구현했기 때문에 ArrayList와 내부구현방법만 다를뿐 제공하는 메서드의 종류와 기능은 거의 같기 때문에 이에 대한 설명은 생략하고, 대신 두 가지 예제를 통해 ArrayList와 LinkedList의 성능차이를 비교하고, 그 결과를 살펴보자.

### 예제1

```java
import java.util.*;

public class ArrayListLinkedListTest {
  public static void main(String args[]) {
    // 추가할 데이터의 개수를 고려하여 충분히 잡아야한다.
    ArrayList al = new ArrayList(2000000);
    LinkedList ll = new LinkedList();

    System.out.println("= 순차적으로 추가하기 =");
    System.out.println("ArrayList : " + add1(al));
    System.out.println("LinkedList : " + add1(ll));
    System.out.println();
    System.out.println("= 중간에 추가하기 =");
    System.out.println("ArrayList : " + add2(al));
    System.out.println("LinkedList : " + add2(ll));
    System.out.println();
    System.out.println("= 중간에서 삭제하기 =");
    System.out.println("ArrayList : " + remove2(al));
    System.out.println("LinkedList : " + remove2(ll));
    System.out.println();
    System.out.println("= 순차적으로 삭제하기 =");
    System.out.println("ArrayList : " + remove1(al));
    System.out.println("LinkedList : " + remove1(ll));
  }
}

public static long add1(List list) {
  long start = System.currentTimeMillis();
  for(int i = 0; i < 1000000; i++) list.add(i + "");
  long end = System.currentTimeMillis();
  return end - start;
}

public static long add2(List list) {
  long start = System.currentTimeMillis();
  for(int i = 0; i < 10000; i++) list.add(500, "X");
  long end = System.currentTimeMillis();
  return end - start;
}

public static long remove1(List list) {
  long start = System.currentTimeMillis();
  for(int i = list.size() - 1; i >= 0; i--) list.remove(i);
  long end = System.currentTimeMillis();
  return end - start;
}

public static long remove2(List list) {
  long start = System.currentTimeMillis();
  for(int i = 0; i < 10000; i++) list.remove(i);
  long end = System.currentTimeMillis();
  return end - start;
}

/** 결과

= 순차적으로 추가하기 =
ArrayList : 284
LinkedList : 406

= 중간에 추가하기 =
ArrayList : 3453
LinkedList : 16

= 중간에서 삭제하기 =
ArrayList : 2641
LinkedList : 234

= 순차적으로 삭제하기 =
ArrayList : 0
LinkedList : 31

*/
```

위 예제를 통해 우리는 2가지의 결론을 도출해낼 수 있다.

* __결론1 : 순차적으로 추가/삭제하는 경우에는 ArrayList가 LinkedList보다 빠르다.__  
<br>단순히 저장하는 시간만을 비교할 수 있도록 하기 위해 ArrayList를 생성할 때는 저장할 데이터의 개수만큼 충분한 초기용량을 확보해서, 저장공간이 부족해 새로운 ArrayList를 생성해야하는 상황이 발생하지 않도록 했다. 만일 ArrayList의 크기가 충분하지 않으면, 새로운 크기의 ArrayList를 생성하고 데이터를 복사하는 일이 발생하게 되므로 순차적으로 데이터를 추가해도 ArrayList보다 LinkedList가 더 빠를 수 있다.<br><br>순차적으로 삭제한다는 것은 마지막 데이터부터 역순으로 삭제해나간다는 것을 의미하며, ArrayList는 마지막 데이터부터 삭제할 경우 각 요소들의 재배치가 필요하지 않기 때문에 상당히 빠르다. 단지 마지막 요소의 값을 null로만 바꾸면 되기 때문이다.

* __결론2 : 중간 데이터를 추가/삭제하는 경우에는 LinkedList가 ArrayList보다 빠르다.__  
<br>중간 요소를 추가 또는 삭제하는 경우, LinkedList는 각 요소간의 연결만 변경해주면 되기 때문에 처리속도가 상당히 빠르다. 반면에 ArrayList는 각 요소들을 재배치하여 추가할 공간을 확보하거나 빈 공간을 채워야하기 때문에 처리속도가 늦다.<br><br>예제에서는 ArrayList와 LinkedList의 차이를 비교하기 위해 데이터의 개수를 크게 잡았는데, 사실 데이터의 개수가 그리 크지 않다면 어느 것을 사용해도 큰 차이가 나지는 않는다. 그래도 ArrayList와 LinkedList의 장단점을 잘 이해하고 상황에 따라 적합한 것을 선택해서 사용하는 것이 좋다.

### 예제2
```java
import java.util.*;

public class ArrayListLinkedListTest2 {
  public static void main(String args[]) {
    ArrayList al = new ArrayList(1000000);
    LinkedList ll = new LinkedList();
    add(al);
    add(ll);

    System.out.println("= 접근시간테스트 =");
    System.out.println("ArrayList : " + access(al));
    System.out.println("LinkedList : " + access(ll));
  }

  public static void add(List list) {
    for(int i = 0; i < 100000; i++) list.add(i + "");
  }

  public static long access(List list) {
    long start = System.currentTimeMillis();
    for(int i = 0; i < 10000; i++) list.get(i);
    long end = System.currentTimeMillis();
    return end - start;
  }
}

/** 결과

= 접근시간테스트 =
ArrayList : 1
LinkedList : 317

 */
```

배열의 경우 만일 인덱스가 n인 원소의 값을 얻어 오고자 한다면 단순히 아래와 같은 수식을 계산함으로써 해결된다.

> 인덱스가 n인 데이터의 주소 = 배열의 주소 + n * 데이터 타입의 크기

아래와 같이 Object배열이 선언되었을 때 arr[2]에 저장된 값을 읽으려 한다면 n은 2, 모든 참조형 변수의 크기는 4byte이고 생성된 배열의 주소는 0x100이므로 3번째 데이터가 저장되어 있는 주소는 0x100 + 2 * 4 = 0x108이 된다.

![배열의 메모리구조](/posts/20240510/Screenshot_6.png "배열의 메모리구조"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">배열의 메모리구조</div>

배열은 각 요소들이 연속적으로 메모리상에 존재하기 때문에 이처럼 간단한 계산만으로 원하는 요소의 주소를 얻어서 저장된 데이터를 곧바로 읽어올 수 있지만, LinkedList는 불연속적으로 위치한 각 요소들이 서로 연결된 것이라 처음부터 n번째 데이터까지 차례대로 따라가야만 원하는 값을 얻을 수 있다. 그래서 LinkedList는 저장해야하는 데이터의 개수가 많아질수록 데이터를 읽어 오는 시간, 즉 접근시간이 길어진다는 단점이 있다.

<table border="1" width="100%">
    <colgroup>
        <col style="width:20%">
        <col style="width:20%">
        <col style="width:20%">
        <col style="width:40%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">컬렉션</th>
        <th style="text-align:center;">읽기(접근시간)</th>
        <th style="text-align:center;">추가 / 삭제</th>
        <th style="text-align:center;">비고</th>
    </tr>
    <tr>
        <td style="text-align:center;">
            ArrayList
        </td>
        <td style="text-align:center;">
            빠르다
        </td>
        <td style="text-align:center;">
            느리다
        </td>
        <td style="text-align:left;">
            순차적인 추가삭제는 더 빠름.
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">
            LinkedList
        </td>
        <td style="text-align:center;">
            느리다
        </td>
        <td style="text-align:center;">
            빠르다
        </td>
        <td style="text-align:left;">
            데이터가 많을수록 접근성이 떨어짐
        </td>
    </tr>
</table>

다루고자 하는 데이터의 개수가 변하지 않는 경우라면, ArrayList가 최상의 선택이 되겠지만, 데이터 개수의 변경이 잦다면 LinkedList를 사용하는 것이 더 나은 선택이 될 것이다. 두 클래스의 장점을 이용해서 두 클래스를 조합해서 사용하는 방법도 생각해 볼 수 있다. 처음에 작업하기 전에 데이터를 저장할 때는 ArrayList를 사용한 다음, 작업할 때는 LinkedList로 데이터를 옮겨서 작업하면 좋은 효율을 얻을 수 있을 것이다.

```java
ArrayList al = new ArrayList(1000000);
for(int i = 0; i < 100000; i++) al.add(i + "");

LinkedList ll = new LinkedList(al);
for(int i = 0; i < 1000; i++) ll.add(500, "X");
```

컬렉션 프레임워크에 속한 대부분의 컬렉션 클래스들은 이처럼 서로 변환이 가능한 생성자를 제공하므로 이를 이용하면 간단히 다른 컬렉션 클래스로 데이터를 옮길 수 있다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  