# 9th meeting log
## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/06/26

## 오늘 회의 토픽 요약

- 필요 API 러프하게 정리한 것 체크
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
    - 보드 코멘트 조회
    - 보드 코멘트 작성
    - 보드 코멘트 수정
    - 보드 코멘트 삭제    
    - 스마트 에디터 기반 API

3. Admin 페이지
    - 페이지 관리
    - 서비스 네임 조회 API
    - 서비스 네임 설정 API
    - 서비스 상태 조회 API
    - 서비스 상태 설정 API
    - 어드민 landing 페이지
    - 통계 관련 조회 API
    - 통계 관리
        - 필터에 따른 통계 조회
    - 메뉴 관리
        - 보드 CRUD API
    - 유저 관리
        - 유저 목록 조회 API
        - 유저 조회 API
        - 유저 정보 업데이트 API
        - 유저 비활성화 API
```


## 다음 회의 토픽

- Light-Board static page branch push review!
- 숙제 chk -> feedback
- JPA 공부한 것 리뷰! 
- Aphache kafka repo use 후기

## 미 해결 문제

- 도커라이징 잊지마러~
- Kafka핵심입니다~
- mongodb 세팅 해줘야 합니다~

## 숙제
- [ ] [공통] JPA 환경 구축
- [ ] 최초 Admin 가입자 [ @세영 ]
- [ ] ID 중복 체크 [ @세영 ]
- [ ] 보드 프레임 조회 [ @현우 ]
- [ ] kafka 전용 repo [ @현우 ]
- [ ] [공통] API 명세서 → swagger yam 알아오기

