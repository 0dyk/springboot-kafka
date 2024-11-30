# SpringBoot-Kafka

### Description

[EUREKA](https://github.com/0dyk/eureka) 프로젝트를 진행하며, 한 건의 결제에 대해 발생하는 연쇄적인 동작으로 인해 API 응답시간이 증가하는 문제가 있었습니다.

프로젝트 기간에는 @Async를 활용하여 비동기로 처리하였으나, 트래픽 증가 및 서비스 확장성을 고려하여 메시지 큐(kafka) 도입을 위해 프로토타입을 제작하고자 합니다.

### Task
- MA(sync, async), MSA 성능 비교
- MSA 환경에서 장애 상황 테스트
- 추가 작업 예정

### Stack
- Java 21
- SpringBoot 3.4.x
- kafka
