---
title: "[SQL] Top N 쿼리"
categories:
  - SQL
tags:
  - SQL
  - Top N query
  - Top N 쿼리
toc: true
toc_sticky: true
toc_label: "목차"
---

# ROWNUM 슈도 컬럼
---
오라클의 ROWNUM은 컬럼과 비슷한 성격의 Pseudo Column으로서 SQL 처리 결과 집합의 각 행에 대해 임시로 부여되는 일련번호다. 테이블이나 집합에서 원하는 만큼의 행만 가져오고 싶을 때 WHERE 절에서 행의 개수를 제한하는 목적으로 사용한다.

## 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.ENAME
     , A.SAL
  FROM (
        SELECT ENAME
             , SAL
          FROM EMP
         ORDER BY SAL DESC
       ) A
 WHERE ROWNUM <= 3;
```

- 결과

![ROWNUM 예제](/blog/assets/img/posts/20221030/query-example.png "ROWNUM 예제"){: width="100%"}

<br>

# ROW LIMITING 절
---
오라클은 12.1버전, SQL Server는 2012 버전부터 ROW LIMITING 절로 Top N 쿼리를 작성할 수 있다. ROW LIMITING 절은 ANSI 표준 SQL 문법이다. 아래는 ROW LIMITING 절의 구문이다. ROW LIMITING 절은 ORDER BY 절 다음에 기술하며, ORDER BY 절과 함께 수행된다. ROW와 ROWS는 구분하지 않아도 된다.

```sql
[OFFSET offset {ROW | ROWS}]
[FETCH {FIRST | NEXT} [{rowcount | percent PERCENT}] {ROW | ROWS} {ONLY | WITH TIES}]
```

- OFFSET offset : 건너뛸 행의 개수를 지정한다.
- FETCH : 반환할 행의 개수나 백분율을 지정한다.
- ONLY : 지정된 행의 개수나 백분율만큼 행을 반환한다.
- WITH TIES : 마지막 행에 대한 동순위를 포함해서 반환한다.

## 예제
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
SELECT A.EMPNO
     , A.SAL
  FROM EMP
 ORDER BY SAL, EMPNO FETCH FIRST 5 ROWS ONLY;
```

- 결과

![ROW LIMITING 예제](/blog/assets/img/posts/20221030/query-example2.png "ROW LIMITING 예제"){: width="100%"}

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  