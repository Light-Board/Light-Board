---
sort: 5
---
# 5th meeting log
## 기본 사항
- 참가자 [정현우](https://github.com/Nuung), [전세영](https://github.com/SeyoungJeon)
- 일시 2021/04/24

## 오늘 회의 토픽 요약

- admin 관련 스키마 생각해보기
    - ***어드민 대쉬보드 ~ static하게 통일하는 걸로***
    - 보여줄 통계적 자료가 풍부하지는 않다.
    - 만들면서 설계로 돌아와도 크게 영향을 줄 관계가 없다.
    - *일단 만들면서 고민*하기로 함
- 사용 시나리오
    - 관리자 [2021.04.24 1차 완성]
    - 관리자 ←→ 일반 유저 ~ 알림에 대해 kafka → mongodb
    - 일반 유저
    - 게시판은 새로고침?
    - 게시글에서 댓글 달때 실시간 처럼 느껴져야? 폴링
- kafka가 어디에 어떤 기능으로 존재해야할까?
    - n/w 모든 request에 대한 로그 메시지
        - admin이 로그에 대한 control 권한 X
        - key[type] : value[msg]
        - filter key에 해당하는 타겟 컨트롤러를 정해놓고
            - 그리고 filter거칠때 해당하는 컨트롤러에 대한 로그 msg
    - 신고
        - 신고 게시판 / 댓글 둘 다 ~ 왜냐면 신고는 user table을 업데이트 할꺼니까
        - 신고를 하면 → admin에게 알람 → admin에게 선택 권한이 있다!
            - admin에게 선택 권한 ~ view와 정보에 대해 따로 설계가 필요할 것 같다.
    - 알람 / 알람 설정 기준
        - 내가 남긴 ***게시글에 따봉 추가***되었을때
        - 사람을 태그하는 기능 있나요? 태그된 유저가 알람을 받음! → 언제, 어디서, 어떻게 태그를 하느냐에 따라 달라질 듯 하다!
- 코딩을 하기 전에
    - DB setting
    - env setting (환경 설정 통일 화)
    - 최소한의 단위 ~ mvp
        - 도커 ~ devOps 생각 접어두고
        - front도 ~ 최소한 규격만 맞춘다고 생각하고
    - BE만큼 정도는 완성도 있게 돌리는 걸로
    - 세부 기능 명세
        - 해당 기능에 따른 역할 분담
        - 기능 개발 역할 분담

# 다음 회의 토픽

- 워크 벤치로 ERD 그리기! → 워크밴치
- DB 1차 세팅 setting!!
    - 마지막으로 DB check up
    - 만들어 놓은 view 명세로 다시 기능 체크

# 미 해결 문제

- 코딩 시작
- 세부 기능 명세서

# 숙제

- [ ]  [공통] spring - gradle -docker
- [x]  [공통] mysql → data type에 대해 심도 있는 고민
    - [x]  mysql 어떤 타입이 어떻게 언제 유리한지 공부해오기!
    - [x]  관계도 고민해오자!
    - [x]  user / user_question / profile / board_frame / board / board_image / board_comment
- [x]  [공통] kafka가 어디에 어떤 기능으로 존재해야할까? 고민해오기