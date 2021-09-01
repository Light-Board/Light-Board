---
sort: 16
---

# 16th meeting log

## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/08/31

## 오늘 회의 토픽 요약

1. CRUD interface + Controller CRUD Abstract class 
    - readAll 버리고 페이지 네이션 도입, 추상화 도입 고고 

2. coding issue
    - Header 부분 -> BO에서 response method
    - Header 부분에서 result code 
    - Enum: 코드로 그냥 체크하는 걸로 갑시다. 
        - 필요할 때 엑셀이던 어디던 정리하는 걸로 
    - Json data type
        - JPA entitiy
        - 그 외 object에서,, 

3. 하단 API 명시사항 체크
    - 1. 회원가입 API + 명세
    - 2. 보드프레임 API + 명세

4. 알림 부분 먼저 하자
    - 어떻게 카프카를 도입해서 구축을 할 것 인가?

5. 피그마 프로젝트 생성
    - FE 뷰 단위, 가벼운 디자인 부터 시작

## 다음 회의 토픽

1. JPA 마이그레이션
    - Database schema create check!!
    - 우리가 만들어 둔 sql query 기반 OK!

2. "board_frame / user 1차 완성 API 부분"  
    - swagger 정리해서 올리기

3. 글 정리 UP-LOAD
    - JPA 환경 구축 한것 글 정리 해서 올려주세요.
    - Kafka 구축 글 정리해서 올려주세요.
    - **서로 글정리한거 코멘트 달기 ^^**
        - 궁금한 것 적어도 하나 만들어오기

4. 유저 AUTH (인증)
    - 보통 스프링에서는 어떻게 하나? 

## 미 해결 문제

- schema: 관계 설정
- Docker, Docker-compose
- Kafka and mongodb

## 숙제

- [ ] [공통] 서로 글정리한거 코멘트 -> 궁금한 것 적어도 하나 만들어오기
- [ ] 현우 / 세영 API 1차 완성 pull request [ @현우 @세영 ]
- [x] 현우가 올린 것 기반으로 HEADER 수정 [ @세영 ]