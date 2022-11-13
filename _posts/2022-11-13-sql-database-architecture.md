---
title: "[SQL] 데이터베이스 아키텍처"
categories:
  - SQL
tags:
  - SQL
  - Database Architecture
  - 데이터베이스 아키텍처
toc: true
toc_sticky: true
toc_label: "목차"
---

\* 해당 포스팅은 오라클 DB 관련 정보만 다룬다.

# DB 구조
---
DBMS마다 DB에 대한 정의가 조금씩 다르다. 오라클에서는 디스크에 저장된 데이터 집합(Datafile, Redo Log File, Control File 등)을 DB라고 부른다. 그리고 SGA 공유 메모리 영역과 이를 엑세스하는 프로세스 집합을 합쳐서 인스턴스라고 부른다.

![Oracle Architecture](/blog/assets/img/posts/20221113/oracle-architecture.png "Oracle Architecture"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">Oracle Architecture</div>

기본적으로 하나의 인스턴스가 하나의 DB만 액세스하지만, RAC(Real Application Cluster) 환경에서는 여러 인스턴스가 하나의 DB를 액세스할 수 있다. 하나의 인스턴스가 여러 DB를 액세스할 수는 없다.

<br>

# 프로세스
---
오라클은 기본적으로 프로세스를 사용하며, 윈도우 버전에서는 쓰레드를 사용한다. 여기서는 프로세스와 쓰레드를 구분짓지 않고 프로세스로 통칭하여 설명하겠다.

프로세스는 서버 프로세스와 백그라운드 프로세스 집합으로 나뉜다. 서버 프로세스는 전면에서 사용자로부터 전달받은 각종 명령을 처리하고, 백그라운드 프로세스는 뒤에서 묵묵히 할당받은 역할을 수행한다.

## 서버 프로세스
서버 프로세스는 사용자 프로세스와 통신하면서 사용자의 각종 명령을 처리한다. 좀 더 구체적으로 언급하면, SQL을 파싱하고 필요하면 최적화를 수행한다. 커서를 열어 SQL을 실행하면서 블록을 읽어 이 데이터를 정렬해 클라이언트가 요청한 결과 집합을 만들어 네트워크를 통해 전송하는 일련의 작업을 모두 서버 프로세스가 처리해 준다. 스스로 처리하도록 구현되지 않은 기능, 이를 테면 데이터 파일로부터 DB 버퍼 캐시로 블록을 적재하거나 Dirty 블록을 캐시에서 밀어냄으로써 Free 블록을 확보하는일, Redo 로그 버퍼를 비우는 일 등은 OS와 I/O 서브시스템, 백그라운드 프로세스가 대시 처리하도록 시스템 Call을 통해 요청한다.

클라이언트가 서버 프로세스와 연결하는 방식은 DBMS마다 다르지만, 오라클은 전용 서버 방식과 공유 서버 방식 2가지가 존재한다.

### 전용 서버 방식
![전용 서버 방식](/blog/assets/img/posts/20221113/dedicated-server-method.png "전용 서버 방식"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">전용 서버 방식</div>

위 그림은 전용 서버 방식으로 접속할 때 내부적으로 어떤 과정을 거쳐 세션을 수립하고 사용자 명령을 처리하는지 보여준다.

처음 연결 요청을 받은 리스너가 서버 프로세스(Window 환경에서는 쓰레드)를 생성해 주고, 이 서버 프로세스가 단 하나의 사용자 프로세스를 위해 전용 서비스를 제공한다는 점이 특징이다.

만약 SQL을 수행할 때마다 연결 요청을 반복하면 서버 프로세스의 생성과 해제도 반복하게 되므로 DBMS에 매우 큰 부담을 주고 성능을 크게 떨어뜨린다. 따라서 전용 서버 방식을 사용하는 OLTP성 애플리케이션에선 Connection Pooling 기법을 필수적으로 사용해야 한다. 예를 들어 50개의 서버 프로세스와 연결된 50개의 사용자 프로세스를 공유해 반복 재사용하는 방식이다.

### 공유 서버 방식
공유 서버는 말 그대로 하나의 서버 프로세스를 여러 사용자 세션이 공유하는 방식이다. 앞서 설명한 Connection Pooling 기법을 DBMS 내부에 구현해 놓은 것으로 생각하면 쉽다. 즉, 미리 여러 개의 서버 프로세스를 띄어 놓고 이를 공유해 반복 재사용한다.

![공유 서버 방식](/blog/assets/img/posts/20221113/shared-server-method.png "공유 서버 방식"){: width="100%"}
<div style="color: gray; text-align: center; margin-bottom: 30px;">공유 서버 방식</div>

위 그림에서 보듯, 공유 서버 방식으로 오라클에 접속하면 사용자 프로세스는 서버 프로세스와 직접 통신하지 않고 Dispatcher 프로세스를 거친다. 사용자 명령이 Dispatcher에게 전달되면 Dispatcher는 이를 SGA에 있는 요청 큐에 등록한다. 이후 가장 먼저 가용한 서버 프로세스가 요청 큐에 있는 사용자 명령을 꺼내서 처리하고, 그 결과를 응답 큐에 등록한다.

응답 큐를 모니터링하던 Dispatcher가 응답 결과를 발견하면 사용자 프로세스에게 전송해 준다.

## 백그라운드 프로세스

---

읽어주셔서 감사합니다. 😊 

__Reference__  
SQL 전문가 가이드 - Kdata 한국데이터산업진흥원  