---
title: "[Java] 형식화 클래스"
categories:
  - Java
tags:
  - Java
  - 자바
  - Formatting class
  - 형식화 클래스
toc: true
toc_sticky: true
toc_label: "목차"
comments: true
---

평소 개발하면서 숫자 또는 날짜 데이터를 계산할 때 애먹은 경험이 많다. 이럴 때 형식화 클래스를 활용하면 보다 편리하게 처리할 수 있는데, 그 중 대표적인 몇가지를 살펴보도록 하자.

# DecimalFormat
---
형식화 클래스 중에서 숫자를 형식화 하는데 사용되는 것이 DecimalFormat이다. DecimalFormat을 이용하면 숫자 데이터를 정수, 부동소수점, 금액 등의 다양한 형식으로 표현할 수 있으며, 반대로 일정한 형식의 텍스트 데이터를 숫자로 쉽게 변환하는 것도 가능하다.

DecimalFormat을 사용하는 방법은 간단하다. 먼저 원하는 출력형식의 패턴을 작성하여 DecimalFormat인스턴스를 생성한 다음, 출력하고자 하는 문자열로 format메서드를 호출하면 원하는 패턴에 맞게 변환된 문자열을 얻게 된다.

```java
double number = 1234567.89;
DecimalFormat df = new DecimalFormat("#.#E0");
String result = df.format(number);
```

이제 예제를 통해 DecimalFormat의 다양한 활용방법을 알아보자.

```java
import java.text.*;

class DecimalFormatEx1 {
  public static void main(String[] args) throws Exception {
    double number = 1234567.89;
    String[] pattern = {
      "0",
      "#",
      "0.0",
      "#.#",
      "0000000000.0000",
      "##########.####",
      "#.#-",
      "-#.#",
      "#,###.##",
      "#,####.##",
      "#E0",
      "0E0",
      "##E0",
      "00E0",
      "####E0",
      "0000E0",
      "#.#E0",
      "0.0E0",
      "0.000000000E0",
      "00.00000000E0",
      "000.0000000E0",
      "#.#########E0",
      "##.########E0",
      "###.#######E0",
      "#,###.##_;#,###.##-",
      "#.#%",
      "#.#\u2030",
      "\u00A4 #,###",
      "'#'#,###",
      "''#,###",
    };

    for(int i = 0; i < pattern.length; i++) {
      DecimalForamt df = new DecimalFormat(pattern[i]);
      System.out.printf("%19s : %s\n",pattern[i], df.format(number));
    }
  }
}
```

