---
title: "[Java] java.time 패키지"
categories:
  - Java
tags:
  - Java
  - 자바
  - java.time 패키지
  - java.time package
toc: true
toc_sticky: true
toc_label: "목차"
comments: true
---

# java.time 하위패키지
---
JDK 1.8 이전부터 사용해오던 Date와 Calendar의 단점을 해소하기 위해 1.8부터 java.time 패키지가 추가되었다.
이 패키지는 다음과 같이 4개의 하위 패키지를 가지고 있다.

<table border="1">
    <colgroup>
        <col style="width:20%">
        <col style="width:60%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">패키지</th>
        <th style="text-align:center;">설명</th>
    </tr>
    <tr>
        <td style="text-align:center;">java.time</td>
        <td style="text-align:left;">
            날짜와 시간을 다루는데 필요한 핵심 클래스들을 제공
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">java.time.chrono</td>
        <td style="text-align:left;">
            표준(ISO)이 아닌 달력 시스템을 위한 클래스들을 제공
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">java.time.format</td>
        <td style="text-align:left;">날짜와 시간을 파싱하고, 형식화하기 위한 클래스들을 제공</td>
    </tr>
    <tr>
        <td style="text-align:center;">java.time.temporal</td>
        <td style="text-align:left;">
            날짜와 시간의 필드(field)와 단위(unit)을 위한 클래스들을 제공
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">java.time.zone</td>
        <td style="text-align:left;">
            시간대(time-zone)와 관련된 클래스들을 제공
        </td>
    </tr>
</table>

위의 패키지들에 속한 클래스들의 가장 큰 특징은 String클래스처럼 '불변'이라는 것이다. 그래서 날짜나 시간을 변경하는 메서드들은 기존의 객체를 변경하는 대신 항상 변경된 새로운 객체를 반환한다. 기존 Calendar클래스는 변경가능하므로, 멀티쓰레드 환경에서 안전하지 못하다.
멀티쓰레드 환경에서는 동시에 여러 쓰레드가 같은 객체에 접근할 수 있기 때문에, 변경 가능한 객체는 데이터가 잘못될 가능성이 있으며, 이를 쓰레드에 안전(thread-safe)하지 않다고 한다.

# java.time 패키지의 핵심 클래스
---
날짜와 시간을 하나로 표현하는 Calendar클래스와 달리, java.time패키지에서는 날짜와 시간을 별도의 클래스로 분리해 놓았다. 시간을 표현할 때는 LocalTime클래스를 사용하고, 날짜를 표현할 때는 LocalDate 클래스를 사용한다. 그리고 날짜와 시간이 모두 필요할 때는 LocalDatetime 클래스를 사용하면 된다.

> LocalDate(날짜) + LocalTime(시간) -> LocalDateTime(날짜 & 시간)

여기에 시간대(time-zone)까지 다뤄야 한다면, ZonedDateTime클래스를 사용하자.

> LocaldateTime + 시간대 -> ZonedDateTime

Calendar는 ZonedDateTime처럼, 날짜와 시간 그리고 시간대까지 모두 가지고 있다. Date와 유사한 클래스로는 Instant가 있는데, 이 클래스는 날짜와 시간을 초 단위(정확히는 나노초)로 표현한다.
날짜와 시간을 초단위로 표현한 값을 타임스탬프라고 부르는데, 이 값은 날짜와 시간을 하나의 정수로 표현할 수 있으므로 날짜와 시간의 차이를 계산하거나 순서를 비교하는데 유리해서 데이터베이스에 많이 사용된다. 이외에도 날짜를 더 세부적으로 다룰 수 있는 Year, YearMonth, MonthDay와 같은 클래스도 있다.

## Period와 Duration
날짜와 시간의 간격을 표현하기 위한 클래스도 있는데, Period는 두 날짜간의 차이를 표현하기 위한 것이고, Duration은 시간의 차이를 표현하기 위한 것이다.

> 날짜 - 날짜 = Period  
> 시간 - 시간 = Duration

## 객체 생성하기 - now(), of()
java.time 패키지에 속한 클래스의 객체를 생성하는 가장 기본적인 방법은 now()와 of()를 사용하는 것이다. now()는 현재 날짜와 시간을 저장하는 객체를 생성한다.

```java
LocalDate date = LocalDate.now();
LocalDate time = LocalTime.now();
LocalDateTime dateTime = LocalDateTime.now();
ZonedDateTime dateTimeInKr = ZonedDateTime.now();
```

of()는 단순히 해당 필드의 값을 순서대로 지정해 주기만 하면 된다. 각 클래스마다 다양한 종류의 of()가 정의되어 있다.

```java
LocalDate date = LocalDate.of(2024, 04, 03); // 2024년 04월 03일
LocalTime time = LocalTime.of(23, 59, 59);   // 23시 59분 59초
LocalDateTime dateTime = LocalDateTime.of(date, time);
ZonedDateTime zDateTime = ZonedDateTime.of(dateTime, ZoneId.of("Asia/Seoul"));
```

## Temporal과 TemporalAmount
LocalDate, LocalTime, LocalDateTime, ZonedDateTime 등 날짜와 시간을 표현하기 위한 클래스들은 모두 Temporal, TemporalAccessor, TemporalAdjuster 인터페이스를 구현했고, Duration과 Period는 TemporalAmount 인터페이스를 구현하였다. 앞으로 소개할 메서드 중에서 매개변수의 타입이 Temporal로 시작하는 것들이 자주 등장할텐데 대부분 날짜와 시간을 위한 것이므로, TemporalAmount인지 아닌지만 확인하면 된다.

>- __Temporal, TemporalAccessor, TemporalAdjuster를 구현한 클래스__  
>LocalDate, LocalTime, LocalDateTime, ZonedDateTime, Instant 등
>
>- __TemporalAmount를 구현한 클래스__  
>Period, Duration

## TemporalUnit과 TemporalField
날짜와 시간의 단위를 정의해 놓은 것이 TemporalUnit인터페이스이고, 이 인터페이스를 구현한 것이 열거형 ChronoUnit이다. 그리고 TemporalField는 년, 월, 일 등 날짜와 시간의 필드를 정의해 놓은 것으로, 열거형 ChronoField가 이 인터페이스를 구현하였다.

