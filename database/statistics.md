# 통계정보

## 개념
> 통계정보란, 데이터베이스 객체(테이블, 인덱스 등의 데이터)의 데이터 분포와 저장 특성을 요약한 메타데이터

## 특징
- Oracle의 옵티마이저가 실행계획을 작성할 때 참조하는 정보
- 데이터베이스의 테이블, 인덱스 등과 관련된 객체의 특성 분석 및 수집
- 통계는 DBMS 내부 데이터 사전(dataa dictionary)에 저장되며, 질의 옵티마이저(query optimizer)가 SQL 문의 최적 실행 계획을 선택하는 데 사용

## 고려사항
통계정보 수집은 CPU, 시스템 I/O 자원을 많이 사용하는 작업으로, 아래 사항들을 고려해야한다.
- 시간: 시스템 부하가 적은 날짜와 시간을 산정해 수집
- 샘플 크기: 데이터베이스와 세그먼트의 크기예 비례해 일정 부분만 추출
- 정확성: 오브젝트의 데이터와 통계정보의 데이터가 근접해야 함
- 안정성: 통계정보 수집으로 인한 데이터베이스 성능 저하를 최소화해야 함

## 종류
### 테이블 통계 (Table Statistics)
테이블 전체 규모와 저장 특성을 나타내는 기본 지표

- 총 행(Row) 수 (NUM_ROWS): 테이블에 저장된 레코드 개수
- 블록 수 (BLOCKS / NUM_BLOCKS): 테이블이 차지하는 데이터 블록(페이지) 수
- 평균 행 길이 (AVG_ROW_LEN): 평균 레코드 크기(바이트 단위)
- Empty Blocks (EMPTY_BLOCKS): 비어 있는 블록 개수
- 체인/마이그레이션 행 수 (CHAIN_CNT): 행 체인/마이그레이션 발생 건수(행이 분산 저장된 경우)

### 컬럼 통계 (Column Statistics)
컬럼 값의 분포도를 나타내는 지표

- 카디널리티 (NUM_DISTINCT): 해당 컬럼의 고유 값 개수
- NULL 값 개수 (NUM_NULLS): 컬럼에 저장된 NULL 값의 개수
- 최소/최대 값 (LOW_VALUE / HIGH_VALUE): 컬럼 값의 범위
- 데이터 분포 히스토그램 (HISTOGRAM): 값의 분포(균일/편향 여부)를 구간(bucket)으로 나눠 저장
- Density (밀도): 특정 값 선택 확률의 역수. 선택도 계산 시 활용

### 인덱스 통계 (Index Statistics)
인덱스 구조 및 테이블과의 정렬 관계를 나타냄

- 인덱스 높이 (BLEVEL): 루트에서 리프까지의 레벨 수
- 리프 블록 수 (LEAF_BLOCKS): 인덱스 리프 블록 개수
- 클러스터링 팩터 (CLUSTERING_FACTOR): 인덱스 키 순서와 실제 테이블 행 순서의 일치 정도. 값이 작을수록 정렬이 잘 되어 있음
- Distinct Keys (DISTINCT_KEYS): 인덱스 키 값의 고유 개수
- Average Leaf Entries (AVG_LEAF_BLOCKS_PER_KEY): 키당 평균 리프 블록 개수
- Average Data Blocks (AVG_DATA_BLOCKS_PER_KEY): 키당 평균 데이터 블록 개수

### 파티션 통계 (Partition Statistics)
파티션된 테이블/인덱스 단위 통계

- 파티션 수 및 각 파티션의 행 수 / 블록 수
- 파티션별 컬럼/인덱스 통계도 별도 수집 가능
- Oracle은 파티션 통계를 합산하여 글로벌(Global) 통계로 보정할 수도 있음

### 도메인 인덱스 통계 (Domain Index Statistics)
텍스트 인덱스, 공간 인덱스 등 비전통적 인덱스용 통계

- 인덱스 구조 높이 / 블록 수
- 클러스터링 팩터

### 시스템/운영 통계 (System & Usage Statistics)
SQL 최적화를 위한 환경 및 운영 지표.

- 시스템 통계: CPU 속도, I/O 성능 등 하드웨어 성능 지표. (DBMS_STATS.GATHER_SYSTEM_STATS)
- 사용 통계: 실행 시점에 수집되는 사용률(usage), 최근 DML 변경 내역
 - DML 변경률: INSERT, UPDATE, DELETE 횟수 (→ 통계가 오래되었는지 판단하는 기준)
 - 인덱스 선택도 (Selectivity): 인덱스가 특정 조건에서 얼마나 잘 걸러내는지 비율
 - 테이블 사용률: 전체 대비 특정 조건에서 참조되는 행 비율

## Oracle DB의 통계정보 수집 방식: DBMS_STATS
> EXEC DBMS_STATS.GATHER_SCHEMA_STATS('OE', ESTIMATE_PERCENT => DBMS_STATS.AUTO_SAMPLE_SIZE);
### 1. 전체 스키마 통계 수집
```sql
BEGIN
  DBMS_STATS.GATHER_SCHEMA_STATS(
    ownname          => 'HR',
    estimate_percent => DBMS_STATS.AUTO_SAMPLE_SIZE,
    method_opt       => 'FOR ALL COLUMNS SIZE AUTO',
    cascade          => TRUE
  );
END;
/
```
- AUTO_SAMPLE_SIZE: Oracle이 자동으로 샘플링 비율 결정
- FOR ALL COLUMNS SIZE AUTO: 필요한 경우 히스토그램 자동 생성
- cascade=>TRUE: 관련 인덱스까지 통계 수집

