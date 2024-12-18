# SpringBoot-Kafka

### Description

[EUREKA](https://github.com/0dyk/eureka) 프로젝트를 진행하며, 한 건의 결제에 대해 발생하는 연쇄적인 동작으로 인해 API 응답시간이 증가하는 문제가 있었습니다.

프로젝트 기간에는 @Async를 활용하여 비동기로 처리하였으나, 다양한 방법을 통해 API 응답시간을 테스트해보고자 합니다.

---
### Task
- MA(sync, async), MSA 성능 비교
- DB 최적화
- 장애 상황 테스트(MSA)
- 추가 작업 예정
### Stack
- Java 21
- SpringBoot 3.4.x
- kafka
---
### API 명세
<details>
  <summary>테스트용 API</summary>
프로젝트 목적에 맞게 API를 간소화하였습니다.

#### Request
```json
{
    "orderId": "BBBBB22222",
    "userCardId": 123,

    // 기존에 orderId로 조회하는 내용
    "userId": 123,
    "storeName": "스타벅스",
    "storeRegNo": "AAA111",
    "orderName": "아메리카노 1잔",
    "totalAmount": 10000,
    "vat": 100,
    "totalInstallCnt": 0,
    "requestedAt": "2024-12-31T23:59:00",
    "redirectUrl": "https://www.redirect.com",
    "largeCategoryId": 1,
    "smallCategoryId": 1,
    "recommendCardId": 123,
    "recommendDiscount": 1000
}
```
#### Logic
```
  0. 결제 승인 (생략)
  1. DB 작업
      - user_card table update
      - pay_history table insert
      - static table update/insert * 6
      - tag table update
```
#### Response
```json
{
    "code": 200,
    "message": "",
    "data": {}
}
```

</details>

<details>
  <summary>기존 결제 승인 API 로직</summary>

#### Request
```json
{
     "orderId": "BBBBB22222",
     "userCardId": "1"
}
```
#### Logic
```
0. 결제 정보 조회 및 삭제 (redis)
1. 사용자 카드 정보(결제) 조회
2. 카드사에 결제 승인 요청 (http)
3. 결제가 승인된 경우 DB 작업
    - user_card table update
    - pay_history table insert
    - static table update/insert * 6
    - tag table update
```
#### Response
```json
{
    "code": 200,
    "message": "",
    "data": {}
}
```
</details>

---
### Test
#### V1 - sync
- DB 작업을 순차적으로 처리

#### V2 - async