```java
LocalTime now = LocalTime.now(); // 현재 시간
int minute = now.getMinute();    // 현재 시간에서 분(minute)만 뽑아낸다.
int minute = now.get(ChronoField.MINUTE_OF_HOUR); // 위의 문장과 동일
```

날짜와 시간에서 특정 필드의 값만을 얻을 때는 get()이나, get으로 시작하는 이름의 메서드를 이용한다. 그리고 아래와 같이 특정 날짜와 시간에서 지정된 단위의 값을 더하거나 뺄 때는 plus() 또는 minus()에 값과 함께 열거형 ChronoUnit을 사용한다.

```java
LocalDate today = LocalDate.now(); // 오늘
LocalDate tomorrow = today.plus(1, ChronoUnit.DAYS); // 오늘에 1일을 더한다.
LocalDate tomorrow = today.plusDays(1); // 위의 문장과 동일
```

참고로 get()과 plus()의 정의는 아래와 같다.

```java
int get(TemporalField field)
LocalDate plus(long amountToAdd, TemporalUnit unit)
```

특정 TemporalField나 TemporalUnit을 사용할 수 있는지 확인하는 메서드는 다음과 같다. 이 메서드들은 날짜와 시간을 표현하는 데 사용하는 모든 클래스에 포함되어 있다.

```java
boolean isSupported(TemporalUnit unit) // Temporal에 정의
boolean isSupported(TemporalField field) // TemporalAccessor에 정의
```

<br>

# LocalDate와 LocalTime
---
LocalDate와 LocalTime은 java.time 패키지의 가장 기본이 되는 클래스이며, 나머지 클래스들은 이들의 확장이므로 이 두 클래스만 잘 이해하고 나면 나머지는 아주 쉬워진다. 객체를 생성하는 방법은 현재의 날짜와 시간을 LocalDate와 LocalTime으로 각각 반환하는 now()와 지정된 날짜와 시간으로 LocalDate와 LocalTime객체를 생성하는 of()가 있다. 둘 다 static메서드이다.

```java
LocalDate today = LocalDate.now(); // 오늘의 날짜
LocalTime now = LocalTime.now(); // 현재 시간

LocalDate birthDate = LocalDate.of(1999, 12, 31); // 1999년 12월 31일
LocalTime birthTime = LocalTime.of(23, 59, 59); // 23시 59분 59초
```

of()는 다음과 같이 여러 가지 버전이 제공된다.

static LocalDate of(int year, Month month, int dayOfMonth)
static LocalDate of(int year, int month, int dayOfMonth)

static LocalDate of(int hour, int min)
static LocalDate of(int hour, int min, int sec)
static LocalDate of(int year, int min, int sec, int nanoOfSecond)

참고로 다음과 같이 일 단위나 초 단위로도 지정할 수 있다. 아래의 첫 번째 문장은 1999년의 365번째 날, 즉 마지막 날을 의미하며, 두 번째 문장은 그 날의 0시 0분 0초부터 86399초(하루는 86400초)가 지난 시간, 즉 23시 59분 59초를 의미한다.

```java
LocalDate birthDate = LocalDate.ofYearDay(1999, 365); // 1999년 12월 31일
LocalTime birthTime = LocalTime.ofSecondDay(86399); // 23시 59분 59초
```

또는 parse()를 이용하면 문자열을 날짜와 시간으로 변환할 수도 있다.

```java
LocalDate birthDate = LocalDate.parse("1999-12-31"); // 1999년 12월 31일
LocalTime birthTime = LocalTime.parse("23:59:59");   // 23tl 59분 59초
```

## 특정 필드의 값 가져오기 - get(), getXXX()
LocalDate와 LocalTime의 객체에서 특정 필드의 값을 가져올 때는 아래의 표에 있는 메서드를 사용한다. '1999년 12월 31일 23:59:59'를 예로 들어 각 메서드의 호출결과를 적어 놓았으니 어렵지 않게 이해할 수 있을 것이다. 주의할 점은 Calendar와 달리 월(month)의 범위가 1~12이고, 요일은 월요일이 1, 화요일이 2, ... , 일요일은 7이라는 것이다.

>💡 __참고__  
> Calendar는 1월을 0으로 표현해서 11로 끝난다.

<table border="1">
    <colgroup>
        <col style="width:20%">
        <col style="width:20%">
        <col style="width:60%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">클래스</th>
        <th style="text-align:center;">메서드</th>
        <th style="text-align:center;">설명(1999-12-31 23:59:59)</th>
    </tr>
    <tr>
        <td style="text-align:center;" rowspan="9">LocalDate</td>
        <td style="text-align:center;">int getYear()</td>
        <td style="text-align:left;">년도(1999)</td>
    </tr>
    <tr>
        <td style="text-align:center;">int getMonthValue()</td>
        <td style="text-align:left;">월(12)</td>
    </tr>
    <tr>
        <td style="text-align:center;">Month getMonth()</td>
        <td style="text-align:left;">월(DECEMBER) getMonth().getValue() = 12</td>
    </tr>
    <tr>
        <td style="text-align:center;">int getDayOfMonth()</td>
        <td style="text-align:left;">일(31)</td>
    </tr>
    <tr>
        <td style="text-align:center;">int getDayOfYear()</td>
        <td style="text-align:left;">같은 해의 1월 1일부터 몇번째 일(365)</td>
    </tr>
    <tr>
        <td style="text-align:center;">DayOfWeek getDayOfWeek()</td>
        <td style="text-align:left;">요일(FRIDAY) getDayOfWeek().getValue() = 5</td>
    </tr>
    <tr>
        <td style="text-align:center;">int lengthOfMonth</td>
        <td style="text-align:left;">같은 달의 총 일수(31)</td>
    </tr>
    <tr>
        <td style="text-align:center;">int lengthOfYear()</td>
        <td style="text-align:left;">같은 해의 총 일수(365), 윤년이면 366</td>
    </tr>
    <tr>
        <td style="text-align:center;">boolean isLeapYear()</td>
        <td style="text-align:left;">윤년여부 확인(false)</td>
    </tr>
    <tr>
        <td style="text-align:center;" rowspan="4">LocalTime</td>
        <td style="text-align:center;">int getHour()</td>
        <td style="text-align:left;">시(23)</td>
    </tr>
    <tr>
        <td style="text-align:center;">int getMinute()</td>
        <td style="text-align:left;">분(59)</td>
    </tr>
    <tr>
        <td style="text-align:center;">int getSecond()</td>
        <td style="text-align:left;">초(59)</td>
    </tr>
    <tr>
        <td style="text-align:center;">int getNano()</td>
        <td style="text-align:left;">나노초(0)</td>
    </tr>