### 2. 특정 테이블 통계 수집
```sql
EXEC DBMS_STATS.GATHER_TABLE_STATS('HR', 'EMPLOYEES');
```

### 3. 시스템 통계 수집 (CPU/I/O 환경)
```sql
EXEC DBMS_STATS.GATHER_SYSTEM_STATS('start');
```
-- 일정 시간 부하를 준 후
```sql
EXEC DBMS_STATS.GATHER_SYSTEM_STATS('stop');
```
- 자동 통계수집: Oracle 10g 이상은 매일 밤 자동 잡(GATHER_STATS_JOB)으로 통계를 자동 업데이트

## 문제 발생 원인
통계정보 유무에 따라 plan을 다르게 탐
- 통계정보의 유무에 따라 옵티마이저가 카디널리티를 다르게 추정하여, 조인 순서와 액세스 경로(인덱스 스캔 방식)을 바꿔 잡은 케이스
- 카디널리티 추정의 차이
 - 통계가 있으면: 테이블/인덱스/컬럼(히스토그램 포함) 통계를 기반으로 더 정확한 선택도 추정 → “비용이 낮다”고 판단되는 조인 순서/인덱스를 선택
 - 통계가 없으면: Dynamic Sampling(동적 샘플링)이나 기본 휴리스틱에 의존 → 선택도가 왜곡되어 다른 조인 순서가 선택되거나, UNIQUE SCAN ↔ RANGE SCAN 같은 스캔 타입이 달라짐
- 인덱스 고유성/히스토그램 인식
 - 특정 컬럼 분포(편향)가 히스토그램으로 반영되지 않으면, 옵티마이저가 “생각보다 넓게(또는 좁게)” 잡아 RANGE SCAN을 선호하거나, 조인 순서 바꿈
 - PK/UK 인덱스라도, 조건식 형태(함수 적용, 데이터 타입 미스매치, 암묵적 형변환 등)와 통계 부재가 겹치면 UNIQUE SCAN 대신 RANGE SCAN으로 떨어짐
- 바인드 피킹 & ACS(Adaptive Cursor Sharing)
 - 통계가 부실하면 초기 바인드 값으로 만든 플랜이 고착되거나(바인드 피킹), 공유 커서가 쪼개지는 과정에서 서로 다른 플랜 공존 가능
ex)
|유|무|
|---|---|
|SELECT STATE Optimizer=FIRST_ROWS|SELECT STATE Optimizer=FIRST_ROWS|
 VIEW| VIEW|
|   COUNT|   COUNT|
|     NESTED LOOPS|     NESTED LOOPS|
|       NESTED LOOPS|       NESTED LOOPS|
|         NESTED LOOPS|         NESTED LOOPS|
|          TABLE ACCESS (BY INDEX ROWID) OF 'A' (TABLE)|          TABLE ACCESS (BY INDEX ROWID) OF 'A' (TABLE)|
|             INDEX (UNIQUE SCAN) OF 'A_PK' (INDEX (UNIQUE))|             INDEX (UNIQUE SCAN) OF 'A_PK' (INDEX (UNIQUE))|
|          TABLE ACCESS (BY INDEX ROWID) OF 'B' (TABLE)|          TABLE ACCESS (BY INDEX ROWID) OF 'C' (TABLE)|
|            INDEX (UNIQUE SCAN) OF 'B_PK' (INDEX (UNIQUE))|            INDEX (RANGE SCAN) OF 'C_PK' (INDEX (UNIQUE))|
|        INDEX (RANGE SCAN) OF 'C_PK' (INDEX (UNIQUE))|        INDEX (UNIQUE SCAN) OF 'B_IDX' (INDEX (UNIQUE))|
|       TABLE ACCESS (BY INDEX ROWID) OF 'C' (TABLE)|       TABLE ACCESS (BY INDEX ROWID) OF 'B' (TABLE)|

<br/>
<br/>
<br/>
출처

- https://docs.oracle.com/cd/B19306_01/server.102/b14211/stats.htm#:~:text=Optimizer%20statistics%20are%20a%20collection,Optimizer%20statistics%20include%20the%20following
- https://yoon001.tistory.com/92#:~:text=%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4%20%EA%B0%9D%EC%B2%B4%EC%9D%98%20%ED%86%B5%EA%B3%84%20%EC%A0%95%EB%B3%B4%EB%A5%BC%20%EC%88%98%EC%A7%91%ED%95%98%EA%B3%A0%20%EA%B4%80%EB%A6%AC%ED%95%98%EB%8A%94%20%ED%8C%A8%ED%82%A4%EC%A7%80%EC%9D%B4%EB%8B%A4.,%EB%8D%B0%EC%9D%B4%ED%84%B0%EB%B2%A0%EC%9D%B4%EC%8A%A4%EC%97%90%20%EC%9E%88%EB%8A%94%20%EC%BB%AC%EB%9F%BC%2C%20%ED%85%8C%EC%9D%B4%EB%B8%94%2C%20%EB%8D%B0%EC%9D%B4%ED%84%B0%20%EC%82%AC%EC%A0%84(DD:%20Data
- https://maybetoday.tistory.com/entry/ORACLE-%ED%86%B5%EA%B3%84%EC%A0%95%EB%B3%B4-%EC%88%98%EC%A7%91%EA%B3%BC-%EB%B0%B1%EC%97%85-%EC%BF%BC%EB%A6%AC
