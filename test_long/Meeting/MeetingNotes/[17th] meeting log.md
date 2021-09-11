---
sort: 17
---

# 17th meeting log

## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/09/11

## 오늘 회의 토픽 요약

0. ***알림 관련은 16번째 회의록 참고***

1. CRUD Swagger 공통적으로 설정
    - abstract CRUD class에 공통 설명 / API 달아둠
    - https://github.com/Light-Board/light-board-back/issues/21 
    - 위 링크 주석에 swagger 관련 어노테이션 값 정리 

2. Pagination 작업 및 Header 모델 수정 - 현우
    - 지금 상속된 형태 때문에 reqeust 파라미터값을 넣어줘야함 
    - default 값은 size 20

3. user/admin 분리 - 세영
    - 분리 완료

4. configuration API 작업 - 세영

## 다음 회의 토픽

0. api 작업 계속 진행
    - 이전 작업 PR 모두 APPROVAL 해주기
    - [@세영] user API test 끝내기 / profile 작업
    - [@현우] board 작업

1. swagger 정리
    - 공통부분 말고, 개인 추가 API 부분 정의

2. toString 메서드 오버라이딩 해주기 / 디버깅 편하게 
    - 해더 모델에다가
    - 각 entity 모델에다가 

3. mysql timestamp 9시간 차이 왜나노? 

4. 유저 AUTH (인증)
    - 보통 스프링에서는 어떻게 하나? 

5. JPA 마이그레이션
    - Database schema create check!!
    - 우리가 만들어 둔 sql query 기반 OK!

## 미 해결 문제

- schema: 관계 설정
- Docker, Docker-compose
- Kafka and mongodb

## 숙제

- [ ] 현우 / 세영 API 1차 완성 pull request [ @현우 @세영 ]
- [ ] 스프링 시큐리티 VELOG에 올리기 [ @현우 @세영 ]