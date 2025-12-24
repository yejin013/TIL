# LightGBM

## 개념
> LightGBM이란, Light Gradient-Boosting Machine의 약자로, 기계 학습을 위한 자유-오픈 소스 분산 그라디언트 부스팅 프레임워크

## 알고리즘
### 리프 중신 트리 분할
- LightGBM은 Tree가 수직적으로 확장
- 최대 손실 값을 가지는 리프 노드를 지속적으로 분할하면서 트리의 깊이가 깊어지고 비대칭적인 트리 생성
- 학습을 반복하면 균형 트리 분할 방식보다 예측 오류 손실을 최소화할 수 있음

### Gradient Boostring Machine (GBM) 부스팅
- GBM 알고리즘: 틀린 부분에 가중치를 더하면서 진행하는 알고리즘

## 특징
- 결정 트리 알고리즘 기반
- 순위 학습, 통계적 분류 및 기타 기계 학습 작업에 사용

## 장단점
### 장점
- 더 빠른 학습과 예측 수행 시간: 통상 XGBoost 학습속도의 1.3~1.5배
- 더 작은 메모리 사용량
- 표로 정리된 데이터에서 Catboost, XGBoost와 함께 가장 좋은 성능 보임 (LightGBM ≒ Catboost > XGBoost)

### 단점
- 데이터가 적은 경우 과적합 가능성 큼

## 하이퍼 파라미터 튜닝 (LGBMClassifier)
- 트리구조
  - max_depth
  - num_leaves
  - min_child_samples
  - min_child_weight
- 샘플링 비율
  - subsample : record 기반으로 100만건 있으면 거기에 몇 퍼센트를 할 것인가
  - colsample_bytree : 컬럼 샘플링을 하지 않는 1이 기본값이나, 0.7~0.9 정도로 세팅하는 편이 일반적이다
- 손실 함수 규제 (loss값만 가지고 하면 overfitting 나기 쉬움)
  - reg_lambda
  - reg_alpha

<br/>
<br/>
<br/>
출처

- https://ko.wikipedia.org/wiki/LightGBM
- https://databoom.tistory.com/entry/LightGBM%EC%84%A4%EB%AA%85
- https://day-to-day.tistory.com/37
- https://gyun930.tistory.com/28
