---
title: "[SQL] 날짜형 함수"
description: 
author: Enxec
date: 2022-09-28
categories: [Language, SQL]
tags: [sql, 날짜형 함수]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/sql-logo.png
  lqip: 
  alt: 
---

날짜형 함수는 단일행 함수의 한 종류로 DATE 타입의 값을 연산하는 함수다. 날짜형 함수는 어떤 것들이 있는지 함께 알아보자.

## SYSDATE
---
SYSDATE는 현자 날짜와 시각을 출력한다.

- 사용법
  >SYSDATE

- 예제

```sql
SELECT SYSDATE AS 결과
  FROM DUAL;
```

![SYSDATE함수 예제](/posts/20220928/query-example23.png "SYSDATE함수 예제"){: width="100%"}

<br>

## EXTRACT
---
EXTRACT은 날짜 데이터에서 연월일 데이터를 출력할 수 있다. 시분초도 가능하다.

- 사용법
  >EXTRACT('YEAR' | 'MONTH' | 'DAY' FROM D)

- 예제

```sql
SELECT EXTRACT(YEAR FROM SYSDATE) AS 입사년도
	   , EXTRACT(MONTH FROM SYSDATE) AS 입사월
	   , EXTRACT(DAY FROM SYSDATE) AS 입사일
  FROM DUAL;
```

![EXTRACT함수 예제](/posts/20220928/query-example24.png "EXTRACT함수 예제"){: width="100%"}

<br>

## TO_NUMBER
---
TO_NUMBER는 날짜 데이터에서 연월일 데이터를 출력할 수 있다. Oracle EXTRACT YEAR / MONTH / DAY 옵션이나 SQL Server DEPART YEAR / MONTH / DAY 옵션과 같은 기능이다. TO_NUMBER 함수 제외 시 문자형으로 출력된다.

- 사용법
  >TO_NUMBER(TO_CHAR(d, 'YYYY'))
  >TO_NUMBER(TO_CHAR(d, 'MM'))
  >TO_NUMBER(TO_CHAR(d, 'DD'))

- 예제

```sql
SELECT TO_NUMBER(TO_CHAR(SYSDATE, 'YYYY')) AS 입사년도
	   , TO_NUMBER(TO_CHAR(SYSDATE, 'MM')) AS 입사월
	   , TO_NUMBER(TO_CHAR(SYSDATE, 'DD')) AS 입사일
  FROM DUAL;
```

![TO_NUMBER함수 예제](/posts/20220928/query-example24.png "TO_NUMBER함수 예제"){: width="100%"}

<br>

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원