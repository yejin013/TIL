# Oracle Hints
> Oracle Hint란, SQL 실행 계획을 옵티마이저가 기본적으로 선택하는 방식이 아닌, 사용자가 원하는 방식으로 유도하기 위해 SQL에 삽입하는 지시문
> 옵티마이저(Optimizer)가 자동으로 가장 효율적이라고 판단하는 실행 계획 대신, 개발자나 DBA가 직접 특정 인덱스를 사용하거나 조인 방식을 강제하도록 지시할 수 있는 기능

## Oracle의 특징
- 비용 기반 옵티마이저(Cost-Based Optimizer, CBO)를 사용하여 통계정보와 규칙에 따라 자동으로 최적의 실행 계획 선택
- 힌트: 옵티마이저의 의사결정에 명시적인 지침을 제공하여, 특정 접근 경로나 조인 방법 등을 강제하도록 설계

## 힌트 기본 문법
```sql
SELECT /*+ HINT_NAME */ column_list
FROM table_name
WHERE condition;
```
- /*+ ... */ : 일반 주석처럼 보이지만 +가 있으면 Oracle은 힌트로 인식
- 대소문자 구분하지 않음: USE_NL, use_nl 동일
- 힌트가 잘못 작성되면 단순 주석으로 무시: 에러 나지 않음

## 힌트 주요 목적
1. 옵티마이저 접근 방법(Access Path) 지정
  - 인덱스 사용 강제 (INDEX, FULL 등)
2. 조인 순서 지정 (Join Order)
  - 어떤 테이블을 먼저 읽고, 뒤에 읽을지 순서 고정
3. 조인 방식 지정 (Join Method)
  - NESTED LOOP, HASH JOIN, MERGE JOIN 등 강제
4. 병렬 처리 유도 (Parallel Execution)
  - PARALLEL 힌트
5. 쿼리 변환 제어 (Query Transformation)
  - Subquery unnesting, Merge 등 옵티마이저의 변환 기능을 제한/허용

## 주의사항
- 남용 금지 : 옵티마이저는 최신 통계정보를 바탕으로 실행계획을 고르는데, 힌트를 과도하게 사용하면 오히려 성능 저하.
- 통계 최신화 : 잘못된 통계정보가 있으면 힌트가 필요할 수 있음. (ANALYZE / DBMS_STATS)
- 환경 변화 대응 어려움 : 데이터량이 변하면 힌트로 강제한 실행계획이 더 느려질 수도 있음.
- SQL 튜닝의 최후 수단 : 인덱스 생성, 쿼리 구조 개선 후 마지막에 적용하는 게 일반적.

## 힌트 무시되는 상황
- 문법 오류 또는 오타
- 해결 불가한 힌트: 힌트에 지정한 대상 객체나 인덱스 등이 SQL에 존재하지 않는 경우
- 적용 불가능한 힌트: 해당 쿼리 상황에서 힌트를 적용할 수 없는 경우 (ex. 비동가 조인에 해시 조인 힌트 준 경우)
- 상충하는 힌트: 하나의 SQL에 서로 모순되는 지시가 여러 개 들어온 경우

## 자주 사용되는 힌트
### 최적화 목표
- /*+ ALL_ROWS */ : 전체 처리속도 최적화
- /*+ FIRST_ROWS(N) */ : 최초 N건 응답속도 최적화
- /*+ CHOOSE */ : 테이블의 통계정보 유무에 따라 규칙기반 또는 비용기반으로 최적화
- /*+ RULE */ : 규칙기반 옵티마이저를 이용 최적화

### Index Scan 방식
- /*+ FULL(<Table>) */ : 인덱스 타지말고 바로 테이블 풀스캔 유도
- /*+ INDEX(<Index>) */ : 지정한 B-Tree 인덱스를 통해 테이블 액세스 유도
- /*+ NO_INDEX(<Table> <Index>) */ : 해당 테이블에 특정 인덱스를 사용하지 않도록 옵티마이저 유도
- /*+ INDEX_DESC(<Table> <Index>) */ : 인덱스를 ORDER BY DESC 역순 유도
- /*+ INDEX_FFS(<Table> <Index>) */ : INDEX FAST FULL SCAN 유도
- /*+ INDEX_SS(<Table> <Index>) */ : INDEX SKIP SCAN 유도

