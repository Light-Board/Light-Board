---
sort: 11
---

# 11th meeting log

## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/07/18

## 오늘 회의 토픽 요약

1. configue file
    - yaml 환경 구축하기

2. **DB user name / user password : [ local DB user configue ]**
    - user_name : light_board
    - user_password : lightboard1!
    - ip만 암호화 하자! => yaml, configue파일 암호화 방법과 기법

3. 하단 API 명시사항 체크 
    ```
    1. 회원가입 API 명세
        - 최초 어드민 존재하는지 여부 ~ FE단에서 chk
            1) request: .../isAdmin 
            2) user table -> count(*) -> length == 0

        - 최초 Admin 가입자 [ @세영 ]
            - 회원가입 완료
        - 일반 가입자
        - ID 중복 체크 [ @세영 ] [ O.K. ]
        - 이메일 인증 [ 뒤로 넘기고 ]


    2. 메인 페이지
        - default 랜딩 페이지
        - 랜딩 페이지 테이블 필요
        - 기업명 조회 API
        - 보드 프레임 조회 [ @현우 ]
        - 보드 목록 조회
        - 보드 컨텐츠 조회
        - 보드 컨텐츠 작성
        - 보드 컨텐츠 수
        - 보드 컨텐츠 삭제
    ```

4. 기타 정보 
    - 엄청 러프한 서비스에서 Data 접근 구도
        > service -> model : DAO (DTO) with [JPA] -> JDBC with DBCP2 -> DB

    - JAP spring configuration 한 step들 공유 + 정리

    - Apache Kafka configuration 부터 ~ 메시지 리턴~ [페이지체크](https://velog.io/@qlgks1/1장.-카프카-알아보기와-설치)


## 다음 회의 토픽

1. DB remote server 구축

2. schema 중간 체크 
    - 관계 설정 확실하게! -> table설명과 column목적 확실하게!

3. kafka 공부해오기!! 
    - 어디에 어떻게 적용시킬지 제대로! 

4. Junit 설정 생각하기!

5. 숙제 체크하기!!

## 미 해결 문제

- 도커라이징 잊지마러~
- Kafka핵심입니다~ / kafka 시작했음!!
- mongodb 세팅 해줘야 합니다~ / kafka로 'log' 연동에 초점 맞춰서 
- API 명세서 → swagger yam 알아오기

## 숙제

- ***미 해결 금액은 10,000 원 입니다.*** 
<br/>

- [ ] [공통] yaml, configue파일 암호화 방법과 기법
- [ ] JPA 환경 구축 한것 글 정리 해서 올려주세요 [ @세영 ]
- [ ] 최초 Admin 가입자 [ @세영 ]
- [x] ID 중복 체크 [ @세영 ]
- [ ] 보드 프레임 조회 [ @현우 ]
- [x] kafka 전용 repo [ @현우 ]
- [ ] remote DB server configue / mysql [ @현우 ]