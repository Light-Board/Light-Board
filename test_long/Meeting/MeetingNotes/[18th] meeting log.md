---
sort: 18
---

# 18th meeting log

## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/09/24

## 오늘 회의 토픽 요약

0. ***알림 관련은 16번째 회의록 참고***

1. configuration API 수정 작업 - 세영 **[완]**

2. api 작업 계속 진행
    - 이전 작업 PR 모두 APPROVAL 해주기
    - [@세영] user API test 끝내기 / profile 작업
        - 이슈: User / AdminUser crud extends 같은 object 접근 -> 불가능
        - admin User Controller + User Controller 합치기
        - 권한의 경우 BO > if 처리에서 확인해서
    - [@현우] board 작업 **[완]**
    - user, profile table 통합 **[완]**
    ```sql
        -- -----------------------------------------------------
        -- Table `light_board`.`user`
        -- -----------------------------------------------------
        CREATE TABLE IF NOT EXISTS `light_board`.`user` (
        `user_no` BIGINT NOT NULL,
        `user_name` VARCHAR(100) NULL DEFAULT NULL,
        `user_id` VARCHAR(16) NOT NULL,
        `image_url` VARCHAR(300) NULL DEFAULT NULL,
        `password` VARCHAR(200) NOT NULL,
        `password_sort` VARCHAR(200) NOT NULL,
        `email` VARCHAR(100) NULL DEFAULT NULL,
        `authority` CHAR(1) NOT NULL,
        `created_at` TIMESTAMP NOT NULL,
        `updated_at` TIMESTAMP NULL DEFAULT NULL,
        `updated_user_no` BIGINT NULL DEFAULT NULL,
        `updated_detail` VARCHAR(100) NULL DEFAULT NULL,
        `status` CHAR(1) NOT NULL,
        `reported_cnt` TINYINT(1) NULL DEFAULT NULL,
        `user_interest` VARCHAR(200) NULL DEFAULT NULL,
        `question_answer` VARCHAR(100) NULL DEFAULT NULL,
        `question_type` TINYINT NOT NULL,
        PRIMARY KEY (`user_no`))
        ENGINE = InnoDB
        DEFAULT CHARACTER SET = utf8;  
    ```

3. 유저 AUTH (인증)
    - 보통 스프링에서는 어떻게 하나?
    - 필터 / 토큰 / 분석 
    - USER API에서 해당 부분 고도화 작업 필요 

## 다음 회의 토픽

1. toString 메서드 오버라이딩 해주기 / 디버깅 편하게 
    - 해더 모델에다가
    - 각 entity 모델에다가 

2. JPA 마이그레이션
    - Database schema create check!!
    - 우리가 만들어 둔 sql query 기반 OK!

3. 알림 처리를 위한 kafka 세팅 시작 

4. User authority 도메인 관리 table 추가 고려 
    - admin이 직접 등급을 관리할 수 있게 (등급을 CRUD 가능하게)

5. CRUD Swagger 공통적으로 설정
    - abstract CRUD class에 공통 설명 / API 달아둠
    - https://github.com/Light-Board/light-board-back/issues/21 
    - 위 링크 주석에 swagger 관련 어노테이션 값 정리 
    - 공통부분 말고, 개인 추가 API 부분 정의

## 미 해결 문제

- schema: 관계 설정
- Docker, Docker-compose
- Kafka and mongodb
- mysql timestamp 9시간 차이 왜나노? 

## 숙제

- [ ] comment API 완성 pull request [ @현우 ]
- [ ] user API 완성 + 테스트 pull request [ @세영 ]
- [ ] 스프링 시큐리티 VELOG에 올리기 [ @현우 @세영 ]