### Join 순서
- /*+ ORDERED */ : FROM절에 나열된 순으로 Join
- /*+ LEADING<Table1> <Table2>...) */ : 조인 순서를 힌트에 나열한 테이블 순서대로 사용
- /*+ SWAP_JOIN_INPUTS(<Table>) */ : 해시 조인에서 해시 테이블(build)과 프로브 테이블을 뒤바꾸도록 지시. 입력을 교체해 작은 테이블을 Build, 큰 테이블을 Probe로 강제 (해시 조인 튜닝에 특화된 힌트)
  - 잘못된 통계정보 때문에 옵티마이저가 큰 테이블을 Build로 선택해 성능이 나빠질 때.
  - 파티션 조인 등에서 Build/Probe 선택이 비효율적일 때.
  - 조인 순서를 힌트로 제어했지만, 여전히 Build/Probe 방향을 반대로 잡아야 효율적인 경우.

### Join 방식
- /*+ USE_NL(<Table>) */ : NL(NESTED LOOP - 중첩루프)방식 조인 유도. 드라이빙 테이블의 결과를 기준으로 차테이블을 반복 탐색하는 방식으로, 보통 선행 결과셋이 작을 때 유리
  - 두 테이블 중 하나를 드라이빙(Driving, Outer) 테이블, 다른 하나를 드리븐(Driven, Inner) 테이블로 선택
  - 드라이빙 테이블의 행을 한 건씩 읽으면서, 매 건마다 드리븐 테이블 탐색
  - 소량 데이터 조회에 매우 빠름
  - 인덱스 기반 검색에 최적
  ```pqsql
    FOR each row in Outer_Table
     LOOP
        Find matching rows in Inner_Table (using index if possible)
     END LOOP
  ```
- /*+ NO_USE_NL(<Table>) */ : NL Join이 아닌 방식 유도
- /*+ USE_NL_WITH_INDEX(<Table> <Index>) */ : NL Join을 하면서 인덱스 사용
- /*+ USE_MERGE(<Table>) */: SORT MERGE 조인으로 유도. 양쪽 데이터셋을 조인 키로 정렬한 후 병합하는 방식으로, 정렬 비용이 들지만 비등치 조인이나 두 테이블 모두 매우 큰 경우에 고려
  - 두 입력 테이블을 조인 키 기준으로 각각 정렬한 뒤, 정렬된 상태에서 병합하면서 매칭
  - 입력이 이미 정렬되어 있거나 인덱스로 정렬 액세스하면 효율적
  - 비등치 조인 가능
- /*+ USE_HASH(<Table> */: HASH 조인으로 유도
  - 작은 테이블 (Build Input)을 메모리에 올려 해시 테이블 생성
  - 큰 테이블 (Probe Input)을 스캔하면서 해시 값을 계산해 Build Input의 해시 테이블과 매칭
  - 조인 키 기준 등치(=) 조건일 때만 사용 가능
- /*+ NO_USE_HASH(<Table> */ : HASH 조인 아닌 방식 유도
- /*+ NL_SJ */: NL SEMI조인으로 유도
- /*+ MERGE_SJ */: 소트머지 세미조인으로 유도
- /*+ HASH_SJ */: 해시 세미조인으로 유도

### 쿼리변환
- /*+ MERGE */ : 뷰 머징 유도 (COMPLEX_VIEW_MERGING = FALSE로 되어 있을 때 view 또는 subquery의 내용을 merge 가능
- /*+ NO_MERGE */ : 뷰 머징 방지 (COMPLEX_VIEW_MERGING = TRUE)
- /*+ UNNEST */ : 서브쿼리 UNNESTING 유도. EXISTS 또는 IN 형태의 서브쿼리를 일반 조인으로 변환하도록 옵티마이저에 지시
- /*+ NO_UNNEST */ : 서브쿼리 UNNESTING 방지
- /*+ PUSH_PRED */ : 조인조건 PUSHDOWN 유도
- /*+ NO_PUSH_PRED */ : 조인조건 PUSHDOWN 방지
- /*+ USE_CONCAT */ : OR 또는 IN-LIST조건을 OR-EXPANSION으로 유도
- /*+ NO_EXPAND */ : OR 또는 IN-LIST 조건에 대한 OR-EXPANSION방지
- /*+ PUSH_SUBQ */ : 비병합 서브쿼리(non-merged subquery)를 가능한 한 일찍 실행하도록 유도
- /*+ NO_PUSH_SUBQ */ : 서브쿼리 실행을 최대한 나중으로 미루도록 (메인쿼리 다 수행 후) 유도
- /*+MATERIALIZE */: WITH문으로 정의한 집합을 물리적으로 생성하도록 유도 
  - ex) WITH /*+ MATERIALIZE*/ T AS (SELECT ...)
