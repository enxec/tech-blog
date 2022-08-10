---
title: "[Java] JDK & JRE & JVM"
categories:
  - Java
tags:
  - java
  - 자바
  - JDK
  - JRE
  - JVM
toc: true
toc_sticky: true
toc_label: "목차"
---
![JDK & JRE & JVM](/blog/assets/img/posts/20220626/jdk-jre.png "JDK & JRE & JVM"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">JDK & JRE & JVM</div>

<br>

# JDK?
---
JDK는 Java Development Kit의 약자로 Java 애플리케이션을 만드는데 사용되는 소프트웨어 개발 환경이자 자바용 SDK이다. Java개발자는 JDK를 여러 OS환경에서 사용할 수 있으며, Java 프로그램을 코드화하고 실행하는데 사용한다.  
JDK에는 JAVA 프로그램을 작성하는데 필요한 도구와 이를 실행하는데 필요한 JRE가 포함되어 있으며, 컴파일러(javac), JAVA 애플리케이션 시작 프로그램, 애플릿 뷰어 등도 포함되어 있다.

<br>

# JRE?
---
JRE는 Java Runtime Environment의 약자로 다른 소프트웨어를 실행하도록 설계된 소프트웨어의 일부이다. 여기에는 클래스 라이브러리, 로더클래스 및 JVM이 포함되어 있다.  
간단히 말해, Java 프로그램을 실행하려면 JRE가 필요하다. 기존에 JDK를 들고 있다면 위에서 언급했듯이 JDK는 JRE를 포함하고 있기 때문에 따로 설치가 필요하지않다. 하지만 개발자가 아닌 경우라면 JDK가 존재하지 않을테니 JRE를 따로 설치해주어야한다.

<br>

# JDK & JRE 상세구조
---
JDK와 JRE의 각각 구성요소에 대한 정보는 오라클 문서 참고.  
[JDK & JRE docs - Oracle](https://docs.oracle.com/javase/8/docs/technotes/tools/windows/jdkfiles.html)

![JDK & JRE Detail Structure](/blog/assets/img/posts/20220626/jdk-jre-structure.png "JDK & JRE Detail Structure"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">JDK & JRE Detail Structure</div>

<br>

# JVM?
---
JVM은 Java Virtual Machine의 약어로 자바를 실행하기 위한 가상 컴퓨터라고 할 수있다. 가상 기계는 소프트웨어로 구현된 하드웨어를 뜻하는 넒은 의미의 용어이며 컴퓨터의 성능이 향상됨에 따라 점점 더 많은 하드웨어들이 소프트웨어화되어 컴퓨터 속으로 들어가고 있다.  
요약해 JVM은 컴퓨터 속의 컴퓨터라고 생각하면 된다. 자바로 작성된 애플리케이션은 모두 JVM에서만 실행되기 때문에, 자바 애플리케이션이 실행되기 위해서는 반드시 JVM이 필요하다.

<br>

# JVM 특징
---
일반 애플리케이션의 코드는 OS만 거치고 하드웨어로 전달되는데 비해 Java애플리케이션은 JVM을 한번 더 거치기 때문에, 그리고 하드웨어에 맞게 완전히 컴파일된 상태가 아니고 실행 시에 해석되기 때문에 속도가 느리다는 단점을 가지고 있다. 그러나 요즘엔 바이트 코드(컴파일된 자바코드)를 하드웨어의 기계어로 바로 변환해주는 JIT컴파일러와 향상된 최적화 기술이 적용되어 속도의 격차를 많이 줄였다.

![일반프로그램과 자바프로그램 실행 프로세스 비교](/blog/assets/img/posts/20220626/general-java-compare.png "일반프로그램과 자바프로그램 실행 프로세스 비교"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">일반프로그램과 자바프로그램 실행 프로세스 비교</div>
위 이미지를 보면 일반 프로그램은 OS와 바로 맞붙어 있기 때문에 OS종속적이다. 때문에 다른 OS에서 실행시키기 위해서는 애플리케이션을 그 OS에 맞게 변경해주어야한다. 반면 자바 프로그램은 JVM하고만 상호작용을 하기 때문에 OS와 하드웨어에 독립적이라 다른 OS에서도 프로그램 변경없이 실행이 가능한 것이다. 단, JVM은 OS에 종속적이여서 해당 OS에서 실행가능한 JVM이 필요하다.

![JVM은 각각의 OS에 맞는 버전이 필요하다.](/blog/assets/img/posts/20220626/java-os-relation.png "JVM은 각각의 OS에 맞는 버전이 필요하다."){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">JVM은 각각의 OS에 맞는 버전이 필요하다.</div>

<br>

# 바이트 코드
---
JVM과 중요시 되는 요소는 바이트 코드이다. 바이트 코드는 JVM에서만 실행되는 기계어로서, 어떤 CUP와도 관계없는 코드이다.  
자바 컴파일러는 자바 소스 프로그램을 컴파일하여 바이트 코드로 된 클래스 파일을 생성한다. 이 클래스 파일은 컴퓨터의 CPU에 의해 직접 실행되지 않고, JVM이 인터프리터 방식으로 실행시킨다. 이 클래스 파일은 어떤 OS를 탑재하든 CPU의 종류가 무엇이든, 어떤 하드웨어든 상관없이 JVM만 있으면 바로 실행 가능하며, 실행시 JVM의 JIT컴파일러에 의해 바이너리 코드로 변환되어 프로그램을 실행시킨다.  

__알아두기__  
- 바이너리 코드
    >이진코드라고도 하며, 컴퓨터가 인식할 수 있는 0과 1로 구성되어 있다.  
- 기계어
    >0과 1로 이루어진 바이너리 코드이며, 모든 이진코드가 기계어인 컷은 아니다. 또한 기계어는 특정한 언어가 아니라 CPU가 이해하는 명령어 집합으로, CPU 제조사마다 기계어가 다를 수 있다.

<br>

# JVM 구조
---
![JVM Structure](/blog/assets/img/posts/20220626/jvm-structure.png "JVM Structure"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">JVM Structure</div>
>JVM의 구조
- 클래스 로더
    >JVM 내로 클래스 파일을 로드하고, 링크를 통해 배치하는 작업을 수행하는 모듈이다.
    런 타임시 동적으로 클래스를 로드하고 jar 파일 내 저장된 클래스들을 JVM 위에 탑재한다.
    즉, 클래스를 처음으로 참조할 때, 해당 클래스를 로드하고 링크한는 역할을 한다.
- 실행 엔진
    >클래스를 실행시키는 역할이다.
    클래스 로더가 JVM내의 런타임 데이터 영역에 바이트 코드를 배치시키고, 이것은 실행 엔진에 의해 실행된다.
    자바 바이트 코드는 기계가 바로 수행할 수 있는 언어보다는 비교적 인간이 보기 편한 형태로 기술된 것이다. 그래서 실행 엔진은 이와 같은 바이트 코드를 실제로 JVM 내부에서 기계가 실행할 수 있는 형태로 변경한다.
    - 인터프리터
        >실행 엔진은 자바 바이트 코드를 명령어 단위로 읽어서 실행한다.
        하지만 한 줄씩 수행하기 때문에 느리다는 단점이 있다.
    - JIT 컴파일러
        >JIT 컴파일 또는 동적 번역이라고하며, 프로그램을 실제 실행하는 시점에 기계어로 번역하는 컴파일러다.
    - 가비지 컬렉터
        >더이상 사용되지 않는 인스턴스를 찾아 메모리에서 삭제함.
- 런타임 데이터 영역
    >프로그램을 수행하기 위해 OS에서 할당받은 메모리 공간
    ![Runtime Area](/blog/assets/img/posts/20220626/runtime-area.png "Runtime Area"){: width="100%"}
    <div style="color: gray; text-align: center; margin-bottom: 30px;">Runtime Area</div>
    - PC Register
        >Thread가 시작될 때 생성되며 생성될 때마다 생성되는 공간으로, 스레드마다 하나씩 존재한다.
        Thread가 어떤 부분을 어떤 명령으로 실행해야할 지에 대한 기록을 하는 부분으로 현재 수행 중인 JVM 명령의 주소를 갖는다.
    - JVM 스택 영역
        >프로그램 실행과정에서 임시로 할당되었다가 메소드를 빠져나가면 바로 소멸되는 특성의 데이터를 저장하기 위한 영역이다.
        각종 형태의 변수나 임시 데이터, 스레드나 메소드의 정보를 저장한다.
        메소드 호출 시마다 각각의 스택 프레임(그 메서드만을 위한 공간)이 생성된다. 메서드 수행이 끝나면 프레임 별로 삭제를 한다.
        메소드 안에서 사용되는 값들을 저장한다. 또 호출된 메소드의 매개변수, 지역변수, 리턴 값 및 연산 시 일어나는 값들을 임시로 저장한다.
    - Native method stack
        >자바 프로그램이 컴파일되어 생성되는 바이트 코드가 아닌 실제 실행할 수 있는 기계어로 작성된 프로그램을 실행시키는 영역.
        JAVA가 아닌 다른 언어로 작성된 코드를 위한 공간.
        Java Native Interface를 통해 바이트 코드로 전환하여 저장하게 된다.
        일반 프로그램처럼 커널이 스택을 잡아 독자적으로 프로그램을 실행시키는 영역이다.
    - Method Area (= Class Area = Static area)
        >클래스 정보를 처음 메모리 공간에 올릴 때 초기화되는 대상을 저장하기 위한 메모리 공간이며
        멤버변수, 메서드, 그리고 타입에 대한 정보가 저장된다.
        - Runtime Constant Pool
            >스태틱 영역에 존재하는 별도의 관리영역.
            상수 자료형을 저장하여 참조하고 중복을 막는 역할을 수행한다.
    - Heap
        >객체를 저장하는 가상메모리 공간. new 연산자로 생성되는 객체와 배열을 저장하며
        Class Area(Static Area)에 올라온 클래스들만 객체로 생성할 수 있다.
        힙 영역은 다음과 같이 세 부분으로 나뉘어 진다.
        ![Heap Structure](../assets/img/posts/20220626/heap-structure.jpg "Heap Structure"){: width="100%"}
        <div style="color: gray; text-align: center; margin-bottom: 30px;">Heap Structure</div>
        - Young 영역
            >이곳의 인스턴스들은 추후 가비지 콜렉터에 의해 사라진다.
            생명 주기가 짧은 “젊은 객체”를 GC 대상으로 하는 영역이다.
            여기서 일어나는 가비지 콜렉트를 Minor GC 라고 한다.
            - Eden
                >객체들이 최초로 생성되는 공간
            - Survivor 0, 1
                >Eden에서 참조되는 객체들이 저장되는 공간
        - Old 영역
            >이곳의 인스턴스들은 추후 가비지 콜렉터에 의해 사라진다.
            생명 주기가 긴 “오래된 객체”를 GC 대상으로 하는 영역이다.
            여기서 일어나는 가비지 콜렉트를 Major GC 라고 한다. Minor GC에 비해 속도가 느리다.
            Young Area에서 일정시간 참조되고 있는, 살아남은 객체들이 저장되는 공간이다.
        - Permanent 영역
            >직역하면 영구적인 세대이다.
            생성된 객체들의 정보의 주소값이 저장된 공간이다. 클래스 로더에 의해 load되는 Class, Method 등에 대한 Meta 정보가 저장되는 영역이고 JVM에 의해 사용된다.
            Reflection을 사용하여 동적으로 클래스가 로딩되는 경우에 사용된다.  

---

읽어주셔서 감사합니다. 😊

__Reference__  
[JVM이란? 개념 및 구조 - Tistory](https://doozi0316.tistory.com/entry/1%EC%A3%BC%EC%B0%A8-JVM%EC%9D%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%9E%90%EB%B0%94-%EC%BD%94%EB%93%9C%EB%8A%94-%EC%96%B4%EB%96%BB%EA%B2%8C-%EC%8B%A4%ED%96%89%ED%95%98%EB%8A%94-%EA%B2%83%EC%9D%B8%EA%B0%80)