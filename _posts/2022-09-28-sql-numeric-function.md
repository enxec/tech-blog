---
title: "[SQL] 숫자형 함수"
description: 
author: Enxec
date: 2022-09-28
categories: [Language, SQL]
tags: [sql, 숫자형 함수]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/sql-logo.png
  lqip: 
  alt: 
---

숫자형 함수는 단일행 함수의 한 종류로 숫자 데이터를 입력받아 처리하고 숫자를 리턴하는 함수이다. 숫자형 함수는 어떤 것들이 있는지 함께 알아보자.

## ABS
---
ABS는 숫자의 절댓값을 돌려준다.

- 사용법
  >ABS(숫자)

- 예제

```sql
SELECT ABS(-15) AS 결과
  FROM DUAL;
```

![ABS함수 예제](/posts/20220928/query-example11.png "ABS함수 예제"){: width="100%"}

<br>

## SIGN
---
SIGN은 숫자가 양수인지, 음수인지 0인지를 구별한다. 양수면 1, 음수면 -1, 0이면 0을 리턴한다.

- 사용법
  >SIGN(숫자)

- 예제

```sql
SELECT SIGN(-15) AS 결과
  FROM DUAL;
```

![SIGN함수 예제](/posts/20220928/query-example12.png "SIGN함수 예제"){: width="100%"}

<br>

## MOD
---
MOD는 숫자1을 숫자2로 나누어 나머지 값을 리턴한다. MOD 함수는 % 연산자로도 대체 가능(예, 7%3)

- 사용법
  >MOD(숫자1, 숫자2)

- 예제

```sql
SELECT MOD(7, 3) AS 결과
  FROM DUAL;
```

![MOD함수 예제](/posts/20220928/query-example13.png "MOD함수 예제"){: width="100%"}

<br>

## CEIL
---
CEIL은 숫자보다 크거나 같은 최소 정수를 리턴한다.

- 사용법
  >CEIL(숫자)

- 예제

```sql
SELECT CEIL(42.195) AS 결과
  FROM DUAL;
```

![CEIL함수 예제](/posts/20220928/query-example14.png "CEIL함수 예제"){: width="100%"}

<br>

## FLOOR
---
FLOOR는 숫자보다 작거나 같은 최대 정수를 리턴한다.

- 사용법
  >FLOOR(숫자)

- 예제

```sql
SELECT FLOOR(42.195) AS 결과
  FROM DUAL;
```

![FLOOR함수 예제](/posts/20220928/query-example15.png "FLOOR함수 예제"){: width="100%"}

<br>

## ROUND
---
ROUND는 숫자를 소수점 m자리에서 반올림해 리턴한다. m이 생략되면 디폴트 값은 0이다.

- 사용법
  >ROUND(숫자 [, m])

- 예제

```sql
SELECT ROUND(42.1954, 3) AS 결과
  FROM DUAL;
```

![ROUND함수 예제](/posts/20220928/query-example16.png "ROUND함수 예제"){: width="100%"}

<br>

## TRUNC
---
TRUNC는 숫자를 소수 m자리에서 잘라서 버린다. m이 생략되면 디폴트 값은 0이다. SQL Server에서 TRUNC 함수는 제공하지 않는다.

- 사용법
  >TRUNC(숫자 [, m])

- 예제

```sql
SELECT TRUNC(42.1954, 3) AS 결과
  FROM DUAL;
```

![TRUNC함수 예제](/posts/20220928/query-example16.png "TRUNC함수 예제"){: width="100%"}

<br>

## SIN, COS, TAN
---
SIN, COS, TAN는 숫자의 삼각함수 값을 리턴한다.

- 사용법
  >SIN(숫자)
  >COS(숫자)
  >TAN(숫자)

- 예제

```sql
SELECT SIN(0) AS 결과1
	   , COS(0) AS 결과2
	   , TAN(0) AS 결과3
  FROM DUAL;
```

![SIN, COS, TAN함수 예제](/posts/20220928/query-example17.png "SIN, COS, TAN함수 예제"){: width="100%"}

<br>

## EXP
---
EXP는 숫자의 지수 값을 리턴한다. 즉, e(e=2.7182813...)의 숫자 제곱 값을 리턴한다.(=e숫자)

- 사용법
  >EXP(숫자)

- 예제

```sql
SELECT EXP(2) AS 결과
  FROM DUAL;
```

![EXP함수 예제](/posts/20220928/query-example18.png "EXP함수 예제"){: width="100%"}

<br>

## POWER
---
POWER는 숫자의 거듭제곱 값을 리턴한다. 즉, 숫자1의 숫자2 제곱 값(=숫자1숫자2)을 리턴한다.

- 사용법
  >POWER(숫자1, 숫자2)

- 예제

```sql
SELECT POWER(2, 3) AS 결과
  FROM DUAL;
```

![POWER함수 예제](/posts/20220928/query-example19.png "POWER함수 예제"){: width="100%"}

<br>

## SQRT
---
SQRT는 숫자의 제곱근값을 리턴한다.

- 사용법
  >SQRT(숫자)

- 예제

```sql
SELECT SQRT(4) AS 결과
  FROM DUAL;
```

![SQRT함수 예제](/posts/20220928/query-example20.png "SQRT함수 예제"){: width="100%"}

<br>

## LOG
---
LOG는 숫자1을 밑수로 하는 숫자2의 로그 값(=LOG숫자1숫자2)을 리턴한다. SQL Server는 숫자2를 밑수로 하는 숫자1의 로그 값(=LOG숫자2숫자1)을 리턴한다.

- 사용법
  >LOG(숫자1, 숫자2)

- 예제

```sql
SELECT LOG(10, 100) AS 결과
  FROM DUAL;
```

![LOG함수 예제](/posts/20220928/query-example21.png "LOG함수 예제"){: width="100%"}

<br>

## LN
---
LN은 숫자의 자연 로그 값(=LOGe숫자)을 리턴한다. SQL Server에서 LN 함수는 제공하지 않는다.

- 사용법
  >LN(숫자)

- 예제

```sql
SELECT LN(7.3890561) AS 결과
  FROM DUAL;
```

![LN함수 예제](/posts/20220928/query-example22.png "LN함수 예제"){: width="100%"}

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원