</table>

위의 표에 소개된 메서드 외에도 get()과 getLong()이 있는데, 원하는 필드를 직접 지정할 수 있다. 대부분의 필드는 int타입의 범위에 속하지만, 몇몇 필드는 int타입의 범위를 넘을 수 있다.
그럴 때 get() 대신 getLong()을 사용해야 한다.

```java
int get (TemporalField field)
long getLong (TemporalField field)
```

이 메서드들의 매개변수로 사용할 수 있는 필드의 목록은 아래와 같으며, getLong()을 사용해야 하는 필드는 아래표에서 '*' 표시가 되어있다.

<table border="1">
    <colgroup>
        <col style="width:20%">
        <col style="width:60%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">TemporalField(ChronoField)</th>
        <th style="text-align:center;">설명</th>
    </tr>
    <tr>
        <td style="text-align:center;">ERA</td>
        <td style="text-align:left;">시대</td>
    </tr>
    <tr>
        <td style="text-align:center;">YEAR_OF_ERA, YEAR</td>
        <td style="text-align:left;">년</td>
    </tr>
    <tr>
        <td style="text-align:center;">MONTH_OF_YEAR</td>
        <td style="text-align:left;">월</td>
    </tr>
    <tr>
        <td style="text-align:center;">DAY_OF_WEEK</td>
        <td style="text-align:left;">요일(1:월요일, 2:화요일, ... 7:일요일)</td>
    </tr>
    <tr>
        <td style="text-align:center;">DAY_OF_MONTH</td>
        <td style="text-align:left;">일</td>
    </tr>
    <tr>
        <td style="text-align:center;">AMPM_OF_DAY</td>
        <td style="text-align:left;">오전/오후</td>
    </tr>
    <tr>
        <td style="text-align:center;">HOUR_OF_DAY</td>
        <td style="text-align:left;">시간(0~23)</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLOCK_HOUR_OF_DAY</td>
        <td style="text-align:left;">시간(1~24)</td>
    </tr>
    <tr>
        <td style="text-align:center;">HOUR_OF_AMPM</td>
        <td style="text-align:left;">시간(0~11)</td>
    </tr>
    <tr>
        <td style="text-align:center;">CLOCK_HOUR_OF_AMPM</td>
        <td style="text-align:left;">시간(1~12)</td>
    </tr>
    <tr>
        <td style="text-align:center;">MINUTE_OF_HOUR</td>
        <td style="text-align:left;">분</td>
    </tr>
    <tr>
        <td style="text-align:center;">SECOND_OF_MINUTE</td>
        <td style="text-align:left;">초</td>
    </tr>
    <tr>
        <td style="text-align:center;">MILLI_OF_SECOND</td>
        <td style="text-align:left;">천분의 일초(=10<sup>-3</sup>초)</td>
    </tr>
    <tr>
        <td style="text-align:center;">MICRO_OF_SECOND *</td>
        <td style="text-align:left;">백만분의 일초(=10<sup>-6</sup>초)</td>
    </tr>
    <tr>
        <td style="text-align:center;">NANO_OF_SECOND *</td>
        <td style="text-align:left;">10억분의 일초(=10<sup>-9</sup>초)</td>
    </tr>
    <tr>
        <td style="text-align:center;">DAY_OF_YEAR</td>
        <td style="text-align:left;">그 해의 몇번째 날</td>
    </tr>
    <tr>
        <td style="text-align:center;">EPOCH_DAY *</td>
        <td style="text-align:left;">EPOCH(1970.1.1)부터 몇번째 날</td>
    </tr>
    <tr>
        <td style="text-align:center;">MINUTE_OF_DAY</td>
        <td style="text-align:left;">그 날의 몇 번째 분(시간을 분으로 환산)</td>
    </tr>
    <tr>
        <td style="text-align:center;">SECOND_OF_DAY</td>
        <td style="text-align:left;">그 날의 몇 번째 초(시간을 초로 환산)</td>
    </tr>
    <tr>
        <td style="text-align:center;">MILLI_OF_DAY</td>
        <td style="text-align:left;">그 날의 몇 번째 밀리초(=10<sup>-3</sup>초)</td>
    </tr>
    <tr>
        <td style="text-align:center;">MICRO_OF_DAY *</td>
        <td style="text-align:left;">그 날의 몇 번쨰 마이크로초(=10<sup>-6</sup>초)</td>
    </tr>
    <tr>
        <td style="text-align:center;">NANO_OF_DAY *</td>
        <td style="text-align:left;">그 날의 몇 번째 나노초(=10<sup>-9</sup>초)</td>
    </tr>
    <tr>
        <td style="text-align:center;">ALIGNED_WEEK_OF_MONTH</td>
        <td style="text-align:left;">그 달의 n번째 주(1~7일 1주, 8~14일 2주, ...)</td>
    </tr>
    <tr>
        <td style="text-align:center;">ALIGNED_WEEK_OF_YEAR</td>
        <td style="text-align:left;">그 해의 n번째 주(1월 1~7일 1주, 8~14일 2주, ...)</td>
    </tr>
    <tr>
        <td style="text-align:center;">ALIGNED_DAY_OF_WEEK_IN_MONTH</td>
        <td style="text-align:left;">요일(그 달의 1일을 월요일로 간주하여 계산)</td>
    </tr>
    <tr>
        <td style="text-align:center;">ALIGNED_DAY_OF_WEEK_IN_YEAR</td>
        <td style="text-align:left;">요일(그 해의 1월 1일을 월요일로 간주하여 계산)</td>
    </tr>
    <tr>
        <td style="text-align:center;">INSTANT_SECONDS</td>
        <td style="text-align:left;">년월일을 초단위로 환산(1970-01-01 00:00:00 UTC를 0초로 계산) Instant에만 사용가능</td>
    </tr>
    <tr>
        <td style="text-align:center;">OFFSET_SECONDS</td>
        <td style="text-align:left;">UTC와의 시차. ZoneOffset에만 사용가능</td>
    </tr>
    <tr>
        <td style="text-align:center;">PROLEPTIC_MONTH</td>
        <td style="text-align:left;">년월을 월단위로 환산(2015년11월=2015*12+11)</td>
    </tr>