- /*+INLINE */: WITH문으로 정의한 집합을 물리적으로 생성하지않고 INLINE 처리하도록 유도
  - ex)WITH /*+ INLINE*/ T AS (SELECT ...)

### 병렬처리
- /*+ PARALLEL(<Table> <병렬수>) */ : 병렬 수 만큼 병렬 처리 수행 지시
  - ex) /*+ PARALLEL(T1 4)*/
- /*+ NOPARALLEL(<Table> */ : 병렬처리 하지 않고 테이블 액세스 수행
- /*+ PARALLEL_INDEX(<Index> <병렬수>) */ : 인덱스 스캔을 병렬방식으로 처리하도록 유도
- /*+ PQ_DISTRIBUTE */: 병렬수행시 데이터 분배방식 결정
  - ex) PQ_DISTRIBUTE(T1 HASH(--BUILD INPUT) HASH(--PROBE TABLE))
 
### 그외 기타
- /*+APPEND*/ : DIRECT PATH INSERT유도로  INSERT  문에 주로 많이 쓴다
- /*+DRIVING_SITE */ : DB LINK REMOTE쿼리에 대한 최적화 및 실행 주체 지정 (LOCAL 또는  REMOTE)
- /*+PUSH_SUBQ */ : 서브쿼리를 가급적 빨리 필터링하도록 유도
- /*+NO_PUSH_SUBQ */ : 서브쿼리를 가급적 늦게 필터링 하도록 유도 

cf) 실무에서 자주 사용하는 힌트 조합 예시
- 대용량 배치 쿼리 : FULL + USE_HASH + PARALLEL
  - 수백만 건 이상의 대용량 테이블들을 조인/집계하는 배치 작업에서, 전체 테이블 스캔과 해시 조인, 그리고 병렬 처리를 결합하여 최대 처리량 확보
  - /*+ FULL(t1) FULL(t2) USE_HASH(t2) PARALLEL(t1 8) PARALLEL(t2 8) */ SELECT ... JOIN ...
  - 두 테이블 t1, t2를 모두 풀스캔하고, 조인 시 t2를 해시조인 방식으로 결합하며, 각 테이블 스캔을 8개 프로세스로 병렬화하도록 지시
- OLTP 소량 검색 쿼리 : INDEX + USE_NL (+ ORDERED 또는 LEADING)
  - 건별 조회나 소량 결과를 얻는 OLTP 쿼리에서, 인덱스를 활용한 중첩 루프 조인으로 응답속도 최소화
- 서브쿼리 성능 개선 : NO_UNNEST + PUSH_SUBQ 또는 UNNEST (+ 조인힌트)
  - 상관 서브쿼리가 다중 실행되어 과도한 반복 부하를 유발할 때, 서브쿼리 처리를 변경하여 성능 높임
- 데이터 적재 작업 : APPEND + PARALLEL
  - 대량의 데이터를 테이블에 적재하거나 테이블 간 이관할 때, 디렉트 패스 로드와 병렬 처리를 결합하여 최대 삽입
  - INSERT /*+ APPEND PARALLEL(a 4) */ INTO BIG_TABLE a SELECT ... FROM SOURCE_TABLE;
  - APPEND로 인해 REDO 로그 기록이 최소화되고 버퍼캐시 경유 없이 블록이 바로 디스크에 기록되므로, 일반 삽입보다 훨씬 빠르게 완료
  - 동시에 PARALLEL 4로 소스 읽기와 타겟 적재를 4배속으로 처리하여 거의 선형적으로 성능 높임

<br/>
<br/>
<br/>
출처

- https://bommbom.tistory.com/entry/%EC%98%A4%EB%9D%BC%ED%81%B4-%ED%9E%8C%ED%8A%B8hint-%EC%A2%85%EB%A5%98-%EB%B0%8F-%EC%82%AC%EC%9A%A9%EB%B2%95
- https://gurume.tistory.com/entry/오라클-자주사용하는-힌트목록-정리친절한-sql-튜닝 [Data + Marketing + INFP:티스토리]
- https://jack-of-all-trades.tistory.com/198
