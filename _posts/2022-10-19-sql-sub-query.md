---
title: "[SQL] 서브쿼리(Subquery)"
description: 
author: Enxec
date: 2022-10-19
categories: [Language, SQL]
tags: [sql, Subquery, 서브쿼리]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/sql-logo.png
  lqip: 
  alt: 
---

## 서브쿼리란?
---
서브쿼리는 하나의 SQL문안에 포함돼 있는 또 다른 SQL문을 말하며, 메인쿼리에 종속되어 있다. 때문에 서브쿼리는 메인 쿼리의 컬럼을 모두 사용할 수 있지만, 메인쿼리는 서브 쿼리의 컬럼을 사용할 수없다. 질의 결과에 서브 쿼리 컬럼을 표시해야 한다면 조인방식으로 변환하거나 함수, 스칼라 서브쿼리 등을 사용해야한다.

__알아두기__
- SELECT절에 작성되는 서브쿼리 : 스칼라 서브쿼리
- FROM절에 작성되는 서브쿼리 : 인라인뷰

<br>

## 특징
---
서브 쿼리는 다음과 같은 특징을 가지고 있다.
- 서브쿼리는 괄호로 감싸서 기술해야한다.
- 서브쿼리는 단일행 또는 복수 행 비교 연산자와 함꼐 사용 가능하다. 단일행 비교 연산자는 서브 쿼리의 결과가 반드시 1건 이하이어야 하고, 복수 행 비교 연산자는 서브 쿼리의 결과 건수와 상관 없다.
- 중첩 서브 쿼리 및 스칼라 서브 쿼리에서는 ORDER BY를 사용하지 못한다.

<br>

## 종류
---
서브쿼리는 동작방식이나 반환되는 데이터의 형태에 따라 분류되며, 각 분류의 종류에 대한 내용은 다음과 같다.
- 반환되는 데이터의 형태에 따른 서브쿼리 분류
  - 단일 행 서브쿼리
  - 다중 행 서브쿼리
  - 다중 컬럼 서브쿼리

- 동작하는 방식에 따른 서브쿼리 분류
  - 연관 서브쿼리
  - 비연관 서브쿼리

### 단일 행 서브쿼리
서브쿼리의 실행 결과가 여러 건인 서브쿼리를 의미한다. 다중 행 서브쿼리는 다중행 비교 연산자와 함께 사용된다. 다중 행 비교 연산자에는 IN, ALL, ANY, SOME, EXISTS가 있다.

#### 예제
---
예제에 사용되는 릴레이션은 아래와 같다.

![DEPT 릴레이션](/posts/20221004/dept-relation.png "DEPT 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">DEPT 릴레이션</div>

- 쿼리
  
```sql
SELECT A.DEPTNO
	   , A.DNAME 
  FROM DEPT A
 WHERE A.LOC = (
                SELECT B.LOC
                  FROM DEPT B
                 WHERE B.DNAME = 'SALES'
               );
```

- 결과

![단일 행 서브쿼리 예제](/posts/20221019/query-example.png "단일 행 서브쿼리 예제"){: width="100%"}

### 다중 행 서브쿼리
서브쿼리의 실행 결과가 여러 건인 서브쿼리를 의미한다. 다중 행 서브쿼리는 다중행 비교 연산자와 함께 사용된다. 다중 행 비교 연산자에는 IN, ALL, ANY, SOME, EXISTS가 있다.

#### 예제
---
예제에 사용되는 릴레이션은 아래와 같다.

![DEPT 릴레이션](/posts/20221004/dept-relation.png "DEPT 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">DEPT 릴레이션</div>

- 쿼리
  
```sql
SELECT A.DEPTNO
	   , A.DNAME 
  FROM DEPT A
 WHERE A.LOC IN (
                 SELECT B.LOC
                   FROM DEPT B
                  WHERE B.DNAME LIKE '%A%'
                );
```

- 결과

![다중 행 서브쿼리 예제](/posts/20221019/query-example2.png "다중 행 서브쿼리 예제"){: width="100%"}

### 다중 컬럼 서브쿼리
서브쿼리의 실행 결과로 여러 컬럼을 반환한다. 메인쿼리의 조건절에 여러 컬럼을 동시에 비교할 수 있다. 서브쿼리와 메인쿼리에서 비교하고자 하는 컬럼개수와 컬럼의 위치가 동일해야 한다.
SQL Server에서는 지원되지 않는 문법이다.

#### 예제
---
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT *
  FROM EMP A
 WHERE (A.DEPTNO, A.SAL) IN (
                             SELECT B.DEPTNO, MAX(B.SAL)
                               FROM EMP B
                              GROUP BY B.DEPTNO
                            );
```

- 결과

![다중 컬럼 서브쿼리 예제](/posts/20221019/query-example3.png "다중 컬럼 서브쿼리 예제"){: width="100%"}

### 연관 컬럼 서브쿼리
서브쿼리가 메인쿼리 컬럼을 갖고 있는 형태의 서브쿼리다. 일반적으로 메인쿼리가 먼저 수행되어 읽혀진 데이터를 서브쿼리에서 조건이 맞는지 확인하고자 할 때 주로 사용한다.

#### 예제
---
예제에 사용되는 릴레이션은 아래와 같다.

![DEPT 릴레이션](/posts/20221004/dept-relation.png "DEPT 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">DEPT 릴레이션</div>

![EMP 릴레이션](/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT *
  FROM EMP A, DEPT B
 WHERE A.SAL < (
                SELECT AVG(X.SAL)
                  FROM EMP X
                 WHERE X.DEPTNO = A.DEPTNO 
                 GROUP BY X.DEPTNO
               )
   AND B.DEPTNO = A.DEPTNO;
```

- 결과

![연관 컬럼 서브쿼리 예제](/posts/20221019/query-example4.png "연관 컬럼 서브쿼리 예제"){: width="100%"}

### 비연관 컬럼 서브쿼리
서브쿼리가 메인쿼리 컬럼을 갖고 있지 않는 형태의 서브쿼리이며, 메인쿼리에 값(서브쿼리가 실행된 결과)을 제공하기 위한 목적으로 주로 사용한다. 비연관 컬럼 서브쿼리는 예제를 생략하겠다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  