</table>

이 목록은 ChronoField에 정의된 모든 상수를 나열한 것일 뿐, 사용할 수 있는 필드는 클래스마다 다르다. 예를 들어 LocalDate는 날짜를 표현하기 위한 것이므로, MINUTE_OF_HOUR와 같이 시간에 관련된 필드는 사용할 수 없다.

>💡 __참고__  
> 만일 해당 클래스가 지원하지 않는 필드를 사용하면, UnsupportedTemporalTypeException이 발생한다.

```java
LocalDate today = LocalDate.now(); // 오늘의 날짜
System.out.println(today.get(ChronoField.MINUTE_OF_HOUR)); // 예외 발생
```

참고로 특정 필드가 가질 수 있는 값의 범위를 알고 싶으면, 다음과 같이 하면 된다.

```java
System.out.println(ChronoField.CLOCK_HOUR_OF_DAY.range()); // 1 - 24
System.out.println(ChronoField.HOUR_OF_DAY.range());       // 0 - 23
```

HOUR_OF_DAY는 밤 12시를 0으로 표현하고, CLOCK_HOUR_OF_DAY는 24로 표현한다는 것을 알 수 있다.

## 필드의 값 변경하기 - with(), plus(), minus()
날짜와 시간에서 특정 필드 값을 변경하려면, 다음과 같이 with로 시작하는 메서드를 사용하면 된다.

```java
LocalDate withYear(int year)
LocalDate withMonth(int month)
LocalDate withDayOfMonth(int dayOfMonth)
LocalDate withDayOfYear(int dayOfYear)

LocalTime withHour(int hour)
LocalTime withMinute(int minute)
LocalTime withSecond(int second)
LocalTime withNano(int nanoOfSecond)
```

with()를 사용하면, 원하는 필드를 직접 지정할 수 있다. 위의 메서드들은 모두 with()로 작성된 것이라는 것을 짐작할 수 있다.

```java
LocalDate with(TemporalField field, long newValue)
```

앞서 언급한 것과 같이 필드를 변경하는 메서드들은 항상 새로운 객체를 생성해서 반환하므로 아래와 같이 대입 연산자를 같이 사용해야한다는 것을 잊으면 안된다.

```java
date = date.withYear(2000); // 년도를 2000년으로 변경
time = time.withHour(12);   // 시간을 12시로 변경
```

이 외에도 특정 필드에 값을 더하거나 빼는 plus()와 minus()가 있는데, 아래에는 plus()만 표시하였다.

```java
LocalTime plus(TemporalAmount amountToAdd)
LocalTime plus(long amountToAdd, TemporalUnit unit)

LocalDate plus(TemporalAmount amountToAdd)
LocalDate plus(long amountToAdd, TemporalUnit unit)
```

그리고 LocalTime의 truncatedTo()는 지정된 것보다 작은 단위의 필드를 0으로 만든다.

```java
LocalTime time = LocalTime.of(12, 34, 56); // 12시 34분 56초
time = time.truncatedTo(ChronoUnit.HOURS); // 시(hour)보다 작은 단위를 0으로
Syetem.out.println(time);                  // 12:00
```

## 날짜와 시간의 비교 - isAfter(), isBefore(), isEqual()
LocalDate와 LocalTime도 compareTo()가 오버라이딩되어 있어서, 아래와 같이 compareTo()로 비교할 수 있다.

```java
int result = date1.compareTo(date2); // 같으면 0, date1이 이전이면 -1, 이후면 1
```

그런데도 보다 편리하게 비교할 수 있는 메서드들이 추가로 제공된다.

```java
boolean isAfter(ChronoLocalDate other)
boolean isBefore(ChronoLocalDate other)
boolean isEqual(ChronoLocalDate other) // LocalDate에만 있다.
```

equals()가 있는데도, isEqual()을 제공하는 이유는 연표가 다른 두 날짜를 비교하기 위해서이다. 모든 필드가 일치해야하는 equals()와 달리 isEqual()은 오직 날짜만 비교한다. 그래서 대부분의 경우 equals()와 isEqual()의 결과는 같다.

```java
LocalDate kDate = LocalDate.of(1999, 12, 31);
JapaneseDate jDate = JapaneseDate.of(1999, 12, 31);

Syetem.out.println(kDate.equals(jDate));  // false 연대가 다름
Syetem.out.println(kDate.isEqual(jDate)); // true
```

<br>

# Instant
---
Instant는 에포크 타임부터 경과된 시간을 나노초 단위로 표현한다. 사람에겐 불편하지만, 단일 진법으로만 다루기 때문에 계산하기 쉽다. 사람이 사용하는 날짜와 시간은 여러 진법이 섞여있어서 계산하기 어렵다.

```java
Instant now = Instant.now();
Instant now2 = Instant.ofEpochSecond(now.getEpochSecond());
Instant now3 = Instant.ofEpochSecond(now.getEpochSecond(), now.getNano());
```

Instant를 생성할 때는 위와 같이 now()와 ofEpochSecond()를 사용한다. 그리고 필드에 저장된 값을 가져올 때는 다음과 같이한다.

```java
long epochSec = now.getEpochSecond();
int nano = now.getNano();
```

위의 코드에서 짐작할 수 있듯이, Instant는 시간을 초 단위와 나노초 단위로 나누어 저장한다. 오라클 데이터베이스의 타임스탬프처럼 밀리초 단위의 EPOCH TIME을 필요로 하는 경우를 위해 toEpochMilli()가 정의되어 있다.

```java
long toEpochMilli()
```

