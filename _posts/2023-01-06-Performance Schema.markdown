---
layout: post
title:  "3. Performance Schema"
date:   2023-01-06 10:11:54 +0900
categories: mysql
---
* performance schema : MYSQL 서버 내에서 실행되는 작업에 대한 상세한 메트릭 제공(disk wait time, response time, query execute time.. etc).
* instrument : 정보를 얻고자 하는 mysql 코드의 어느 부분을 나타냄. 
* consumer : 어떤 코드가 수행되었는지에 대한 정보를 저장하는 단순한 테이블(instrument가 정보를). 
* digest(바인드 변수 활용) : 쿼리에서 변경되는 부분을 제거하고 쿼리를 집계하는 방법. <br/>
**1. 성능 스키마**  <br/>
[1] <u>instrument 요소</u>
 - performance_schema의 setup_instruments 테이블에는 지원되는 모든 instrument 목록이 포함.
 - 특정 instrument가 검사하는 내용을 이해하려면 instrument 명, 직관, 그리고 mysql 소스 코드에 대한 지식을 활용해야함. because almost document's row has null value. 
 - ex) statement/sql/error, statement/abstract/Query, Statement/abstract/new_packet, statement/abstract/relay_log, memory/performance_schema/mutex_instances <br/>
[2] <u>컨슈머 체계</u>
 - consumer는 instrument가 정보를 보내는 대상.
 - _current : 현재 서버에서 발생하고 있는 이벤트
 - _history/_history_long : 스레드당 최종 100/10000개의 완료된 이벤트
 - events_waits : low level server
 - events_statements : SQL문
 - events_stages : 임시 테이블 생성이나 데이터 전송과 같은 profile 정보
 - event_transactions : transaction들<br/><br/>
[3] <u>요약테이블과 digest</u>
- 요약테이블 : 테이블이 제안하는 모든 것에 대한 집계된 정보를 저장.
- instance : MYSQL 설치에 사용할 수 있는 객체 인스턴스. 
- 자원 소비 : consumer의 최대 사이즈를 설정해서 사용하는 메모리양 제한 가능. performance_schema의 일부 테이블은 자동 크기 조정을 지원. <br/><br/>
[4] <u>sys 스키마</u>
 - performance_schema에 대한 뷰와 stored routine 으로만 구성.
 - performance_schema를 보다 손쉽게 사용하도록 설계. 자체적으로 데이터 저장 x.(performance_schema의 테이블에 저장된 데이터만 액세스 할 수 있다.)<br/>
[5] <u>스레드 이해하기</u> 
 - 각 스레드는 최소 두개의 고유 식별자가 있음. ex) 운영체제 스레드 id + 내부적인 MYSQL 스레드 id.
 - mysql 스레드 id = performance_schema의 thread_id
 - performance_schema는 모든 곳에서 thread_id를 사용하는 반면, processlist_id 는 threads 테이블에서만 사용 가능. ex) processlist_id를 가져와서 잠금을 가지고 있는 연결을 종류하는 경우, 해당 값을 얻기 위해 threads 테이블을 조회해야함.<br/>

**2. 설정**<br/>
[1] <u>활성화</u>
 - performance_schema 변수 on/off
 - instrument 활성화 방법<br/>
1. update performance_schema.setup_instruments set enable Where name = "statement/sql/select"<br/>
2. sys schema에서 ps_setup_enable_instrument stored procedure 호출<br/>
ex) CALL sys.ps_setup_enable_instrument('statement/sql/select'); 
3. 서버 시작 때 performance-schema-instrument parameter 사용<br/>
ex) performance-schema-instrument='statement/sql/select=ON'<br/>
 - 컨슈머 활성화와 비활성화
[2] <u>특정 개체에 대한 모니터링 튜닝</u>
[3] <u>스레드 모니터링 튜닝 </u>
[4] <u>성능 스키마에 대한 메모리 크기 조정</u>
[5] <u>기본값</u>
**3. 성능 스키마 사용** <br/>
[1]
