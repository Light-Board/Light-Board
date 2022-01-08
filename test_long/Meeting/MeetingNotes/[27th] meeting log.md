---
sort: 27
---

# 27th meeting log

## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2022/01/08


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

2. 우리플젝에 Kafka 세팅하기 [전세영]

3. DB있는 서버에 Kafka 세팅하기 with Docker **[정현우 완]**
    - IP CONFIG: 104.249.128.241 

4. 기존 board_comment 관련 API, 모델 및 트렌잭션 따봉값 수정 **[정현우 완]**

5. **[금 1돈 🐝. 채용 리스트 (Jobs_Scraper) 컨테스트](https://nomadcoders.co/community/thread/1622)** => 금 한돈 결정


## 다음 회의 토픽

1. Kafka sub 비즈니스 로직

2. FE 작업 첫삽 생각하기

3. 금 1돈 플젝, 수욜까지 결정
    - 개발 과정 (어려웠던 점과 해결방법) 컨셉을 잡고가는게 좋음


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