Instant는 항상 UTC(+00:00)를 기준으로 하기 때문에, LocalTime과 차이가 있을 수 있다. 예를 들어 한국은 시간대가 '+09:00'이므로 Instant와 LocalTime간에는 9시간의 차이가 있다. 시간대를 고려해야하는 경우 OffsetDateTime을 사용하는 것이 더 나은 선택일 수 있다.

UTC(세계 협정시)는 1972년 1월 1일부터 시행된 국제 표준시이다. 이전에 사용되던 GMT와 UTC는 거의 같지만, UTC가 좀 더 정확하다.

## Instant와 Date간의 변환
Instant는 기존의 java.util.Date를 대체하기 위한 것이며, JDK1.8부터 Date에 Instant로 변환할 수 있는 새로운 메서드가 추가되었다.

```java
static Date        from(Instant instant) // Instant -> Date
Instant            toInstant()           // Date -> Instant
```

<br>

# LocalDateTime과 ZonedDateTime
---
앞서 언급한 것과 같이 LocalDate와 LocalTime을 합쳐 놓은 것이 LocalDateTime이고, LocalDateTime에 시간대(time zone)를 추가한 것이 ZonedDateTime이다.

>   LocalDate   + LocalTime  -> LocalDateTime  
> LocalDateTIme +   시간대   -> ZonedDateTIme

## LocalDate와 LocalTime으로 LocalDateTime만들기
LocalDate와 LocalTime으로 합쳐서 하나의 LocalDateTime을 만들 수 있다. 다음은 LocalDateTime을 만들 수 있는 다양한 방법을 보여준다.

```java
LocalDate date = LocalDate.of(2015, 12, 31);
LocalTime time = LocalTime.of(12, 34, 56);

LocalDateTime dt  = LocalDateTime.of(date, time);
LocalDateTime dt2 = date.atTime(time);
LocalDateTime dt3 = time.atDate(date);
LocalDateTime dt4 = date.atTime(12, 34, 56);
LocalDateTime dt5 = time.atDate(LocalDate.of(2015, 12, 31));
LocalDateTime dt6 = date.atStartOfDay(); // dt6 = date.atTime(0, 0, 0);
```

물론 LocalDateTime에도 날짜와 시간을 직접 지정할 수 있는 다양한 버젼의 of()와 now()가 정의되어 있다.

```java
// 2015년 12월 31일 12시 34분 56초
LocalDateTime dateTime = LocalDateTime.of(2015, 12, 31, 12, 34, 56);
LocalDateTime today = LocalDateTime.now();
```

## LocalDateTime의 변환
반대로 LocalDateTime을 LocalDate 또는 LocalTime으로 변환할 수 있다.

```java
LocalDateTime dt = LocalDateTime.of(2015, 12, 31, 12, 34, 56);
LocalDate date = dt.toLocalDate(); // LocalDateTime -> LocalDate
LocalTime time = dt.toLocalTime(); // LocalDateTime -> LocalTime
```

## LocalDateTime으로 ZonedDateTime만들기
LocalDateTime에 시간대를 추가하면, ZonedDateTime이 된다. 기존에는 TimeZone클래스로 시간대를 다뤘지만 새로운 시간 패키지에서는 ZoneId라는 클래스를 사용한다. ZoneId는 일광 절약시간을 자동적으로 처리해주므로 더 편리하다.

LocalDate에 시간 정보를 추가하는 atTime()을 쓰면 LocalDateTime을 얻을 수 있는 것처럼, LocalDateTime에 atZone()으로 시간대 정보를 추가하면, ZonedDateTime을 얻을 수 있다.

```java
ZoneId zid = ZoneId.of("Asia/Seoul");
ZonedDateTime zdt = dateTime.atZone(zid);
```

LocalDate에 atStartOfDay()라는 메서드가 있는데, 이 메서드에 매개변수로 ZoneId를 지정해도 ZonedDateTime을 얻을 수 있다.

```java
ZonedDateTime zdt = LocalDate.now().atStartOfDay(zid);
```

메서드의 이름(atStartOfDay)에서 알 수 있듯이 시간이 0시 0분 0초로 되어 있는 것을 확인할 수 있다.
만일 현재 특정 시간대의 시간, 예를 들어 뉴욕을 알고 싶다면 다음과 같이 하면 된다.

```java
ZoneId nyId = ZoneId.of("America/New_York");
ZonedDateTime nyTime = ZonedDateTime.now().withZoneSameInstant(nyId);
```

## ZoneOffset
UTC로부터 얼마만큼 떨어져 있는지를 ZoneOffSet으로 표현한다. 위의 결과에서 알 수 있듯이 서울은 '+9'이다. 즉, UTC보다 9시간이 빠르다.

```java
ZoneOffset krOffset = ZonedDateTime.now().getOffset();
int krOffsetInSec = krOffset.get(ChronoField.OFFSET_SECONDS);
```

## OffsetDateTime
ZonedDateTime은 ZoneId로 구역을 표현하는데, ZoneId가 아닌 ZoneOffset을 사용하는 것이 OffsetDateTime이다. ZoneId는 일광절약시간처럼 시간대와 관련된 규칙들을 포함하고 있는데, ZoneOffset은 단지 시간대를 시간의 차이로만 구분한다. 컴퓨터에게 일광절약시간처럼 계절별로 시간을 더했다 뺏다하는 것과 같은 행위는 위험하기 그지없다.

아무런 변화없이 일관된 시간체계를 유지하는 것이 더 안전하다. 같은 지역 내의 컴퓨터 간에 데이터를 주고받을 때, 전송기간을 표현하기에 LocalDateTime이면, 충분하겠지만 서로 다른 시간대에 존재하는 컴퓨터간의 통신에는 OffsetDateTime이 필요하다.

```java
ZonedDateTime zdt = ZonededDateTime.of(date, time, zid);
OffsetDateTime odt = OffsetDateTime.of(date, time, krOffset);

// ZonedDateTime -> OffsetDateTime
OffsetDateTime odt = zdt.toOffsetDateTime();
```

OffsetDateTime은 ZonedDateTime처럼, LocalDate와 LocalTime에 ZoneOffset을 더하거나, ZonedDateTime에 toOffsetDateTime()을 호출해서 얻을 수도 있다.

