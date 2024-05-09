---
title: "[SQL] GROUP BY절과 HAVING절"
categories:
  - SQL
tags:
  - SQL
  - GROUP BY
  - HAVING
toc: true
toc_sticky: true
toc_label: "목차"
comments: true
---

WHERE절을 통해 조건에 맞는 데이터를 조회했지만 테이블에 1차적으로 존재하는 데이터 이외의 정보, 예를 들면 팀별로 선수가 몇명인지, 선수들의 평균 신장과 몸무게가 얼마나 되는지, 또는 각팀에서 가장 큰 키의 선수가 누구인지 등의 2차 가공 정보도 필요하다. 이런 경우 사용되는 것이 GROUP BY절이며, 특징이 무엇이고 어떻게 활용하는지 함께 알아보도록 하자.

# GROUP BY절
---
GROUP BY절은 SQL문에서 FROM절과 WHERE 절 뒤에 오며, 데이터들을 작은 그룹으로 분류해 소그룹에 대한 항목별로 통계 정보를 얻을 때 추가로 사용된다. 이러한 GROUP BY의 특징은 다음과 같다.
- GROUP BY절을 통해 소그룹별 기준을 정한 후, SELECT절에 집계함수를 사용한다.
- GROUP BY절에서는 SELECT절과는 달리 ALIAS명을 사용할 수 없다.

추가 특징으로 일부 DB의 과거 버전에서 DB가 GROUP BY절에 명시된 컬럼의 순서대로 오름차순 정렬을 자동으로 실시(비공식적인 지원이었음)하는 경우가 있었다. 하지만 원칙적으로 RDBMS환경에서는 ORDER BY절을 명시해야 데이터 정렬이 수행된다. ANSI/ISO 기준에서도 데이터 정렬에 대한 내용은 ORDER BY절에서만 언급돼 있지, GROUP BY절에는 언급되어 있지 않다.

## 사용법
```sql
SELECT 컬럼명
  FROM 테이블명
 GROUP BY 컬럼 및 표현식;
```

## 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/assets/img/posts/20220925/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 예제 쿼리 및 결과

```sql
SELECT A.JOB AS 직업
     , AVG(A.COMM) AS "커미션 평균"
  FROM EMP A
 GROUP BY A.JOB;
```

![GROUP BY 예제](/assets/img/posts/20221010/query-example2.png "GROUP BY 예제"){: width="100%"}

<br>

# HAVING절
---
HAVING절은 WHERE절과 비슷하지만 그룹을 나타내는 결과 집합의 행에 조건이 적용된다는 점에서 차이가 있다. 이러한 HAVING절의 특징은 다음과 같다.
- GROUP BY절의 기준 항목이나 소그룹의 집계함수를 이용한 조건을 표시할 수 있다.
- GROUP BY절에 의한 소그룹별로 만들어진 집계 데이터에 한해, HAVING절에서 제한 조건을 두어 조건을 만족하는 내용만 출력한다.
- HAVING절은 일반적으로 GROUP BY절 뒤에 위치한다.

## 사용법
```sql
SELECT 컬럼명
  FROM 테이블명
 GROUP BY 컬럼 및 표현식
HAVING 그룹조건식;
```

## 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/assets/img/posts/20220925/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 예제 쿼리 및 결과

```sql
SELECT A.JOB AS 직업
     , ROUND(AVG(A.SAL)) AS "연봉 평균"
  FROM EMP A
 GROUP BY A.JOB
HAVING AVG(A.SAL) > 2500;
```

![GROUP BY 예제](/assets/img/posts/20221010/query-example2.png "GROUP BY 예제"){: width="100%"}

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원