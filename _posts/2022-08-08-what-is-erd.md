---
title: "[SQL] ERD란?"
categories:
  - SQL
tags:
  - SQL
  - ERD
  - 데이터 모델
  - 표기법
toc: true
toc_sticky: true
toc_label: "목차"
comments: true
---

현업에서 데이터 모델링을 할때 반드시 거치는 작업이 있다. 바로 데이터 모델의 표기법인 ERD를 작성하는 것이다. 이번 포스팅에서는 ERD가 무엇이고 어떻게 작성하는 것인지 알아보자.

<br>

# ERD란?
---
ERD는 데이터 모델에 대한 표기법을 활용해 다이어그램 형태로 표현한 것을 말한다. 1976년 피터첸은 E-R 모델이라는 표기법을 만들었다. 엔터티를 사각형으로 표현하고 관계를 마름모, 속성을 타원형으로 표현하는 이 표기법은 데이터 모델링에 대한 이론을 배울 때 많이 활용된다. DB설계에 대해 우리나라 대학에서는 주로 이 첸의 모델 표기법을 통해 배우고 있으며, 이외에도 다양한 표기법을 활용해 다이어그램 형태인 ERD를 학교 또는 기업 등에서 많이 활용하고 있다.

다시 말해 ERD는 E-R 모델 즉, 개체관계 모델 표기법을 다이어그램 형태로 작성한 것이다.

<br>

# E-R 표기법의 종류
---
E-R 표기법에는 다양한 종류가 있으며, 내용은 다음과 같다.

![다양한 형태의 E-R 표기법](/blog/assets/img/posts/20220808/er-type.png "다양한 형태의 E-R 표기법"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">다양한 형태의 E-R 표기법</div>

<br>

# ERD로 모델링하는 방법
---
ERD를 구성하는 방법은 ERD를 작성하는 작성자마다 조금씩 차이가 있을 수 있겠지만 원칙의 틀인 비슷하다. 아래 제시되는 ERD 모델링 방법은 무엇을 도출할지에 대한 관점을 제시하고, 전문 데이터 모델링 툴이 없을 때 어떠한 순서로 표현해야 하는지를 알 수 있도록 설명한다.

ERD 작업 순서는 다음과 같다.
1. 엔터티를 그린다.
  >엔터티는 직사각형으로 그린다.
2. 엔터티를 적절히 배치한다.
  >엔터티를 처음에 어디에 배치하는지는 데이터 모델링 툴 사용 여부와 관계없이 중요한 문제이다. 일반적으로 사람의 눈은 왼쪽에서 오른쪽, 위쪽에서 아래쪽으로 이동하는 경향이 있기 때문에 데이터 모델링에서도 가장 중요한 엔터티를 왼쪽 상단에 배치하고, 이것을 중심으로 다른 엔터티를 나열하면서 전개하면 사람의 눈이 따라가기에 편리한 데이터 모델링을 할 수 있다. 해당 업무에서 가장 중요한 엔터티는 왼쪽 상단에서 조금 아래쪽 중앙에 배치하여 전체 엔터티와 어울리게 하면, 향후 관계를 연결할 때 선이 꼬이지 않고 효과적으로 배치할 수 있다.
  >
  >![엔터티 배치의 예](/blog/assets/img/posts/20220808/place-entity.png "엔터티 배치의 예"){: width="100%"}
  ><div style="color: gray; text-align: center; margin-bottom: 30px;">엔터티 배치의 예</div>
3. 엔터티 간 관계를 설정한다.
  >엔터티에 배치가 되면 관계를 정의한 분석서를 보고 서로 관련 있는 엔터티 간에 관계를 설정한다. 초기에는 모두 Primary Key로 속성이 상속되는 식별자 관계를 설정한다. 중복되는 관계가 발생되지 않도록 하고, Circle 관계도 발생하지 않도록 유의하여 작성한다.
  >
  >![ERD 관계연결의 예](/blog/assets/img/posts/20220808/erd-relation-connect.png "ERD 관계연결의 예"){: width="100%"}
  ><div style="color: gray; text-align: center; margin-bottom: 30px;">ERD 관계연결의 예</div>
4. 관계명을 기술한다.
  >관계 설정이 완료되면 연결된 관계에 관계 이름을 부여한다. 관계 이름은 현재형을 사용하고 지나치게 포괄적인 용어(예, 이다, 가진다 등)는 사용하지 않도록 한다.
  >
  >![ERD 관계명 표시의 예](/blog/assets/img/posts/20220808/erd-relation-name-mark.png "ERD 관계명 표시의 예"){: width="100%"}
  ><div style="color: gray; text-align: center; margin-bottom: 30px;">ERD 관계명 표시의 예</div>

5. 관계의 참여도 기술 및 필수여부 기술
  >관계에 대한 이름을 모두 지정하였다면 관계가 참여하는 성격 중 엔터티 내에 인스턴스들이 얼마나 관계에 참여하는지를 나타내는 관계차수를 표현한다.
  >
  >![관계의 참여도 기술 및 필수여부 기술의 예](/blog/assets/img/posts/20220808/erd-relational-order-and-selectivity-display.png "관계의 참여도 기술 및 필수여부 기술의 예"){: width="100%"}
  ><div style="color: gray; text-align: center; margin-bottom: 30px;">관계의 참여도 기술 및 필수여부 기술의 예</div>
  >위 이미지는 관계의 관계차수를 지정한 ERD의 모습을 보여준다. IE 표기법으로는 하나의 관계는 실선으로 표기하고, 바커 표기법으로는 점선과 실선을 혼합하여 표기한다. 다수 참여의 관계는 까마귀발과 같은 모양으로 그려준다. 또한 관계의 필수 및 선택 표시는 관계선에 원을 표현하여 ERD를 그리도록 한다.

---

읽어주셔서 감사합니다. 😊

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  
[데이터 모델의 이해 - dataonair](https://dataonair.or.kr/db-tech-reference/d-guide/sql/?mod=document&uid=330)