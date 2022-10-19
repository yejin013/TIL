# Spring Batch

## Batch Application

- 데이터를 실시간으로 처리하는 것이 아닌 일괄적으로 모아서 처리하는 것
- 사용자에게 빠른 응답이 필요하지 않은 서비스에 적용한다.
- 작업을 실행한 특정 시간 이후에는 자원을 거의 소비하지 않는다.
- 대량의 데이터를 특정 시간에 처리한다.

## Spring Batch

- 엔터프라이즈 시스템의 운영에 있어 대용량 일괄처리의 편의를 위해 설계된 가볍고 포괄적인 배치 프레임워크
    
    
- 로깅/추적, 트랜잭션 관리, job 프로세싱 통계, job 재시작, 스킵, 리소스 관리 같은 대용량 데이터 처리에 필수적인 기능을 재사용할 수 있는 형태로 제공한다.
- Spring Batch는 Batch Job을 관리하지만 Job을 구동하거나 실행시키는 기능은 지원하고 있지 않다. 스케줄러와 함께 사용해야한다.
- 최적화 및 파티셔닝 기술을 통해 대용량 및 고성능 일괄 작업을 가능하게 하는 고급 기술 서비스 및 기능을 제공한다.
- 사용 예시
    1. 대용량의 비즈니스 데이터를 복잡한 작업으로 처리해야하는 경우
    2. 특정한 시점에 스케쥴러를 통해 자동화된 작업이 필요한 경우 (ex. 푸시알림, 월 별 리포트)
    3. 대용량 데이터의 포맷을 변경, 유효성 검사 등의 작업을 트랜잭션 안에서 처리 후 기록해야하는 경우

## Spring Batch Architecture

<img src="https://user-images.githubusercontent.com/45252618/196648764-66af5002-3a05-40e7-bf26-576303c3e4a8.png">

- Application
    - 스프링 배치를 사용하는 개발자가 만드는 모든 배치 job과 커스텀 코드
- Batch Core
    - job을 실행하고 제어하는데 필요한 핵심 런타임 클래스
    - JobLauncher, Job, Step 구현체
- Batch Infrastructure
    - 개발자와 Application에서 사용하는 Reader와 Writer, RetryTemplate 등의 서비스

### Spring Batch 용어

<img src="https://user-images.githubusercontent.com/45252618/196648851-076d93b3-0a67-4b92-9abf-1eaf5d926b5f.png">

- **JobRepository**
    - 다양한 배치 수행과 관련된 수치 데이터와 잡의 상태를 유지 및 관리
    - 주로 관계형 데이터베이스를 사용하며 스프링 배치 내의 대부분의 주요 컴포넌트 공유
    - 실행된 Step, 현재 상태, 읽은 아이템 및 처리된 아이템 수 등 JobRepository에 저장
    - Job이 실행되게 되면 JobRepository에 JobExecution과 StepExecution을 생성하게 되며 JobRepository에서 Execution 정보들을 저장하고 조회하며 사용하게 된다.
- **JobLauncher**
    - Job과 JobParameters를 사용하여 Job을 실행하는 객체
    - Job.execute을 호출하는 역할
    - Job의 재실행 가능 여부 검증, 잡의 실행 방법, 파라미터 유효성 검증 등 수행
- **Job**
    - 배치처리 과정을 하나의 단위로 만들어 놓은 객체
    - 배치처리 과정에 있어 전체 계층 최상단에 위치
- **JobInstance**
    - Job의 실행의 단위
    - Job을 실행시키게 되면 하나의 JobInstance 생성
    - 예를들어 1월 1일 실행, 1월 2일 실행을 하게 되면 각각의 JobInstance가 생성되며 1월 1일 실행한 JobInstance가 실패하여 다시 실행을 시키더라도 이 JobInstance는 1월 1일에 대한 데이터만 처리한다.
- **JobParameters**
    - JobInstance 구별
    - 개발자 JobInstacne에 전달되는 매개변수 역할
    - String, Double, Long, Date 4가지 형식만을 지원
- **JobExecution**
    - JobInstance에 대한 실행 시도에 대한 객체
    - 1월 1일에 실행한 JobInstacne가 실패하여 재실행을 하여도 동일한 JobInstance를 실행시키지만 이 2번에 실행에 대한 JobExecution은 개별로 생긴다.
    - JobInstance 실행에 대한 상태, 시작시간,  종료시간, 생성시간 등의 정보를 담고 있다.
- **Step**
    - Job의 배치처리를 정의하고 순차적인 단계 캡슐화
    - Job은 최소한 1개 이상의 Step을 가져야 한다.
    - Job의 실제 일괄 처리를 제어하는 모든 정보가 들어있다.
- **StepExecution**
    - JobExecution과 동일하게 Step 실행 시도에 대한 객체
    - Job이 여러개의 Step으로 구성되어 있을 경우 이전 단계의 Step이 실패하게 되면 다음 단계가 실행되지 않음으로 실패 이후 StepExecution은 생성되지 않는다.
    - 실제 시작이 될 때만 생성된다.
    - JobExecution에 저장되는 정보 외에 read 수, write 수, commit 수, skip 수 등의 정보들도 저장이 됩니다.
- **ExecutionContext**
    - ExecutionContext란 Job에서 데이터를 공유 할 수 있는 데이터 저장소입니다. Spring Batch에서 제공하느 ExecutionContext는 JobExecutionContext, StepExecutionContext 2가지 종류가 있으나 이 두가지는 지정되는 범위가 다릅니다. JobExecutionContext의 경우 Commit 시점에 저장되는 반면 StepExecutionContext는 실행 사이에 저장이 되게 됩니다. ExecutionContext를 통해 Step간 Data 공유가 가능하며 Job 실패시 ExecutionContext를 통한 마지막 실행 값을 재구성 할 수 있습니다.
- **ItemReader**
    - ItemReader는 Step에서 Item을 읽어오는 인터페이스입니다. ItemReader에 대한 다양한 인터페이스가 존재하며 다양한 방법으로 Item을 읽어 올 수 있습니다.
- **ItemWriter**
    - ItemWriter는 처리 된 Data를 Writer 할 때 사용한다. Writer는 처리 결과물에 따라 Insert가 될 수도 Update가 될 수도 Queue를 사용한다면 Send가 될 수도 있다. Writer 또한 Read와 동일하게 다양한 인터페이스가 존재한다. Writer는 기본적으로 Item을 Chunk로 묶어 처리하고 있습니다.
- **ItemProcessor**
    - Item Processor는 Reader에서 읽어온 Item을 데이터를 처리하는 역할을 하고 있다. Processor는 배치를 처리하는데 필수 요소는 아니며 Reader, Writer, Processor 처리를 분리하여 각각의 역할을 명확하게 구분하고 있습니다.
    

## 실제 적용 예시 (ODDOK 프로젝트)

```java
@Configuration
@EnableBatchProcessing
public class BatchConfig {
    @Autowired
    public JobBuilderFactory jobBuilderFactory;

    @Bean
    public Job job() {
        return jobBuilderFactory.get("job").start(step()).build();
    }

}
```

참고자료 : [https://godekdls.github.io/Spring Batch/introduction/](https://godekdls.github.io/Spring%20Batch/introduction/), [https://devbksheen.tistory.com/m/284](https://devbksheen.tistory.com/m/284)
