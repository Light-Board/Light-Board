---
sort: 26
---

# 26th meeting log

## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2022/01/02


## 오늘 회의 토픽 요약

1. 카프카 활용 방안
    - 댓글 따봉 -> 추후에 등록할 예정
    ![24번째회의 첨부이미지2](https://raw.githubusercontent.com/Light-Board/Light-Board/master/assets/images/24th_meeting_2.png)

    a. FE에서 댓글 따봉 
    b. controller url:commnetPK 
    c. Service 여기서 "kafka msg produce"
    d. 내용: "thumbs", "{"user":"user_id", "comment":"comment_id"}"
    e. kafka msg sub - "thumbs" TOPIC을 구독하는 쉑 한테 들어옴
    f. 구독하는 쉑 -> row 찾아 -> thumbs_cnt + 1 -> logging -> update / **끝**
    g. FE에서 polling으로 댓글 컴포넌트 업데이트 -> 실시간 처럼 잘 보임
    => **race confidion** -> kafka만 통해서 업데이트를 하면 고려 대상이 아니다!
    
    - board_comment -> field thumbs_cnt 추가
    ```sql
    -- -----------------------------------------------------
    -- Table `light_board`.`board_comment`
    -- -----------------------------------------------------
    CREATE TABLE IF NOT EXISTS `light_board`.`board_comment` (
    `board_comment_id` BIGINT NOT NULL,
    `created_user_no` BIGINT NOT NULL,
    `created_at` TIMESTAMP NOT NULL,
    `updated_at` TIMESTAMP NULL DEFAULT NULL,
    `updated_user_no` BIGINT NULL DEFAULT NULL,
    `comment` TEXT NULL DEFAULT NULL,
    `board_id` BIGINT NOT NULL,
    `thumbs_cnt` BIGINT NOT NULL,
    PRIMARY KEY (`board_comment_id`))
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;
    ```

2. 주키퍼 + 카프카 + 스프링(부트) 세팅 [전세영 / 정현우 **완**]

3. 몽고디비 세팅 [정현우 **완**]


## 다음 회의 토픽

1. 우리 플젝 <-> Kafka(local server) ~ 연동
    - 이거는 우리 둘 설정이 다를 수 도 있으니까 각자 일단 해오기 ㅇㅋ?

2. DB schema update

3. 댓글 작업 바로 고고

4. FE 작업 첫삽 생각하기

---


## 후 우선순위 다음 회의 토픽 


1. 모든 API에 대한 response status code 핸들링
    - http://localhost:8080/v1/api/board-comment/1?sdf=&gdxdf=&sfdksdfd=?? 와 같은 형태로 쓸모 없는 query params에 대한 처리
    - not null 컬럼 500에러 헨들링
    - 400 에러 / 500 등의 status code 도 따라서 return
        - 해당 부분 vaildation 어떻게 처리할지 

2. ADMIN 페이지 dash board에서 보여줄 통계 정보
    - 어떤 정보? 어떤 API?

3. User authority 도메인 관리 table 추가 고려 
    - 커스텀 등급
    - admin이 직접 등급을 관리할 수 있게 (등급을 CRUD 가능하게)
    - boardFrame에서도 해당 커스텀 등급으로 access level 설정

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

- Kafka and mongodb / Kafka broker server setting / PRO-SUB modelAPI
- schema: 관계 설정
- Docker, Docker-compose
- mysql timestamp 9시간 차이 왜나노? 


## 숙제

- 없음 