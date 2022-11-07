# Cron
> cron이란, 유닉스 계열의 job scheduler.

## 특징 
- 작업이 고정된 시간, 날짜, 간격에 주기적으로 실행할 수 있도록 스케줄링 하기 위해 사용된다.
- 셸 명령어들이 주어진 일정에 주기적으로 실행하도록 규정해놓은 crontab 파일에 의해 구동된다.
- crontab 파일들은 잡 목록 및 cron 데몬에 대한 다른 명령들이 보관된 위치에 저장되어 있다.

## Cron 표현식 

7개의 각 필드로 구성되어 있다.

| 필드명 | 값의 허용 범위 | 허용된 특수 문자 |
|---|---|---|
|초 (Seconds)|0 ~ 59|, - * /|
|분 (Minutes)|0 ~ 59|, - * /|
|시 (Hours)|0 ~ 23|, - * /|
|일 (Day)|1 ~ 31|, - * ? / L W|
|월 (Month)|1 ~ 12 or JAN ~ DEC|, - * /|
|요일 (Week)|0 ~ 6 or SUN ~ SAT|, - * ? / L #|
|연도 (Year)|empty or 1970 ~ 2099|, - * /|

- Comma(,) : 특정한 값일 때만 동작 (월, 수, 금 등)
- Dash(-) : 범위 (2000-2010)
- Asterisk(*) : 항상, 모든 값
- Question Mark(?) : 특정한 값이 없음
- Slash(/) : 시작시간 / 단위 (0분부터 매 5분 = 0/5)
- L : 일에서 사용하면 마지막 일, 요일에서는 마지막 요일(토요일)
- W : 가장 가까운 평일 
- Number Sign(#) : 몇 째주의 무슨 요일 표현 (2번째주 수요일 = 3#2)

연도의 경우에는 아예 생략해도 된다. 아래의 실제 사용 예시를 보면 연도를 생략하고 6개 필드로 구성한 것을 볼 수 있다.

```java
// 나머지 import 생략
import org.springframework.scheduling.annotation.Scheduled;

@Component
public class BatchScheduler {
    @Autowired
    private JobLauncher jobLauncher;
    @Autowired
    private BatchConfig batchConfig;

    @Scheduled(cron = "0 0 0 * * *") // 매일 0시 정각 0초
    public void runJob() {
        // job parameter 설정
        Map<String, JobParameter> confMap = new HashMap<>();
        confMap.put("time", new JobParameter(System.currentTimeMillis()));
        JobParameters jobParameters = new JobParameters(confMap);

        try {
            jobLauncher.run(batchConfig.job(), jobParameters);
        } catch (JobExecutionAlreadyRunningException | JobInstanceAlreadyCompleteException
                | JobParametersInvalidException | JobRestartException e) {
            log.error("batch job error = {}", e.getMessage());
        }
    }
}
```
