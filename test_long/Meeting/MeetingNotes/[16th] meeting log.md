---
sort: 16
---

# 16th meeting log

## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/08/29

## 오늘 회의 토픽 요약

1. CRUD interface + Controller CRUD Abstract class 
    - 추상화 도입
    - 추상화에 대한 고찰

2. coding issue
    - Header 부분 -> BO에서 response method
    - Header 부분에서 result code 
    - Enum
    - Json data type
        - JPA entitiy
        - 그 외 object에서,, 

3. 하단 API 명시사항 체크
    - 1. 회원가입 API + 명세
    - 2. 보드프레임 API + 명세

4. API 추가 해야 할 부분에 대해서?

5. 페이지 네이션! 

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

4. 추상화에 대해 체크

## 미 해결 문제

- schema 중간 체크 
    - 관계 설정 확실하게! -> table설명과 column목적 확실하게!
- 도커라이징 잊지마러~
- Kafka핵심입니다~ / kafka 시작했음!!
- mongodb 세팅 해줘야 합니다~ / kafka로 'log' 연동에 초점 맞춰서 

## 숙제

- [ ] [공통] 서로 글정리한거 코멘트 -> 궁금한 것 적어도 하나 만들어오기
- [ ] 현우 / 세영 API 1차 완성 pull request [ @현우 @세영 ]
- [x] 현우가 올린 것 기반으로 HEADER 수정 [ @세영 ]