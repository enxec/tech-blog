---
title: "[SQL] 데이터베이스 I/O 메커니즘"
categories:
  - SQL
tags:
  - SQL
  - Database I/O Mechanism
  - 데이터베이스 I/O 메커니즘
toc: true
toc_sticky: true
toc_label: "목차"
---

이번 포스팅에서는 DB I/O 효율화 및 버퍼캐시 최적화 방법을 이해하는 데 필요한 기본 개념과 원리를 알아보자. 데이터베이스 I/O 튜닝을 위해서는 인덱스, 조인, 옵티마이저 원리, 소트 원리 등에 관한 종합적인 이해가 필요하다.

<br>

# 블록 단위 I/O
---
오라클을 포함한 모든 DBMS에서 I/O는 블록(SQL Server 등 다른 DBMS는 페이지라는 용어를 사용) 단위로 이루어진다. 즉 하나의 레코드를 읽더라도 레코드가 속한 블록 전체를 읽는다.

SQL 성능을 좌우하는 가장 중요한 성능지표는 액세스하는 블록 개수이며, 옵티마이저의 판단에 가장 큰 영향을 미치는 것도 액세스해야 할 블록 개수다. 블록 단위 I/O는 버퍼 캐시와 데이터 파일 I/O 모두에 적용된다.

- 데이터 파일에서 DB 버퍼 캐시로 블록을 적재할 때
- 데이터 파일에서 블록을 직접 읽고 쓸 때
- 버퍼 캐시에서 블록을 읽고 쓸 때
- 버퍼 캐시에서 변경된 블록을 다시 데이터 파일에 쓸 때

<br>

# 메모리 I/O vs 디스크 I/O

## I/O 효율화 튜닝의 중요성
디스크를 경유한 데이터 입출력은 디스크의 액세스 암(Arm)이 움직이면서 헤드를 통해 데이터를 읽고 쓰기 때문에 느린 반면, 메모리를 통한 입출력은 전기적 신호에 불과하기 때문에 디스크를 통한 I/O에 비해 비교할 수 없을 정도로 빠르다. 모든 DBMS는 읽고자 하는 블록을 먼저 버퍼 캐시에서 찾아보고, 없을 경우에만 디스크에서 읽어 버퍼 캐시에 적재한 후 읽기/쓰기 작업을 수행한다. 물리적인 디스크 I/O가 필요할 때면 서버 프로세스는 시스템에 I/O Call을 하고 잠시 대기 상태에 빠진다. 디스크 I/O 경합이 심할수록 대기 시간도 길어진다.

