---
sort: 21
---

# 21th meeting log

## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/10/16

## 오늘 회의 토픽 요약

0. ***알림 관련은 16번째 회의록 참고***

1. 유저 AUTH (인증) / spring security **[공동 진행 중]**
    - 보통 스프링에서는 어떻게 하나?
    - 필터 / 토큰 / 분석 
    - USER API에서 해당 부분 고도화 작업 필요 
    - 처음 : https://mangkyu.tistory.com/77
    - 공식
        - https://spring.io/guides/gs/securing-web/ 
        - https://spring.io/guides/topicals/spring-security-architecture
    - ***[restapi security]*** : https://sas-study.tistory.com/357

    ```sql
        -- -----------------------------------------------------
        -- Table `light_board`.`member`
        -- -----------------------------------------------------
        CREATE TABLE IF NOT EXISTS `light_board`.`member` (
        `id` BIGINT NOT NULL,
        `email` VARCHAR(40) NULL DEFAULT NULL,
        `password` VARCHAR(300) DEFAULT NULL,
        PRIMARY KEY (`id`))
        ENGINE = InnoDB
        DEFAULT CHARACTER SET = utf8;  
    ```

2. ㅇㅣ씨발것 시큐리티 사전 공부하기
    - 브랜치 푸시 -> 인가된 사용자 / 인가안된 사용자 filter 작동되게
    - 안하면 시팔 치킨 기프티콘 ㅇㅋ?

## 다음 회의 토픽

0. 모든 API에 대한 response status code 핸들링
    - http://localhost:8080/v1/api/board-comment/1?sdf=&gdxdf=&sfdksdfd=?? 와 같은 형태로 쓸모 없는 query params에 대한 처리
    - not null 컬럼 500에러 헨들링
    - 400 에러 / 500 등의 status code 도 따라서 return
        - 해당 부분 vaildation 어떻게 처리할지 

1. ADMIN 페이지 dash board에서 보여줄 통계 정보
    - 어떤 정보? 어떤 API?

2. User authority 도메인 관리 table 추가 고려 
    - 커스텀 등급
    - admin이 직접 등급을 관리할 수 있게 (등급을 CRUD 가능하게)
    - boardFrame에서도 해당 커스텀 등급으로 access level 설정

3. 알림 처리를 위한 kafka 세팅 시작 

4. toString 메서드 오버라이딩 해주기 / 디버깅 편하게 
    - 해더 모델에다가
    - 각 entity 모델에다가 

5. JPA 마이그레이션
    - Database schema create check!!
    - 우리가 만들어 둔 sql query 기반 OK!

6. CRUD Swagger 공통적으로 설정
    - abstract CRUD class에 공통 설명 / API 달아둠
    - https://github.com/Light-Board/light-board-back/issues/21 
    - 위 링크 주석에 swagger 관련 어노테이션 값 정리 
    - 공통부분 말고, 개인 추가 API 부분 정의

## 미 해결 문제

- schema: 관계 설정
- Docker, Docker-compose
- Kafka and mongodb / Kafka broker server setting / PRO-SUB modelAPI
- mysql timestamp 9시간 차이 왜나노? 

## 숙제

- [ ] 스프링 시큐리티 VELOG에 올리기 **[ @현우 @세영 ]**
    - [ ] 우리가 스프링 시큐리티를 어떻게 적용할까? 까지 생각해서 오기
    - [ ] restapi security 적용 로직 알아오기 **[ @현우 @세영 ]**