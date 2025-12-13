# AI Agent VS Agentic AI

## 개념
> AI Agent란, 사용자의 목표를 대신 달성하기 위해 환경을 인지하고, 추론하며, 자율적으로 계획을 세우고 행동하는 지능형 소프트웨어 시스템

> Agentic AI란, 인간의 지시나 개입 없이도 스스로 목표를 설정하고, 문제를 인식하며, 적절한 도구를 활용해 작업을 수행하고, 그 결과를 학습에 반영하는 자율 실행형 AI

> 핵심 개념 차이 : AI Agent는 보통 “하나의 작업이나 역할에 특화된 실행 단위”를 의미하는 반면, Agentic AI는 “목표를 스스로 정의·분해하고 여러 에이전트와 도구를 orchestration하는 자율적 시스템/아키텍처”를 가리키는 말

<img width="725" height="378" alt="Image" src="https://github.com/user-attachments/assets/3a125394-01a2-4589-9605-bc4fba05f6f5" />

## 핵심 특징
### AI Agent
- 자율성 (Autonomy): 사람의 직접적인 개입 없이 할당된 작업을 독립적으로 수행한다.
- 반응성 (Reactivity): 주변 환경의 변화를 감지하고 그에 맞춰 적절하게 대응한다.
- 목표 지향성 (Goal-Oriented): 명확하게 정의된 목표를 달성하기 위해 일련의 행동을 계획하고 실행한다.
- 학습 능력 (Learning): 경험을 통해 시간이 지남에 따라 성능을 개선하고 새로운 상황에 적응한다.

### Agentic AI
- 고도의 자율성과 주도성: 단순히 주어진 목표를 수행하는 것을 넘어, 스스로 목표를 설정하거나 모호한 목표를 구체화하고 이를 달성하기 위한 전략을 주도적으로 수립한다.
- 멀티 에이전트 협업 (Multi-Agent Collaboration): 하나의 거대한 목표를 여러 하위 작업으로 분해한 뒤, 각 작업에 특화된 여러 AI 에이전트에게 할당하고 그 과정을 조율한다. 예를 들어, '시장 분석 보고서 작성'이라는 목표를 위해 '데이터 수집 에이전트', '데이터 분석 에이전트', '보고서 작성 에이전트'가 서로 협력하는 식
- 동적 적응 및 학습: 변화하는 환경과 예기치 못한 문제에 실시간으로 적응하며 계획을 수정한다. 각 에이전트의 피드백과 전체 시스템의 성공/실패 경험을 통해 시스템 전체가 학습하고 발전한다.
- 복잡한 문제 해결: 단일 에이전트로는 해결하기 어려운, 여러 단계와 전문성이 요구되는 복잡하고 동적인 문제를 해결하는 데 강점을 가진다.

## 기술적 구조와 메모리

| 구분    | AI Agent                | Agentic AI                      |
| ----- | ----------------------- | ------------------------------- |
| 기본 단위 | 단일 에이전트(LLM+도구) 중심 실행자  | 여러 서브 에이전트가 협업하는 상위 시스템         |
| 목표 구조 | 외부에서 주어진 단일·단기 목표       | 상위 목표 → 하위 목표 자동 분해             |
| 메모리   | 에피소드/세션 중심, 제한적 상태 유지   | 장기·지속 메모리 기반 컨텍스트 관리            |
| 자율성   | 제한적·반응형, 미리 짜둔 워크플로우 범위 | 높은 자율성, 계획 수정·전략 변경 가능          |
| 역할    | “실행자(Worker)”           | “조율자·지휘자(Planner/Orchestrator)” |

- AI Agent: 보통 특정 API와 도구를 호출하면서 한 태스크를 잘 수행하도록 설계
- Agentic AI: 이런 에이전트들을 여러 개 묶어 워크플로우를 구성하고, 과거 실행 이력과 장기 메모리를 기반으로 전략을 계속 바꾸는 구조

## 행동 양식 및 사용 예시
- AI Agent 사례
  - FAQ 챗봇, 이메일 자동 분류, 간단한 RAG 기반 질의응답처럼 “요청이 들어오면 정해진 방식으로 처리하는” 패턴에 강하다.​
  - 보안 이상 징후 알림, 정기 리포트 생성 등 반복·루틴 업무 자동화에 적합하다.​

- Agentic AI 사례
    - 보안에서는 네트워크를 지속 모니터링하면서 이상 탐지뿐 아니라, 자체적으로 방어 규칙을 조정하고 새로운 공격 패턴에 학습하며 대응 전략을 갱신하는 시스템으로 활용된다.​
    - IT 운영·업무 자동화에서는 인프라 병목을 스스로 찾아내고, 설정 변경·실험·롤백까지 포함한 전체 워크플로우를 거의 사람 개입 없이 수행하는 방향으로 발전하고 있다.​

출처
- https://modulabs.co.kr/blog/ai-agent-vs-agentic-ai
- https://www.f5.com/ko_kr/company/blog/ai-agents-vs-agentic-ai-understanding-the-difference
- https://www.solix.com/ko/blog/agentic-ai-and-generative-ai-understanding-the-difference/
- https://www.samsungsds.com/kr/insights/agentic-ai-the-autonomous-era-of-artificial-intelligence.html
- AI Agents vs. Agentic AI: A Conceptual taxonomy, applications and challenges 논문
