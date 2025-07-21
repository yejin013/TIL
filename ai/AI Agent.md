# AI Agent
> AI Agent란, 자율적으로 환경과 상호작용하고, 의사결정을 내리며, 복잡한 목표를 달성하기 위해 행동할 수 있는 시스템 또는 프로그램
- 환경으로부터 인지(perception)하고 행동(action)하며, 자율성(autonomy)과 학습 능력을 갖춤으로써 주어진 목표(goal)를 달성하려 노력하는 인공지능 시스템
- cf) 환경: 에이전트가 처해있는 세계

## 특징
- 자율성 및 목표 지향적 행동
    - 사용자 또는 다른 시스템을 대신하여 스스로 작업 수행
    - 환경 피드백과 축적된 경험을 바탕으로 행동 조정
- 추론 및 계획 수립
    - 복잡한 문제를 관리 가능한 하위 작업으로 분해
    - 사용 가능한 정보를 바탕으로 추론
    - 목표 달성을 위한 일련의 행동을 순서대로 정리
- 도구 사용 및 환경 상호작용
    - 외부 API, 데이터베이스, 계산 도구, 심지어 물리적 장치와도 원활하게 통합되어 기능 확장 가능
- 학습 및 적응
    - 경험과 피드백을 통해 성능을 지속적으로 개선
    - 인간 피드백 기반 강화 학습(RLHF)은 에이전트의 행동을 인간의 선호도에 맞춰 조정하는 데 활용
    - 인간 피드백 기반 강화 학습(RLHF) : AI 에이전트의 행동을 인간의 선호도와 의도에 맞춰 조정하고 성능을 개선하는 강력한 기술
        1. 인간 평가 수집: 먼저 에이전트의 출력물에 대한 인간의 판단(선호도) 수집
        2. 보상 모델 학습: 보상 모델(reward model) 훈련 (에이전트가 어떤 행동을 했을 때 인간이 선호하는 보상을 받을지 예측)
        3. 에이전트 행동 최적화: 에이전트의 행동을 최적화하여 예상 보상 극대화

## 핵심 구성 요소
- 인지(Perception): 에이전트와 환경 간의 인터페이스로, 외부 정보를 수집하고 처리
- 지식 표현(Knowledge Representation): 에이전트 내부에 정보 저장, 구성, 검색하는 구조와 메커니즘 제공 (사실(declarative knowledge), 절차(procedural knowledge), 경험(episodic knowledge), 메타 지식(meta-knowledge) 등 다양한 유형의 지식)
- 추론 및 의사결정(Reasoning & Decision Making): 사용 가능한 정보를 처리하고, 대안을 평가하며, 적절한 행동 선택
- 행동 선택 및 실행(Action Selection & Execution): 의사결정을 환경에 영향을 미치는 구체적인 행동으로 전환. 외부 도구 및 시스템과의 통합을 통해 에이전트의 행동 범위 확장
- 학습 및 적응(Learning & Adaptation): 경험과 피드백을 기반으로 시간이 지남에 따라 성능을 개선하는 메커니즘 포함
- 계획 모듈(Planning Modules): 원하는 목표를 달성하기 위한 일련의 행동 구성하는 역할
- 기억 관리 시스템(Memory Management Systems): 여러 상호 작용 및 시간 스케일에서 정보를 유지하여 에이전트가 경험으로부터 학습하고 컨텍스트 유지
- 자체 모니터링 및 메타인지(Self-monitoring & Metacognitive Components): 에이전트가 자신의 성능 평가, 한계 인식하여 접근 방식 조정
- 통신 인터페이스(Communication Interfaces): 사용자와 다른 에이전트와의 상호작용 가능
- 안전 및 정렬 메커니즘(Safety & Alignment Mechanisms): 에이전트의 행동이 적절한 범위 내에 유지되고 인간의 가치 및 의도와 일치하도록 보장

## 주요 아키텍처
- ReAct (Reasoning and Action)
- ReWOO (Reasoning WithOut Observation)
- LLMCompiler

## 예시
- 물리적 에이전트 : 자율주행 자동차, 공장 현장의 로봇 조수 등
- 소프트웨어 에이전트 : 챗봇, 가상 비서, 정보 검색 에이전트, 추천 시스템, 자동 거래 프로그램 등

<br/>
<br/>
<br/>
출처 

- AI Agents: Evolution, Architecture, and Real-World Applications/Naveen Krishnan/March 2025
- [huyenchip.com](https://huyenchip.com/2025/01/07/agents.html)
