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

## User Requirement Specification

> ### Functional Requirements(기능적 요구사항)

> #### 지역검색
> 지역 검색의 경우 동단위 이하가 아닌, 해당 구만의 정보를 받아 검색이 이루어지도록 한다. 사용자의 입력이 이루어지는 동안 사용자 입력과 유사한 구를 미리 리스틩해서 보여준다.

> #### 대기환경 정보 표출
> 대기환경 정보의 경우 사용자가 한눈에 알아볼 수 있도록 필요한 최소한의 요소만을 표출한다. 반드시 보여지는 대기환경 정보와(예를 들어 미세먼지 혹은 자외선지수) 사용자의 특정한 동작이 일어났을 때 표출되는 대기환경의 정보를(예를 들어 오존지수, 이산화질소, 일산화탄소 등) 구분한다.

> #### 산책로/공원 표출
> 산책로와 공원의 표출은 기존 공공데이터의 산책로/공원과 사용자들이 선호하는 산책로/공원 섹션이 구분되어야 한다. 

> #### 사용자 선호 산책로/공원 입력
> 사용자가 지도에서 본인이 선호하는 산책로나 공원 위치를 클릭하면 본인이 선호하는 산책로/공원의 입력이 완료된 것으로 확인한다.


> ### Non-functional Requirements(비기능적 요구사항)

> #### Usability Requirement
> 차칫 표출되는 정보가 많아질 수 있다. 사이트에 접속하여 10초 내에 사용자가 원하는 정보를 바로 찾을 수 있도록 UI는 처음 시스템을 사용하는 사용자도 알아보기 쉽도록 구성되어야 한다. 또한 사용자가 웹앱의 기능을 이용할 때에 최소한의 노력만으로 기능을 이용할 수 있도록 앱을 만들어야 한다. 예를 들어 사용자 선호 산책로/공원 입력 시, 회원가입이나 자판의 입력 없이 단순한 동작만으로 입력이 가능해야 한다. 사용자가 페이지간의 이동을 최소한으로 유지하며 본인이 원하는 정보를 찾을 수 있도록 인터페이스가 구성되어야 한다. 

> #### Perfromance Requirement
> 해당 웹앱의 페이지가 그려지는 시간은 최대 3초를 넘어야 하지 않는다. 또한 사용자의 동작변경에 따른 페이지에서의 정보 표출은 끊기지 않고 부드럽게 진행되어야 한다.

> #### Environmental Requirement
> 어떠한 사용자의 브라우저 환경에서도 정보가 가려지는 일이 없어야 한다. 예를 들어 해당 윕앱을 사용하는 사용자의 브라우저 창의 크기에 따라 보여지는 정보의 레이아웃 또한 사용자가 한눈에 알아볼 수 있도록 변경되어야 한다.

> #### Security Requirement
> Business Goal인 재방문률을 계산하기 위해 접속 ip-address를 활용한다. 이때 database에 기록되는 정보는 ip-address의  hashing값을 저장하여 사용자의 아이피주소는 외부로 노출되거나 개발자가 열람할 수 없도록 한다. 본 사이트는 회원의 정보를 저장하지 않는다. 따라서 회원정보 관리에 대한 보안문제는 고려하지 않는다.

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
