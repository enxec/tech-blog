---
title: "[SQL] 윈도우 함수"
categories:
  - SQL
tags:
  - SQL
  - Window Function
  - 윈도우 함수
toc: true
toc_sticky: true
toc_label: "목차"
---

# 개요
---
기존 관계형 DB는 컬럼과 컬럼 간의 연산·비교·연결이나 집합에 대한 집계는 쉬운 반면, 행과 행간의 관계를 정의하거나 행과 행간을 비교·연산하는 것을 하나의 SQL문으로 처리하는 것은 매우 어려운 문제였다. PL/SQL, SQL/PL, T-SQL, PRO*C 같은 절차형 프로그램을 작성하거나, INLINE VIEW를 이용해 복잡한 SQL문을 작성해야 하던 것을 부분적이나마 행과 행간의 관계를 쉽게 정의하기 위해 만든 함수가 바로 WINDOW FUNCTION이다. 윈도우 함수를 화용하면 복잡한 프로그램을 하나의 SQL문장으로 쉽게 해결할 수 있다.

분석함수나 순위함수로도 알려진 윈도우 함수는 데이터 웨어하우스에서 발전한 기능이다. SQL사용자로서는 INLINE VIEW 이후 SQL의 중요한 기능이 추가됬다고 할 수 있으며, 많은 프로그램이나 튜닝 팁을 대체할 수 있을 것이다.

복잡하거나 자원을 많이 사용하는 튜닝 기법들을 대체할 수 있는 DBMS의 새로운 기능은 튜닝 관점에서도 최적화된 방법이므로 적극적으로 활용할 필요가 있다. 같은 결과가 나오는 변형된 튜닝 문장보다는 DBMS벤더에서 최적화한 자원을 사용하도록 만들어진 새로운 기능을 사용하는 것이 일반적으로 더욱 효과가 좋기 때문이다.

윈도우 함수는 기존에 사용하던 집계함수도 있고, 새로이 윈도우 함수 전용으로 만들어진 기능도 있다. 그리고 윈도우 함수는 다른 함수와는 달리 중첩해서 사용하지 못하지만, 서브 쿼리에서는 사용할 수 있다.

<br>

# 구문
윈도우 함수에는 OVER 문구가 키워드로 필수 포함되며, 사용법은 다음과 같다.

```sql
SELECT WINDOW_FUNCTION (ARGUMENTS) OVER ([PARTITION BY 컬럼] [ORDER BY 절] [WINDOWING 절])
  FROM 테이블명;
```

- WINDOW_FUNCTION 
  >기존에 사용하던 함수도 있고, 새롭게 WINDOW 함수용으로 추가된 함수도 있다.
- ARGUMENTS (인수)
  >함수에 따라 0 ~ N개의 인수가 지정될 수 있다.
- PARTITION BY 절
  >전체 집합을 기준에 의해 소그룹으로 나눌 수 있다.
- ORDER BY 절
  >어떤 항목에 대해 순위를 지정할지 ORDER BY 절을 기술한다.
- WINDOWING 절
  >WINDOWING 절은 함수의 대상이 되는 행 기준의 범위를 강력하게 지정할 수 있다.
  >
  >WINDOWING 절은 ROWS와 RANGE 옵션을 가지는데, ROWS는 물리적인 결과 행의 수를, RANGE는 논리적인 값에 의한 범위를 나타내며,  둘 중 하나를 선택해서 사용할 수 있다. 다만 WINDOWING 절은 SQL Server에서는 지원하지 않는다.

<br>

# 종류
---
윈도우 함수의 종류는 크게 다섯 개의 그룹으로 분류할 수 있는데 벤더별로 지원하는 함수에는 차이가 있다. 내용은 다음과 같다.

## 그룹 내 순위 관련 함수
그룹 내 순위 관련 함수는 ANSI, Oracle, SQL Server 등 대부분의 DBMS에서 지원한다.

### RANK
RANK는 ORDER BY를 포함한 QUERY 문에서 특정 항목(컬럼)에 대한 순위를 구하는 함수다. 이때 특정 범위(PARTITION) 내에서 순위를 구할 수도 있고, 전체 데이터에 대한 순위를 구할 수도 있다. 또한 동일한 값에 대해서는 동일한 순위를 부여한다.

#### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.JOB
     , A.ENAME
     , A.SAL
     , RANK() OVER(ORDER BY A.SAL DESC) AS ALL_PK
     , RANK() OVER(PARTITION BY A.JOB ORDER BY A.SAL DESC) AS JOB_RK
  FROM EMP A;
```

- 결과

![RANK 예제](/blog/assets/img/posts/20221023/query-example4.png "RANK 예제"){: width="100%"}

### DENSE_RANK
DENSE_RANK 함수는 RANK와 유사하나 동일한 순위를 하나의 건수로 취급하는 점이 다르다.

#### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.JOB
     , A.ENAME
     , A.SAL
     , RANK() OVER(ORDER BY A.SAL DESC) AS RK
     , DENSE_RANK() OVER(ORDER BY A.SAL DESC) AS DK
  FROM EMP A;
```

- 결과

![DENSE_RANK 예제](/blog/assets/img/posts/20221023/query-example5.png "DENSE_RANK 예제"){: width="100%"}

### ROW_NUMBER
ROW_NUMBER 함수는 RANK나 DENSE_RANK 함수가 동일한 값에 대해서는 동일한 순위를 부여하는데 반해, 동일한 값이라도 고유한 순위를 부여한다.

#### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.JOB
     , A.ENAME
     , A.SAL
     , RANK() OVER(ORDER BY A.SAL DESC) AS RK
     , ROW_NUMBER() OVER(ORDER BY A.SAL DESC) AS DK
  FROM EMP A;
```

- 결과

![ROW_NUMBER 예제](/blog/assets/img/posts/20221023/query-example6.png "ROW_NUMBER 예제"){: width="100%"}

## 그룹 내 집계 관련 함수
그룹 내 집계 관련 함수는 ANSI, Oracle, SQL Server 등 대부분의 DBMS에서 지원하고 있는데, SQL Server의 경우 집계함수는 뒤에서 설명할 OVER절 내의 ORDER BY 구문을 지원하지 않는다.

### SUM
SUM함수를 이용해 파티션별 윈도우의 합을 구할 수 있다.

#### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.JOB
     , A.ENAME
     , A.SAL
     , SUM(SAL) OVER(PARTITION BY MGR) AS SAL_SUM
  FROM EMP A;
```

- 결과

![SUM 예제](/blog/assets/img/posts/20221023/query-example7.png "SUM 예제"){: width="100%"}

### MAX
MAX 함수는 파티션별 윈도우의 최댓값을 구할 수 있다.

#### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.JOB
     , A.ENAME
     , A.SAL
     , MAX(SAL) OVER(PARTITION BY MGR) AS SAL_MAX
  FROM EMP A;
```

- 결과

![MAX 예제](/blog/assets/img/posts/20221023/query-example8.png "MAX 예제"){: width="100%"}

### MIN
MIN 함수를 이용해 파티션별 윈도우의 최솟값을 구할 수 있다.

#### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.JOB
     , A.ENAME
     , A.SAL
     , MIN(SAL) OVER(PARTITION BY MGR) AS SAL_MIN
  FROM EMP A;
```

- 결과

![MIN 예제](/blog/assets/img/posts/20221023/query-example9.png "MIN 예제"){: width="100%"}

### AVG
AVG함수와 파티션별 ROWS 윈도우를 이용해 원하는 조건에 맞는 데이터에 대한 통곗값을 구할 수 있다.

#### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.JOB
     , A.ENAME
     , A.SAL
     , AVG(SAL) OVER(PARTITION BY MGR) AS SAL_AVG
  FROM EMP A;
```

- 결과

![AVG 예제](/blog/assets/img/posts/20221023/query-example10.png "AVG 예제"){: width="100%"}

### COUNT
COUNT 함수와 파티션별 ROWS 윈도우를 이용해 원하는 조건에 맞는 데이터에 대한 통곗값을 구할 수 있다.

#### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.ENAME
     , A.SAL
     , COUNT(*) OVER(ORDER BY SAL RANGE BETWEEN 50 PRECEDING AND 150 FOLLOWING) AS EMP_CNT
  FROM EMP A;
```

