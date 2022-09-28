---
title: "[SQL] 문자형 함수"
categories:
  - SQL
tags:
  - SQL
  - 문자형 함수
toc: true
toc_sticky: true
toc_label: "목차"
---

문자형 함수는 단일행 함수의 한 종류로 문자 데이터를 매개 변수로 받아들여 문자나 숫자 값의 결과를 돌려주는 함수이다. 문자형 함수는 어떤 것들이 있는지 함께 알아보자.

# LOWER
---
LOWER는 문자열의 알파벳 문자를 소문자로 바꾸어주는 함수다.

- 사용법
  >LOWER(문자열)

- 예제

```sql
SELECT LOWER('HELLO WORLD') AS 결과
  FROM DUAL;
```

![LOWER함수 예제](/blog/assets/img/posts/20220928/query-example1.png "LOWER함수 예제"){: width="100%"}

<br>

# UPPER
---
UPPER는 문자열의 알파벳 문자를 대문자로 바꾸어주는 함수다.

- 사용법
  >UPPER(문자열)

- 예제

```sql
SELECT UPPER('hello world') AS 결과
  FROM DUAL;
```

![UPPER함수 예제](/blog/assets/img/posts/20220928/query-example2.png "UPPER함수 예제"){: width="100%"}

<br>

# ASCII
---
ASCII는 문자나 숫자를 ASCII 코드 번호로 바꾸어 준다.

- 사용법
  >ASCII(문자)

- 예제

```sql
SELECT ASCII('A') AS 결과
  FROM DUAL;
```

![ASCII함수 예제](/blog/assets/img/posts/20220928/query-example3.png "ASCII함수 예제"){: width="100%"}

<br>

# CHR
---
CHR는 ASCII 코드 번호를 문자나 숫자로 바꾸어 준다.

- 사용법
  >CHR(ASCII번호)

- 예제

```sql
SELECT CHR(65) AS 결과
  FROM DUAL;
```

![CHR함수 예제](/blog/assets/img/posts/20220928/query-example4.png "CHR함수 예제"){: width="100%"}

<br>

# CONCAT
---
CONCAT은 문자열1과 문자열2를 연결한다. 합성 연산자 '||'(Oracle)나 '+'(SQL Server)와 동일하다.

- 사용법
  >CONCAT(문자열1, 문자열2)

- 예제

```sql
SELECT CONCAT('Hello', ' World') AS 결과
  FROM DUAL;
```

![CONCAT함수 예제](/blog/assets/img/posts/20220928/query-example5.png "CONCAT함수 예제"){: width="100%"}

<br>

# SUBSTR
---
SUBSTR은 문자열 중 m위치에서 n개의 문자 길이에 해당하는 문자를 돌려준다. n이 생략되면 마지막 문자까지이다.

- 사용법
  >SUBSTR(문자열, m[, n])

- 예제

```sql
SELECT SUBSTR('Hello World', 7, 5) AS 결과
  FROM DUAL;
```

![SUBSTR함수 예제](/blog/assets/img/posts/20220928/query-example6.png "SUBSTR함수 예제"){: width="100%"}

<br>

# LENGTH
---
LENGTH는 문자열의 개수를 숫자값으로 돌려준다.

- 사용법
  >LENGTH(문자열)

- 예제

```sql
SELECT LENGTH('Hello World') AS 결과
  FROM DUAL;
```

![LENGTH함수 예제](/blog/assets/img/posts/20220928/query-example7.png "LENGTH함수 예제"){: width="100%"}

<br>

# LTRIM
---
LTRIM은 문자열의 첫 문자부터 확인해서 지정 문자가 나타나면 해당 문자를 제거한다.(지정 문자가 생략되면 공백값이 디폴트)  
SQL Server에서는 LTRIM 함수에 지정문자 사용 불가. 공백만 제거할 수 있다.

- 사용법
  >LTRIM(문자열 [, 지정문자])  
  >LTRIM(문자열)

- 예제

```sql
SELECT LTRIM('xxxYYZZxYZ', 'x') AS 결과
  FROM DUAL;
```

![LTRIM함수 예제](/blog/assets/img/posts/20220928/query-example8.png "LTRIM함수 예제"){: width="100%"}

<br>

# RTRIM
---
RTRIM은 문자열의 마지막 문자부터 확인해서 지정 문자가 나타나는 동안 해당 문자를 제거한다.(지정 문자가 생략되면 공백값이 디폴트)  
SQL Server에서는 RTRIM 함수에 지정문자를 사용할 수 없다. 즉 공백만 제거할 수 있다.

- 사용법
  >RTRIM(문자열 [, 지정문자])  
  >RTRIM(문자열)

- 예제

```sql
SELECT RTRIM('XXYYzzXYzz', 'z') AS 결과
  FROM DUAL;
```

![RTRIM함수 예제](/blog/assets/img/posts/20220928/query-example9.png "RTRIM함수 예제"){: width="100%"}

<br>

# TRIM
---
TRIM은 문자열의 머리말, 꼬리말 또는 양쪽에 있는 지정 문자를 제거한다.(leading | trailing | both가 생략되면 both가 디폴트)  
SQL Server에서는 TRIM 함수에 leading, trailing, both를 사용할 수 없다. 즉, 양쪽에 있는 지정 문자만 제거할 수 있다.

- 사용법
  >TRIM([leading | trailing | both] 지정문자 FROM 문자열)
  >TRIM(지정문자 FROM 문자열)  

- 예제

```sql
SELECT TRIM('x' FROM 'xxYYZZxYZxx') AS 결과
  FROM DUAL;
```

![TRIM함수 예제](/blog/assets/img/posts/20220928/query-example10.png "TRIM함수 예제"){: width="100%"}

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원