---
layout: post
title:  "3. Performance Schema"
date:   2023-01-06 10:11:54 +0900
categories: mysql
---
* performance schema : MYSQL 서버 내에서 실행되는 작업에 대한 상세한 메트릭 제공(disk wait time, response time, query execute time.. etc).
* instrument : 정보를 얻고자 하는 mysql 코드의 어느 부분을 나타냄. 
* consumer : 어떤 코드가 수행되었는지에 대한 정보를 저장하는 단순한 테이블(instrument가 정보를). 
* digest(바인드 변수 활용) : 쿼리에서 변경되는 부분을 제거하고 쿼리를 집계하는 방법. 
1. **성능 스키마**  
[1] instrument 요소
- performance_schema의 setup_instruments 테이블에는 지원되는 모든 instrument 목록이 포함.
- 오픈 소스 옵션 : 확실한 오프 소스 옵션 = PMM(Percona Monitoring and Management). 클라/서버 쌍으로 작동하며 매트릭을 수집하여 서버 부분으로 보내는 클라이언트를 DB instance 에 설치. -> 장점 : mysql 성능 모니터링에 대한 Percona 커뮤니티의 오랜 경험 바탕으로 대시보드 구성 가능.
