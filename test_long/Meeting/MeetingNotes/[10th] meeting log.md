# 10th meeting log
## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/07/10

## 오늘 회의 토픽 요약

- 필요 API 러프하게 정리한 것 체크 + 연장

```
1. 회원가입 API 명세
    - 최초 어드민 존재하는지 여부 ~ FE단에서 chk
        1) request: .../isAdmin 
        2) user table -> count(*) -> length == 0

    - 최초 Admin 가입자 [ @세영 ]
        - 회원가입 완료
    - 일반 가입자
    - ID 중복 체크 [ @세영 ]
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

- 엄청 러프한 서비스에서 Data 접근 구도
    > service -> model : DAO (DTO) with [JPA] -> JDBC with DBCP2 -> DB

- JAP spring configuration 한 step들 공유 + 정리
    - [정리] 아시겠어요?

- Apache Kafka configuration 부터 ~ 메시지 리턴~
    - 바닐라 spring이 아닐 수 도 있다
    - 왜? Kafka를 사용하는 것에만 목표를 잡고 repo를 파는 것임


## 다음 회의 토픽

- Light-Board static page branch push review!
- 숙제 chk -> feedback
- JPA 공부한 것 리뷰! 
- Aphache kafka repo use 후기

## 미 해결 문제

- 도커라이징 잊지마러~
- Kafka핵심입니다~
- mongodb 세팅 해줘야 합니다~
- API 명세서 → swagger yam 알아오기

## 숙제
- ***미 해결 금액은 10,000 원 입니다.*** 
<br/>
- [ ] [공통] JPA 환경 구축
- [ ] 최초 Admin 가입자 [ @세영 ]
- [ ] ID 중복 체크 [ @세영 ]
- [ ] 보드 프레임 조회 [ @현우 ]
- [ ] kafka 전용 repo [ @현우 ]