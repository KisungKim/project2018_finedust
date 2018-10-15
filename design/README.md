#  Requirement Specification

<br/>
 
| 구분 | 항목 | 요약 |
|:-----:|:-----:|:-----:|
| 1 | 서론 | [들어가며](#preface)
| 2 | 시스템 아키텍쳐 | [시스템의 개괄적인 설계](#system-architecture)
| 3 | 데이터 갱신 시스템 | [대기환경 데이터의 정기적인 갱신](#cron-update)
| 4 | 사용자 조작 시스템 | [기능적, 비기능적 요구사항 및 서비스가 따르는 표준](#data-rendering-routing)
| 7 | Appendix | [첨부문서](#appendix)

<br/>

## Preface

> ### 위 프로젝트 제안서의 예상 구독자는 
> 1. 프로젝트 개발에 직접 참여하는 팀원 
> 2. 해당 프로젝트의 개발 과정을 열람하고 싶은 모든 사람입니다. 

> ### Document History

| 구분 | Modified Date | Explanation |
|:-----:|:-----:|:-----:|
| 1 | 2018.10.- | 설계명세서 파일 초안 작성

<br/>

## System Architecture

> ### 본 프로젝트의 전체 시스템은 크게 8부분으로 나뉜다. 
> 종속 시스템의 경우 1차적으로 직접 관여하는 시스템의 관계만 서술하였다.

| 타입 | 구분 | 항목 | Explanation | 종속 시스템 |
|:-----:|:-----:|:-----:|:-----:|:-----:|
| 외부 | 1 | [Data Storage](#data-storage) | 데이터 저장소 | - |
| 외부 | 2 | [Data Api](#data-api) | 데이터를 받아오기 위한 api source | - |
| 외부 | 3 | [Map Api](#map-api) | 지도를 사용하기 위한 api | - |
| 외부 | 4 | [Server](#server) | 웹앱이 구동하기 위한 메인 서버 | - |
| 내부 | 5 | [Data Processing](#data-processing) | 시스템 간 데이터 처리 | 1 |
| 내부 | 6 | [Page Request&Response](#page-request-response) | 웹 페이지의 요청과 응답을 처리 | 4, 5, 7|
| 내부 | 7 | [Page Rendering&Routing](#page-rendering-routing) | 페이지간 이동 및 페이지 렌더링 | 3, 6 |
| 내부 | 8 | [Cron Update](#cron-update) | 정기적으로 데이터 갱신 | 1, 2, 4 |

![image03](https://drive.google.com/uc?id=1ZioHPGayUj4klDVqMq65HoS6DhXjNN0W)

> ## Cron Update
> 본 웹앱의 경우, 날짜별로 달라지는 대기환경을 보여주어야 하기 때문에 정보의 갱신이 필요하다. 크론 업데이트는 이를 위한 시스템이며 서버에 지정된 명령을 확인하고 데이터를 외부 api로부터 받아와 database에 저장한다. 

+ Interaction

--image--

+ Interface Diagram

--image--

> ### Cron Update에 관여하는 Data Storage의 각 Class는 다음과 같다.

| 타입 | 항목 | Explanation | 외부 소스 | 갱신주기 |
|:-----:|:-----:|:-----|:-----:|:-----:|
| 대기 | [Data Mise](#data-mise) | 각 측정소별 대기오염정보를 조회하기 위한 서비스로 기간별, 시도별 대기오염 정보와 통합대기환경지수 나쁨 이상 측정소 내역, 대기질(미세먼지/오존) 예보 통보 내역 등을 조회할 수 있다. | 공공데이터포털>한국환경공단_대기오염정보 조회 서비스 | 1day |
| 대기 | [Data Ultraviolet](#data-ultraviolet) | 자외선 지수(*자료제공 기간 : 3월 ~ 11월 )) | 공공데이터포털>(신)생활기상지수조회 | 1day |
| 대기 | [Data CleanWind](#data-clean-wind) | 대기확산 지수(*자료제공 기간 : 11월 ~ 5월 )) | 공공데이터포털>(신)생활기상지수조회 | 1day |

+ Class Diagram

+ Sequence Diagram

<br/>

> ## Data Rendering Routing
> 사용자가 웹 페이지 내에서 이동하며 페이지 내의 정보들을 볼 수 있도록 가능하게 해준다. 지도를 활용해야 하는 경우 지도 api에 작업을 요청하며, 특정 페이지에서 사용자가 정보들을 요구했을 때 페이지 요청/응답 시스템에 필요한 데이터를 받아오기 위한 rest-framework-page를 요청한다.

+ Interaction

--image--

+ Interface Diagram

--image--

> ### Data Storage의 각 Class는 다음과 같다.

| 타입 | 항목 | Explanation | 외부 소스 | 갱신주기 |
|:-----:|:-----:|:-----|:-----:|:-----:|
| 대기 | [Data Mise](#data-mise) | 각 측정소별 대기오염정보를 조회하기 위한 서비스로 기간별, 시도별 대기오염 정보와 통합대기환경지수 나쁨 이상 측정소 내역, 대기질(미세먼지/오존) 예보 통보 내역 등을 조회할 수 있다. | 공공데이터포털>한국환경공단_대기오염정보 조회 서비스 | 1day |
| 대기 | [Data Ultraviolet](#data-ultraviolet) | 자외선 지수(*자료제공 기간 : 3월 ~ 11월 )) | 공공데이터포털>(신)생활기상지수조회 | 1day |
| 대기 | [Data CleanWind](#data-clean-wind) | 대기확산 지수(*자료제공 기간 : 11월 ~ 5월 )) | 공공데이터포털>(신)생활기상지수조회 | 1day |
| 산책로/공원 | [Data BigPark](#data-big-park) | 서울시 공원/산책로 정보(대) | 서울열린데이터광장>환경>공원녹지 | 1month |
| 산책로/공원 | [Data SmallPark](#data-small-park) | 서울시 공원/산책로 정보(중/소) | 서울열린데이터광장>환경>공원녹지 | 1month |
| 산책로/공원 | [Data SpecialPark](#data-special-park) | 서울시 공원/산책로 정보(테마) | 서울열린데이터광장>환경>공원녹지 | 1month |
| 산책로/공원 | [Data Dulle](#data-dulle) | 서울시 둘레길 정보 | 서울열린데이터광장>환경>공원녹지 | 1month |
| 산책로/공원 | [Data UserSelectedPark](#data-user-selected-park) | 사용자 선호 산책로 | 자체 수집 | 데이터 처리 완료 후 |

+ Class Diagram

> #### 대기 데이터 클래스 
> [Cron Update](#cron-update) 참고

> #### 산책로 데이터 클래스

+ Sequence Diagram

> #### 지역 검색(서울시 행정구, 대기환경)

> #### 데이터 조회(대기환경)

> #### 데이터 조회(산책로/공원/사용자 선호 산책로)

> #### 데이터 입력(사용자 선호 산책로)

<br/>
