---
title: "[Java] Stack과 Queue"
description: 
author: Enxec
date: 2024-06-14
categories: [Language, Java]
tags: [java, 자바, 컬렉션 프레임워크, Collections Framework, Stack, Queue]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/java-logo.png
  lqip: 
  alt: 
---

자바에서 제공하는 Stack과 Queue에 대해 알아보기 이전에 두 자료구조의 기본개념과 특징에 대해 먼저 알아보자.

## 기본개념과 특징
---
스택은 마지막에 저장한 데이터를 가장 먼저 꺼내게 되는 LIFO 구조로 되어있고, 큐는 처음에 저장한 데이터를 가장 먼저 꺼내게 되는 FIFO 구조로 되어 있다. 쉽게 얘기하자면 스택은 동전통과 같은 구조로 양 옆과 바닥이 막혀 있어서 한 방향으로만 뺄 수 있는 구조이고, 큐는 양 옆만 막혀 있고 위아래로 뚫려 있어서 한 방향으로는 넣고 한 방향으로는 빼는 파이프와 같은 구조로 되어 있다.

![스택과 큐](/posts/20240614/Screenshot_1.png "스택과 큐"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">스택과 큐</div>

예를 들어 스택에 0, 1, 2의 순서로 데이터를 넣었다면 꺼낼 때는 2, 1, 0의 순서로 꺼내게 된다. 즉, 넣은 순서와 꺼낸 순서가 뒤집어지게 되는 것이다. 이와 반대로 큐에 0, 1, 2의 순서로 데이터를 넣었다면 꺼낼 때 역시 0, 1, 2의 순서로 꺼내게 된다. 순서의 변경 없이 먼저 넣은 것을 먼저 꺼내게 되는 것이다.

그렇다면 스택과 큐를 구현하기 위해서는 어떤 컬렉션 클래스를 사용하는 것이 좋을까? 순차적으로 데이터를 추가하고 삭제하는 스택에는 ArrayList와 같은 배열기반의 컬렉션 클래스가 적합하지만, 큐는 데이터를 꺼낼 때 항상 첫 번째 저장된 데이터를 삭제하므로, ArrayList와 같은 배열기반의 컬렉션 클래스를 사용한다면 데이터를 꺼낼 때마다 빈 공간을 채우기 위해 데이터의 복사가 발생하므로 비효율적이다. 그래서 큐는 ArrayList보다 데이터의 추가/삭제가 쉬운 LinkedList로 구현하는 것이 더 적합하다.

<table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
        <col style="width:30%;">
        <col style="width:70%;">
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
                boolean empty()
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Stack이 비어있는지 알려준다.
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Object peek()
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Stack의 맨 위에 저장된 객체를 반환. pop()과 달리 Stack에서 객체를 꺼내지는 않음. (비었을 때는 EmptyStackException 발생)
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Object pop()
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Stack의 맨 위에 저장된 객체를 꺼낸다. (비었을 때는 EmptyStack Exception 발생)
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Object push(Object item)
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Stack에 객체(item)를 저장한다.
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                int search(Object o)
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Stack에서 주어진 객체(o)를 찾아서 그 위치를 반환. 못 찾으면 -1을 반환. (배열과 달리 위치는 0이 아닌 1부터 시작)
            </td>
        </tr>
    </tbody>
</table>

<p align="center">▲ Stack의 메서드</p>
<br>

<table style="width:100%; border-collapse: collapse; table-layout: fixed;">
    <colgroup>
        <col style="width:30%;">
        <col style="width:70%;">
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
                boolean add(Object o)
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                지정된 객체를 Queue에 추가한다. 성공하면 true를 반환. 저장공간이 부족하면 illegalStateException 발생
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Object remove()
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Queue에서 객체를 꺼내 반환. 비어있으면 NoSuchElementException 발생
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Object element()
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                삭제 없이 요소를 읽어온다. peek와 달리 Queue가 비었을 때 NoSuchElementException 발생
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                boolean offer(Object o)
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Queue에 객체를 저장. 성공하면 true, 실패하면 false를 반환
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Object poll()
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Queue에서 객체를 꺼내서 반환. 비어있으면 null을 반환
            </td>
        </tr>
        <tr>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                Object peek()
            </td>
            <td style="text-align:left; border: 1px solid black; word-wrap: break-word; white-space: normal;">
                삭제 없이 요소를 읽어 온다. Queue가 비어있으면 null을 반환
            </td>
        </tr>
    </tbody>
</table>

<p align="center">▲ Queue의 메서드</p>
<br>

```java
import java.util.*;

class StackQueueEx {
    public static void main(String[] args) {
        Stack st = new Stack();
        Queue q = new LinkedList(); // Queue인터페이스의 구현체인 LinkedList를 사용

        st.push("0");
        st.push("1");
        st.push("2");

        q.offer("0");
        q.offer("1");
        q.offer("2");

        System.out.println("= Stack =");
        While(!st.empty()) {
            System.out.println(st.pop());
        }

        System.out.println("= Queue =");
        while(!q.isEmpty()) {
            System.out.println(q.poll());
        }
    }
}

/** 결과

= Stack =
2
1
0
= Queue =
0
1
2

 */
```

스택과 큐에 각각 "0", "1", "2" 를 같은 순서로 넣고 꺼내었을 때의 결과가 다른 것을 알 수 있다. 큐는 먼저 넣은 것이 먼저 꺼내지는 구조(FIFO)이기 때문에 넣을 때와 같은 순서이고, 스택은 먼저 넣은 것이 나중에 꺼내지는 구조(LIFO)이기 때문에 넣을 때의 순서와 반대로 꺼내진 것을 알 수 있다.

자바에서는 스택을 Stack클래스로 구현하여 제공하고 있지만 큐는 Queue인터페이스로만 정의해 놓았을 뿐 별도의 클래스를 제공하고 있지 않다. 대신 Queue인터페이스를 구현한 클래스들이 있어서 이 들 중의 하나를 선택해서 사용하면 된다.

<br>

### 인터페이스를 구현한 클래스 찾기
---
위의 예제에서와 같이 Queue인터페이스의 기능을 사용하고자 할 때는 다음과 같이 Java API 문서를 참고하면 된다. 아래 그림의 하단에 보면 'All Known Implementing Classes' 라는 항목이 있는데 여기에 나열된 클래스들이 바로 Queue 인터페이스를 구현한 클래스들이다.

![Java API 문서에 작성된 Queue](/posts/20240614/Screenshot_2.png "Java API 문서에 작성된 Queue"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">Java API 문서에 작성된 Queue</div>

각 클래스들은 각자 나름대로의 용도가 있겠지만 적어도 Queue인터페이스에 정의된 메서드를 모두 작성해 놓았으며, 이 메서드들에 대해서는 내용은 좀 다를 수 있겠지만 대부분 거의 같은 기능을 한다. 그래서 Queue 인터페이스에 정의된 기능을 사용하고 싶다면 'All Known Implementing Classes'에 적혀있는 클래스들 중에서 적당한 것을 하나 골라 잡아서 'Queue q = new LinkedList();' 와 같은 식으로 객체를 생성해서 사용하면 된다.

<br>

### Stack직접 구현하기
---
Stack은 컬렉션 프레임워크 이전부터 존재하던 것이기 때문에 ArrayList가 아닌 Vector로부터 상속받아 구현하였다. Stack의 실제코드를 이해하기 쉽게 약간 수정해서 MyStack을 만들어 보았다. 스택을 자바코드로 어떻게 구현하였는지 이해하는데 많은 도움이 될 것이다.

```java
import java.util.*;

class MyStack extends Vector {
    public Object push(Object item) {
        addElement(item);
        return item;
    }

    public Object pop() {
        Object obj = peek(); // Stack에 저장된 마지막 요소를 읽어온다.
        // 만일 Stack이 비어있으면 peek()메서드가 EmptyStackException을 발생시킨다.
        // 마지막 요소를 삭제한다. 배열의 index 0부터 시작하므로 1을 빼준다.
        removeElementAt(size() - 1);
        return obj;
    }

    public Object peek() {
        int len = size();

        if(len == 0) {
            throw new EmptyStackException();
        }

        // 마지막 요소를 반환한다. 배열의 index가 0부터 시작하므로 1을 빼준다.
        return elementAt(len - 1);
    }

    public boolean empty() {
        return size() == 0;
    }

    public int search(Object o) {
        int i = lastIndexOf(o); // 끝에서부터 객체를 찾는다.
                                // 반환값은 저장된 위치(배열의 index)이다.
        if(i >= 0) { // 객체를 찾은 경우
            return size() - i; // Stack은 맨 위에 저장된 객체의 index를 1로 정의하기 때문에
                               // 계산을 통해서 구한다.
        }
        return - 1; // 해당 객체를 찾지 못하면 -1을 반환한다.
    }
}
```

<br>

## 스택과 큐의 활용
---
지금까지 스택과 큐의 개념과 구현에 대해서 알아보았는데 이제는 스택과 큐를 어떻게 활용할 것인가에 대해서 살펴보자. 우리가 쉽게 찾아볼 수 있는 스택과 큐의 활용 예는 다음과 같다.

> - 스택의 활용 예  
>
>   수식계산, 수식괄호검사, 워드프로세서의 undo/redo, 웹브라우저의 뒤로/앞으로
>
> - 큐의 활용 예  
>
>   최근사용문서, 인쇄작업 대기목록, 버퍼

<br>

### 예제1
---
```java
import java.util.*;

public class StackEx1 {
    public static Stack back = new Stack();
    public static Stack forward = new Stack();

    public static void main(String[] args) {
        goURL("1.네이트");
        goURL("2.야휴");
        goURL("3.네이버");
        goURL("4.다음");

        printStatus();

        goBack();
        System.out.println("= 뒤로가기 버튼을 누른 후 =");
        printStatus();

        goBack();
        System.out.println("= '뒤로' 버튼을 누른 후 =");
        printStatus();

        goBack();
        System.out.println("= '앞으로' 버튼을 누른 후 =");
        printStatus();

        goBack();
        System.out.println("= 새로운 주소로 이동 후 =");
        printStatus();
    }

    public static void printStatus() {
        System.out.println("back:"+back);
        System.out.println("forward:"+forward);
        System.out.println("현재화면은 '" + back.peek()+"' 입니다.");
        System.out.println();
    }

    public static void goURL(String url) {
        back.push(url);
        if(!forward.empty()) {
            forward.clear();
        }
    }

    public static void goForward() {
        if(!forward.empty()) {
            back.push(forward.pop());
        }
    }

    public static void goBack() {
        if(!back.empty()) {
            forward.push(back.pop());
        }
    }  
}

/** 결과

back:[1.네이트, 2.야후, 3.네이버, 4.다음]
forward:[]
현재화면은 '4.다음' 입니다.

= 뒤로가기 버튼을 누른 후 =
back:[1.네이트, 2.야후, 3.네이버]
forward:[4.다음]
현재화면은 '3.네이버' 입니다.

= '뒤로' 버튼을 누른 후 =
back:[1.네이트, 2.야후]
forward:[4.다음, 3.네이버]
현재화면은 '2.야후' 입니다.

= '앞으로' 버튼을 누른 후 =
back:[1.네이트, 2.야후, 3.네이버]
forward:[4.다음]
현재화면은 '3.네이버' 입니다.

= 새로운 주소로 이동 후 =
back:[1.네이트, 2.야후, 3.네이버, codechobo.com]
forward:[]
현재화면은 'codechobo.com' 입니다.

 */
```

위 예제는 웹브라우저의 '뒤로', '앞으로' 버튼의 기능을 구현한 것이다. 이 기능을 구현하기 위해서는 2개의 스택을 사용해야한다. 웹브라우저로 위의 예제에서 구성한 시나리오대로 동작해보면 쉽게 이해할 수 있을 것이다.

<br>

### 예제2
---
```java
import java.util.*;

public class ExpValidCheck {
    public static void main(String[] args) {
        if(args.length != 1) {
            System.out.println("Usage : java ExpValidCheck \"EXPRESSION\"");
            System.out.println("Example : java ExpValidCheck \"((2+3)*1)+3\"");
            System.exit(0);
        }

        Stack st = new Stack();
        String expression = args[0];

        System.out.println("expression:"+expression);

        try {
            for(int i = 0; i < expression.length(); i++) {
                char ch = expression.charAt(i);

                if(ch=='(') {
                    st.push(ch+"");
                } else if(ch==')') {
                    st.pop();
                }
            }

            if(st.isEmpty()) {
                System.out.println("괄호가 일치합니다.");
            } else {
                System.out.println("괄호가 일치하지 않습니다.");
            }
        } catch (EmptyStackException e) {
            System.out.println("괄호가 일치하지 않습니다.");
        }
    }
}

/** 결과

Usage : java ExpValidCheck "EXPRESSION"
Example : java ExpValidCheck "((2+3)*1)+3"

expression:(2+3)*1

 */
```

입력한 수식의 괄호가 올바른지를 체크하는 예제이다. '('를 만나면 스택에 넣고 ')'를 만나면 스택에서 '('를 꺼낸다. ')'를 만나서 '('를 꺼내려 할 때 스택이 비어있거나 수식을 검사하고 난 후에도 스택이 비어있지 않으면 괄호가 잘못된 것이다.

')'를 만나서 '('를 꺼내려 할 때 스택이 비어있으면 EmptyStackException이 발생하므로 try - catch문을 이용해서 EmptyStackException이 발생하면 괄호가 일치하지 않는다는 메시지를 출력하도록 했다.

<br>

### 예제3
---
```java
import java.util.*;

class QueueEx1 {
    static Queue q = new LinkedList();
    static final int MAX_SIZE = 5;  // Queue에 최대 5개까지만 저장되도록 한다.

    public static void main(String[] args) {
        System.out.println("help를 입력하면 도움말을 볼 수 있습니다.");

        while(true) {
            System.out.println(">>");
            try {
                // 화면으로부터 라인단위로 입력받는다.
                Scanner s = new Scanner(System.in);
                String input = s.nextLine().trim();

                if("".equals(input)) continue;

                if(input.equalsIgnoreCase("q")) {
                    System.exit(0);
                } else if(input.equalsIgnoreCase("help")) {
                    System.out.println(" help - 도움말을 보여줍니다.");
                    System.out.println(" q 또는 Q - 프로그램을 종료합니다.");
                    System.out.println(" history - 최근에 입력한 명령어를 " + MAX_SIZE + "개 보여줍니다.");
                } else if(input.equalsIgnoreCase("history")) {
                    int i = 0;
                    // 입력받은 명령어를 저장하고,
                    save(input);

                    // LinkedList의 내용을 보여준다.
                    LinkedList tmp = (LinkedList)q;
                    ListIterator it = tmp.listIterator();

                    while(it.hasNext()) {
                        System.out.println(++i+"."+it.next());
                    }
                } else {
                    save(input);
                    System.out.println(input);
                }
            } catch(Exception e) {
                System.out.println("입력오류입니다.");
            }
        }
    }

    public static void save(String input) {
        // queue에 저장한다.
        if(!"".equals(input)) {
            q.offer(input);
        }

        // queue의 최대크기를 넘으면 제일 처음 입력된 것을 삭제한다.
        if(q.size() > MAX_SIZE) { // size()는 Collection인터페이스에 정의
            q.remove();
        }
    }
}

/** 결과

help를 입력하면 도움말을 볼 수 있습니다.
>>help
 help - 도움말을 보여줍니다.
 q 또는 Q - 프로그램을 종료합니다.
 history - 최근에 입력한 명령어를 5개 보여줍니다.
>>dir
dir
>>cd
cd
>>mkdir
mkdir
>>dir
dir
>>history
1.dir
2.cd
3.mkdir
4.dir
5.history
>>q

 */
```

이 예제는 유닉스의 history명령어를 Queue를 이용해서 구현한 것이다. history명령어는 사용자가 입력한 명령어의 이력을 순서대로 보여준다. 여기서는 최근 5개의 명령어만을 보여주는데 MAX_SIZE의 값을 변경함으로써 더 많은 명령어 입력기록을 남길 수 있다.

<br>

## PriorityQueue
---
Queue인터페이스의 구현체 중의 하나로, 저장한 순서에 관계없이 우선순이(priority)가 높은 것부터 꺼내게 된다는 특징이 있다. 그리고 null은 저장할 수 없다. null을 저장하면 NUllPointException이 발생한다.

PriorityQueue는 저장공간으로 배열을 사용하며, 각 요소를 '힙(heap)'이라는 자료구조의 형태로 저장한다. 힙은 잠시 후에 배울 이진 트리의 한 종류로 가장 큰 값이나 가장 작은 값을 빠르게 찾을 수 있다는 특징이 있다.

>💡 __참고__  
> 자료구조 힙은 JVM 힙과 이름만 같을 뿐, 다른 것이다.

```java
import java.util.*;

class PriorityQueueEx {
    public static void main(String[] args) {
        Queue pq = new PriorityQueue();
        pq.offer(3);  // pq.offer(new Integer(3)); 오토박싱
        pq.offer(1);
        pq.offer(5);
        pq.offer(2);
        pq.offer(4);
        System.out.println(pq); // pq의 내부 배열을 출력

        Object obj = null;

        // PriorityQueue에 저장된 요소를 하나씩 꺼낸다.
        while((obj = pq.poll()) != null) {
            System.out.println(obj);
        }
    }
}

/** 결과

[1, 2, 5, 3, 4]
1
2
3
4
5

 */
```

저장순서가 3,1,5,2,4인데도 출력결과는 1,2,3,4,5이다. 우선순위는 숫자가 작을수록 높은 것이므로 1이 가장 먼저 출력된 것이다. 물론 숫자뿐만 아니라 객체를 저장할 수도 있는데 그럴 경우 각 객체의 크기를 비교할 수 있는 방법을 제공해야한다. 예제에서는 정수를 사용했는데, 컴파일러가 Integer로 오토박싱 해준다. Integer와 같은 Number의 자손들은 자체적으로 숫자를 비교하는 방법을 정의하고 있기 때문에 비교 방법을 지정해 주지 않아도 된다.

참조변수 pq를 출력하면, PriorityQueue가 내부적으로 가지고 있는 배열의 내용이 출력되는데, 저장한 순서와 다르게 저장되었다. 앞서 설명한 것과 같이 힙이라는 자료구조의 형태로 저장된 것이라 그렇다.

<br>

## Deque(Double-Ended Queue)
---
Queue의 변형으로, 한 쪽 끝으로만 추가/삭제할 수 있는 Queue와 달리, Deque(덱, 또는 디큐라고 읽음)은 양쪽 끝에 추가/삭제가 가능하다. Deque의 조상은 Queue이며, 구현체로는 ArrayDeque과 LinkedList 등이 있다.

![큐와 덱](/posts/20240614/Screenshot_3.png "큐와 덱"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">큐와 덱</div>

덱은 스택과 큐를 하나로 합쳐놓은 것과 같으며 스택으로 사용할 수도 있고, 큐로 사용할 수도 있다. 위의 그림과 아래의 그림을 같이보면 어렵지않게 이해가능하다.

![덱의 메서드에 대응하는 큐와 스택의 메서드](/posts/20240614/Screenshot_4.png "덱의 메서드에 대응하는 큐와 스택의 메서드"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">덱의 메서드에 대응하는 큐와 스택의 메서드</div>

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  