![디스크 I/O 경합](/blog/assets/img/posts/20221115/disk-io-contention.png "디스크 I/O 경합"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">디스크 I/O 경합</div>

모든 데이터를 메모리에 올려 놓고 사용할 수 있다면 좋겠지만 비용과 기술 측면에 한계가 있다. 메모리는 물리적으로 한정된 자원이므로, 결국 디스크 I/O를 최소화하고 버퍼 캐시 효율을 높이는 것이 데이터베이스 I/O 튜닝의 목표가 된다.

## 버퍼 캐시 히트율
버퍼 캐시 효율을 측정하는 지표로서, 전체 읽은 블록 중에서 메모리 버퍼 캐시에서 찾은 비율을 나타낸다. 즉, 버퍼 캐시 히트율(Buffer Cache Hit Ratio, 이하 BCHR)은 물리적인 디스크 읽기를 수반하지 않고 곧바로 메모리에서 블록을 찾은 비율을 말한다. 

Direct Path Read 방식 이외의 모든 블록 읽기는 버퍼 캐시를 통해 이뤄진다. 읽고자 하는 블록을 먼저 버퍼 캐시에서 찾아보고, 없을 때만 디스크로부터 버퍼 캐시에 적재한 후 읽어 들인다.

```html
BCHR = (버퍼 캐시에서 곧바로 찾은 블록 수 / 총 읽은 블록 수) × 100
```

BCHR은 주로 시스템 전체적인 관점에서 측정하지만, 개별 SQL 측면에서 구해볼 수도 있는데 이 비율이 낮은 것이 SQL 성능을 떨어뜨리는 주원인이라고 할 수 있다.

```html
 call  count  cpu  elapsed  disk query current rows 
------ ----- ----- -------- ---- ----- ------- ---- 
Parse    15   0.00   0.08     0    0      0      0 
Execute  44   0.03   0.03     0    0      0      0 
Fetch    44   0.01   0.13    18   822     0     44 
------ ----- ----- -------- ---- ----- ------- ---- 
total   103   0.04   0.25    18   822     0     44
```

위에서 Disk 항목이 디스크를 경유한 블록 수를 의미하며, 버퍼 캐시에서 읽은 블록 수는 Query와 Current 항목을 더해서 구하게 된다. 따라서 위 샘플에서 BCHR은 98%다. 즉 100개 블록읽기를 요청하면 98개는 메모리에서 찾고, 나머지 2개는 디스크 I/O를 발생시켰다는 뜻이다.

- 총 읽은 블록 수 = 822
- 버퍼 캐시에서 곧바로 찾은 블록 수 = 822 - 18 = 804
- CHR = (822 - 18) / 822 = 97.8%

모든 블록 읽기는 버퍼 캐시를 경유하며, 디스크 I/O가 수반되더라도 먼저 버퍼 캐시에 적재한 후 읽는다고 했다. 총 읽은 블록 수(Query + Current)가 디스크로부터 읽은 블록 수를 이미 포함하므로, 총 읽은 블록 수를 840개(Disk + Query + Current)로 잘못 해석하지 않도록 주의하기 바란다. 

논리적인 블록 요청 횟수를 줄이고, 물리적으로 디스크에서 읽어야 할 블록 수를 줄이는 것이 I/O 효율화 튜닝의 핵심 원리다. 

같은 블록을 반복적으로 액세스하는 형태의 SQL은 논리적인 I/O 요청이 비효율적으로 많이 발생함에도 불구하고 BCHR은 매우 높게 나타난다. 이는 BCHR이 성능지표로서 갖는 한계점이라 할 수 있다. 예를 들어 NL Join에서 작은 Inner 테이블을 반복적으로 룩업(Lookup)하는 경우가 그렇다. 작은 테이블을 반복 액세스하면 모든 블록이 메모리에서 찾아져 BCHR은 높겠지만 일량이 작지 않고, 블록을 찾는 과정에서 래치(Latch) 경합과 버퍼 Lock 경합까지 발생한다면 메모리 I/O 비용이 디스크 I/O 비용보다 커질 수 있다. 따라서 논리적으로 읽어야 할 블록 수의 절대량이 많다면 반드시 튜닝을 통해 논리적인 블록 읽기를 최소화해야 한다.

## 네트워크, 파일시스템 캐시가 I/O 효율에 미치는 영향
대용량 데이터를 읽고 쓰는 데 다양한 네트워크 기술(DB서버와 스토리지 간에 NAS 서버나 SAN을 사용)이 사용됨에 따라 네트워크 속도도 SQL 성능에 크게 영향을 미치고 있다. 이에 하드웨어나 DBMS 벤더는 네트워크를 통한 데이터 전송속도를 향상시키려고 노력하고 있지만, 네트워크 전송량이 많을 수 밖에 없도록 SQL을 작성한다면 결코 좋은 성능을 기대할 수 없다. 따라서 SQL을 작성할 때는 다양한 I/O 튜닝 기법을 사용해서 네트워크 전송량을 줄이려고 노력하는 것이 중요하다. 

RAC 같은 클러스터링(Clustering) 데이터베이스 환경에선 인스턴스 간 캐시된 블록을 공유하므로 메모리 I/O 성능에도 네트워크 속도가 지대한 영향을 미치게 되었다. 같은 양의 디스크 I/O가 발생하더라도 I/O 대기 시간이 크게 차이 날 때가 있다. 디스크 경합 때문일 수도 있고, OS에서 지원하는 파일 시스템 버퍼 캐시와 SAN 캐시 때문일 수도 있다. SAN 캐시는 크다고 문제될 것이 없지만, 파일 시스템 버퍼캐시는 최소화해야 한다. 데이터베이스 자체적으로 캐시 영역을 갖고 있으므로 이를 위한 공간을 크게 할당하는 것이 더 효과적이다. 네트워크 문제이든, 파일시스템 문제이든 I/O 성능에 관한 가장 확실하고 근본적인 해결책은 논리적인 블록 요청 횟수를 최소화하는 것이다.

<br>

# Sequential I/O vs Random I/O
---
![시퀀셜 I/O와 랜덤 I/O](/blog/assets/img/posts/20221115/sequential-io-and-random-io.png "시퀀셜 I/O와 랜덤 I/O"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">시퀀셜 I/O와 랜덤 I/O</div>

Sequential 액세스는 레코드간 논리적 또는 물리적인 순서를 따라 차례대로 읽어 나가는 방식이다. 인덱스 리프 블록에 위치한 모든 레코드는 포인터를 따라 논리적으로 연결돼 있고, 이 포인터를 따라 스캔하는 것은 Sequential 액세스 방식이다. 테이블 레코드 간에는 포인터로 연결되지 않지만 테이블을 스캔할 때는 물리적으로 저장된 순서대로 읽어 나가므로 이것 또한 Sequential 액세스 방식이다. 

Random 액세스는 레코드간 논리적, 물리적인 순서를 따르지 않고, 한 건을 읽기 위해 한 블록씩 접근하는 방식을 말한다. 블록 단위 I/O를 하더라도 한번 액세스할 때 Sequential 방식으로 그 안에 저장된 모든 레코드를 읽는다면 비효율은 없다. 반면, 하나의 레코드를 읽으려고 한 블록씩 Random 액세스한다면 매우 비효율적이라고 할 수 있다. 여기서 I/O튜닝의 핵심 원리 두 가지를 발견할 수 있다.

- Sequential 액세스에 의한 선택 비중을 높인다.
- Random 액세스 발생량을 줄인다

## 시퀀셜 액세스에 의한 선택 비중 높이기
Sequential 액세스 효율성을 높이려면, 읽은 총 건수 중에서 결과집합으로 선택되는 비중을 높여야 한다. 즉, 같은 결과를 얻기 위해 얼마나 적은 레코드를 읽느냐로 효율성을 판단할 수 있다. 예제를 통해 살펴보자.

```sql
-- 테스트용 테이블 생성 
CREATE TABLE T AS
SELECT * 
  FROM ALL_OBJECTS
 ORDER BY DBMS_RANDOM.VALUE;

-- 테스트용 테이블 데이터 건수 : 49,906
SELECT COUNT(*)
  FROM T;

  COUNT(*) 
----------
     49906
```

T 테이블에는 49,906건의 레코드가 저장되어 있다.

```sql
SELECT COUNT(*) 
  FROM T
 WHERE OWNER LIKE 'SYS%'

 Rows Row Source Operation 
 ---- ------------------------------ 
    1 SORT AGGREGATE (cr=691 pr=0 pw=0 time=13037 us) 
24613 TABLE ACCESS FULL T (cr=691 pr=0 pw=0 time=98473 us)
```

위 SQL은 24,613개 레코드를 선택하려고 49,906개 레코드를 읽었으므로 49%가 선택되었다. Table Full Scan에서 이 정도면 나쁘지 않다. 읽은 블록 수는 691개였다.

```sql
SELECT COUNT(*)
  FROM T
 WHERE OWNER LIKE 'SYS%'
   AND OBJECT_NAME = 'ALL_OBJECTS'

 Rows Row Source Operation 
 ---- ------------------------------ 
    1 SORT AGGREGATE (cr=691 pr=0 pw=0 time=7191 us) 
    1 TABLE ACCESS FULL T (cr=691 pr=0 pw=0 time=7150 us)
```

위 SQL은 49,906개 레코드를 스캔하고 1개 레코드를 선택했다. 선택 비중이 0.002%밖에 되지 않으므로 Table Full Scan 비효율이 높다. 여기서도 읽은 블록 수는 똑같이 691개다. 이처럼 테이블을 스캔하면서 읽은 레코드 중 대부분 필터링되고 일부만 선택된다면 아래처럼 인덱스를 이용하는 것이 효과적이다.

```sql
CREATE INDEX T_IDX ON T(OWNER, OBJECT_NAME);

SELECT /*+ INDEX(T T_IDX) */ COUNT(*)
  FROM T
 WHERE OWNER LIKE 'SYS%'
   AND OBJECT_NAME = 'ALL_OBJECTS'

 Rows Row Source Operation 
 ---- ------------------------------ 
    1 SORT AGGREGATE (cr=76 pr=0 pw=0 time=7009 us) 
    1 INDEX RANGE SCAN T_IDX (cr=76 pr=0 pw=0 time=6972 us)(Object ID 55337)
```

위 SQL에서 참조하는 컬럼이 모두 인덱스에 있으므로 인덱스만 스캔하고 결과를 구할 수 있었다. 하지만 1개의 레코드를 읽기 위해 76개의 블록을 읽어야 했다. 테이블뿐만 아니라 인덱스를 시퀀셜 액세스 방식으로 스캔할 때도 비효율이 나타날 수 있다. 조건절에 사용된 컬럼과 연산자 형태, 인덱스 구성에 의해 효율성이 결정된다.

다음은 인덱스 구성 컬럼의 순서를 변경한 후에 테스트한 결과다.

```sql
DROP INDEX T_IDX;

CREATE INDEX T_IDX ON T(OBJECT_NAME, OWNER);

SELECT /*+ INDEX(T T_IDX) */ COUNT(*)
  FROM T
 WHERE OWNER_LIKE 'SYS%'
   AND OBJECT_NAME = 'ALL_OBJECTS'

 Rows Row Source Operation 
 ---- ------------------------------ 
    1 SORT AGGREGATE (cr=2 pr=0 pw=0 time=44 us) 
    1 INDEX RANGE SCAN T_IDX (cr=2 pr=0 pw=0 time=23 us)(Object ID 55338)
```

루트와 리프, 단 2개의 인덱스 블록만 읽었다. 한 건을 얻으려고 읽은 건수도 한 건일 것이므로 가장 효율적인 방식으로 Sequential 액세스를 수행했다.

## 랜덤 액세스 발생량 줄이기
Random 액세스 발생량을 낮추는 방법을 살펴보자. 인덱스에 속하지 않는 칼럼(object_id)을 참조하도록 쿼리를 변경함으로써 테이블 액세스가 발생하도록 할 것이다.

```sql
DROP INDEX I_IDX;

CREATE INDEX T_IDX ON T(OWNER);

SELECT OBJECT_ID
  FROM T
 WHERE OWNER = 'SYS'
   AND OBJECT_NAME = 'ALL_OBJECTS'

 Rows Row Source Operation 
 ---- ------------------------------ 
    1 TABLE ACCESS BY INDEX ROWID T (cr=739 pr=0 pw=0 time=38822 us) 
22934 INDEX RANGE SCAN T_IDX (cr=51 pr=0 pw=0 time=115672 us)(Object ID 55339)
```

인덱스로부터 조건을 만족하는 22,934건을 읽어 그 횟수만큼 테이블을 Random 액세스하였다. 최종적으로 한 건이 선택된 것에 비해 너무 많은 Random 액세스가 발생했다. 아래는 인덱스를 변경하여 테이블 Random 액세스 발생량을 줄인 결과다.

```sql
DROP INDEX T_IDX;

CREATE INDEX T_IDX ON T(OWNER, OBJECT_NAME);

SELECT OBJECT_ID
  FROM T
 WHERE OWNER = 'SYS'
   AND OBJECT_NAME = 'ALL_OBJECTS'

 Rows Row Source Operation 
 ---- ------------------------------ 
    1 TABLE ACCESS BY INDEX ROWID T (cr=4 pr=0 pw=0 time=67 us) 
    1 INDEX RANGE SCAN T_IDX (cr=3 pr=0 pw=0 time=51 us)(Object ID 55340)
```

인덱스로부터 1건을 출력했으므로 테이블을 1번 방문한다. 실제 발생한 테이블 Random 액세스도 1(=4-3)번이다. 같은 쿼리를 수행했는데 인덱스 구성이 바뀌자 테이블 Random 액세스가 대폭 감소한 것이다.

<br>

# Single Block I/O vs MultiBlock I/O
---
Single Block I/O는 한번의 I/O Call에 하나의 데이터 블록만 읽어 메모리에 적재하는 방식이다. 인덱스를 통해 테이블을 액세스할 때는, 기본적으로 인덱스와 테이블 블록 모두 이 방식을 사용한다. 

MultiBlock I/O는 I/O Call이 필요한 시점에, 인접한 블록들을 같이 읽어 메모리에 적재하는 방식이다. Table Full Scan처럼 물리적으로 저장된 순서에 따라 읽을 때는 인접한 블록들을 같이 읽는 것이 유리하다. ‘인접한 블록’이란, 한 익스텐트(Extent)내에 속한 블록을 말한다. 달리 말하면, MultiBlock I/O 방식으로 읽더라도 익스텐트 범위를 넘어서까지 읽지는 않는다. 인덱스 스캔 시에는 Single Block I/O 방식이 효율적이다. 인덱스 블록간 논리적 순서(이중 연결 리스트 구조로 연결된 순서)는 데이터 파일에 저장된 물리적인 순서와 다르기 때문이다. 물리적으로 한 익스텐트에 속한 블록들을 I/O Call 시점에 같이 메모리에 올렸는데, 그 블록들이 논리적 순서로는 한참 뒤쪽에 위치할 수 있다. 그러면 그 블록들은 실제 사용되지 못한 채 버퍼 상에서 밀려나는 일이 발생한다. 하나의 블록을 캐싱하려면 다른 블록을 밀어내야 하는데, 이런 현상이 자주 발생한다면 앞에서 소개한 버퍼 캐시 효율만 떨어뜨리게 된다. 

대량의 데이터를 MultiBlock I/O 방식으로 읽을 때 Single Block I/O 보다 성능상 유리한 이유는 I/O Call 발생 횟수를 줄여주기 때문이다. 아래 예제를 통해 Single Block I/O 방식과 MultiBlock I/O 방식의 차이점을 알아보자.

```sql

```

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  