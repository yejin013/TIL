# MSA
> MSA (MicroService Architecture)는 애플리케이션을 느슨하게 결합된 서비스의 모임으로 구조화하는 서비스 지향 아키텍처 스타일의 일종인 소프트웨어 개발 기법을 가진 아키텍처. 작고 독립적으로 배포 가능한 각각의 기능을 수행하는 서비스로 구성된 프레임워크

## 특징

- 애플리케이션을 조그마한 여러 서비스로 분리
- API를 통해서만 상호작용 가능
- 서비스의 end-point을 API 형태로 외부에 노출하고, 실질적인 세부 사항은 추상화

## 장점

- 모듈성 개선
- 애플리케이션의 이해, 개발, 테스트를 더 쉽게 함
- 애플리케이션 침식에 탄력적
- 병렬 개발 가능
- 서비스별 개별 배포로, 지속적 배포와 전개 가능
- 서비스의 교체 용이
- 특정 서비스에 대한 확장성 용이
    - 클라우드 사용에 적합한 아키텍처
- 부분적 장애 처리 용이

## 단점

- 통신의 장애와 서버의 부하 등 고려 필요
- 시스템 통신에 대한 고민 필요
- 통합 테스트의 불편함
- 성능 - 통신 비용이나 Latency 증가 (api 통신으로 인함)

참고자료 : [https://ko.wikipedia.org/wiki/마이크로서비스](https://ko.wikipedia.org/wiki/%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%84%9C%EB%B9%84%EC%8A%A4), [https://wooaoe.tistory.com/57](https://wooaoe.tistory.com/57), [http://clipsoft.co.kr/wp/blog/마이크로서비스-아키텍처msa-개념/](http://clipsoft.co.kr/wp/blog/%EB%A7%88%EC%9D%B4%ED%81%AC%EB%A1%9C%EC%84%9C%EB%B9%84%EC%8A%A4-%EC%95%84%ED%82%A4%ED%85%8D%EC%B2%98msa-%EA%B0%9C%EB%85%90/), [https://velog.io/@tedigom/MSA-제대로-이해하기-1-MSA의-기본-개념-3sk28yrv0e](https://velog.io/@tedigom/MSA-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%9D%B4%ED%95%B4%ED%95%98%EA%B8%B0-1-MSA%EC%9D%98-%EA%B8%B0%EB%B3%B8-%EA%B0%9C%EB%85%90-3sk28yrv0e)