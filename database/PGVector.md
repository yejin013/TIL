# PGVector
> PGVector란, PostgreSQL의 확장(extension) 중 하나로, 데이터베이스에 벡터(vector) 데이터 타입과 벡터 연산 기능을 추가해주는 모듈

## 벡터
> 벡터란, 다차원 공간에서 데이터를 나타내는 수학적 표현으로, AI 모델에서 이미지, 텍스트, 음성 데이터를 벡터로 변환하여 벡터 간 유사도를 계산하는 데 사용

## 주요 기능
- 벡터 타입 지원 및 데이터 저장: vector(n) 형태로 차원(n)을 가진 컬럼 정의 및 PostgreSQL 내에서 벡터 데이터 효율적으로 저장
- 거리/유사도 계산 연산자 제공 및 유사도 검색: 코사인 유사도, 유클리드 거리, 내적 등 다양한 벡터 유사도 검색
  - <-> : 유클리드 거리(L2 distance) → 값이 작을수록 유사
  - <=> : 코사인 거리(cosine distance) → 값이 0에 가까울수록 유사
  - <#> : 내적(dot product) (음수화된 값 반환) → 값이 작을수록 유사
  - <+> : 맨해튼 거리(L1 distance) → 값이 작을수록 유사
  - <~>, <%> : 이진 벡터용 (해밍 거리, 자카드 거리)
- 인덱스 최적화 지원: 대량 데이터에서 빠른 검색을 위해 IVFFlat, HNSW 같은 근사 최근접 탐색(ANN) 인덱스 제공
  ```
  CREATE INDEX ON items USING hnsw (embedding vector_cosine_ops);
  ``` 
- PostgreSQL과 완벽 통합: 별도 벡터DB를 쓰지 않아도 Postgres 하나로 RDB + 벡터 검색을 동시에 쓸 수 있음.
   ```
  -- 코사인 유사도 기준 Top-3 검색
  SELECT id, content, embedding <=> '[0.10, 0.95, ...]' AS distance
  FROM documents
  ORDER BY distance ASC
  LIMIT 3;
  ```

## PGVector 설치
- Ubuntu
   ```
  sudo apt-get update
  sudo apt-get install postgresql-15-pgvector
   ```
- MacOS (Homebrew)
   ```
  brew install pgvector
   ```
- Docker
   ```
  FROM postgres:15
  
  RUN apt-get update \
   && apt-get install -y postgresql-15-pgvector \
   && rm -rf /var/lib/apt/lists/*
   ```
- PostgreSQL에서 확장 활성화
   ```
  -- 슈퍼유저 권한 필요
  CREATE EXTENSION vector;
   ```


   
<br/>
<br/>
<br/>
출처

- https://ace28.tistory.com/entry/PostgreSQL%EC%97%90%EC%84%9C%EC%9D%98-pgvector-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%96%B4%EB%96%BB%EA%B2%8C-%ED%99%9C%EC%9A%A9%ED%95%A0-%EC%88%98-%EC%9E%88%EC%9D%84%EA%B9%8C
- https://edbkorea.com/blog/pgvector%EB%9E%80-%EB%AC%B4%EC%97%87%EC%9D%B4%EB%A9%B0-%EC%96%B4%EB%96%BB%EA%B2%8C-%EB%8F%84%EC%9B%80%EC%9D%B4-%EB%90%A0-%EC%88%98-%EC%9E%88%EC%9D%84%EA%B9%8C%EC%9A%94/