![제목](/assets/img/posts/20240401/example-1.png "결과"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">결과</div>

위 예제는 자주 사용될 만한 패턴들을 테스트한 것이다. 각 패턴에 의한 결과를 비교해 보고 이 패턴들을 변형하여 새로운 패턴을 만들어 테스트해보자.

```java
import java.text.*;

class DecimalFormatEx2 {
  public static void main(String[] args) {
    DecimalFormat df = new DecimalFormat("#,###.##");
    DecimalFormat df2 = new DecimalFormat("#.###E0");

    try {
      Number num = df.parse("1,234,567.89");
      System.out.print("1,234,567.89" + " -> ");

      double d = num .doubleValue();
      System.out.print(d + " -> ");

      System.out.println(df2.format(num));
    } catch(Exception e) {

    }
  }
}

/* 결과
1,234,567.89 - > 1234567.89 -> 1.235E6
*/
```

패턴을 이용해서 숫자를 다르게 변환하는 예제이다. parse메서드를 이용하면 기호와 문자가 포함된 문자열을 숫자로 쉽게 변환할 수 있다.

>💡 __참고__  
>Integer.parseInt메서드는 콤마(.)가 포함된 문자열을 숫자로 변환하지 못한다.

parse(String source)는 DecimalFormat의 조상인 NumberFormat에 정의된 메서드이며, 이 메서드의 선언부는 다음과 같다.

```java
public Number parse(String source) throws ParseException
```

Number클래스는 Integer, Double과 같은 숫자를 저장하는 래퍼 클래스의 조상이며, doubleValue()는 Number에 저장된 값을 double형의 값으로 변환하여 반환한다. 이 외에도 intValue(), floatValue()등의 메서드가 Number클래스에 정의되어 있다.

<br>

# SimpleDateFormat
---
이전 포스팅에서 날짜를 계산할 때 Date와 Calendar를 사용해서 날짜를 계산하는 방법을 살펴보았는데, 이제는 출력하는 방법을 알아보자.
Date와 Calendar만으로 날짜 데이터를 원하는 형태로 다양하게 출력하는 것은 불편하고 복잡하다. 그러나 SimpleDateFormat을 사용하면 이러한 문제들이 간단히 해결된다.

>💡 __참고__  
>DateFormat은 추상클래스로 SimpleDateFormat의 조상이다. DateFormat은 추상클래스이므로 인스턴스를 생성하기 위해서는 getDateInstance()와 같은 static메서드를 이용해야 한다. getDateInstance()에 의해서 반환되는 것은 DateFormat을 상속받아 완전하게 구현한 SimpleDateFormat인스턴스이다.

SimpleDateFormat을 사용하는 방법은 간단하다. 먼저 원하는 출력형식의 패턴을 작성하여 SimpleDateFormat인스턴스를 생성한 다음, 출력하고자 하는 Date인스턴스를 가지고 format(Date d)를 호출하면 지정한 출력형식에 맞게 변환된 문자열을 얻게 된다.

```java
Date today = new Date();
SimpleDateFormat df = new SimpleDateFormat("yyyy-MM-dd");

// 오늘 날짜를 yyyy-MM-dd형태로 변환하여 반환한다.
String result = df.format(today);
```

예제를 통해 다양한 활용방법 살펴보자.

```java
import java.util.*;
import java.text.*;

class DateFormatEx1 {
  public static void main(String[] args) {
    Date today = new Date();

    SimpleDateFormat sdf1, sdf2, sdf3, sdf4;
    SimpleDateFormat sdf5, sdf6, sdf7, sdf8, sdf9;

    sdf1 = new SimpleDateFormat("yyyy-MM-dd");
    sdf2 = new SimpleDateFormat("''yy년 MMM dd일 E요일");
    sdf3 = new SimpleDateFormat("yyyy-MM-dd HH:mm:ss.SSS");
    sdf4 = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss a");

    sdf5 = new SimpleDateFormat("오늘은 올 해의 D번째 날입니다.");
    sdf6 = new SimpleDateFormat("오늘은 이 달의 d번째 날입니다.");
    sdf7 = new SimpleDateFormat("오늘은 올 해의 w번째 주입니다.");
    sdf8 = new SimpleDateFormat("오늘은 이 달의 W번째 주입니다.");
    sdf9 = new SimpleDateFormat("오늘은 이 달의 F번째 E요일입니다.");

    System.out.println(sdf1.format(today));
    System.out.println(sdf2.format(today));
    System.out.println(sdf3.format(today));
    System.out.println(sdf4.format(today));
    System.out.println();
    System.out.println(sdf5.format(today));
    System.out.println(sdf6.format(today));
    System.out.println(sdf7.format(today));
    System.out.println(sdf8.format(today));
    System.out.println(sdf9.format(today));
  }
}

/* 결과
2024-04-01
'24년 04월 01일 월요일
2024-04-01 14:46:49.189
2024-04-01 02:46:49. 오후

오늘은 올 해의 92번째 날입니다.
오늘은 이 달의 1번째 날입니다.
오늘은 올 해의 14번째 주입니다.
오늘은 이 달의 1번째 주입니다.
오늘은 이 달의 1번째 Mon요일입니다.
*/
```

<br>

# ChoiceFormat
---
ChoiceFormat은 특정 범위에 속하는 값을 문자열로 변환해준다. 연속적 또는 불연속적인 범위의 값들을 처리하는데 있어서 if문이나 switch문은 적절하지 못한 경우가 많다. 이럴 때 ChoiceFormat을 잘 사용하면 복잡하게 처리될 수 밖에 없었던 코드를 간단하고 직관적으로 만들 수 있다.

```java
import java.text.*;

class ChoiceFormatEx1 {
  public static void main(String[] args) {
    double[] limits = {60, 70, 80, 90}; // 낮은 값부터 큰 값의 순서로 적어야한다.
    // limits, grades간의 순서와 개수를 맞추어야 한다.
    String[] grades = {"D", "C", "B", "A"};

    int[] scores = { 100, 95, 88, 70, 52, 60, 70};

    ChoiceFormat form = new ChoiceFormat(limits, grades);

    for(int i = 0; i < scores.length; i++) {
      System.out.println(scores[i] + ":" + form.format(scores[i]));
    }
  }
}
```

두 개의 배열이 사용되었는데 하나(limits)는 범위의 경계값을 저장하는데 사용하였고, 또 하나(grades)는 범위에 포함된 값을 치환할 문자열을 저장하는데 사용되었다.
경계값은 double형으로 반드시 모두 오름차순으로 정렬되어 있어야 하며, 치환 될 문자열의 개수는 경계값에 의해 정의된 범위의 개수와 일치해야한다. 그렇지 않으면 IllegalArgumentException 이 발생한다.

다음 예제를 보자.

```java
import java.text.*;

class ChoiceFormatEx2 {
  public static void main(String[] args) {
    String pattern = "60#D|70#C|80<B|90#A";
    int[] scores = { 91, 90, 80, 88, 70, 52, 60};

    ChoiceFormat form = new ChoiceFormat(pattern);

    for(int i = 0; i < scores.length; i++) {
      System.out.println(scores[i] + ":" + form.format(scores[i]));
    }
  }
}
```

이 예제는 이전 예제를 패턴방법으로 사용하도록 변경한 것이다. 배열 대신 패턴을 사용해서 보다 간결하게 처리할 수도 있다. 패턴은 구분자로 '#'와 '<' 두 가지를 제공하는데 'limit#value'의 형태로 사용한다. '#'는 경계값을 범위에 포함시키지만, '<'는 포함시키지 않는다.

<br>

# MessageFormat
---
MessageFormat은 데이터를 정해진 양식에 맞게 출력할 수 있도록 도와준다. 데이터가 들어갈 자리를 마련해 놓은 양식을 미리 작성하고 프로그램을 이용해서 다수의 데이터를 같은 양식으로 출력할 때 사용하면 좋다. 예를 들어 고객들에게 보낼 안내문을 출력할 때 같은 양식에 받는 사람의 이름과 같은 데이터만 달라지도록 출력할 때, 또는 하나의 데이터를 다양한 양식으로 출력할 때 사용한다. 그리고 SimpleDateFormat의 parse처럼 MessageFormat의 parse를 이용하면 지정된 양식에서 필요한 데이터만을 손쉽게 추출해 낼 수도 있다.

MessageFormat의 예제는 다음과 같다.

```java
import java.text.*;

class MessageFormatEx1 {
  public static void main(String[] args) {
    String msg = "Name: {0} \nTel: {1} \nAge:{2} \nBirthday:{3}";

    Object[] arguments = {
      "이자바", "02-123-1234", "27", "07-09"
    };

    String result = MessageFormat.format(msg, arguments);
    System.out.println(result);
  }
}

/* 결과
Name: 이자바
Tel: 02-123-1234
Age:27
Birthday:07-09
*/
```

MessageFormat에 사용할 양식인 문자열 msg를 작성할 때 '{숫자}'로 표시된 부분이 데이터가 출력될 자리이다. 이 자리는 순차적일 필요는 없고 여러번 반복해서 사용할 수도 있다. 여기에 사용되는 숫자는 배열처럼 인덱스가 0부터 시작하며 양시게 들어갈 데이터는 객체배열인 arguments에 지정되어 있음을 알 수 있다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  