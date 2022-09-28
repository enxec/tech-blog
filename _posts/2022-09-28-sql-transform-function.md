---
title: "[SQL] 변환형 함수"
categories:
  - SQL
tags:
  - SQL
  - 변환형 함수
toc: true
toc_sticky: true
toc_label: "목차"
---

변환형 함수는 단일행 함수의 한 종류로 특정 데이터 타입을 다양한 형식으로 출력하고 싶을 경우에 사용한다. 변환형 함수는 어떤 것들이 있는지 함께 알아보자.

# TO_NUMBER
---
TO_NUMBER는 숫자로 변환 가능한 문자열을 숫자로 변환한다.

- 사용법
  >TO_NUMBER(문자열)

- 예제

```sql
SELECT TO_NUMBER('4321') AS 결과
  FROM DUAL;
```

![TO_NUMBER함수 예제](/blog/assets/img/posts/20220928/query-example25.png "TO_NUMBER함수 예제"){: width="100%"}

<br>

# TO_CHAR
---
TO_CHAR는 숫자나 날짜를 주어진 FORMAT 형태인 문자열 타입으로 변환한다. expression을 주어진 style 형태인 목표 데이터 유형으로 변환한다.

- 사용법
  >TO_CHAR(숫자 | 날짜 [, FORMAT])

- 예제

```sql
SELECT TO_CHAR(SYSDATE, 'DD') AS 일
  FROM DUAL;
```

![TO_CHAR함수 예제](/blog/assets/img/posts/20220928/query-example26.png "TO_CHAR함수 예제"){: width="100%"}

<br>

# TO_DATE
---
TO_DATE는 문자열을 주어진 FORMAT 형태인 날짜 타입으로 변환한다. expression을 주어진 style 형태인 목표 데이터 유형으로 변환한다.

- 사용법
  >TO_DATE(문자열 [, FORMAT])

- 예제

```sql
SELECT TO_DATE('20220811', 'YYYY/MM/DD') AS 일자
  FROM DUAL;
```

![TO_DATE함수 예제](/blog/assets/img/posts/20220928/query-example27.png "TO_DATE함수 예제"){: width="100%"}

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원