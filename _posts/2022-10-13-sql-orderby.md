---
title: "[SQL] ORDER BY절"
description: 
author: Enxec
date: 2022-10-13
categories: [Language, SQL]
tags: [sql, ORDER BY]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/sql-logo.png
  lqip: 
  alt: 
---

ORDER BY절은 SQL문장으로 조회한 데이터들을 다양한 목적에 맞게 특정 컬럼을 기준으로 정렬 및 출력하는데 사용한다. 어떻게 사용하는지 바로 알아보자.

## 사용법
---
```sql
SELECT 컬럼명
  FROM 테이블명
 ORDER BY 컬럼 또는 표현식 [ASC 또는 DESC];
```

ORDER BY절에 컬럼명 대신 SELECT절에서 사용한 ALIAS명이나 컬럼 순서를 나타내는 정수도 사용 가능하다. 그리고 별도로 정렬 방식을 지정하지 않으면 기본적으로 오름차순이 적용되며, SQL 문장의 제일 마지막에 위치한다.

ORDER BY절은 2가지의 정렬방식이 있으며 내용은 다음과 같다.
- ASC(Ascending)
  >조회한 데이터를 오름차순으로 정렬한다.(기본 값이므로 생략 가능)
- DESC(Descending)
  >조회한 데이터를 내림차순으로 정렬한다.

## 예제
---
예제에 사용되는 릴레이션은 아래와 같다.

![DEPT 릴레이션](/posts/20221004/dept-relation.png "DEPT 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">DEPT 릴레이션</div>

- 쿼리

```sql
SELECT DEPTNO
     , DNAME
     , LOC
  FROM DEPT
 ORDER BY LOC;
```

- 결과

![ORDER BY절 예제](/posts/20221013/query-example.png "ORDER BY절 예제"){: width="100%"}

## ORDER BY 특징
---
위 예제를 보면 LOC에 값이 존재하지 않는 컬럼이 있다. 현재 40번 부서 LOC가 NULL인데, 오름차순에서 NULL이 밑에 출력됬다는 것은 오라클이 NULL 값을 가장 높은 값으로 취급했음을 말한다. 반면 SQL Server는 반대의 정렬 순서를 가진다. 때문에 벤더별로 ORDER BY 사용 시 주의해야하며, 이 점 외에 ORDER BY의 특징이 몇가지 더 있는데 잠깐 살펴보고 글을 마무리한다.
- 기본적인 정렬 순서는 오름차순(ASC)이다.
- 숫자형 데이터 타입은 오름차순으로 정렬했을 경우에 가장 작은 값부터 출력된다.
- 날짜형 데이터 타입은 오름차순으로 정렬했을 경우 날짜가 가장 빠른 값이 먼저 출력된다.
- 오라클에서는 NULL 값을 가장 큰 값으로 간주해 오름차순으로 정렬했을 경우에는 가장 마지막에, 내림차순으로 정렬했을 경우에는 가장 먼저 위치한다.
- 반면 SQL Server에서는 NULL 값을 가장 작은 값으로 간주하므로 오름차순으로 정렬했을 경우에는 가장 먼저, 내림차순으로 정렬했을 경우에는 가장 마지막에 위치한다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  