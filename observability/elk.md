# ELK

## 개념
> ELK란, Elasticsearch(검색/분석 엔진) + Logstash(데이터 수집/변환) + Kibana(데이터 시각화)로 구성되어 시스템 모니터링 및 문제 해결에 쓰이는 오픈 소스 솔루션
> 다양한 소스에서 오는 데이터를 실시간으로 수집하고, 인덱싱해서 검색 가능하게 만들며, 대시보드로 시각화해 운영·보안 인사이트 제공

## ELK 스택 구성 요소

### Elasticsearch
- 검색 및 분석을 담당하는 분산형 데이터 저장소
- 분산 검색·분석 엔진으로, 수집된 로그/메트릭 데이터를 인덱스로 저장하고 매우 빠른 검색·집계 제공
- JSON 문서를 기반으로 스키마 유연성이 높고, 수평 확장이 쉬워 대용량 로그 분석에 적합
- 데이터는 역색인(Inverted Index) 구조로 관리되어 빠른 전체 텍스트 검색이 가능하며, 거의 실시간에 가까운 색인/검색 성능 제공
- NoSQL의 key-value DB와 같이 저장되며, 수정과 삭제가 많은 경우에는 많은 리소스가 소비될 수 있지만 검색 속도는 빠름

### Logstash
- 데이터 처리를 위한 파이프라인
- 다양한 입력 플러그인으로 로그/이벤트를 수집하고, 필터 단계에서 파싱·정규화·마스킹 등을 수행한 뒤 Elasticsearch나 기타 저장소로 전송
- Input, Filter, Output Plugin으로 크게 구성

### Kibana
- Elasticsearch에 저장된 데이터를 기반으로 검색, 차트, 대시보드, 알림 등을 제공하는 시각화 UI
- 로그 탐색, 시간 기반 분석, 필드별 필터링, 보안·APM·업타임 등 솔루션 전용 화면까지 제공해 운영/보안 콘솔 역할 제공

### ELK 아키텍처
<img width="748" height="201" alt="image" src="https://github.com/user-attachments/assets/55d8e7f9-9bee-4c0e-9be4-fe640efd3f59" />
1. 각 서버에서 수집된 log 데이터가 Logstash에 수집
2. Logstash는 수집된 데이터를 구조화하고 ElasticSearch에 저장
3. ElasticSearch는 자체 자료구조와 NoSQL이라는 특성을 통해 저장된 데이터의 빠른 검색 및 분석 기능 제공
4. Kibana가 ElasticSearch의 검색 기능을 이용하여 저장된 데이터를 가져와 시각화 진행

## 주요 사용 사례
- 로그 중앙화 및 시스템 모니터링
- 보안 로그 분석 및 SIEM(Security Information and Event Management)
- 애플리케이션 성능 모니터링(APM)
- 비즈니스 데이터 분석

## 장점
- 오픈소스 기반으로 시작 비용 낮으
- 대규모 데이터에 대한 수평 확장성과 빠른 검색/집계: Lucene 기반의 강력한 색인화 및 검색 엔진으로, 대용량 데이터에 대해서도 고속으로 복잡한 쿼리를 수행할 수 있음
  - 점수화 또는 머신러닝 기반 이상탐지 가능
- 다양한 데이터 소스(로그, 메트릭, 문서 등) 통합 분석 가능
- 분산 아키텍처를 기반으로 설계되어 뛰어난 확장성 지님: 노드를 추가하여 수평 확장 가능, 샤딩을 통해 병렬 처리 가능
- 장애 노드 감지 및 자동 복구 매커니즘이 내장되어 있어, 실무 환경에서 고가용성을 유지하며 안정적으로 운영 가능

## 단점
- 높은 자원 소모 및 저장 공간 부담: JVM 기반이라 메모리와 CPU 자원 소모가 큰 편이며, 대량의 로그를 처리하려면 클러스터에 상당한 사양의 하드웨어 자원 할당 필요
- 운영 및 관리의 복잡성: 클러스터 노드 구성, 샤드/레플리카 개수 튜닝, 인덱스 최적화, 성능 모니터링(GC 튜닝 등), 백업 및 장애조치 등 신경 써야 할 요소가 많아, 잘못된 설정이나 관리 부실로 인해 클러스터 상태가 불안정해지면, 검색 지연이나 노드 다운과 같은 문제가 발생할 수 있음

<br/>
<br/>
<br/>
출처
- https://velog.io/@kangking/ELK-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90
- https://www.devopsschool.com/blog/what-is-elk-stack-and-use-cases-of-elk-stack/
- https://tech.ktcloud.com/entry/ELK-%EC%8A%A4%ED%83%9D-%EA%B0%84%EB%8B%A8%ED%9E%88-%EA%B5%AC%EC%B6%95%ED%95%98%EA%B8%B0
- https://hstory0208.tistory.com/entry/ELK-ElasticSearch%EB%9E%80-ELK%EB%9E%80-%EC%9E%A5%EB%8B%A8%EC%A0%90-RDB%EC%99%80-%EC%B0%A8%EC%9D%B4#:~:text=%EB%98%90%ED%95%9C%20%EB%AA%A8%EB%93%A0%20%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%A5%BC%20%EC%97%AD%20%EC%83%89%EC%9D%B8,%EC%A0%80%EC%9E%A5%ED%95%B4%20%EA%B0%80%EA%B3%B5%EB%90%9C%20%ED%85%8D%EC%8A%A4%ED%8A%B8%EB%A5%BC%20%EA%B2%80%EC%83%89%20%ED%95%9C%EB%8B%A4