## ZonedDateTime의 변환
ZonedDateTime도 LocalDateTime처럼 날짜와 시간에 관련된 다른 클래스로 변환하는 메서드들을 가지고 있다.

```java
LocalDate toLocalDate()
LocalTime toLocalTime()
LocalDateTime toLocalDateTime()
OffsetDateTime toOffsetDateTime()
long toEpochSecond()
Instant toInstant()
```

<br>

# TemporalAdjusters
---
앞서 plus(), minus()와 같은 메서드로 날짜와 시간을 계산할 수 있다는 것을 배웠다. 지난 주 토요일이 며칠인지, 또는 이번 달의 3번째 금요일은 며칠인지와 같은 날짜계산을 plus(), minus()로 하기엔 좀 불편하다. 그래서 자주 쓰일만한 날짜 계산들을 대신 해주는 메서드를 정의해놓은 것이 TemporalAdjusters클래스이다.

```java
LocalDate today = LocalDate.now();
LocalDate nextMonday = today.with(TemporalAdjusters.next(DayOfWeek.MONDAY));
```

위의 코드는 다음 주 월요일의 날짜를 계산할 때 TemporalAdjusters에 정의된 next()를 사용하였다. 이 외에도 다음과 같이 더 많은 유용한 메서드들이 TemporalAdjusters에 정의되어 있다.

<table border="1">
    <colgroup>
        <col style="width:60%">
        <col style="width:40%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">메서드</th>
        <th style="text-align:center;">설명</th>
    </tr>
    <tr>
        <td style="text-align:center;">firstDayOfNextYear()</td>
        <td style="text-align:left;">
            다음 해의 첫 날
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">firstDayOfNextMonth()</td>
        <td style="text-align:left;">
            다음 달의 첫 날
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">firstDayOfYear()</td>
        <td style="text-align:left;">올 해의 첫 날</td>
    </tr>
    <tr>
        <td style="text-align:center;">firstDayOfMonth()</td>
        <td style="text-align:left;">
            이번 달의 첫 날
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">lastDayOfYear()</td>
        <td style="text-align:left;">
            올 해의 마지막 날
        </td>
    </tr>
    <tr>
        <td style="text-align:center;">lastDayOfMonth()</td>
        <td style="text-align:left;">이번 달의 마지막 날</td>
    </tr>
    <tr>
        <td style="text-align:center;">firstInMonth(DayOfWeek dayOfWeek)</td>
        <td style="text-align:left;">이번 달의 첫 번째 ?요일</td>
    </tr>
    <tr>
        <td style="text-align:center;">lastInMonth(DayOfWeek dayOfWeek)</td>
        <td style="text-align:left;">이번 달의 마지막 ?요일</td>
    </tr>
    <tr>
        <td style="text-align:center;">previous(DayOfWeek dayOfWeek)</td>
        <td style="text-align:left;">지난 ?요일(당일 미포함)</td>
    </tr>
    <tr>
        <td style="text-align:center;">previousOrSame(DayOfWeek dayOfWeek)</td>
        <td style="text-align:left;">지난 ?요일(당일 포함)</td>
    </tr>
    <tr>
        <td style="text-align:center;">next(DayOfWeek dayOfWeek)</td>
        <td style="text-align:left;">다음 ?요일(당일 미포함)</td>
    </tr>
    <tr>
        <td style="text-align:center;">nextOrSame(DayOfWeek dayOfWeek)</td>
        <td style="text-align:left;">다음 ?요일(당일 포함)</td>
    </tr>
    <tr>
        <td style="text-align:center;">dayOfWeekInMonth(int ordinal, DayOfWeek dayOfWeek)</td>
        <td style="text-align:left;">이번 달의 n번째 ?요일</td>
    </tr>
</table>

## TemporalAdjuster직접 구현하기
보통은 TemporalAdjusters에 정의된 메서드로 충분하겠지만, 필요하면 자주 사용되는 날짜계산을 해주는 메서드를 직접 만들 수도 있다. LocalDate의 with()는 다음과 같이 정의되어있으며, TemporalAdjuster인터페이스를 구현한 클래스의 객체를 매개변수로 제공해야한다.

```java
LocalDate with(TemporalAdjuster adjuster)
```

with()는 LocalTime, LocalDateTime, ZonedDateTime, Instant 등 대부분의 날짜와 시간에 관련된 클래스에 포함되어 있다. TemporalAdjuster인터페이스는 다음과 같이 추상 메서드 하나만 정의되어 있으며, 이 메서드만 구현하면 된다.

```java
@FunctionalInterface
public interface TemporalAdjuster {
    Temporal adjustInto(Temporal temporal);
}
```

실제로 구현해야하는 것은 adjustInto()지만, 우리가 TemporalAdjuster와 같이 사용해야 하는 메서드는 with()이다. with()와 adjustInto() 중에서 어느 쪽을 사용해도 되지만, adjustInto()는 내부적으로만 사용할 의도로 작성된 것이기 때문에, with()를 사용하도록 하자.

앞서 언급한 것과 같이 날짜와 시간에 관련된 대부분의 클래스는 Temporal인터페이스를 구현하였으므로 adjustInto()의 매개변수가 될 수 있다.
예를 들어, 특정 날짜로부터 2일 후의 날짜를 계산하는 DayAfterTomorrow는 다음과 같이 작성할 수 있다.

```java
class DayAfterTomorrow implements TemporalAdjuster {
    @Override
    public Temporal adjustInto(Temporal temporal) {
        return temporal.plus(2, ChronoUnit.DAYS); // 2일을 더한다.
    }
}
```

<br>

# Period와 Duration
---
앞서 잠시 언급한 것과 같이 Period는 날짜의 차이를, Duration은 시간의 차이를 계산하기 위한 것이다.

> 날짜 - 날짜 = Period  
> 시간 - 시간 = Duration

## between()
예를 들어 두 날짜 date1과 date2의 차이를 나타내는 Period는 between()으로 얻을 수 있다.

```java
LocalDate date1 = LocalDate.of(2014, 1, 1);
LocalDate date2 = LocalDate.of(2015, 12, 31);

Period pe = Period.between(date1, date2);
```

