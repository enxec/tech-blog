---
title: "[SQL] PIVOT절과 UNPIVOT절"
description: 
author: Enxec
date: 2022-10-31
categories: [Language, SQL]
tags: [sql, PIVOT, UNPIVOT]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/sql-logo.png
  lqip: 
  alt: 
---

## 개요
---
PIVOT은 회전시킨다는 의미를 갖고 있다. PIVOT 절은 행을 열로 회전시키고, UNPIVOT 절은 열을 행으로 회전시킨다.

<br>

## PIVOT 절
---
PIVOT 절은 행을 열로 전환한다. PIVOT 절의 구문은 아래와 같다.

### 사용법

```sql
-- 중간에 &#123; 는 중괄호 오류때문에 문자로 대체해서 넣었다.
PIVOT [XML]
       (
        aggregate_function (expr) [[AS] alias]
        [, aggregate_function (expr) [[as] alias]] ...
        FOR {column | (column [, column]...)}
        IN (&#123;&#123;&#123;expr | (expr [, expr]...)} [[AS] alias]} ...
            | subquery
            | ANY [, ANY]...
           })
       )
```

- aggregate_function은 집계할 열을 지정한다.
- FOR 절은 PIVOT할 열을 지정한다.
- IN 절은 PIVOT할 열 값을 지정한다.

다음은 PIVOT 절을 사용한 쿼리다. PIVOT 절은 집계함수와 FOR 절에 지정되지 않은 열을 기준으로 집계되기 때문에 인라인 뷰를 통해 사용할 열을 지정해야 한다.

### 예제 1
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT *
  FROM (
        SELECT A.JOB
             , A.DEPTNO
             , A.SAL
          FROM EMP A
       )
 PIVOT (SUM(SAL) FOR DEPTNO IN(10, 20, 30))
 ORDER BY 1;
```

- 결과

![PIVOT 예제](/posts/20221031/query-example3.png "PIVOT 예제"){: width="100%"}

### 예제 2
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT *
  FROM (
        SELECT TO_CHAR(HIREDATE, 'YYYY') AS YYYY
             , JOB
             , DEPTNO
             , SAL 
         FROM EMP
       )
 PIVOT (SUM(SAL) FOR DEPTNO IN(10, 20, 30))
 ORDER BY 1, 2;
```

- 결과

![PIVOT 예제](/posts/20221031/query-example4.png "PIVOT 예제"){: width="100%"}

## UNPIVOT 절
---
UNPIVOT 절은 PIVOT 절과 반대로 동작한다. 열이 행으로 전환된다. UNPIVOT 절의 구문은 아래와 같다.

### 사용법
```sql
UNPIVOT [{INCLUDE | EXCLUDE} NULLS]
       (
            {column | (column [, col]...)}
        FOR {column | (column [, col]...)}
        IN ({column | (column [, col]...)} [AS {literal | (literal [, literal]...)}]
            [, {column | (column [, col]...)} [AS {literal | (literal [, literal]...)}]]...
           )
       )
```

- UNPIVOT column 절은 UNPIVOT된 값이 들어갈 열을 지정한다.
- FOR 절은 UNPIVOT된 값을 설명할 값이 들어갈 열을 지정한다.
- IN 절은 UNPIVOT할 열과 설명할 값의 리터럴 값을 지정한다.

### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![T1 릴레이션](/posts/20221031/t1-relation.png "T1 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">T1 릴레이션</div>

- 쿼리
  
```sql
 SELECT JOB
      , DEPTNO
      , SAL
   FROM T1
UNPIVOT (SAL FOR DEPTNO IN (D10_SAL, D20_SAL))
 ORDER BY 1, 2;
```

- 결과

![UNPIVOT 예제](/posts/20221031/query-example5.png "UNPIVOT 예제"){: width="100%"}

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  