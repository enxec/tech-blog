---
title: "[SQL] Null 속성의 이해"
categories:
  - SQL
tags:
  - SQL
  - Null
  - Null의 이해
toc: true
toc_sticky: true
toc_label: "목차"
---

DBMS를 사용하다 보면 Null값으로 인한 많은 특이사항들을 접하게 된다. Null값이 가지는 특성을 이해하지 못한다면 데이터 오류를 경험할 수 있으므로 반드시 숙지해야 할 부분 중 하나다. 이번 포스팅에서는 각 사례를 통해 Null에 대해 이해해보자.

<br>

# 모델링에서의 Null 허용 여부 표현
---
Null 속성을 이해하기 전 모델링에서 속성의 Null 허용 여부를 어떻게 표현하는지 아래 이미지를 통해 잠깐 짚고가자.

![결제 모델](/blog/assets/img/posts/20220830/example-of-performance-improvement-of-semi-normalized-model.png "결제 모델"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">결제 모델</div>

위 모델을 보면 결제수단번호와 결제일시, 고객번호는 '#' 이나 '*' 이아닌 'o'로 표시되어 있다. 위와 같은 바커 표기법에서는 'o'는 Null 허용을 의미하며, IE표기법에서는 속성 앞에 어떠한 표시도 되어 있지 않기 때문에 Null 허용여부를 파악할 수 없다. 따라서 모델링 시 바커표기법 채택을 지향하는 것이 좋다.

<br>

# Null의 성질
모델에서 속성이 가지는 Null은 다양한 성질이 있다. 필자는 그 중 2가지 정도만 언급하겠다.

## Null 값의 연산은 언제나 Null이다.
Null 값은 '공백이나 숫자 0'과는 전혀 다른 의미이며, '아직 정의되지 않은 미지의 값' 또는 '현재 데이터를 입력하지 못하는 경우'를 의미한다. 즉, Null은 값이 존재하지 않음을 말한다.  
이론적으로만 보았을 때 이해가 잘 안갈 수 있다. 예제를 통해 살펴보자.

![주문 데이터](/blog/assets/img/posts/20220915/order-data-example_1.png "주문 데이터"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">주문 데이터</div>

위 데이터는 주문 모델의 예제 데이터다. 본 테이터를 바탕으로 작성한 아래쿼리를 보자.

```sql
SELECT 주문금액 - 주문취소금액 COL1
     , NVL(주문금액 - 주문취소금액, 0) COL2
     , NVL(주문금액, 0) - NVL(주문취소금액, 0) COL3
  FROM 주문;
```

COL1, COL2, COL3은 최종 주문금액을 구하는 산식이다. 최종 주문금액은 각 주문의 주문금액에서 취소된 주문금액을 제외한 결과다. 이와 같은 요건에서는 주로 COL1, COL2, COL3의 방식으로 SQL을 작성할 것이다. 이때 동일한 목적으로 작성된 각각의 컬럼 COL1, COL2, COL3은 모두 결과가 동일할까? 쿼리의 결과는 아래와 같다.

![쿼리 수행 결과](/blog/assets/img/posts/20220915/query-result_1.png "쿼리 수행 결과"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">쿼리 수행 결과</div>

Null 값이 포함되었을 경우, 각 컬럼 결과가 모두 다르게 출력된다. 이유는, Null값의 연산은 언제나 Null이기 때문이다. Null은 아직 값이 존재하지 않는 것으로, 아무것도 존재하지 않는 값에 연산이 가능할까? 불가능하다. 때문에 Null 연산은 언제나 Null을 결과로 반환한다. 그렇다면 Null 값으로 연산이 가능하긴한걸까? 가능하다. 'IS NULL'과 'IS NOT NULL'로 말이다.

## Null값은 집계함수에서 제외되어 처리된다.
Null값은 집계함수에서 제외되어 처리된다. 바로 예제를 살펴보자.

![주문 데이터](/blog/assets/img/posts/20220915/order-data-example_2.png "주문 데이터"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">주문 데이터</div>

위 데이터는 아직 취소된 주문이 없어 주문취소금액이 전부 Null 값으로 이루어진 예제 데이터다. 본 데이터에 집계함수를 사용하여 다음 쿼리의 결과를 예측해보자.

```sql
SELECT SUM(주문금액) - SUM(주문취소금액) COL1
     , NVL(SUM(주문금액 - 주문취소금액), 0) COL2
     , NVL(SUM(주문금액), 0) - NVL(SUM(주문취소금액), 0) COL3
  FROM 주문;
```

COL1, COL2, COL3은 최종주문금액 총합을 구하는 산식이다. 최종주문금액은 각 주문의 주문금액에서 취소된 주문금액을 제외하고, 총합은 이를 합산한 결과다. 이와 같은 요건이 있을 경우 주로 COL1, COL2, COL3 방식으로 쿼리를 작성할 것이다. 동일한 목적으로 작성된 COL1, COL2, COL3의 결과는 모두 동일할까? 아래 결과를 보자.

![쿼리 수행 결과](/blog/assets/img/posts/20220915/query-result_2.png "쿼리 수행 결과"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">쿼리 수행 결과</div>

COL1, COL2, COL3은 모두 다른 결과를 출력한다. 앞서 배웠듯이 Null 값의 연산은 언제나 Null이다. 또한 집계함수는 Null 값의 경우는 제외하고 연산한다. SUM 함수는 정의된 칼럼의 값을 모두 합산하는 함수로서 Null 값이 들어올 경우 이는 제외하고 처리한다.

이렇게 집계함수의 경우 Null값을 제외한다는 특성을 이해하여야만 올바른 결과를 출력할 수 있다.

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  