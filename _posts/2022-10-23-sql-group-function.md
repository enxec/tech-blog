---
title: "[SQL] 그룹 함수"
categories:
  - SQL
tags:
  - SQL
  - Group Function
  - 그룹 함수
toc: true
toc_sticky: true
toc_label: "목차"
comments: true
---

# 개요
---
결산 개념의 업무를 가지는 원가나 판매 시스템의 경우는 소계, 중계, 합계, 총합계 등 여러 레벨의 결산 보고서를 만드는 것이 중요 업무 중 하나이다. 개발자들이 이런 보고서를 작성하기 위해서는 SQL이 포함된 3GL로 배치 프로그램을 작성하거나, 레벨별 집계를 위한 여러 단계의 SQL을 UNION과 UNION ALL로 묶은 후 하나의 테이블을 여러 번 읽어 다시 재정렬하는 복잡한 단계를 거쳐야 했다. 그러나 그룹함수가 등장하고난 이후로 하나의 SQL로 테이블을 한번만 읽어서 빠르게 원하는 리포트를 작성할 수 있다. 추가로 소계 및 합계를 표시하기 위해 GROUPING 함수와 CASE 함수를 이용하면 쉽게 원하는 포맷의 보고서 작성도 가능하다.

<br>

# 종류
---
그룹함수는 집계함수를 제외하고, 소그룹 간의 소계를 계산하는 ROLLUP 함수, GROUP BY 항목 간 다차원적인 소계를 계산할 수 있는 CUBE 함수, 특정 항목에 대한 소계를 계산하는 GRUOPING SETS 함수가 있다.

## ROLLUP
ROLLUP은 GROUP BY의 확장된 형태로 사용하기가 쉬우며 병렬로 수행할 수 있어 매우 효과적일 뿐 아니라 시간 및 지역처럼 계층적 분류를 포함하고 있는 데이터의 집계에 적합하다. ROLLUP에 지정된 Grouping Columns의 List는 Subtotal을 생성하기 위해 사용되며, Grouping Columns의 수를 N이라고 했을 때 N+1 Level의 Subtotal이 생성된다. 중요한 것은, ROLLUP의 인수는 계층 구조이므로 인수 순서가 바뀌면 수행 결과도 바뀌게 되므로 인수으ㅢ 순서에도 주의해야 한다.

### 사용법
```sql
SELECT 컬럼명, 컬럼명, ...
  FROM 테이블명
 GROUP BY ROLLUP(컬럼명, 컬럼명, ...);
```

### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.DEPTNO 
     , A.JOB
     , AVG(A.SAL) 
  FROM EMP A
 GROUP BY ROLLUP(A.DEPTNO, A.JOB);
```

- 결과

![ROLLUP 예제](/blog/assets/img/posts/20221023/query-example.png "ROLLUP 예제"){: width="100%"}

## CUBE
CUBE는 결합 가능한 모든 값에 대해 다차원적인 집계를 생성한다. ROLLUP에 비해 다양한 데이터를 얻는 장점이 있는 반면, 시스템에 부하를 많이 주는 단점이 있다. CUBE를 사용할 때는 내부적으로 Grouping Columns의 순서를 바꾸어서 또 한번의 쿼리를 추가 수행해야 한다. 그뿐만 아니라 Grand Total은 양쪽의 쿼리에서 모두 생성되므로 한 번의 쿼리에서는 제거되어야 하며, ROLLUP에 비해 시스템의 연산 대상이 많다. 

이처럼 GROUPING 컬럼이 가질 수 있는 모든 경우에 대해 Subtotal을 생성해야 하는 떄는 CUBE를 사용하는 것이 바람직하나, ROLLUP에 비해 시스템에 많은 부담을 주므로 사용에 주의해야 한다. CUBE함수의 경우 표시된 인수들에 대한 계층별 집계를 구할 수 있으며, 이때 표시된 인수 간에는 계층 구조인 ROLLUP과는 달리 평등한 관계이므로 인수의 순서가 바뀌는 경우 행간에 정렬 순서는 바뀔 수 있어도 데이터 결과는 같다.

### 사용법
```sql
SELECT 컬럼명, 컬럼명, ...
  FROM 테이블명
 GROUP BY CUBE(컬럼명, 컬럼명, ...);
```

### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.DEPTNO 
     , A.JOB
     , AVG(A.SAL) 
  FROM EMP A
 GROUP BY CUBE(A.DEPTNO, A.JOB)
 ORDER BY A.DEPTNO;
```

- 결과

![CUBE 예제](/blog/assets/img/posts/20221023/query-example2.png "CUBE 예제"){: width="100%"}

## GROUPING SETS
GROUPING SETS를 이용해 더욱 다양한 소계 집합을 만들 수 있는데, GROUP BY SQL 문장을 여러번 반복하지 않아도 원하는 결과를 쉽게 얻을 수 있게 됬다.

GROUPING SETS에 표시된 인수들에 대한 개별 집계를 구할 수 있으며, 이때 표시된 인수 간에는 계층 구조인 ROLLUP과 달리 평등한 관계이므로 인수의 순서가 바뀌어도 결과는 같다. 그리고 GROUPING SETS함수도 결과에 대한 정렬이 필요한 경우는 ORDER BY절에 명시적으로 정렬 컬럼이 표시되야 한다.

### 사용법
```sql
SELECT 컬럼명, 컬럼명, ...
  FROM 테이블명
 GROUP BY GROUPING SETS(컬럼명, 컬럼명, ...);
```

### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.DEPTNO 
     , A.JOB
     , AVG(A.SAL) 
  FROM EMP A
 GROUP BY GROUPING SETS(A.DEPTNO, A.JOB);
```

- 결과

![GROUPING SETS 예제](/blog/assets/img/posts/20221023/query-example3.png "GROUPING SETS 예제"){: width="100%"}

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  