# MCP (Model Context Protocol)
> MCP란, AI 애플리케이션 및 에이전트가 데이터 소스(예: 로컬 파일, 데이터베이스 또는 콘텐츠 저장소) 및 도구(예: GitHub, Google Maps, Puppeteer)에 연결하고 함께 작업하는 표준 방식

<img width="1406" height="791" alt="image" src="https://github.com/user-attachments/assets/7f388e91-34f4-451a-b80d-d4629f8cecdd" />

<img width="1282" height="970" alt="image" src="https://github.com/user-attachments/assets/edd3e852-9f2f-48a4-983e-132ce799f022" />

## 개념
- LLM(대규모 언어 모델) 애플리케이션과 통합 간의 원활한 통신
- MCP는 AI 애플리케이션을 위한 범용 어댑터와 유사
  * 예)  USB-C가 물리적 장치를 다양한 주변 장치 및 액세서리에 연결하기 위한 범용 어댑터 역할을 하는 것처럼, MCP는 AI 모델을 다양한 데이터 소스 및 도구에 연결하는 표준화된 방법 제공

## 아키텍처
<img width="1302" height="1086" alt="image" src="https://github.com/user-attachments/assets/2b4fb3bc-7910-407e-9985-2e90bff5df03" />

- 클라이언트-호스트-서버 아키텍처
- 클라이언트(Clients): 호스트 애플리케이션 내에서 실행되며, 각 서버와 1:1 연결 유지 (프로토콜 협상, 기능 교환, 메시지 라우팅을 처리하고 서버 간 보안 경계 유지)
- 서버(Servers): 특정 기능(예: 수학 연산, 날씨 정보 조회)을 노출하는 경량 프로그램

1. MCP 호스트 (Host)
- LLM 기반 애플리케이션 (예: Claude Desktop, IDE)
- 여러 MCP 서버와 동시 연결 가능
- 사용자 인터페이스 제공
- 보안 및 권한 관리

2. MCP 클라이언트 (Client)
- 호스트 애플리케이션 내 프로토콜 구현체
- 서버와 1:1 연결 유지
- 메시지 직렬화/역직렬화 처리
- 상태 관리 및 에러 핸들링

3. MCP 서버 (Server)
- 특정 기능이나 리소스 제공
- JSON-RPC 기반 API 구현
- 보안 및 접근 제어 관리
- 상태 및 리소스 관리

## MCP의 3가지 핵심 요소(Primitive)
- 리소스(Resources): 클라이언트가 읽을 수 있는 파일과 같은 데이터 (예: API 응답 또는 파일 내용)
  * "애플리케이션 제어(application-controlled)"로, 클라이언트 애플리케이션이 어떻게, 언제 사용할지 결정 
- 도구(Tools): LLM이 호출할 수 있는 함수로, 외부 시스템과 상호 작용하고 계산을 수행하며 실제 세계에서 작업 수행
- 프롬프트(Prompts): LLM 상호 작용을 돕기 위해 서버가 정의하는 재사용 가능한 템플릿

## 장점
- 표준화된 통합 방식: AI 애플리케이션이 다양한 데이터 소스(예: 로컬 파일, 데이터베이스, 콘텐츠 저장소) 및 도구(예: GitHub, Google Maps, Puppeteer)에 연결하고 함께 작업하는 표준화된 방법 제공
- 개발 시간 및 복잡성 감소
- AI 애플리케이션의 유용성 증대
- 유연성 및 상호 운용성
- 데이터 및 기능에 대한 구조화된 접근
- 복잡한 워크플로우 및 에이전트 지원
- 다양한 전송(Transport) 지원

## 통신 메커니즘 (Transports)
- stdio 전송(Standard Input/Output): 표준 입력/출력을 사용하여 통신. 로컬 프로세스에 주로 사용
- 스트림 가능 HTTP(Streamable HTTP): HTTP POST 및 GET 요청을 사용하며, 서버가 여러 클라이언트 연결 처리

## MCP 서버 실행 방식
- 직접 실행
- 기존 ASGI 서버에 마운트 (Streamable HTTP)
- Claude Desktop 앱 통합

### MCP 서버와 도구
- MCP 서버: 이 언어를 사용하여 다양한 데이터와 기능을 제공하는 '전문가' 역할
- 도구: LLM이 이러한 전문가에게 '지시'를 내려 실제 작업을 수행하도록 하는 '행동 지침' 또는 '명령어 세트'

<br/>
<br/>
<br/>
출처

- https://modelcontextprotocol.io/llms-full.txt
- https://wikidocs.net/268795
