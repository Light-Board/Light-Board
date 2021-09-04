---
sort: 16
---

# 16th meeting log

## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/09/04

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

4. 알림
    - 스프링 / 카프카 / 몽고디비 구축 필요 
    - BE, 스프링 서버 내부에 kafka 패키지 생성 (global)
        1) 해당 패키지에 kafka lib import, object 및 send메서드 세팅
        2) send (produce) 가 필요한 BO에서 해당 obj @Autowired 가져와서 메서드 호출 
        3) 해당 페키지 내부에 consumer가 존재 / while true로 계속 컨슘만 하고 있음 / 쓰레드 Runable
        4) 해당 consumer process가 mongodb에 메시지 기반 내용을 계속 insert 함!! 
    - FE, 알림 컴포넌트에서 long polling 
        1) 해당 유저 ID 값 기반으로 알맞은 알림 정보를 계속해서 srping BE로  request 를 날림! 

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