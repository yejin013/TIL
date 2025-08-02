# vLLM
> vLLM이란, 대형 언어 모델(LLM) 추론 및 제공을 위해 특별히 설계된 고처리량 및 메모리 효율적인 라이브러리

## 특징
- PagedAttention: 기존 시퀀스 기반 Attention 방식 대신 페이지 기반의 유연한 메모리 할당 전략을 도입하여 배치 효율 극대화
- 비동기 엔진 구조: 추론 요청을 효율적으로 처리하기 위한 비동기 처리 구조로 높은 처리량 유지

<img width="1280" height="867" alt="image" src="https://github.com/user-attachments/assets/13e652fa-e4fa-4b1f-a9bb-29917c4abbfa" />

## PagedAttention
- 운영 체제 내의 페이징 시스템과 가상 메모리에서 영감을 얻은 vLLM을 통해 도입된 메모리 관리 기술
- 키 값(KV) 캐시(LLM의 단기 메모리)가 처리 중에 어떻게 감소하고 증가하는지 확인하고 더욱 안정적인 방식으로 공간과 컴퓨팅 성능을 관리하기 위한 솔루션
### KV 캐시
- KV: 키 값(Key Value) / 캐시: 단기 메모리 스토리지
- LLM이 단어 또는 문구의 의미를 형성하는 방식
- 각 키(또는 토큰)에 해당하는 값을 캐시에 저장
- ex) 프렌치프라이(키)의 가격은 3.99달러(값)로 책정 -> 계산원이 프렌치프라이 주문을 등록하면 해당 '키'의 계산된 '값'은 3.99달러
### 연속적 배치 처리
- 전반적인 처리 효율을 개선하기 위해 여러 쿼리를 동시에 처리하는 데 사용되는 기법
- 새로운 응답을 생성하는 대신 KV 캐시에 메모리를 저장하고 이전에 수행한 계산과 유사한 새 쿼리에 대한 바로가기를 만들 수 있음
- vLLM을 사용하면 챗봇은 이 토큰 문자열('수도가 어디야')을 단기 메모리(KV 캐시)에 저장하고 번역 요청을 두 개 대신 하나만 보낼 수 있음

## 장점
- 최첨단 성능: vLLM은 많은 기준 구현에 비해 상당히 높은 처리량 제공
- 효율적인 메모리 관리: 핵심 혁신인 PagedAttention은 KV 캐시를 보다 효과적으로 관리하여 메모리 낭비를 극적으로 줄임
- 지속적인 배치 처리: 요청이 도착하는 대로 동적으로 처리하여 GPU 활용도를 크게 높이고 개별 요청의 대기 시간 줄임
- OpenAI 호환 서버: OpenAI API를 모방하는 내장 서버 포함
- 사용 용이성: 오프라인 추론(LLM 클래스) 및 온라인 제공(vllm serve 명령) 모두에 대해 상대적으로 간단한 API 제공
- 광범위한 모델 호환성: Hugging Face Hub(및 가능성 있는 ModelScope)에 있는 다양한 인기 오픈 소스 LLM 지원
- 활발한 개발 및 커뮤니티: 지속적으로 개선되고 버그 수정이 이루어지는 활성 유지 관리 오픈 소스 프로젝트
- 최적화된 커널: CUDA 커널을 사용하여 NVIDIA GPU에서 성능 향상

## 성능 최적화 기능
- PagedAttention
- PyTorch 컴파일/CUDA 그래프로 GPU 메모리 최적화
- 양자화(Quantization)로 모델 실행에 필요한 메모리 공간 절약
- 텐서 병렬 처리로 여러 GPU에 처리 작업 분할
- 추측 디코딩(Speculative Decoding)으로 작은 모델을 사용하여 토큰을 예측하고 더 방대한 모델을 사용하여 해당 예측을 검증하는 방식으로 텍스트 생성 속도 향상
- 플래시 어텐션(Flash Attention)으로 트랜스포머 모델의 효율성 개선

<br/>
<br/>
<br/>
출처
- https://apidog.com/kr/blog/vllm-kr/
- [https://ariz1623.tistory.com/375](https://blog.vllm.ai/2024/09/05/perf-update.html?utm_source=chatgpt.com#appendix)
- https://www.redhat.com/ko/topics/ai/what-is-vllm
