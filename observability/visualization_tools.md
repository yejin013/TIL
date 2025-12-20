# 시각화도구 비교 (Grafana vs Kibana)

## 가장 큰 특징
> Grafana는 “여러 데이터 소스를 한 화면에서 시각화하는 모니터링 플랫폼”
  Kibana는 “Elasticsearch에 저장된 데이터를 검색·분석·시각화하는 로그 분석 도구”

## 항목별 특징 비교
| 항목            | **Grafana**                                          | **Kibana**                                       |
| ------------- | ---------------------------------------------------- | ------------------------------------------------ |
| **역할**        | 다양한 데이터 소스의 **통합 시각화·모니터링 도구**                       | Elasticsearch에 저장된 데이터의 **검색·분석·시각화 도구**         |
| **핵심 목적**     | 인프라 모니터링(Observability)                              | 로그 분석(Log Analytics)                             |
| **대표 데이터 소스** | Prometheus, InfluxDB, Loki, MySQL, CloudWatch 등 여러 개 | **Elasticsearch** (고정)                           |
| **데이터 흐름 구조** | 여러 소스 → Grafana 대시보드                                 | Logstash/Filebeat → Elasticsearch → Kibana       |
| **설치 구성 예시**  | Prometheus + Grafana                                 | ELK Stack (Elasticsearch + Logstash + Kibana)    |
| **시각화 초점**    | 메트릭(metric) 기반 (시간 시계열 데이터 중심)                       | 로그(log)와 검색 쿼리 중심                                |
| **확장성/플러그인**  | 다양한 데이터 플러그인 지원                                      | Elasticsearch 기반 확장 기능 중심                        |
| **알림 기능**     | 내장 Alerting (Slack, Email 등 통합)                      | 기본 Kibana에는 없고, Elastic Stack Alerting 기능을 통해 가능 |
| **운영 형태**     | 오픈소스 / Grafana Cloud / Enterprise                    | Elastic Stack 통합형 (Elastic Cloud / Self-hosted)  |
| **대표 사용 사례**  | 서버·시스템 모니터링, 대시보드, DevOps                            | 로그 수집, 에러 분석, 검색 기반 분석                           |

## 상황별 특징 비교
| 상황                                       | 적합한 도구                             | 이유                                  |
| ---------------------------------------- | ---------------------------------- | ----------------------------------- |
| 서버 CPU·메모리 사용률을 실시간 그래프로 보고 싶다           | 🟢 **Grafana**                     | Prometheus 등 메트릭 수집기와 바로 연동됨        |
| 애플리케이션 로그에서 “에러” 키워드가 얼마나 자주 발생하는지 보고 싶다 | 🟣 **Kibana**                      | Elasticsearch의 로그 인덱스를 실시간 검색/분석 가능 |
| 여러 데이터베이스나 API의 상태를 한 화면에서 보고 싶다         | 🟢 **Grafana**                     | 다양한 데이터 소스 통합 시각화                   |
| 로그를 시계열로 보면서 에러 발생 시점 추적하고 싶다            | 🟣 **Kibana**                      | 로그 탐색 및 필터링 기능 강력                   |
| DevOps 모니터링 시스템을 구축하려고 한다                | 🟢 **Grafana + Prometheus + Loki** | 모니터링/알람 중심 구성                       |
| 로그 분석 기반 SIEM(보안 관제)을 구축하려고 한다           | 🟣 **Kibana + Elasticsearch**      | 보안 이벤트 분석에 최적                       |
