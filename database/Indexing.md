# 인덱스 (Index)
> 인덱스(Index)란, 데이터베이스 분야에 있어서 테이블에 대한 동작의 속도를 높여주는 자료 구조
- 데이터의 위치를 빠르게 찾아주는 역할
- 테이블 내의 1개의 컬럼, 혹은 여러 개의 컬럼을 이용하여 생성될 수 있다.
- 고속의 검색 동작뿐만 아니라 레코드 접근과 관련 효율적인 순서 매김 동작에 대한 기초를 제공한다.
- 주로 컬럼의 값과 물리적 주소를 키-필드로 갖는다.
- RDBMS에서는 인덱스는 테이블 부분에 대한 하나의 사본이다.
- 단일 인덱스를 여러 개 생성할 수도, 여러 컬럼을 묶어 복합 인덱스를 설정할 수도 있다.
- 조회 시 자주 사용하고 고유한 값 위주로 설정하기!

## 인덱스 설정 기준
데이터의 양이 많고, 검색이 변경보다 잦은 경우 
인덱스를 사용하고자 하는 컬럼의 값이 다양한 경우 
- 카디널리티
  - 카디널리티가 높을수록 인덱스 설정에 좋은 컬럼
  - 컬럼에 사용되는 값의 다양성 정도 (중복 수치)를 나타내는 지표
  - 중복 정도가 낮으면 카디널리티가 높다.
- 선택도
  - 선택도가 낮을수록 인덱스 설정에 좋은 컬럼
  - 5 ~ 10% 정도가 적당  
  - 데이터에서 특정 값을 얼마나 잘 선택할 수 있는지에 대한 지표
  - 전체 레코드 중에서 조건절에 의해 선택될 것으로 예상되는 레코드의 비율(%)이다.   
  - 컬럼의 특정 값의 row 수 / 테이블의 총 row 수 * 100
  - 컬럼의 값들의 평균 row 수 / 테이블의 총 row 수 * 100  
- 활용도
  - 활용도가 높을수록 인덱스 설정에 좋은 컬럼
  - 해당 컬럼이 실제 작업에서 얼마나 활용되는지에 대한 값
- 중복도
    - 중복도가 없을수록 인덱스 설정에 좋은 컬럼

## 장점
- 테이블을 검색하는 속도와 성능 향상

### 조건 검색 Where절의 효율성
- where절에 맞는 데이터를 빠르게 찾을 수 있다.
- 인덱싱 없이 테이블을 만들면 where절 검색을 하면 풀 테이블 스캔을 한다.

### 정렬 Order by 절의 효율성
- sort과정을 피할 수 있다.
- order by는 부하가 많이 걸리는 작업 

### MIN, MAX의 효율적인 처리
- 레코드의 시작값과 끝 값 한 건씩만 가져오면 된다.
- 풀 테이블 스캔이 필요 없다.

## 단점 
- 정렬된 상태를 계속 유지시켜줘야 한다.
- 인덱스를 관리하기 위한 추가 작업 필요 
  - INSERT : 새로운 데이터에 대한 인덱스 추가
  - DELETE : 삭제하는 데이터의 인덱스를 사용하지 않는다는 작업 수행
  - UPDATE : 기존의 인덱스를 사용하지 않음 처리, 갱신된 데이터에 대한 인덱스 추가
    - 인덱스를 제거하는 것이 아니라 사용하지 않음 처리 
- 추가 저장 공간 필요 
- 잘못 사용하는 경우 오히려 검색 성능 저하
- 수정이 잦은 경우 성능이 낮아진다. + 실제 데이터에 비해 인덱스가 과도하게 커질 수 있다. 

## 인덱스의 자료구조 
### B+Tree
- leaf node에만 데이터를 저장하고 node에는 자식 포인트만 저장한다.
  - 메모리 확보 가능
  - 하나의 node에 많은 포인터를 가질 수 있어 트리의 높이가 더 낮아져 검색 속도를 높일 수 있음 
- leaf node끼리는 linked list로 연결되어 있다.
  - full scan에서 선형 시간 소모
- 중간 node에서 key를 올바르게 찾아가기 위해 key가 중복될 수 있다.

### 해시 테이블 
- (key, value) = (컬럼의 값, 데이터의 위치)
- key값을 이용해 대응하는 value 값을 구하는 방식 
- 평균적으로 O(1)의 매우 빠른 시간만에 원하는 데이터를 탐색할 수 있는 구조
- 등호(=) 연산에 최적화되어 있다.
- 해시 테이블 내의 데이터들은 정렬되어 있지 않아 특정 기준보다 크거나 작은 값을 빠른 시간 내에 찾을 수 없다.


참고자료: https://ko.wikipedia.org/wiki/인덱스_(데이터베이스), https://rebro.kr/167, https://yurimkoo.github.io/db/2020/03/14/db-index.html
