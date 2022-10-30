---
title: "[SQL] 계층형 질의"
categories:
  - SQL
tags:
  - SQL
  - hierarchical query
  - 계층형 질의
toc: true
toc_sticky: true
toc_label: "목차"
---

# 개요
---
테이블에 계층형 데이터가 존재하는 경우 데이터를 조회하기 위해서 계층형 질의를 사용한다. 계층형 데이터란 동일 테이블에 계층적으로 상위와 하위 데이터가 포함된 데이터를 말한다. 예를 들어 사원 테이블에서는 사원들 사이에 상위 사원(관리자)과 하위 사원 관계가 존재하고, 조직 테이블에서는 조직들 사이에 상위 조직과 하위 조직 관계가 있다. 엔터티를 순환 관계 데이터 모델로 설계할 경우 계층형 데이터가 발생한다. 순환 관계 데이터 모델의 예로는 조직, 사원, 메뉴 등이 있다.

<br>

# 사용법
---
계층형 데이터 조히는 DBMS 벤더와 버전에 따라 다른 방법으로 지원한다. 이 글에서는 오라클 기준으로 살펴보겠다. 사용법은 다음과 같다.

```sql
 SELECT 컬럼명, 컬럼명 ...
   FROM 테이블명
  WHERE 조건 ...
  START WITH 조건
    AND 조건
CONNECT BY [NOCYCLE] 조건
    AND 조건 ...
[ORDER SIBLINGS 컬럼명, 컬럼명, ...];
```

- START WITH
  >계층 구조 전개의 시작 위치를 지정하는 구문이다. 즉 루트 데이터를 지정한다.(액세스)
- CONNECT BY
  >다음에 전개될 자식 데이터를 지정하는 구문이다. 자식 데이터는 CONNECT BY 절에 주어진 조건을 만족해야 한다.(조인)
- PRIOR
  >CONNECT BY절에 사용되며, 현재 읽은 컬럼을 지정한다. (FK) = PRIOR (PK) 형태를 사용하면 부모 데이터에서 자식 데이터(부모 -> 자식) 방향으로 전개하는 순방향 전개를 한다. 그리고 (PK) = PRIOR(FK) 형태를 사용하면 반대로 자식 데이터에서 부모 데이터(자식 -> 부모) 방향으로 전개하는 역방향 전개를 한다.
- NOCYCLE
  >데이터를 전개하면서 이미 나타났던 동일한 데이터가 전개 중에 다시 나타난다면, 이것을 가리켜 사이클이 발생했다고 한다. 사이클이 발생한 데이터는 런타임 오류가 발생한다. NOCYCLE을 추가하면 오류를 발생시키지 않고 사이클이 발생한 이후의 데이터를 전개하지 않는다.
- ORDER SIBLING BY
  >형제 노드(동일 LEVEL) 사이에서 정렬을 수행한다.
- WHERE
  >모든 전개를 수행한 후에 지정된 조건을 만족하는 데이터만 추출한다.(필터링)

<br>

오라클은 계층형 질의를 사용할 때 다음과 같은 가상 컬럼을 제공한다.

- LEVEL
  >루트 데이터이면 1, 그 하위 데이터이면 2다. 리프 데이터까지 1씩 증가한다.
- CONNECT_BY_ISLEAF
  >전개 과정에서 해당 데이터가 리프 데이터이면 1, 그렇지 않으면 0이다.
- CONNECT_BY_ISCYCLE 
  >전개 과정에서 자식을 갖는데, 해당 데이터가 조상으로서 존재하면 1, 그렇지 않으면 0이다. 여기서 조상이란 자신으로부터 루트까지의 경로에 존재하는 데이터를 말한다. CYCLE 옵션을 사용했을 때만 사용할 수 있다.

<br>

그리고 오라클은 계층형 질의를 사용할 때 사용자 편의성을 제공하기 위해서 다음과 같은 함수를 제공한다.

- SYS_CONNECT_BY_PATH
  >루트 데이터부터 현재 전개할 데이터까지의 경로를 표시한다.
  >
  >사용법 : SYS_CONNECT_BY_PATH(컬럼, 경로분리자)
- CONNECT_BY_ROOT
  >현재 전개할 데이터의 루트 데이터를 표시한다. 단항 연산자이다.
  >
  >사용법 : CONNECT_BY_ROOT 컬럼

<br>

# 예제 1
---
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
 SELECT LEVEL AS LV
      , LPAD(' ', (LEVEL - 1) * 2) || EMPNO AS EMPNO
      , ENAME
      , A.MGR
      , CONNECT_BY_ISLEAF AS ISLEAF
   FROM EMP A
  START WITH A.MGR IS NULL
CONNECT BY MGR = PRIOR EMPNO;
```

- 결과

![계층형 질의 예제](/blog/assets/img/posts/20221031/query-example.png "계층형 질의 예제"){: width="100%"}

<br>

# 예제 2
---
예제에 사용되는 릴레이션은 아래와 같다.

![EMP 릴레이션](/blog/assets/img/posts/20221017/emp-relation.png "EMP 릴레이션"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">EMP 릴레이션</div>

- 쿼리
  
```sql
 SELECT CONNECT_BY_ROOT(EMPNO) AS ROOT_EMPNO
      , SYS_CONNECT_BY_PATH(EMPNO, ',') AS PATH
      , EMPNO
      , MGR
   FROM EMP
  START WITH MGR IS NULL
CONNECT BY MGR = PRIOR EMPNO;
```

- 결과

![계층형 질의 예제2](/blog/assets/img/posts/20221031/query-example2.png "계층형 질의 예제2"){: width="100%"}

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  