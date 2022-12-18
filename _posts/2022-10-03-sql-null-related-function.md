---
title: "[SQL] Null 관련 함수"
categories:
  - SQL
tags:
  - SQL
  - Null
  - Null 관련 함수
toc: true
toc_sticky: true
toc_label: "목차"
comments: true
---

SQL에서 Null은 중요한 개념이며 잘 다루어야 문제없이 데이터 처리가 가능하다. Null의 개념과 성질에 대해서는 작성해놓은 포스팅이 있으니 생략하고 바로 관련 함수를 살펴보겠다.

관련 함수를 살펴보기전 Null의 개념을 잡고 싶다면 아래 링크를 참조하길 바란다.  
[[SQL] Null 속성의 이해 - Wonmo Lee's github tech blog](https://wonmolee.github.io/blog/post/sql/understanding-the-null-property/)

# Null 관련 함수 종류
---
RDBMS의 중요한 데이터인 NULL을 처리하는 주요 함수는 다음과 같다.

## NVL
- 사용법
  >NVL(표현식1, 표현식2)

- 예제

```sql
SELECT NVL(NULL, '1') AS "NVL-TEST"
  FROM DUAL;
```

![NVL 함수 예제](/blog/assets/img/posts/20221003/query-example1.png "NVL 함수 예제"){: width="100%"}

## NULLIF
- 사용법
  >NULLIF(표현식1, 표현식2)

- 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20220925/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

```sql
SELECT ENAME
     , EMPNO
     , MGR
     , NULLIF(MGR, 7698) AS NUIF
  FROM EMP;
```

![NULLIF 함수 예제](/blog/assets/img/posts/20221003/query-example2.png "NULLIF 함수 예제"){: width="100%"}

## COALESCE
- 사용법
  >COALESCE(표현식1, 표현식2, ...)

- 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20220925/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

```sql
SELECT ENAME
     , COMM
     , MGR
     , SAL
     , COALESCE(COMM, SAL) AS COAL
  FROM EMP;
```

![COALESCE 함수 예제](/blog/assets/img/posts/20221003/query-example3.png "COALESCE 함수 예제"){: width="100%"}

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원