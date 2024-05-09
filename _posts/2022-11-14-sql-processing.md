---
title: "[SQL] 처리 과정"
categories:
  - SQL
tags:
  - SQL
  - Processing
  - 처리 과정
toc: true
toc_sticky: true
toc_label: "목차"
comments: true
---

# 구조적, 집합적, 선언적 질의 언어
---
SQL은 'Structured Query Language'의 줄임말이다. 말 그대로 구조적 질의 언어다. 오라클 PL/SQL, SQL Server T-SQL 처럼 절차적 프로그래밍 기능을 구현할 수 있는 확장 언어도 제공하지만, SQL은 기본적으로 구조적이고 집합적이고 선언적인 질의 언어이다.

원하는 결과 집합을 구조적·집합적으로 선언하지만, 그 결과 집합을 만드는 과정은 절차적일 수 밖에 없다. 즉 프로시저가 필요한데, 그런 프로시저를 만들어 내는 DBMS 내부 엔진이 바로 SQL 옵티마이저다. 옵티마이저가 프로그래밍을 대신해 주는 셈이다.

<br>

# SQL 처리 과정
---
SQL 처리 과정을 오라클 기준으로 좀 더 자세히 표현하면 아래와 같다.

![SQL 처리 과정](/assets/img/posts/20221114/sql-processing.png "SQL 처리 과정"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">SQL 처리 과정</div>

처리 과정에서 표현된 각 서브엔진의 역할은 다음과 같다.

![서브엔진별 역할](/assets/img/posts/20221114/roles-by-sub-engine.png "서브엔진별 역할"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">서브엔진별 역할</div>

오라클뿐만 아니라 다른 DBMS도 비슷한 처리과정을 통해 실행계획을 생성한다. 쿼리 최적화 과정 즉, Parser와 Optimizer 역할에 해당하는 내용은 다음과 같다.

1. 쿼리를 내부 표현방식으로 변환
2. 표준적인 형태로 변환
3. 후보군이 될 만한 (낮은 레벨의) 프로시저를 선택
4. 실행계획을 생성하고 가장 비용이 적은 것을 선택

<br>

# SQL 옵티마이저
---
SQL 옵티마이저는 사용자가 원하는 작업을 가장 효율적으로 수행할 수 있는 최적의 데이터 액세스 경로를 선택해 주는 DBMS의 핵심 엔진이다. 옵티마이저의 최적화 단계를 요약하면 다음과 같다.
1. 사용자로부터 전달받은 쿼리를 수행하는 데 후보군이 될만한 실행계획들을 찾아낸다.
2. 데이터 딕셔너리에 미리 수집해 둔 오브젝트 통계 및 시스템 통계정보를 이용해 각 실행계획의 예상비용을 산정한다.
3. 최저 비용을 나타내는 실행계획을 선택한다.

<br>

# 실행계획과 비용
---
옵티마이저가 특정 실행계획을 채택하는 근거는 무엇일까. 이를 알아보기 위해 예제를 살펴보자.

## 예제
테스트를 위한 준비는 다음과 같다.

- 테이블 생성

```sql
CREATE TABLE T AS
SELECT D.NO, E.*
  FROM EMP E
     , (SELECT ROWNUM NO FROM DUAL CONNECT BY LEVEL <= 1000) D;
```

- 인덱스 생성

```sql
CREATE INDEX PK_EMP ON T(DEPTNO, NO);
CREATE INDEX PK_EMP2 ON T(DEPTNO, JOB, NO);
```

- 생성한 테이블 통계정보 수집

```sql
EXEC DBMS_STATS.GATHER_TABLE_STATS(OBOB, 'EMP');
```

준비는 끝났다. 이제 실행 계획을 확인해 보자.

```sql
SET AUTOTRACE TRACEONLY EXP;

SELECT *
  FROM EMP
 WHERE DEPTNO = 10
   AND NO = 1;
```

![실행계획 예제](/assets/img/posts/20221114/execution-plan-result-example.png "실행계획 예제"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">실행계획 예제</div>

옵티마이저가 PK_EMP 인덱스를 선택했다. PK_EMP2 인덱스를 선택할 수 있고, 테이블을 Full Scan할 수도 있다. PK_EMP 인덱스를 선택한 근거는 무엇일까?

위 실행계획에서 맨 우측에 Cost가 2로 표시되어있다. 그런데 PK_EMP2 인덱스를 사용하도록 index 힌트를 지정하고 실행계획을 확인해 보면 19, Full Scan하도록 Full 힌트를 지정하고 실행계획을 확인해보면 Cost가 29로 표시된다.

우리는 예제를 통해 옵티마이저가 PK_EMP 인덱스를 선택한 근거가 비용임을 알 수 있다. 비용은 쿼리를 수행하는 동안 발생할 것으로 예상하는 I/O 횟수 또는 예상 소요시간을 표현한 값이다. 실행경로를 선택하기 위해 옵티마이저가 여러 통계정보를 활용해서 계산해 낸 값이다. 실측치가 아니므로 실제 수행할 때 발생하는 I/O 또는 시간과 많은 차이가 날 수 있다.

<br>

# 옵티마이저 힌트
---
통계정보가 정확하지 않거나 기타 다른 이유로 옵티마이저가 잘못된 판단을 할 수 있다. 그럴 때 프로그램이나 데이터 특성 정보를 정확히 알고 있는 개발자가 직접 인덱스를 지정하거나 조인 방식을 변경함으로써 더 좋은 실행계획으로 유도하는 메커니즘이 필요한데, 옵티마이저 힌트가 바로 그것이다.

## 기술 방법
힌트 종류와 구체적인 사용법은 DBMS마다 천차만별이다. 따라서 오라클에서의 힌트 종류와 사용법만 살펴보도록 하겠다. 오라클에서 힌트를 기술하는 방법은 다음과 같다.

### 예제
```sql
SELECT /*+ LEADING(e2 e1) USE_NL(e1) INDEX(e1 emp_emp_id_pk)
           USE_MERGE(j) FULL(j) */
       e1.first_name, e1.last_name, j.job_id, sum(e2.salary) total_sal
  FROM employees e1, employees e2, job_history j
 WHERE e1.employee_id = e2.manager_id
   AND e1.employee_id = j.employee_id
   AND e1.hire_date = j.start_date
 GROUP BY e1.first_name, e1.last_name, j.job_id
 ORDER BY total_sal;
```

index 힌트에는 인덱스명 대신 다음과 같이 컬럼명을 지정할 수 있다.

```sql
SELECT /*+ LEADING(e2 e1) USE_NL(e1) INDEX(e1 (emplyee_id))
```

## 힌트가 무시되는 경우
다음과 같은 경우에 오라클 옵티마이저는 힌트를 무시하고 최적화를 진행한다.

- __문법적으로 맞지않게 힌트를 기술__  

- __의미적으로 맞지않게 힌트를 기술__  
예를 들어 서브쿼리에 unnest와 push_subq를 같이 기술한 경우(unnest되지않은 서브쿼리만이 push_subq 힌트의 적용 대상임)

- __잘못된 참조 사용__  
없는 테이블이나 별칭을 사용하거나 없는 인덱스명을 지정한 경우 등

- __논리적으로 불가능한 액세스 경로__  
조인절에 등치(=)조건이 하나도 없는데 해시 조인으로 유도하거나, 아래처럼 null 허용컬럼에 대한 인덱스를 이용해 전체 건수를 세려고 시도하는 등

```sql
SELECT /*+ index(e emp_ename_idx) */ count(*) FROM EMP e
```

- __버그__  
위 4가지 경우에 해당하지 않는 한 옵티마이저는 힌트를 가장 우선적으로 따른다. 즉, 옵티마이저는 힌트를 선택 가능한 옵션 정도로 여기는 게 아니라 사용자로부터 주어진 명령어로 인식한다. 여기서 주의할 점이 있다. 오라클은 사용자가 힌트를 잘못 기술하거나 잘못된 참조를 사용하더라도 에러가 발생하지 않는다는 사실이다. 힌트와 관련한 오라클의 이런 정책은 프로그램 안정성 측면에 도움이 되는가 하면, 성능 측면에서 불안할 때도 있다. 예를 들어 힌트에 사용된 인덱스를 어느 날 DBA가 삭제하거나 이름을 바꾸었다고하자. 그럴 때 SQL Server에선 에러가 발생하므로 해당 프로그램을 수정하고 다시 컴파일해야 한다. 프로그램을 수정하다 보면 인덱스 변경이 발생했다는 사실을 발견하게 되고, 성능에 문제가 생기지 않도록 적절한 조치를 취할 것이다.

반면 오라클에선 프로그램을 수정할 필요가 없어 좋지만, 내부적으로 Full Table Scan을 하거나 다른 인덱스가 사용되면서 성능이 갑자기 나빠질 수 있다. 애플리케이션 운영자는 사용자가 불평하기 전까지 그런 사실을 알지 못하며, 사용 빈도가 높은 프로그램에서 그런 현상이 발생해 시스템이 멎기도 한다.

DBMS마다 이처럼 차이가 있다는 사실을 미리 숙지하고, 애플리케이션 특정(안정성 우선, 성능 우선 등)에 맞게 개발 표준과 DB 관리정책을 수립할 필요가 있다.

## 힌트 종류
오라클은 공식적으로 다음과 같이 많은 종류의 힌트를 제공하며, 비공식 힌트까지 합치면 350여 개(12c 기준)에 이른다. 비공식 힌트까지 모두 알 필요는 없지만, 최소한 아래 내용의 힌트들은 그용도와 사용법에 맞게 숙지할 필요가 있다.

![오라클 힌트](/assets/img/posts/20221114/oracle-hint-type.png "오라클 힌트"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">오라클 힌트</div>

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  