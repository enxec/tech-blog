---
title: "[Storage] RAID란?"
categories:
  - Storage
tags:
  - 스토리지
  - storage
  - RAID
  - 레이드
toc: true
toc_sticky: true
toc_label: "목차"
---

![RAID](/blog/assets/img/posts/20220802/raid.jpg "RAID"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">RAID</div> 

<br>

# RAID 개념
---
RAID란 여러개의 하드 디스크가 있을 때 동일한 데이터를 다른 위치에 중복해서 저장하는 방법이다. 데이터를 여러개의 디스크에 저장하여 입출력 작업이 균형을 이루게 되어 전체적인 성능을 향상시킨다. 운영체제에서 하나의 RAID는 논리적으로 하나의 디스크로 인식하여 처리된다. 현재 RAID는 데이터를 기록하는 방식과 에러를 체크하는 패리티나 ECC 사용 등 구성방법에 따라 다양한 형태로 존재한다.

<br>

# RAID 구성의 변화
---
초기의 RAID는 저용량 하드 디스크를 하나의 디스크로 확장하여 사용하는 것이 주류였으나 현재는 백업을 가능하게 하고 안정적인 데이터의 보존과 유지기능, 속도 향상 등에 사용한다. 단순히 여러 디스크를 하나로 묶어주는 기술로는 Linear가 있다. Linear는 말 그대로 '순차적' 이라는 뜻으로 여러 개의 디스크를 하나로 묶었다고 하더라도 순서를 정해서 하나의 디스크 공간을 전부 사용해야 다른 디스크에 저장된다. 최근에는 단순히 묶어주는 Linear보다는 스트라이핑이나 미러링 기술이 적영된 RAID구성이 보편적이다. RAID를 구성하는 방법도 소프트웨어적 구현과 하드웨어적 구현 등 다양한 방법으로 가능한데, 소프트웨어 RAID는 비용적인 측면에서 유리하나 성능 측면에서는 하드웨어 RAID보다 불리하다. 하드웨어 수준의 RAID에서 주목할 만한 기능은 전원이 켜져있는 상태에서 하드 드라이브를 교체할 수 있는 핫스왑과 베이가 있다.

<br>

# RAID 기술
---
- 스트라이핑
  >스트라이핑 기술은 연속된 데이터를 여러 개의 디스크에 라운드로빈 방식으로 기록하는 기술이다. 이 기술은 프로세서가 하나의 디스크에서 읽어 들이는 것보다 빠르게 데이터를 읽거나 쓸 수 있다면 매우 유용하다. 즉, 서로 겹쳐서 읽거나 쓸 수 있도록 설계된 4개의 드라이브가 있을 경우, 보통 하나의 섹터를 읽을 수 있는 시간에 4개의 섹터를 동시에 읽을 수 있다.
- 미러링
  >디스크에 에러가 발생 시 데이터 손실을 막기 위해 추가적으로 하나 이상의 장치에 중복 저장하는 기술이다. 2개의 디스크로 구현했을 경우, 하나의 디스크에 에러가 발생해도 다른 디스크의 데이터는 그대로 보존된다. 그래서 미러링 기술을 '결함 허용' 이라고도 부른다. 이 기법은 하드웨어 방법뿐 아니라, 소프트웨어적으로도 구현가능하다.

<br>

# RAID의 종류
---
- RAID-0
  >![RAID-0](/blog/assets/img/posts/20220802/raid0.png "RAID-0"){: width="100%"}
  <div style="color: gray; text-align: center; margin-bottom: 30px;">RAID-0</div>  
  스트라이핑 기술을 사용하여 빠른 입출력 속도를 제공한다. 데이터를 중복이나 패리티 없이 디스크에 분산하여 기록한다. 처리속도는 빠르나, 구성된 디스크 중에 하나라도 오류가 발생하면 데이터 복구가 불가하다.

- RAID-1
  >![RAID-1](/blog/assets/img/posts/20220802/raid1.png "RAID-1"){: width="100%"}
  <div style="color: gray; text-align: center; margin-bottom: 30px;">RAID-1</div> 
  미러링 기술을 사용하여 두 개의 디스크에 데이터를 동일하게 기록한다. 스트라이핑 기술은 사용하지 않으며, 각 드라이브를 동시에 읽을 수 있어서 읽기 성능은 향상되나 쓰기 성능은 단일 디스크와 같다.
  디스크 오류 시 데이터 복구 능력은 탁월하지만, 중복 저장으로 인한 디스크 낭비가 50%에 이른다.

- RAID-2
  >![RAID-2](/blog/assets/img/posts/20220802/raid2.png "RAID-2"){: width="100%"}
  <div style="color: gray; text-align: center; margin-bottom: 30px;">RAID-2</div> 
  디스크들은 스트라이핑 기술을 사용하여 구성하고, 디스크들의 에러를 감지하고 수정하기 위해 ECC(Error Check & Correction) 정보를 사용한다.

- RAID-3
  >![RAID-3](/blog/assets/img/posts/20220802/raid3.png "RAID-3"){: width="100%"}
  <div style="color: gray; text-align: center; margin-bottom: 30px;">RAID-3</div> 
  스트라이핑 기술을 사용하며 디스크를 구성하고, 패리티 정보를 저장하기 위해 별도로 하나의 디스크를 사용한다. 입출력 작업이 동시에 모든 디스크에 대해 이루어지므로 입출력을 겹치게 할 수는 없다.
  보통 대형 레코드가 많은 시스템에서 사용된다.

- RAID-4
  >![RAID-4](/blog/assets/img/posts/20220802/raid4.png "RAID-4"){: width="100%"}
  <div style="color: gray; text-align: center; margin-bottom: 30px;">RAID-4</div>
  블록 형태의 스트라이핑 기술을 사용하여 디스크를 구성하는데, 이는 단일 디스크로부터 레코드를 읽을 수 있고 데이터를 읽을 때 중첩 입출력의 장점이 있다.
  쓰기 작업은 패리티 연산을 해야 하고 패리티 디스크에 저장해야 하기 때문에, 입출력의 중첩이 불가능하고 시스템에 병목현상이 발생할 수 있다.

- RAID-5
  >![RAID-5](/blog/assets/img/posts/20220802/raid5.png "RAID-5"){: width="100%"}
  <div style="color: gray; text-align: center; margin-bottom: 30px;">RAID-5</div> 
  패리티 정보를 이용하여 하나의 디스크가 고장이 발생할 경우에도 사용이 가능한 구성 방식으로 최소 3개의 디스크로 구성해야 한다. 패리티 정보는 별도의 디스크를 사용하지 않고, 구성된 디스크에 분산하여 기록하지만 데이터를 중복 저장하지는 않아 가장 보편적으로 사용된다. 디스크에 쓰기 제한 주소를 지정하므로 모든 읽기 및 쓰기가 중첩될 수 있다. 작고 랜덤한 입출력이 많은 경우에 더 나은 성능을 발휘한다. 디스크 사용 공간을 살펴보면 최소 디스크의 구성이 3개이므로, 3개로 구성 시에는 33.3%, 4개로 구성하면 25%, 5개로 구성하면 20%가 패리티 공간으로 사용된다. RAID-5는 RAID-0의 단점인 결합 허용을 지원하지 않는 점과 RAID-1의 저장 공간의 비효율성을 보완한 레벨로 디시크의 개수를 늘릴수록 저장 공간의 효율성이 높아진다.

- RAID-6
  >![RAID-6](/blog/assets/img/posts/20220802/raid6.png "RAID-6"){: width="100%"}
  <div style="color: gray; text-align: center; margin-bottom: 30px;">RAID-6</div> 
  전체적인 구성은 RAID-5와 비슷하지만 디스크에 2차 패리티 구성을 포함함으로써 매우 높은 고장대비 능력을 발휘한다. RAID-5인 경우에는 1개의 디스크 오류에만 대처가 가능해서 2개의 디스크에 오류가 발생하면 데이터를 복구할 수 없다. 그러나 RAID-6은 2개의 패리티를 사용하여 2개의 디스크 오류에도 데이터를 읽을 수 있다. 2개의 패리티를 사용하므로 최소 4개의 디스크로 구성해야 하여 RAID-5에 비해 디스크의 공간 효율성은 떨어진다. 또한, 복잡한 알고리즘으로 인하여 처리속도는 떨어지나 데이터에 대한 신뢰도는 향상된다. 디스크 사용 공간을 살펴보면 최소 디스크의 구성이 4개이므로 4개로 구성 시에는 50%, 5개로 구성하면 40%, 6개로 구성하면 33.3%가 패리티 공간으로 사용된다.

- RAID-7
  >![RAID-7](/blog/assets/img/posts/20220802/raid7.png "RAID-7"){: width="100%"}
  <div style="color: gray; text-align: center; margin-bottom: 30px;">RAID-7</div> 
  하드웨어 컨트롤러에 내장되어 있는 실시간 운영체제를 사용하여 구성하는 방식으로 속도가 빠른 버스를 이용하고, 독자적인 여러 가지 특성들을 제공한다. 현재 하나의 업체에서만 이 방식을 제공한다.

- RAID 0+1
  >![RAID 0+1](/blog/assets/img/posts/20220802/raid0+1.png "RAID 0+1"){: width="100%"}
  <div style="color: gray; text-align: center; margin-bottom: 30px;">RAID 0+1</div> 
  디스크 2개를 RAID-0의 스트라이핑 기술로 구성하고, 다시 RAID-1의 미러링으로 구성하는 방식으로 최소 4개의 디스크가 필요하다. 만약, 6개의 디스크라면 보통 3개를 RAID-0으로 구성 후에 다시 RAID-1로 구성한다.

- RAID-10
  >![RAID-10](/blog/assets/img/posts/20220802/raid10.png "RAID-10"){: width="100%"}
  <div style="color: gray; text-align: center; margin-bottom: 30px;">RAID-10</div> 
  RAID 0+1의 반대의 개념이라고 볼 수 있는데, 디스크 2개를 먼저 미러링으로 구성하고 다시 스트라이핑하는 방식이다.

- RAID-53
  >![RAID-53](/blog/assets/img/posts/20220802/raid53.png "RAID-53"){: width="100%"}
  <div style="color: gray; text-align: center; margin-bottom: 30px;">RAID-53</div>  
  RAID-3방식에 별도로 스트라이프 어레이를 구성하는 방식이다. 이 방식은 RAID-3보다 높은 성능을 제공하지만 구성비용이 많이 든다.

---

읽어주셔서 감사합니다. 😊

__Reference__  
CentOS7으로 리눅스 마스터 1급 정복하기 - 정성재  