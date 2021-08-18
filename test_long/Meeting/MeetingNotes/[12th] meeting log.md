---
sort: 12
---

# 12th meeting log

## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/07/25

## 오늘 회의 토픽 요약

1. DB remote server 구축
    - db connection test
    - user_name : light_board
    - user_password : lightboard123!#

2. configue file
    - yaml 환경 구축하기
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


## 다음 회의 토픽

1. Junit 설정, controller setting!

2. configue file
    - yaml 환경 구축하기
    - ip만 암호화 하자! => yaml, configue파일 암호화 방법과 기법

3. JPA 환경 구축 한것 글 정리 해서 올려주세요.

4. Kafka 구축 글 정리해서 올려주세요.

## 미 해결 문제

- schema 중간 체크 
    - 관계 설정 확실하게! -> table설명과 column목적 확실하게!
- 도커라이징 잊지마러~
- Kafka핵심입니다~ / kafka 시작했음!!
- mongodb 세팅 해줘야 합니다~ / kafka로 'log' 연동에 초점 맞춰서 
- API 명세서 → swagger yam 알아오기

## 숙제

- ***미 해결 금액은 10,000 원 입니다.*** 
<br/>

- [ ] [공통] yaml, configue파일 암호화 방법과 기법
- [ ] JPA 환경 구축 한것 글 정리 해서 올려주세요 [ @세영 ]
- [ ] Kafka 구축 글 정리해서 올려주세요 [ @현우 ]
- [ ] 최초 Admin 가입자 [ @세영 ]
- [ ] 유저 생성 [ @세영 ]
- [ ] 보드 프레임 생성 [ @현우 ]