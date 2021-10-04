---
sort: 14
---

# 14th meeting log

## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/08/09

## 오늘 회의 토픽 요약

0. 사다리 타기로 CrudInterface 적용하기 선정 되었습니다 ^^

1. configue file : yml 대신 -> config.properties

2. 각 table ID value 
    - ***PK는 무조건 모두 UUID 12자리 / BIGINT***
    - ***FK는 명시만 해놓고 실제 관계 설정은 X / 무조건 모두 UUID 12자리 / BIGINT***
    - user : 사용자 개인 정보 담고 있는 table
    - profile : 사용자 공개 정보 담고 있는 table
        - user 1 : 1 profile
    - user_question : 사용자 회원가입시 비밀번호 찾기 질문
        - profile 1 : 1 user_question
    - board_frame : 게시판 템플릿
    - board : 게시판 글, 컨텐츠
        - board_frame 1 : N board
    - board_comment : 게시판 글에 해당하는 댓글
        - board 1 : N board_comment
    - configuration : 최초 admin (root)이 project init할때 처음으로 설정되는 값
        - 초기값은 임의로 마이그레이션을 해줘야 한다

    ```sql
    -- -----------------------------------------------------
    -- Table `light_board`.`board_frame`
    -- -----------------------------------------------------
    CREATE TABLE IF NOT EXISTS `light_board`.`board_frame` (
    `board_frame_id` BIGINT NOT NULL,
    `board_frame_name` VARCHAR(100) NOT NULL,
    `child_board_frame_id` BIGINT NULL DEFAULT NULL, # TO DO JSON
    `access_level` CHAR(1) NULL DEFAULT NULL,
    `created_at` TIMESTAMP NOT NULL,
    `created_user_no` BIGINT NOT NULL,
    `updated_at` TIMESTAMP NULL DEFAULT NULL,
    `updated_user_no` BIGINT NULL DEFAULT NULL,
    PRIMARY KEY (`board_frame_id`))
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;

    -- -----------------------------------------------------
    -- Table `light_board`.`board`
    -- -----------------------------------------------------
    CREATE TABLE IF NOT EXISTS `light_board`.`board` (
    `board_id` BIGINT NOT NULL,
    `board_title` VARCHAR(100) NOT NULL,
    `board_description` TEXT NOT NULL,
    `board_hits` INT NULL DEFAULT NULL,
    `created_at` TIMESTAMP NOT NULL,
    `created_user_no` BIGINT NOT NULL,
    `updated_at` TIMESTAMP NULL DEFAULT NULL,
    `updated_user_no` BIGINT NULL DEFAULT NULL,
    `tags` VARCHAR(300) NULL DEFAULT NULL, # JSON 
    `board_frame_id` BIGINT NOT NULL,
    PRIMARY KEY (`board_id`))
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;

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
    PRIMARY KEY (`board_comment_id`))
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;

    -- -----------------------------------------------------
    -- Table `light_board`.`configuration`
    -- -----------------------------------------------------
    CREATE TABLE IF NOT EXISTS `light_board`.`configuration` (
    `configuration_id` BIGINT NOT NULL auto_increment,
    `service_status` CHAR(1) NOT NULL,
    `service_name` VARCHAR(100) NOT NULL,
    `fabicon` VARCHAR(300) NULL DEFAULT NULL,
    `updated_at` TIMESTAMP NULL DEFAULT NULL,
    `updated_user_no` BIGINT NULL DEFAULT NULL,
    PRIMARY KEY (`configuration_id`))
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;

    -- -----------------------------------------------------
    -- Table `light_board`.`user_question`
    -- -----------------------------------------------------
    # TINYINT 0~255
    CREATE TABLE IF NOT EXISTS `light_board`.`user_question` (
    `question_type` TINYINT NOT NULL AUTO_INCREMENT, 
    `description` VARCHAR(200) NOT NULL,
    PRIMARY KEY (`question_type`))
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;

    -- -----------------------------------------------------
    -- Table `light_board`.`profile`
    -- [2021.10.04] profile 확실하게 더 이상 안씀
    -- -----------------------------------------------------
    CREATE TABLE IF NOT EXISTS `light_board`.`profile` (
    `profile_user_no` BIGINT NOT NULL AUTO_INCREMENT,
    `user_name` VARCHAR(100) NULL DEFAULT NULL,
    `image_url` VARCHAR(300) NULL DEFAULT NULL,
    `user_interest` VARCHAR(200) NULL DEFAULT NULL,
    `question_answer` VARCHAR(100) NULL DEFAULT NULL,
    `question_type` TINYINT NOT NULL,
    PRIMARY KEY (`profile_user_no`))
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;

    -- -----------------------------------------------------
    -- Table `light_board`.`user`
    -- -----------------------------------------------------
    CREATE TABLE IF NOT EXISTS `light_board`.`user` (
    `user_no` BIGINT NOT NULL,
    `user_id` VARCHAR(16) NOT NULL,
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
    `profile_user_no` BIGINT NOT NULL,
    PRIMARY KEY (`user_no`))
    ENGINE = InnoDB
    DEFAULT CHARACTER SET = utf8;    
    ```

3. JPA 마이그레이션 / DB 생성 + USER , Table 생성
    - 공부해오기 -> 글을 써오자는 말입니다 선생님

4. 하단 API 명시사항 체크 (기본적인 CRUD는 1차완성 목표입니다)
    - 1. 회원가입 API + 명세
    - 2. 보드프레임 API + 명세

![14번째회의 첨부이미지1](https://raw.githubusercontent.com/Light-Board/Light-Board/master/assets/images/14th_meeting_1.png)
## 다음 회의 토픽

1. JPA 마이그레이션 방향 잡기

2. "board_frame / user 1차 완성 API 부분"  
    - swagger 정리해서 올리기

3. 글 정리 UP-LOAD
    - JPA 환경 구축 한것 글 정리 해서 올려주세요.
    - Kafka 구축 글 정리해서 올려주세요.
    - **서로 글정리한거 코멘트 달기 ^^**
        - 궁금한 것 적어도 하나 만들어오기
## 미 해결 문제

- schema 중간 체크 
    - 관계 설정 확실하게! -> table설명과 column목적 확실하게!
- 도커라이징 잊지마러~
- Kafka핵심입니다~ / kafka 시작했음!!
- mongodb 세팅 해줘야 합니다~ / kafka로 'log' 연동에 초점 맞춰서 

## 숙제

- [ ] [공통] JPA 마이그레이션 글 정리해서 쓰기 [ @현우 @세영 ]
- [ ] [공통] 서로 글정리한거 코멘트 -> 궁금한 것 적어도 하나 만들어오기
- [ ] 현우 / 세영 API 1차 완성 pull request [ @현우 @세영 ]
- [x] 현우가 올린 것 기반으로 HEADER 수정 [ @세영 ]