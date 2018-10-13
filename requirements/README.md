#  Requirement Specification

<br/>
 
| 구분 | 항목 | 요약 |
|:-----:|:-----:|:-----:|
| 1 | 서론 | [들어가며](#preface)
| 2 | 서비스 소개 | [Findust(가명) web-app에 대한 소개](#service-description)
| 3 | 사용자 요구사항명세서 | [기능적, 비기능적 요구사항 및 서비스가 따르는 표준](#user-requirement-specification)
| 4 | 시스템 아키텍쳐 | [시스템의 개괄적인 설계](#system-architecture)
| 5 | 시스템 요구사항명세서 | [기능적, 비기능적 요구사항 및 서비스가 따르는 표준](#system-requirement-specification)
| 6 | 시스템 모델 | [세부적인 시스템 구성 모델](#system-models)
| 7 | Appendix | [첨부문서](#appendix)

<br/>

## Preface

> ### 위 프로젝트 제안서의 예상 구독자는 
> 1. 프로젝트 개발에 직접 참여하는 팀원 
> 2. 해당 프로젝트의 개발 과정을 열람하고 싶은 모든 사람입니다. 
> 3. web-app의 사용자 및 사용 예정자입니다.

> ### Document History

| 구분 | Modified Date | Explanation |
|:-----:|:-----:|:-----:|
| 1 | 2018.10.13 | 요구사항명세서 파일 초안 작성

<br/>

## Service Description

> ### FineDust(가명)은 고객의 필요에 따라 서울시의 행정구별 대기환경지수와 산책로/공원정보를 보여주는 행위를 서비스한다. 
> #### (아래 문서는 약식으로 작성되었습니다)

> ### Need
> #### 1. 이미 기술이 대중들에게 많이 공개된 미세먼지 데이터 자체를 보여주는 것이 아닌, 야외여가활동이라는 **부가가치와** 결합한다.
> #### 2. 기존 정보의 제공 이외에도 **개별 사용자의 활동을 기반으로 한 개인적인 데이터를 축적할 수 있는**되는 user-side의 web-app을 추구한다.
>

#### 국내 미세먼지를 비롯한 대기환경에 대한 관심 증대

> ##### 그림출처 : 구글트렌드(검색어 : 미세먼지, 필터 : 5년)
>
> 대기환경의 오염에 대해 인지한 이후, 한국에서 미세먼지에 대한 관심은 높아지고 있다. 관련 검색어로는 미세먼지 농도, 오늘의 날씨가 연결된것을 볼 수 있었다. 미세먼지에 대한 관심은 꽤 오래전에 나타났음에도 불구하고, 이를 활용한 앱은 아직까지 대기환경을 보여주는 기능에만 치중되어 있었다. 안드로이드의 경우 이미 OS에 내장된 소프트웨어도 이에 맞추어 업데이트가 되었으며 대기환경을 보여주는 것만으로는 다른 어플과 차별점이 될 수 없음을 알 수 있다 *(Market-Competitor)* 이는 거꾸로 개인이 대기환경 데이터 자체를 활용할 수 있는 채널이 많이 개방되었음을 의미한다 *(Company-opportunity)*

![image01](https://drive.google.com/uc?id=1ADo7IYVr6f6jQBGwe0gG2cJSOAN_t5sw)

#### 국민 여가활동에서 산책에 대한 관심 증가

> ##### 자료출처 : 통계청(시군구통계>수원시 : 향후 늘어야 하는 여가시설(2015))
>
> 1인가구 증가 및 경기침체에 따라 시민들은 좀 더 저렴하고 가벼운 여가활동을 선호하고 있다(공원/여가 산책로에 대한 니즈가 다른 8개 이상의 국공립시설보다 앞서고 있다) 야외여가활동의 경우 대기환경의 영향을 많이 받는다고 가정, 기존의 통합대기환경지수를 활용할 수 있는 방안으로 가능할 것이라고 보았다 *(Market)*

#### 야외여가활동의 다각화 및 해당 분야에서 사용자 참여형 어플 인기

> ##### 자료출처 : [애견신문](http://www.koreadognews.co.kr/news/view.php?no=1376)
>
> '산책+반려견' 등의 애견시장의 확대와 더불어 결합된 키워드 등은 야외여가활동을 활용한 어플의 활용방안을 높일 것으로 예상된다. 다만 기존의 여러 산책길 어플, 공개된 산책용지에 대한 데이터를 참고했을 때 사용자가 직접 본인의 산책로를 다른사람에게 추천해 주는 수단은 많이 부족하다 *(Competitor)*
>
> ##### 자료출처 : Nike Run Club 구동 화면 예시
>
> 최근 나이키 어플의 경우 조깅과 동시에 사용자가 운동한 구간을 저장해 피드에서 공유하거나 본인이 열람할 수 있는 서비스를 제공하고 있다. 이는 국가나 단체에서 제공하는 정적인 데이터보다는 개인이 만들어가는 데이터가 사용자의 관심을 더 유도할 수 있음을 유추할 수 있다 *(Company-Opportunity)*

<img src="https://drive.google.com/uc?id=1IFosJV5LzJz88Khk4gYDCwrC5YY9EqlJ" width="280px" height="600px"/>

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

> #### Data storage
> 데이터 저장소의 경우 서버를 활용한 외부 데이터 저장 데이터베이스를 활용한다. 대기환경, 산책/공원에 대한 정보 등 활용되는 모든 데이터를 저장한다.

> #### Data Api
> 1차 데이터는 [공공데이터 포털](https://data.go.kr)에서 받아온다. 받아온 데이터를 가공하여 데이터베이스에 저장하는 과정에서 [네이버 지도 api](https://developers.naver.com/docs/map/tutorial/) 또한 활용한다. 

> #### Map Api
> 지도는 [다음 지도 api](http://apis.map.daum.net)를 활용한다.

> #### Server
> 서버의 경우 [AWS EC2](https://aws.amazon.com/ko/ec2/?sc_channel=PS&sc_campaign=acquisition_KR&sc_publisher=google&sc_medium=ACQ-P%7CPS-GO%7CBrand%7CDesktop%7CSU%7CCompute%7CEC2%7CKR%7CEN%7CText&sc_content=ec2_e&sc_detail=aws%20ec2&sc_category=Compute&sc_segment=293634586493&sc_matchtype=e&sc_country=KR&s_kwcid=AL!4422!3!293634586493!e!!g!!aws%20ec2&ef_id=WpeEQQAABY-v9yyW:20181011095431:s)를 사용한다.

> #### Data processing
> 페이지에서 사용자가 요청한 정보를 처리하거나, 사용자가 입력한 정보를 받는 과정에서 활용된다. 데이터 저장소를 필요로 하며, 정보를 활용하는 각기 다른 시스템에 요구에 맞추어 정보의 형태를 가공해준다.

> #### Page Request Response
> 웹 페이지의 요청과 응답을 처리한다. 사용자가 웹 페이지를 요청했을 때, 서버로에 요청하여 받아 응답 페이지를 제공하거나, 페이지의 rendering과정에서 필요한 data들을 받아올 수 있도록 데이터 처리 시스템에 알려준다.

> #### Page Rendering Routing
> 사용자가 웹 페이지 내에서 이동하며 페이지 내의 정보들을 볼 수 있도록 가능하게 해준다. 지도를 활용해야 하는 경우 지도 api에 작업을 요청하며, 특정 페이지에서 사용자가 정보들을 요구했을 때 페이지 요청/응답 시스템에 필요한 데이터를 받아오기 위한 rest-framework-page를 요청한다.

> #### Cron Update
> 본 웹앱의 경우, 날짜별로 달라지는 대기환경을 보여주어야 하기 때문에 정보의 갱신이 필요하다. 크론 업데이트는 이를 위한 시스템이며 서버에 지정된 명령을 확인하고 데이터를 외부 api로부터 받아와 database에 저장한다. 

<br/>

## System Requirement Specification

#### 각 기능 항목들에 대한 설명은 activity diagram, sequence diagram, tabular description을 활용했습니다

### Functional Requirements(기능적 요구사항)

> #### 지역검색

![image_search_gugun](https://drive.google.com/uc?id=1mSUStQSFyACpFPlDs7kCTcecB0FdEonX)

| 항목 | 설명 |
|:-----:|:-----|
| 기능 | 지역검색 |
| 입력 | gugun_name : 유저가 입력한 구의 이름 |
| 출력 | gugun_list : 사용자 입력과 유사한 구의 리스트 |
| 처리 | find mathcing gugun_name : 사용자의 입력과 유사한 구의 리스트를 실시간으로 갱신하여 출력 |
| 조건 | - |

<br/>

> #### 대기환경 정보 표출

![image_show_air](https://drive.google.com/uc?id=1ZxajC999Szuq5uL4osW3os80mwzP5u-K)

| 항목 | 설명 |
|:-----:|:-----|
| 기능 | 대기환경 정보 표출 |
| 입력 | specific gugun_name : 정확한 행정상 구/군의 이름 |
| 출력 | air_data : 미세먼지, 오존지수, 자외선지수, 이산화질소, 일산화탄소, 대기환경 등급에 대한 데이터 |
| 처리 | find matching air_data : 사용자의 입력과 동일한 구의 대기환경 정보를 출력 |
| 조건 | 유저가 gugun_name을 정확히 값으로 넘긴 경우에만 대기환경의 정보를 표출한다 |

<br/>

> #### 산책로/공원 정보 표출

![image_show_sancheck](https://drive.google.com/uc?id=19Wj2o8UXSFxZ64xdhJXJ3IBGh4v28zPW)

| 항목 | 설명 |
|:-----:|:-----|
| 기능 | 산책로/공원 정보 표출 |
| 입력 | specific gugun_name : 정확한 행정상 구/군의 이름 |
| 출력 | sancheck_data : 산책로/공원의 구분, 이름을 출력, 해당 위치를 지도에 출력 |
| 처리 | request map obj : 다음 지도 api로부터 맵 객체 요청<br/>render sancheck data : 받아온 다음 지도 api에 산책로/공원 정보를 지도 api의 메소드를 활용해 그려냄을 의미 |
| 조건 | 유저가 gugun_name을 정확히 값으로 넘긴 경우에만 산책로/공원 정보를 표출한다 |

<br/>

> #### 사용자 선호 산책로/공원 입력

--image--

| 항목 | 설명 |
|:-----:|:-----|
| 기능 | 설명 |
| 입력 | 설명 |
| 출력 | 설명 |
| 처리 | 설명 |
| 조건 | 설명 |

<br/>

### Non-functional Requirements(비기능적 요구사항)

> #### Usability Requirement
> 새로운 양식으로 다시 rendering되어야 하는 페이지는 2개로 한정한다. 

> #### Perfromance Requirement
> concurrent requsts에 반응할 수 있는 라이브러리를 사용한다(예: axios)

> #### Environmental Requirement
> css의 경우 css-grid와 flex를 이용하여 고객의 브라우저 크기 조작에 유연하게 대응할 수 있도록 한다

> #### Security Requirement
> ip-address를 hasing하기 위한 hasing 메소드는 api를 이용한다.

<br/>

## System models

#### 각 기능 항목들에 대한 설명은 activity diagram, sequence diagram, tabular description을 활용했습니다

> #### 웹 페이지 접속

![image_server](https://drive.google.com/uc?id=15ALa9X-Pdsu4V_aICTll2dxhWRPLKYy4)

| 항목 | 설명 |
|:-----:|:-----|
| Use-case | 웹 페이지 접속 |
| 설명 | 고객이 맨 처음 웹 페이지에 접속할 때 서버는 장고프레임웍에 메인 페이지를 요청한다. <br/>장고프레임웍은 해당 html을 호출하며, 이때 react code를 통해 생성된 <br/>js페이지가 rendering된다. |
| 입력 | reqMainPage(1) : 서버 도메인<br/>reqMainPage(2) : main페이지의 html을 호출하는 장고 url<br/>reqMainPage(3) : html페이지 안에서 build된 js파일명 |
| 출력 | resMainPage : js 페이지 |
| 비고 | - |

<br/>

> #### 상세정보 

![image_showdetail](https://drive.google.com/uc?id=1pLtjm-VWITM08l7_eeHbSSqvhfGufX3c)

| 항목 | 설명 |
|:-----:|:-----|
| Use-case | 상세정보 표출 |
| 설명 | 고객이 js페이지에서 특정 정보를 요청한 경우 react code에 따라 rendering페이지는 계속해서 갱신된다. <br/>만약 DataStorage의 정보가 필요한 경우, react code는 장고url을 호출하며 호출된 장고url은 <br/>장고view의 DataProcessing시스템을 호출한다. DataProcessing시스템은 DataStorage로 부터 정보를 받아와 <br/>이를 json객체로 파싱하여 다시 react code에 넘겨주게 된다. 만약 Map객체 내부에서 추가적인 메소드가 필요한 경우, <br/>react code는 daum map api의 라이브러리에서 해당 메소드를 불러와 정보를 표출한다. |
| 입력 | reqRenderingComponent : 고객의 js페이지 조작을 통한 react-component의 변화<br/>reqInfoForRendering(1) : component rendering을 위해 필요한 정보를 제공해 줄 수 있는 장고url <br/>reqInfoForRendering(2) : 장고url에 필요한 정보를 제공해 줄 수 있는 장고view<br/>reqStoredData : 장고view에서 사용할 정보를 DataStorage에 요청해주는 mysql쿼리문 |
| 출력 | resData : DataStorage로부터 응답받은 python class object<br/>resParsedInfo : 장고view에서 json형식으로 파싱된 데이터 |
| 비고 | - |

<br/>

## Appendix

<br/>
