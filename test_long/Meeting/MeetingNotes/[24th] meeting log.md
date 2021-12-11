---
sort: 24
---

# 24th meeting log

## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/12/11


## 오늘 회의 토픽 요약

1. 카프카 활용 방안
    - 자기 쓴 글에 대한 댓글 알람
    ![24번째회의 첨부이미지1](https://raw.githubusercontent.com/Light-Board/Light-Board/master/assets/images/24th_meeting_1.png)

    - 댓글 따봉 -> 추후에 등록할 예정
    ![24번째회의 첨부이미지2](https://raw.githubusercontent.com/Light-Board/Light-Board/master/assets/images/24th_meeting_2.png)

    - 알람은 몽고 DB (nosql) 사용하기로! collection concept

2. 그럼 컨슈머를 while true로 해야하나
    - https://freedeveloper.tistory.com/353 

3. 컨슈머 세팅 모범 사례
    - https://team-platform.tistory.com/37 

4. 로컬 카프카 브로커 서버 구축 해보기
    - 도커 이미지 기반, 카프카 브로커 서버 러닝
    - 우리 기존 spring에 컨슈머 프로듀서 세팅


## 다음 회의 토픽

1. 로컬 카프카 기반으로 우리 스프링 프로젝트에서 어떻게 프로듀서, 컨슈머 세팅을 해야 할 지

2. 토픽과 메시지 정리하기

3. mongodb setting 하기
    - 알람 table(collection) 만들기


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

- [x] 알림 kafka 활용 방안에 5분 PT 준비, ppt 1장
- [ ] 로컬 카프카 세팅 해보기 [전세영]
- [ ] mongodb setting 하기 [정현우]