date1이 date2보다 날짜 상으로 이전이면 양수로, 이후면 음수로 Period에 저장된다. 그리고 시간차이를 구할 때는 Duration을 사용한다는 것을 제외하고는 Period와 똑같다.

```java
LocalTime time1 = LocalTime.of(00, 00, 00);
LocalTime time2 = LocalTime.of(12, 34, 56);

Duration du = Duration.between(time1, time2);
```

Period, Duration에서 특정 필드의 값을 얻을 때는 get()을 사용한다.

```java
long year = pe.get(ChronoUnit.YEARS);
long month = pe.get(ChronoUnit.MONTHS);
long day = pe.get(ChronoUnit.DAYS);

long sec = du.get(ChronoUnit.SECONDS);
long nano = du.get(ChronoUnit.NANOS);
```

그런데, Period와 달리 Duration에는 getHours(), getMinites() 같은 메서드가 없다. 믿기 힘든 사실이니 직접 확인해 보자. getUnits()라는 메서드로 get()에 사용할 수 있는 ChronoUnit의 종류를 확인할 수 있다.

```java
System.out.println(pe.getUnits());
System.out.println(du.getUnits());
```

안타깝게도 Duration에는 Chrono.SECOND와 Chrono.NANOS밖에 사용할 수 없다는 결과가 나왔다. 좀 불편하지만 어쩔 수 없이 다음과 같이 직접 계산해 보았다.

```java
long hour = du.getSeconds() / 3600;
long min = (du.getSeconds() - hour*3600) / 60;
long sec = (du.getSeconds() - hour * 3600 - min * 60) % 60;
int nano = du.getNano();
```

보다 쉬운 방법도 있다.

```java
LocalTime tmpTime = LocalTime.of(0, 0).plusSeconds(du.getSeconds());

int hour = tmpTime.getHour();
int min = tmpTime.getMinute();
int hour = tmpTime.getSecond();
int hour = du.getNano();
```

## between()과 until()
until()은 between()과 거의 같은 일을 한다. between()은 static메서드이고, until()은 인스턴스 메서드라는 차이가 있다.

```java
// Period pe = Period.between(today, myBirthDay);
Period pe = today.until(myBirthDay);
long dday = today.until(myBirthDay, ChronoUnit.DAYS);
```

Period는 년월일을 분리해서 저장하기 때문에, D-day를 구하려는 경우에는 두 개의 매개변수를 받는 until()을 사용하는 것이 낫다. 날짜가 아닌 시간에도 until()을 사용할 수 있지만, Duration을 반환하는 until()은 없다.

```java
long sec = LocalTime.now().until(endTime, ChronoUnit.SECONDS);
```

## of(), with()
Period에는 of(), ofYears(), ofMonths(), ofWeeks(), ofDays()가 있고, Duration에는 of(), ofDays(), ofHours(), ofMinutes(), ofSeconds() 등이 있다. 사용법은 앞서 Local Date와 LocalTime에서 배운 것과 같다.

```java
Period pe = Period.of(1, 12, 31); // 1년 12개월 31일
Duration du = Duration.of(60, ChronoUnit.SECONDS); // 60초
// Duration du = Duration.ofSeconds(60); // 위의 문장과 동일
```

특정 필드의 값을 변경하는 with()도 있다.

```java
pe = pe.withYears(2);       // 1년에서 2년으로 변경. withMonths(), withDays()
du = du.withSeconds(120);   // 60초에서 120초로 변경. withNanos()
```

## 사직연산, 비교연산, 기타 메서드
plus(), minus()외에 곱셈과 나눗셈을 위한 메서드도 있다.

```java
pe = pe.minusYears(1).multipliedBy(2); // 1년을 빼고, 2배를 곱한다.
du = du.plusHours(1).dividedBy(60); // 1시간을 더하고 60으로 나눈다.
```

Period에 나눗셈을 위한 메서드가 없는데, Period는 날짜의 기간을 표현하기 위한 것이므로 나눗셈을 위한 메서드가 별로 유용하지 않기 때문에 넣지 않은 것이다. 그리고 음수인지 확인하는 isNegative()와 0인지 확인하는 isZero()가 있다. 두 날짜 또는 시간을 비교할 때, 사용하면 어느 쪽이 앞인지 또는 같은지 알아낼 수 있다.

```java
boolean sameDate = Period.between(date1, date2).isZero();
boolean isBefore = Duration.between(time1, time2).isNegative();
```

부호를 반대로 변경하는 negate()와 부호를 없애는 abs()가 있다. 아래 양쪽의 코드는 동일하다. Period에는 abs()가 없어서 대신 아래의 오른쪽과 같은 코드를 사용해야 한다.

```java
du = du.abs(); <-> if(du.isNegative()) du = du.negated();
```

Period에 normalized()라는 메서드가 있는데, 이 메서드는 월(month)의 값이 12를 넘지않게, 즉 1년 13개월을 2년 1개월로 바꿔준다. 일(day)의 길이는 일정하지 않으므로 그대로 놔둔다.

```java
pe = Period.of(1, 13, 32).normalized(); // 1년 13개월 32일 -> 2년 1개월 32일
```

## 다른 단위로 변환 - toTotalMonths(), toDays(), toHours(), toMinutes()
이름이 'to'로 시작하는 메서드들이 있는데, 이 들은 Period와 Duration을 다른 단위의 값으로 변환하는데 사용된다. get()은 특정 필드의 값을 그대로 가져오는 것이지만, 아래의 메서드들은 특정 단위로 변환한 결과를 반환한다는 차이가 있다.

>💡 __참고__  
> 이 메서드들의 반환타입은 모두 정수(long타입)인데, 이것은 지정된 단위 이하의 값들은 버려진다는 뜻이다.

