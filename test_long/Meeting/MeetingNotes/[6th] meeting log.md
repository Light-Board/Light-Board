# 6th meeting log
## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/04/28
## 오늘 회의 토픽 요약

- Myslq workbench 고고

```sql
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '12345678';

DROP DATABASE IF EXISTS  light_board;
DROP USER IF EXISTS  light_board@localhost;
create user light_board@localhost identified WITH mysql_native_password  by 'light_board';
create database light_board;
grant all privileges on light_board.* to light_board@localhost with grant option;
commit;

-- MySQL Workbench Forward Engineering

SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0;
SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0;
SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_ENGINE_SUBSTITUTION';

-- -----------------------------------------------------
-- Schema mydb
-- -----------------------------------------------------
-- -----------------------------------------------------
-- Schema light_board
-- -----------------------------------------------------

-- -----------------------------------------------------
-- Schema light_board
-- -----------------------------------------------------
CREATE SCHEMA IF NOT EXISTS `light_board` DEFAULT CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci ;
USE `light_board` ;

-- -----------------------------------------------------
-- Table `light_board`.`board_frame`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `light_board`.`board_frame` (
  `board_frame_id` INT NOT NULL,
  `board_frame_name` VARCHAR(100) NOT NULL,
  `child_board_frame_id` VARCHAR(300) NULL DEFAULT NULL,
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
  `board_no` BIGINT NOT NULL AUTO_INCREMENT,
  `board_id` INT NOT NULL,
  `board_title` VARCHAR(100) NOT NULL,
  `board_description` TEXT NOT NULL,
  `board_hits` INT NULL DEFAULT NULL,
  `created_at` TIMESTAMP NOT NULL,
  `created_user_no` BIGINT NOT NULL,
  `updated_at` TIMESTAMP NULL DEFAULT NULL,
  `updated_user_no` BIGINT NULL DEFAULT NULL,
  `tags` VARCHAR(300) NULL DEFAULT NULL,
  `borad_frame_id` INT NOT NULL,
  PRIMARY KEY (`board_id`),
  UNIQUE INDEX `board_no` (`board_no` ASC) VISIBLE,
  INDEX `fk_board_board_frame1_idx` (`borad_frame_id` ASC) VISIBLE,
  CONSTRAINT `fk_board_board_frame1`
    FOREIGN KEY (`borad_frame_id`)
    REFERENCES `light_board`.`board_frame` (`board_frame_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

-- -----------------------------------------------------
-- Table `light_board`.`board_comment`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `light_board`.`board_comment` (
  `created_user_no` BIGINT NOT NULL,
  `created_at` TIMESTAMP NOT NULL,
  `updated_at` TIMESTAMP NULL DEFAULT NULL,
  `updated_user_no` BIGINT NULL DEFAULT NULL,
  `comment` TEXT NULL DEFAULT NULL,
  `board_id` INT NOT NULL,
  PRIMARY KEY (`board_id`),
  INDEX `fk_board_comment_board1_idx` (`board_id` ASC) VISIBLE,
  CONSTRAINT `fk_board_comment_board1`
    FOREIGN KEY (`board_id`)
    REFERENCES `light_board`.`board` (`board_id`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

-- -----------------------------------------------------
-- Table `light_board`.`configuration`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `light_board`.`configuration` (
  `service_status` CHAR(1) NOT NULL,
  `service_name` VARCHAR(100) NOT NULL,
  `fabicon` VARCHAR(300) NULL DEFAULT NULL,
  `updated_at` TIMESTAMP NULL DEFAULT NULL,
  `updated_user_no` BIGINT NULL DEFAULT NULL)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

-- -----------------------------------------------------
-- Table `light_board`.`image`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `light_board`.`image` (
  `image_no` BIGINT NOT NULL AUTO_INCREMENT,
  `image_type` CHAR(1) NULL DEFAULT NULL,
  `image` LONGBLOB NULL DEFAULT NULL,
  PRIMARY KEY (`image_no`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

-- -----------------------------------------------------
-- Table `light_board`.`user_question`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `light_board`.`user_question` (
  `question_type` TINYINT NOT NULL AUTO_INCREMENT,
  `description` VARCHAR(200) NOT NULL,
  PRIMARY KEY (`question_type`))
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

-- -----------------------------------------------------
-- Table `light_board`.`profile`
-- -----------------------------------------------------
CREATE TABLE IF NOT EXISTS `light_board`.`profile` (
  `user_no` BIGINT NOT NULL AUTO_INCREMENT,
  `user_name` VARCHAR(100) NULL DEFAULT NULL,
  `image_url` VARCHAR(300) NULL DEFAULT NULL,
  `user_interest` VARCHAR(200) NULL DEFAULT NULL,
  `question_answer` VARCHAR(100) NULL DEFAULT NULL,
  `question_type` TINYINT NOT NULL,
  PRIMARY KEY (`user_no`),
  INDEX `fk_profile_user_question1_idx` (`question_type` ASC) VISIBLE,
  CONSTRAINT `fk_profile_user_question1`
    FOREIGN KEY (`question_type`)
    REFERENCES `light_board`.`user_question` (`question_type`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
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
  PRIMARY KEY (`user_no`),
  INDEX `fk_user_profile_idx` (`profile_user_no` ASC) VISIBLE,
  CONSTRAINT `fk_user_profile`
    FOREIGN KEY (`profile_user_no`)
    REFERENCES `light_board`.`profile` (`user_no`)
    ON DELETE NO ACTION
    ON UPDATE NO ACTION)
ENGINE = InnoDB
DEFAULT CHARACTER SET = utf8;

SET SQL_MODE=@OLD_SQL_MODE;
SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS;
SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS;
```

![](/../../assets/images/6th_meeting_1)

# 다음 회의 토픽

- 환경 세팅
    - [x]  local mysql
    - [ ]  spring setting 시작? ⇒ "*RESTFUL API*"
        - [ ]  project 구조!
            - [ ]  MVC pattern
            - [ ]  file directory
        - [ ]  DB connection
        - [ ]  zero to full
        - [ ]  project 생성
        - [ ]  configuration!?
    - [ ]  Aphace Kafka setting

    setting 후 → end point ~ rest API 정리 부터 시작해봅시다! 

    - [ ]  git hub ~ branch
    - [ ]  react
    - [ ]  .env →환경 변수 / DB base user ~ ip ~
    - [ ]  DevOps (분산에 대한?) → 제일 나중에! 제일 마지막에!
        - [ ]  Docker 적용 시켜보자고!
    - [x]  server? not yet! just local!

# 미 해결 문제

- 세부 기능 명세서
- 도커라이징 잊지마러~
- Kafka핵심입니다~
- mongodb 세팅 해줘야 합니다~

# 숙제

- [ ]  [공통] spring - gradle -docker
- [ ]  [공통] spring MVC 패턴 프로젝트 생성할때 설정해줘야 하는 부분
    - [ ]  왜? → 어떤 부분을 위해서?
- [ ]  [공통] API 명세서 → swagger yam 알아오기
- [ ]  [세영] mongodb 설치
    - [ ]  create user !
    - [ ]  테스트 러닝까지!