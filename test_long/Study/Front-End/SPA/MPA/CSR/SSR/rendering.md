- 사용자에게 페이지를 보여주는 방식에 대한 내용을 정리
- MPA/SPA는 페이지를 여러개 쓰는지 또는 한 개 쓰는지에 대한 개념
- SSR/CSR은 렌더링을 서버에서 하는지, 클라이언트에서 하는지에 대한 개념

## 1. SSR (Server Side Rendering)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c5099dcc-39bd-41eb-9da2-9a957b8fc510/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/c5099dcc-39bd-41eb-9da2-9a957b8fc510/Untitled.png)

SSR

- 전통적인 웹 방식
- 서버에서 사용자에게 보여줄 페이지를 모두 구성하여 사용자에게 페이지를 보여주는 방식
- 모든 데이터가 매핑된 서비스 페이지를 클라이언트에게 바로 보여줄 수 있음
- 페이지를 구성하는 속도는 늦어지지만, 전체적으로 사용자에게 보여주는 컨텐츠 구성이 완료되는 시점은 빨라진다는 장점이 있음
- SEO에 사용되는 meta 태그들이 미리 정의되어 SEO에 유리함
    - View를 서버에서 렌더링해 제공하기 때문에(View를 먼저 그리기 때문에) 상대적으로 SEO에 유리

## 2. CSR (Client Side Rendering)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e2a1b3cb-1a13-4aea-81c0-95d31024560e/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/e2a1b3cb-1a13-4aea-81c0-95d31024560e/Untitled.png)

CSR

- 초기 전송되는 페이지의 속도는 빠르지만, 서비스에서 필요한 데이터(HTML, JS, 그외 리소스 모두 포함)를 클라이언트에서 추가로 요청하여 재구성해야하기 때문에 전체적인 페이지 완료 시점은 SSR 보다 느려짐
    - 즉, 초기 구동 속도가 느림
- 사용자의 해동에 따라, 필요한 부분만 다시 읽어들이기 때문에 서버 측에서 렌더링하여 전체 페이지를 다시 읽어들이는 것보다 빠른 인터랙션을 기대할 수 있음
- 검색 엔진의 최적화 문제 존재
    - 대부분이 웹 크롤러, 봇들이 자바스크립트 파일을 실행시키지 못하기 때문에 HTML에서만 컨텐츠를 수집하게 되고 클라이언트 사이드 렌더링되는 페이지를 빈 페이지로 인식하게 됨
    - View를 생성하기 위해서 JS가 필요하고 View를 생성하기 전까지는 검색 엔진 크롤러의 데이터 수집이 제한적이기 때문에 상대적으로 검색 엔진이 이해하는 정보가 부족해 SEO에 유리 하지 않음
- 보안 문제 발생
    - SSR은 사용자에 대한 정보를 서버 측에서 세션으로 관리함
    - 클라이언트 측에는 쿠키말고는 사용자에 대한 정보를 저장할 공간이 마땅치 않음

## 3. SPA (Single Page Application)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/76bdfe2c-a327-4b67-be98-8424a8d7e618/Untitled.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/76bdfe2c-a327-4b67-be98-8424a8d7e618/Untitled.png)

SPA

- 데스크탑에 비해 성능이 낮은 모바일, 스마트폰을 통해 웹 페이지를 출력하기 위해서는 기존에 있었던 방식과는 다른 접근이 필요했고 그에 따라 등잔한 기법
- 하나의 HTML 파일을 기반으로 자바스크립트를 이용해 동적으로 화면의 컨텐츠를 바꾸는 방식의 웹 어플리케이션
- 처음에 한 번만 정적 리소스를 다운받고, 그 이후에는 새로운 요청이 들어왔을 때 필요한 데이터만 부분적으로 바꾸어 줌

- 장점
    - 사용자 경험 측면에서 좋고, 성능이 우수함
    - 디버깅이 상대적으로 쉬움
    - 로컬 데이터를 효과적으로 캐시할 수 있음
- 단점
    - 초기 구동 속도가 느림 (초기에 웹 어플리케이션에 필요한 모든 정적 리소스를 다 받아야 하기 때문)
    - SEO 관점에서 불리

## 4. SEO (Search Engine Optimization)

- 검색엔진 최적화는 웹페이지 검색엔진이 자료를 수집하고 순위를 매기는 방식에 맞게 웹 페이지를 구성하여 검색 결과의 상위에 나올 수 있도록 하는 작업 의미

## 5. MPA (Multiple Page Application)

- 사용자가 페이지를 요청할 때 마다, 웹 서바가 요청한 UI와 필요한 데이터를 HTML로 파싱해서 보여주는 방식의 웹 어플리케이션
- 사용자가 아주 사소한 요청을 하더라도 매번 전체 페이지를 렌더링 해야 함

- 장점
    - SEO(Search Engine Optimization, 검색 엔진 최적화) 관점에서 유리
    - MPA는 완성된 형태의 HTML 파일을 서버에서 전달받기에 검색엔진이 페이지를 크롤링하기에 적합함
- 단점
    - 매번 페이지 전체를 새로 불러와서 렌더링해야하기 때문에 화면이 깜빡이는 등 성능상 이슈가 있음
    - 프론트와 백이 밀접하게 연관되어 있어서 개발 복잡도가 증가함

## 6. 정리

- 전통적인 SSR은 초기 로딩속도가 빠르고 SEO에 유리하지만, View 변경(화면 전환) 시 계속적으로 서버에 요청해야 하므로 서버에 부담이 큼
- CSR은 초기 로딩 속도가 느리고 SEO에 대한 문제가 있지만, 초기 로딩 후에는 View를 서버에 요청하는 것이 아닌 클라이언트에서 직접 렌더링 하기 때문에 화면 전환이 매우 빠르다는 장점이 있음
- 이 2가지 렌더링 방법을 적절하게 사용하여 **첫 번째 페이지 로딩에서는 서버 사이드 렌더링을 사용하고, 그 후에 모든 페이지 로드에는 클라이어느 사이드 렌더링을 활용하는 방법을 많이 사용함**
- **제공할 서비스가 어느 부분에 초점을 맞춰서 진행되느야에 따라서, 장점이 빛을 발하고 단점이 중요하지 않은 요소로 적용될 것임**