<table border="1">
    <colgroup>
        <col style="width:15%">
        <col style="width:15%">
        <col style="width:70%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">클래스</th>
        <th style="text-align:center;">메서드</th>
        <th style="text-align:center;">설명</th>
    </tr>
    <tr>
        <td style="text-align:center;">Period</td>
        <td style="text-align:center;">long toTotalMonths()</td>
        <td style="text-align:left;">년월일을 월단위로 변환해서 반환(일 단위는 무시)</td>
    </tr>
    <tr>
        <td style="text-align:center;" rowspan="5">Duration</td>
        <td style="text-align:left;">long toDays()</td>
        <td style="text-align:left;">일단위로 변환해서 반환</td>
    </tr>
    <tr>
        <td style="text-align:left;">long toHours()</td>
        <td style="text-align:left;">시간단위로 변환해서 반환</td>
    </tr>
    <tr>
        <td style="text-align:left;">long toMinutes()</td>
        <td style="text-align:left;">분단위로 변환해서 반환</td>
    </tr>
    <tr>
        <td style="text-align:left;">long toMillis()</td>
        <td style="text-align:left;">천분의 일초 단위로 변환해서 반환</td>
    </tr>
    <tr>
        <td style="text-align:left;">long toNanos()</td>
        <td style="text-align:left;">나노초 단위로 변환해서 반환</td>
    </tr>
</table>

참고로 LocalDate의 toEpochDay()라는 메서드는 Epoch Day인 '1970-01-01'부터 날짜를 세어서 반환한다. 이 메서드를 이용하면 Period를 사용하지 않고도 두 날짜간의 일수를 편리하게 계산할 수 있다. 단, 두 날짜 모두 Epoch Day이후의 것이어야 한다.

```java
LocalDate date1 = LocalDate.of(2015, 11, 28);
LocalDate date2 = LocalDate.of(2015, 11, 29);

long period = date2.toEpochDay() - date1.toEpochDay(); // 1
```

LocalTime에도 다음과 같은 메서드가 있어서, Duration을 사용하지 않고도 위와 같이 뺄셈으로 시간차이를 계산할 수 있다.

```java
int toSecondOfDay()
long toNanoOfDay()
```

<br>

# 파싱과 포맷
---
형식화와 관련된 클래스들은 java.time.format패키지에 들어있는데, 이 중에서 DateTimeFormatter가 핵심이다. 이 클래스에는 자주 쓰이는 다양한 형식들을 기본적으로 정의하고 있으며, 그 외의 형식이 필요하다면 직접 정의해서 사용할 수도 있다.

```java
LocalDate date = LocalDate.of(2016, 1, 2);
String yyyymmdd = DateTimeFormatter.ISO_LOCAL_DATE.format(date);
String yyyymmdd = date.format(DateTimeFormatter.ISO_LOCAL_DATE);
```

날짜와 시간의 형식화에는 위와 같이 format()이 사용되는데, 이 메서드는 DateTimeFormatter뿐만 아니라 LocalDate나 LocalTime같은 클래스에도 있다. 같은 기능을 하니까 상황에 따라 편한 쪽을 선택해서 사용하면 된다.

## 로케일에 종속된 형식화
DateTimeFormatter의 static메서드 ofLocalizedDate(), ofLocalizedTime(), ofLocalized DateTime()은 로케일에 종속적인 포맷터를 생성한다.

```java
DateTimeFormatter formatter = DateTimeFormatter.ofLocalizedDate(FormatStyle.SHORT);
String shortFormat = formatter.format(LocalDate.now());
```

FormatStyle의 종류에 따른 출력 형태는 다음과 같다.

<table border="1">
    <colgroup>
        <col style="width:15%">
        <col style="width:50%">
        <col style="width:35%">
    </colgroup>
    <tr style="background-color:gray;">
        <th style="text-align:center;">FormatStyle</th>
        <th style="text-align:center;">날짜</th>
        <th style="text-align:center;">시간</th>
    </tr>
    <tr>
        <td style="text-align:center;">FULL</td>
        <td style="text-align:center;">2015년 11월 28일 토요일</td>
        <td style="text-align:center;">N/A</td>
    </tr>
    <tr>
        <td style="text-align:center;">LONG</td>
        <td style="text-align:center;">2015년 11월 28일 (토)</td>
        <td style="text-align:center;">오후 9시 15분 13초</td>
    </tr>
    <tr>
        <td style="text-align:center;">MEDIUM</td>
        <td style="text-align:center;">2015. 11. 28</td>
        <td style="text-align:center;">오후 9:15:13</td>
    </tr>
    <tr>
        <td style="text-align:center;">SHORT</td>
        <td style="text-align:center;">15. 11. 28</td>
        <td style="text-align:center;">오후 9:15</td>
    </tr>
</table>

## 출력형식 직접 정의하기
DateTimeFormatter의 ofPattern()으로 원하는 출력형식을 직접 작성할 수도 있다.

```java
DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyy/MM/dd");
```

## 문자열을 날짜와 시간으로 파싱하기
문자열을 날짜 또는 시간으로 변환하려면 static메서드 parse()를 사용하면 된다. 날짜와 시간을 표현하는데 사용되는 클래스에는 이 메서드가 거의 다 포함되어 있다. parse()는 오버로딩된 메서드가 여러 개 있는데, 그 중에서 다음의 2개가 자주 쓰인다.

```java
static LocalDateTime parse(CharSequence text)
static LocalDateTime parse(CharSequence text, DateTimeFormatter formatter);
```

DateTimeFormatter에 상수로 정의된 형식을 사용할 때는 다음과 같이 한다.

```java
LocalDate date = LocalDate.parse("2016-01-02", DateTimeFormatter.ISO_LOCAL_DATE);
```

자주 사용되는 기본적인 형식의 문자열은 ISO_LOCAL_DATE와 같은 형식화 상수를 사용하지 않고도 파싱이 가능하다.

```java
LocalDate newDate = LocalDate.parse("2001-01-01");
LocalDate newTime = LocalTime.parse("23:59:59");
LocalDateTime newDateTime = LocalDateTime.parse("2001-01-01T23:59:59");
```

다음과 같이 ofPattern()을 이용해서 파싱을 할 수도 있다.

```java
DateTimeFormatter pattern = DateTimeFormatter.ofPattern("yyyy-MM-dd HH:mm:ss");
LocalDateTime endOfYear = LocalDateTime.parse("2015-12-31 23:59:59", pattern);
```

---

읽어주셔서 감사합니다. 😊 

__Reference__  
자바의 정석 - 남궁성  