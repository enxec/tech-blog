---
title: "[SQL] 집합연산자"
categories:
  - SQL
tags:
  - SQL
  - Set operator
  - 집합연산자
toc: true
toc_sticky: true
toc_label: "목차"
comments: true
---

# 집합연산자란
---
두 개 이상의 테이블에서 조인을 사용하지 않고 연관된 데이터를 조회하는 방법이 있다. 바로 집합 연산자를 사용하는 방법이다. 조인은 조인 조건을 사용해 여러 테이블의 행과 행을 서로 연결한다. 하지만 집합연산자는 여러 개의 결과 집합 간의 연산을 통해 결합하는 방식을 사용한다. 즉 집합 연산자는 2개 이상의 질의 결과를 하나의 결과로 만들어 준다.

집합 연산자는 서로 다른 테이블에서 유사한 형태의 결과를 반환하는 것을 하나의 결과로 합치고자 할 때와 동일 테이블에서 서로 다른 질의를 수행해 결과를 합치고자 할 때 사용할 수 있다. 이외에도 튜닝 관점에서 실행계획을 분리하고자 하는 목적으로도 사용할 수 있다.

__중요__
집합 연산자를 사용하기 위해서는 SELECT절의 컬럼수가 동일하고 SELECT절의 동일 위치에 존재하는 컬럼의 데이터 타입이 동일해야 한다.

<br>

# 종류
---
집합 연산자의 종류는 다음과 같다.
- UNION (합집합)
- UNION ALL (합집합)
- INTERSECT (교집합)
- MINUS/EXCEPT (차집합)

## UNION
UNION은 개별 SQL문의 결과에 대해 합집합 연산을 수행한다. 단 결과에서 모든 중복된 행은 하나의 행으로 만든다.

### 예제
---
예제에 사용되는 릴레이션은 아래와 같다.

![DEPT 릴레이션](/blog/assets/img/posts/20221004/dept-relation.png "DEPT 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">DEPT 릴레이션</div>

- 쿼리
  
```sql
SELECT A.DNAME
  FROM DEPT A
 WHERE A.DEPTNO = '10'
 UNION
SELECT B.DNAME 
  FROM DEPT B
 WHERE B.DEPTNO = '20';
```

- 결과

![UNION 예제](/blog/assets/img/posts/20221020/query-example.png "UNION 예제"){: width="100%"}

## UNION ALL
개별 SQL문의 결과에 대해 합집합 연산을 수행하며, 중복된 행도 그대로 표시된다. 즉 단순히 개별 SQL문의 결과를 합쳐 하나의 결과로 출력하는 것이다. 일반적으로 여러 질의 결과가 상호 배타적일 때 많이 사용한다. 개별 SQL문의 결과가 서로 중복되지 않는 경우 UNION과 결과가 동일하다.(결과의 정렬 순서에는 차이가 있을 수 있음)

### 예제
---
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT *
  FROM EMP A
 WHERE A.JOB = 'MANAGER'
UNION ALL
SELECT *
  FROM EMP A
 WHERE A.MGR = '7839'
```

- 결과

![UNION ALL 예제](/blog/assets/img/posts/20221020/query-example2.png "UNION ALL 예제"){: width="100%"}

## INTERSECT
개별 SQL문의 결과에 대해 교집합 연산을 수행한다. 단, 결과에서 모든 중복된 행은 하나의 행으로 만든다.

### 예제
---
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT *
  FROM EMP A
 WHERE A.COMM IS NOT NULL
INTERSECT
SELECT *
  FROM EMP A
 WHERE A.DEPTNO = '30';
```

- 결과

![UNION 예제](/blog/assets/img/posts/20221020/query-example3.png "UNION 예제"){: width="100%"}

## MINUS/EXCEPT
개별 SQL문의 결과에 대해 차집합 연산을 수행한다. 단 결과에서 모든 중복된 행은 하나의 행으로 만든다. (Oracle : MINUS / SQL Server : EXCEPT)

### 예제
---
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT *
  FROM EMP A
MINUS
SELECT *
  FROM EMP A
 WHERE A.COMM IS NOT NULL;
```

- 결과

![MINUS 예제](/blog/assets/img/posts/20221020/query-example4.png "MINUS 예제"){: width="100%"}

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  