- 결과

![COUNT 예제](/blog/assets/img/posts/20221023/query-example11.png "COUNT 예제"){: width="100%"}

## 그룹 내 행 순서 관련 함수
그룹 내 행 순서 관련 함수는 오라클에서만 지원하는 함수이기는 하나, FIRST_VALUE, LAST_VALUE 함수는 MAX, MIN 함수와 비슷한 결과를 얻을 수 있다. LAG, LEAD 함수는 DW에서 유용하게 사용되는 기능이다.

### FIRST_VALUE
FIRST_VALUE 함수를 이용해 파티션별 윈도우에서 가장 먼저 나온 값을 구한다. SQL Server에서는 지원하지 않는 함수다. MIN 함수를 활용해 같은 결과를 얻을 수도 있다.

#### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.DEPTNO
     , A.ENAME
     , A.SAL
     , FIRST_VALUE(ENAME) OVER(PARTITION BY DEPTNO ORDER BY SAL DESC ROWS UNBOUNDED PRECEDING) AS ENAME_FV
  FROM EMP A;
```

- 결과

![FIRST_VALUE 예제](/blog/assets/img/posts/20221023/query-example12.png "FIRST_VALUE 예제"){: width="100%"}

### LAST_VALUE
LAST_VALUE 함수를 이용해 파티션별 윈도우에서 가장 나중에 나온 값을 구한다. SQL Server에서는 지원하지 않는 함수다. MAX 함수를 활용해 같은 결과를 얻을 수도 있다.

#### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.DEPTNO
     , A.ENAME
     , A.SAL
     , LAST_VALUE(ENAME) OVER(PARTITION BY DEPTNO ORDER BY SAL DESC ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING) AS ENAME_LV
  FROM EMP A;
```

- 결과

![LAST_VALUE 예제](/blog/assets/img/posts/20221023/query-example13.png "LAST_VALUE 예제"){: width="100%"}

### LAG
LAG 함수를 이용해 파티션별 윈도우에서 이전 몇 번째 행의 값을 가져올 수 있다. SQL Server에서는 지원하지 않는 함수다.

#### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.ENAME
     , A.HIREDATE
     , A.SAL
     , LAG(SAL) OVER(ORDER BY HIREDATE) AS LAG_SAL
  FROM EMP A
 WHERE JOB = 'SALESMAN';
```

- 결과

![LAG 예제](/blog/assets/img/posts/20221023/query-example14.png "LAG 예제"){: width="100%"}

### LEAD
LEAD 함수를 이용해 파티션별 윈도우에서 이후 몇 번째 행의 값을 가져올 수 있다. 참고로 SQL Server에서는 지원하지 않는 함수다.

#### 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.ENAME
     , A.HIREDATE
     , LEAD(HIREDATE, 1) OVER(ORDER BY HIREDATE) AS LEAD_HIREDATE
  FROM EMP A
 WHERE JOB = 'SALESMAN';
```

- 결과

![LEAD 예제](/blog/assets/img/posts/20221023/query-example15.png "LEAD 예제"){: width="100%"}

## 그룹 내 비율 관련 함수
CUME_DIST, PERCENT_RANK 함수는 ANSI/ISO SQL 표준과 오라클 DBMS에서 지원하며, NTILE함수는 ANSI/ISO SQL 표준에는 없지만, 오라클과 SQL Server에서 지원한다. 그리고 RATIO_TO_REPORT 함수는 오라클에서만 지원한다.

### CUME_DIST


### PERCENT_RANK

### NTILE

### RATIO_TO_REPORT

## 선형분석을 포함한 통계 분석 관련 
위 선형분석을 포함한 통계 분석 관련 함수는 오라클 DBMS에서만 지원한다.

### CORR

### COVAR_POP

### COVAR_SAMP

### STDDEV

### STDDEV_POP

### STDDEV_SAMP

### VARIANCE

### VAR_POP

### VAR_SAMP

### REGR_(LINEAR REGRESSION)

### REGR_SLOPE

### REGR_INTERCEPT

### REGR_COUNT

### REGR_R2

### REGR_AVGX

### REGR_AVGY

### REGR_SXX

### REGR_SYY

### REGR_SXY


---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  