---
title: "[SQL] 집계 함수"
description: 
author: Enxec
date: 2022-10-10
categories: [Language, SQL]
tags: [sql, Aggregate function, 집계 함수]
pin: false
math: true
mermaid: true
image:
  path: /thumbnail/sql-logo.png
  lqip: 
  alt: 
---

집계함수는 여러 행들의 그룹이 모여서 그룹당 단 하나의 결과를 돌려주는 다중행 함수이다. 바로 본론으로 들어가 집계함수의 특징과 종류에는 무엇이 있는지 알아보자.

## 집계함수 특징
---
집계함수의 주요한 특징은 다음과 같다.
- 여러 행들의 그룹이 모여 그룹당 단 하나의 결과를 돌려주는 함수다.
- GROUP BY절은 행들을 소그룹화한다.
- SELECT절, HAVING절, ORDER BY절에 사용할 수 있다.

## 집계함수의 종류
---
빈번히 사용되는 집계함수 종류는 다음과 같다.
- COUNT(*)
  >NULL 값을 포함한 행의 수를 출력한다.
- COUNT(표현식)
  >표현식의 값이 NULL값인 것을 제외한 행 수를 출력한다.
- SUM([DISTINCT | ALL] 표현식)
  >표현식의 NULL값을 제외한 합계를 출력한다.
- AVG([DISTINCT | ALL] 표현식)
  >표현식의 NULL값을 제외한 평균을 출력한다.
- MAX([DISTINCT | ALL] 표현식)
  >표현식의 최댓값을 출력한다.
- MIN([DISTINCT | ALL] 표현식)
  >표현식의 최소값을 출력한다.(문자, 날짜 데이터 타입도 사용가능)
- STDDEV([DISTINCT | ALL] 표현식)
  >표현식의 표준 편차를 출력한다.
- VARIANCE / VAR([DISTINCT | ALL] 표현식)
  >표현식의 분산을 출력한다.
- 기타 통계 함수
  >벤더별로 다양한 통계식을 제공한다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원