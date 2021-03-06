﻿
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190531-lightgray.svg?style=flat-square)

# 스케줄

## 190627 (thur) -  

## 190618 (tue) - flask-board-renewal

- 리펙토링하는게 쉽지 않다..
- 전체적인 구조 짜는데만 한세월이고, 변수명, 경로명과 같이 세세한 부분들에 있어서도 꼼꼼히 체크해야 하는 것.. ㄷㄷ
- 특히 restful 컨셉 + pluggable view 를 결합한 구조로 구상하는 데 많은 시간을 투자했다.
    * 코드를 보기 좋게 만들고 모듈화 및 자동화 고민
- 어쨌든 오늘까지는 기본적인 index, contact, autocomplete 로직과, sqlalchemy 를 적용한 회원 로직 중에서 sign up 부분까지 완료했다.
- 템플릿 중에서 매크로를 사용한다던가, 공통 템플릿 모듈을 사용하는 경우,
	* 너무 공통화를 시키려고 하다보면 나중에 코드 수정이 어려워질 수도 있겠음.
	* 서로 다른 컨셉의 템플릿이라면(혹은 모듈이라면) 공통화보다는 (코드를 더 작성하더라도)분리시키는게 나을 수도 있겠음.

#### 그다음 할거

- signin/signout 및 회원 관련 crud 완성할 것
- boards/todos 로직 완성할 것
- 새로운 상용 프로젝트 구상

## 190617 (mon) - flask-board Refactoring

- pluggable view 를 활용한 뷰 함수를 생성할 때,
- before_request 의 내용은 어떻게 포함시킬까?
    * 각 클래스 인스턴스가 생성될 때 생성자에서 만들도록 하면 될듯?

## 190614 (fri) - flask-board Refactoring

#### flask-board Refactoring

- Refactoring?
    - 외부에서 보여지는 동작의 변화 없이 소프트웨어 구조를 바꾸는 일련의 작업.
    - 목표는 모든 사람이 이해하기 편리한 프로젝트 구조와 코드를 만드는 것이다.
    - 컴퓨터적 성능 면에서 다소 비효율적으로 변화할 수 있으나, 협업 및 유지보수, 기능 개선 등의 측면에서 강점을 가질 수 있다.
    - 한 번에 모든 구조를 완벽하게 구상할 수 없다는 전제 하에, 기존의 코드에 기반해서 점진적으로 더 좋은 디자인으로 개선해 나간다.
    - 그러나 전체적인 디자인이나 구조는 충분히 생각한 다음에 진행한다.
    - 클래스나 메서드를 관련 있는 논리적 기능에 따라 캡슐화하고, 변화의 폭이 적은 방향으로 작성한다.
    - 세 번 이상 반복되는 코드는 함수화 한다. (두려워하지 말 것)
    - 리펙토링을 하는 이유
        * 시간이 흐름에 따라 잘못된 구조로 인한 기술적 부채가 늘어난다.
        * 일관성 및 지속 가능한 소프트웨어를 작성할 수 있다.
        * 개발방법론이 안정화되면 협업에 의한 개발 속도가 향상된다.
        * 중복을 제거하고 응집도가 높고 결합도가 낮은 모듈화를 진행한다
        * 버그를 빠르게 찾아내고 원인을 분석할 수 있다.
    - 리펙토링을 하지 말아야 할 시점
        * 기존 코드의 동작 자체에 문제가 있을 경우
        * 마감 등 시간적 여유가 충분하지 않은 경우
        * 리펙토링이 원만하게 이루어진다면 장기적으로 개발 및 유지보수 시간을 단축할 수 있다.
- 기능별 API 분리
    1. main application
        * run module (ok)
        * application configuration (ok)
            - secret key
            - database config
            - session config
            - debug mode
        * static files / templates (bootstrap, summernote, jquery, socketio / base, home, ...)
        * routing - view function
        * services
        * wtform
        * file upload (?)
    2. mariadb-sqlalchemy-pymysql
        * users management
        * db.session 및 model 작성
        * password encryption
    3. mongodb-pymongo
        * field validation
        * board
        * comments
        * pagination
        * timeline

#### 핸드폰 기능 정리

- 기본
	* 카톡, 카카오맵, 멜론, 네이버, 유투브, 트위치, 배민, chrome, ami mail, gmail
	* github, google drive
- 저장된 데이터
	* 주소록, 메세지, 사진, 녹음, 메모, 캘린더, 파일 관리자
- 금융
	* 국민은행메인(스타뱅킹, 앱카드), 신한국비통장, 기업은행대출통장, 토스
- 구인구직
	* 잡코리아, 사람인, 잡플래닛, 원티드, 워크넷, 크레딧잡, indeed
- 멤버십
	* t membership
	* 이디야, 할리스
- 기타
	* 고속 / 시외 버스 앱, 코레일톡
	* 일터qna, okky, 파워 프리토킹(다음카페), scan news, translate, 성경
	* 파거(네이버카페), otp, 포켓거상

#### 주말간 할거

- pluggable View 제대로 정리
    * 참조문서를 통해 도입 배경, 사용 목적, 구조 및 구현방법, 예제 순으로 정리할 것
- flask-board Refactoring 고민하기, 구현하기
- 안경잡이개발자의 [flask 로 word cloud API](https://ndb796.tistory.com/241?category=1032205) 구현하기 따라해볼 것 (react 연동까지)
- 새로운 프로젝트를 위한 구상. 이를 위한 SW 분석설계 방법론이나 UML 표현방법, 스토리보드 만들기(카카오 오븐) 등을 공부할 것.

## 190613 (thur) - SQLAlchemy 이어서

- 원래 추가적으로 UML, 프론트 프레임워크 탐사, 새 프로젝트 정의 등을 할라했으나..
- 내일채움공제 신청이 이상하게 버벅거려서 시간을 많이 씀.. 괜히 다운돼서 공부속도도 잘 나오지 않음..
- 어쨌든 SQLAlchemy 쿼리공부까지 얼추 마쳤다.

#### 먼저 할일

- Mariadb-SQLAlchemy 로직을 flask-board 프로젝트 회원관리 파트에 추가하기
- flask-board 프로젝트를 뼈대부터 리뉴얼하되,
    * 거시적인 기능별로 API 를 분리하고
    * 논리적인 코드 흐름에 따라 각 파일 및 함수들도 분리하여 배치한다.
    * 하나의 파일이나 함수가 하나의 논리적 기능만을 담당하도록 하며
    * 상호간에 외부적인 영향은 최소화하고 **input/output** 에 대해서만 관여하도록 한다.
- 데이터베이스가 추가되기 때문에
    * 각 로컬에서 작업하려면 mariadb, mongodb 를 설치 및 데몬 실행해두어야 하며,
    * heidisql 이나 python 을 위한 pycharm 등의 유틸리티 툴을 설치해야 한다
    * 프로젝트 자체를 위해 가상환경을 구축해야 하는데, 이를 위해 pipenv 모듈을 사용한다.

#### 기타

- 안경잡이개발자의 [flask 로 word cloud API](https://ndb796.tistory.com/241?category=1032205) 구현하기 따라해볼 것 (react 연동까지)
- 새로운 프로젝트를 위한 구상. 이를 위한 SW 분석설계 방법론이나 UML 표현방법, 스토리보드 만들기(카카오 오븐) 등을 공부할 것.

## 190612 (wed) - SQLAlchemy 공부

#### flask-board

- sign up 시 unique field 유효성 검증 > email, nickname
	* 기존에는 그냥 db 에 때려박아서 unique 필드 중복 오류가 발생하면 임의로 처리했다
	* 이러한 방식은 일단 뭐가 문제인지 명료하지 않고, 처리방법도 주먹구구 식이다.
	* 새롭게 유효성 검증할 방법은, 각각의 필드를 조회하여 투입된 값과 직접 비교하는 것이다.
	* 그러나 이것을 구현하려면 더 많은 함수와 로직이 필요하겠으나,
	* WTForm 의 validation 기능을 활용하면 훨신 간결하게 코드를 추가할 수 있다.
	* 컨셉은, 각 form 클래스에 원하는 field 에 대한 validate 메서드를 만드는 것이다.
	* 이제 중복 데이터를 투입하면 적절한 에러 메시지가 표현된다.
- 기존의 객체 모델인 dict 타입 element 를 class 기반의 구체적인 model 로 만들 것
	* ex) element = {} 방식을 user = User() 방식으로 변경

#### 공부

- SQLAlchemy 공부하기!
    * 개념, 모델 객체 생성 후 앱 구동까지 정리함.
    * 쿼리 수행은 집가서 정리할 것
- uml 공부하기!
- vue 혹은 react 탐사하기!
- 새 프로젝트 구조 정의 및 사용 툴, 요구사항 목록 만들기

#### 집가서

- 이어서 공부
- python clean code 공부

## 190611 (tue) - flask-board, flask-restful

#### 타임라인 수정

- 채팅식이 아니라 히스토리 식으로 구현할 것
- 저장은 디비에

#### user password encryption

- flask-bcrypt 활용하여 패스워드 암호화 하기
- 검증 로직 추가
	* 패스워드 검증
	* unique field 검증 (기존 디비에러 검증에서 바꾸기)

#### flask-restful 나머지 정리

- 참조문서를 통해 충분히 공부하며 정리하기

#### 집가서

- flask-restful 정리
- python clean code 정리
- 회화정리
- 거상
	* 흑룔소환 : 수정단장 or 여와장
	* 신수부 발동? : 조합스킬 굉뇌 >> 가네샤 + 오행기, 고로 가네샤 만들기

## 190610 (mon) - flask-board

#### timeline feature using websocket

- 같은 호스트라면, 브라우저의 서로 다른 탭이라도 상호 구분이 불가능하다
- 게다가 브라우저 closing 이벤트 처리 방법도 잘 모르겠다..
- 오늘 한 작업은 다소 비효율적이고 중복되는 부분들 줄이기였다.
- 타임라인 히스토리 저장
    * db? 별도의 json 파일? 뭘 쓸까??

#### 집가서 할일

- python clean code 정리
- 프로젝트 구조 정의, 요구사항 작성 및 사용 툴 정하기
- 회화 정리

## 190609 (sun) - python clean code

- python clean code 정리

## 190607 (fri) - win7 pro language pack / flask-board timeline

#### lang pack

- 언어팩은 vistalizator 를 활용한다. 이것은 vista 와 win7 용 언어 변경 툴이다.

#### flask-board timeline

- 홈 화면에 실시간으로 반영되는 타임라인을 만들 것
- 실시간 적용을 위해서는 web socket 을 써야 하나?

#### 집 가서?

- flask web socket 공부 후 적용
- 파이썬 클린 코드 정리

## 190605 (wed) - win7_pro / flask-board

#### 부팅..

- 한글판으로 재설치하니 부팅 자체에 문제가 생김
- 컴퓨터 복구를 해도 잘 안됨
- usb 파일에 문제가 있었던가..
- 해결
	* 그냥 영문판 + 한글화 패키지로 처리하기로..

#### 검색 기능 - autocomplete

- 서버 조회 결과 출력하기 완료 (걍 검색결과 alert 으로 띄움)

#### 사용자 게시글 교체

- 회원만 게시글 쓸수 있도록 교체
- view 화면에서 자신의 글 여부에 따라 버튼 분기

#### 타임라인 + 무한 스크롤링

- ui 틀 잡아놓음!

## 190604 (tue) - win7_pro_k / flask-board

#### 업무용 컴퓨터 포매팅 및 한글설치 (Win7 Pro K 정품)

- 문제점
	1. win7 과 intel 스카이레이크 간의 비호환성 문제 - 설치 시 
	2. usb 3.0 인식 문제
	3. 네트워크 드라이버 문제
	4. 한글화 문제
- 해결 (1)
	* iso 파일만을 사용하는 기존의 방식으로는 설치 드라이브 인식 불가의 문제가 발생한다.
	* 그래서 uefi 를 지원하는 메인보드 제조사의 usb patcher 가 필요했음.
	* asus(asrock?) 메인보드였으므로, 해당 홈페이지에서 **Win7UsbPatcher** 를 다운받아
	* iso 이미지와 결합하여 설치용 usb 를 만들었다.
	* 업무용이므로, 설치 시 제품 키를 입력해야 한다.
- 해결 (2)
	* win7 운영체제는 usb3.0 을 인식하지 못한다
	* 사실상 모든 usb 를 인식 못하고, 심지어 2T 짜리 하드도 자동인식하지 못함(이거는 파티션 설정하지 않은 이유인듯)
	* 하드는 추가 파티션 지정으로 해결하고, usb 인식불가 문제는 intel 에서 제공하는 **win7 용 usb driver** 를 다운받음
	* 모든 usb 를 인식하지 못해 다운받은 usb driver 파일을 옮길 방법이 없으므로,
	* 파일이 존재하는 usb 자체로 새롭게 부팅하고,
	* 거기서 ```shift + F10``` 으로 CMD 모드에 들어가 copy 명령으로 기존의 하드 드라이브에 usb 드라이버 파일을 복사했다.
	* 그리고 정상적으로 window 재부팅한 후, 복사된 드라이버 설치 파일을 실행함으로써 해결함 >> 이제는 잘 인식된다
- 해결 (3)
	* 이제 외부 저장소를 인식할 수 있게 되었으므로,
	* 3DP_NET, 3DP_chip 을 설치해 네트워크 드라이브 및 필요 드라이브를 설치/사용 할 수 있게 되었음
- 해결 (4)
	* win7 은 오래된 버전이고 곧 지원이 종료되므로 정품 한글 버전을 확보하기 어려워 우선 영문판으로 설치함
	* 이후 외부 저장소 인식 및 네트워크 연결 문제가 해결되었으므로,
	* (정품인 경우) Windows Update 옵션 중 한글 패키지 업데이트로 해결하거나,
	* (정품인증 x) 한글화 패키지를 별도로 다운받아 설치하는 식으로 해결하면 된다.
	* 그런데 전자는 ultimate 와 enterprise 버전에서만 유효하고, 후자는 라이센스 문제가 발생할 소지가 있으므로, 애초에 professional k 버전을 받는게 가장 낫다
- 추가 (4)
    * windows7 professional k iso 를 구할 수 있다면 바로 해결됨
    * 그러나 공홈에도 없고 신뢰할 수 없는 파일을 아무데서나 받을 수도 없음
    * 구글에서 파일명으로 검색해 나타나는 (이안소프트?)사이트에서 일단 파일을 받긴 했음
    * 순정 파일인지 확인하기 위해 iso 파일의 sha1 해시값을 비교하는 방법을 활용함
    * sha1 변환기 사이트에서 iso 파일을 해싱한 후 값을 비교하는 [샘플 사이트](http://winsha1.yooneed.one/win7ap.html) 에서 비교했더니 일치한다는 판정이 나옴
    * 그래서 이 iso 파일과 usb patcher 를 이용해 부팅 usb 를 다시 만들고 재설치 진행함!

#### flask-board autocomplete 로직 보완 및 ui/ux 작성

- 기존
	* mariadb 에 저장된 countries 정보를 조회해 자동완성
- 목표
	* 같은 기능을 mongodb 에 저장된 데이터로 구현하기
	* 그것을 위해 우선 db 에 다량의 데이터를 삽입하고 적절한 조회 쿼리를 작성하면,
	* 기존의 로직을 응용해서 사용 가능할 것이다
	* 추가적으로 자동완성 화면을 위한 최소한의 ui/ux 도 만들어야 한다
- 과정
	* mongodb countries 컬렉션에 createIndex({code: 1}) 명령으로 unique 인덱스 생성
	* countries json 데이터 삽입(insert, 데이터는 구글링해서 가져옴)
	* 전체 조회
		- db.countries.find({}, {_id: false});
	* name 에 'Republic' 을 포함하는 문서 조회
		- db.countries.find({name: /.*Republic.*/}, {_id: false});
		- 여기서 첫 번째 인자는 filter 를, 두 번째 인자는 projection 을 의미한다
	* pymongo 에서의 조회
		- pymongo 는 python 어플리케이션에서 mongodb 에 connect 하기 위한 모듈이다.
		- 여기서의 명령문은 mongodb 에서의 그것과는 약간 다르다.
		- 전체 조회
			* db\[collection_name].find()
		- name 에 'Republic' 을 포함하는 문서 조회
			* filter 인자에 **{name: /.*Republic.*/}** 이런 형식의 dict 를 바로 넣는 것이 아니라,
			* {'name': {'$regex': 'Republic'}} 이런 식으로 regex 필터를 사용해야 한다.
			* regex 는 regular expression 으로, re 모듈을 이용해 컴파일한 결과를 바로 넣던가, '$regex' 키-값을 추가적으로 작성해 넣어줘야 한다
	* ui/ux : 잘 만들어보자..
		- 포커싱이 가능한 button 태그를 customizing 해서 (어떻게든 끼워맞춰서) 일반적인 autocomplete 처럼 동작하도록 만듦
- 보수 사항
	* 대소문자 구분 문제 >> 대소문자를 구분하므로 서로 다른 조회결과가 나온다
	* ux 부족한 문제 >> 조회된 버튼에 최초 포커스 시 제대로 되지 않음 (한번 더 눌러야 포커싱됨)
	* 서버 조회
		- 이제 선택한 검색어에 대한 결과를 조회해서 보여줘야 한다
		- 추가적인 구상이 필요한 부분!

#### 사용자 게시글 교체

- 테스트 유저로 등록된 게시글들을 삭제하고
- 사용자가 등록하는 글로 교체한다

#### 타임라인 x 무한 스크롤링

- jquery, ajax 활용
- 일단 타임라인 기능을 구현하고,
- 별도의 클릭 없이 스크롤링으로만 타임라인이 계속해서 나타나도록 구현할 것
	* 조회문에서 몇 번째 글부터 몇 번째 글까지 가져올지에 대한 제약조건을 걸어야

## 190603 (mon) - flask-board / autocomplete 구현

- 쉽지는 않았지만 생각했던대로 하나씩 구현은 가능했음
- jquery.ui 나 devbridge 플러그인이 존재했지만, 내 마음대로 사용하기가 까다로움
- 아예 모든걸 내가 만들어보기로 함
- 문제는 트래픽이 많아진다는 사실을 가정하면 발생할 성능이슈인데, 지금은 구현을 먼저 해보기로 함

#### 퇴근하기 전까지 만든 것

- custom autocomplete 를 기능적인 측면에서는 거의 다 구현했음
- keyup 이벤트를 시작으로 서버 조회 후 결과를 button 리스트로 화면에 뿌려주고,
- 각 조건에 따라 키보드 방향 키 위 아래 입력 시 각 항목을 focusing 하도록 함
- 각 항목 식별은 jquery 의 **.is()** 함수과 **:first-child** or **:last-child** 선택자를 이용했음
- ux 를 고려해 이쁘게 표현하는 것이 추가적으로 필요함
- 정리
    * 사실상 서버 조회 로직은 ajax 를 활용하여 매우 단순하다.
    * 분기가 많고 복잡한 부분은 브라우저 스크립트 부분이다.
    * 어차피 한 사용자는 한 번에 하나의 브라우저만 사용할 것이므로, 화면 단에서의 (이 정도) 복잡성은 전혀 문제되지 않을 듯 하다
    * 성능이슈를 고려한다면, ajax 통신으로 서버에 json 데이터를 날리고, db connection 으로 쿼리 조회 후 결과를 응답하는 과정을 최적화해야 할 것이다.

#### 내일 할 것

- 무한 스크롤링 구현 >> 이것도 jquery, ajax 등을 잘 활용하면 될것 같음
- 컴퓨터 포맷 : 일단은 win7 과 스카이레이크 간 호환이 안되는 관계로 asus 사의 설치 usb 만드는 툴로 만듦 > 잘 설치될지는 내일 테스트할 것

## 190601 (sat) - flask-board / 프로젝트 구조 재정비

- 기존 flask-board 의 메인 모듈은 app.py 하나였다.
- 여기에서 (1)```app``` 생성과 (2)모든 라우팅, (3)앱 구동 의 전 과정을 담당했다
- 물론 이렇게 구성해도 동작에 문제는 없지만, 추후 application 확장 및 유지보수 측면에서 시간에 따라 불리해진다.

#### 구동 모듈 분리

- ```(1)app 생성과 (2)모든 라우팅, (3)앱 구동``` 이 세 가지를 키워드로 분리한다
- app 생성
    * flask-board 모듈만을 위한 app 을 생성한다.
    * API 로써 외부 호출에 의해 생성한 app 을 반환해야 하므로, ```__init__.py``` 파일을 생성한다.
- routing
    * 한 프로젝트 application 안에서도 각 기능에 따라 수많은 경로로 라우팅된다.
    * 이러한 라우팅을 하나의 app 파일에 모두 넣는 것은 유지보수에 좋지 않다.
    * 그러므로 각 기능에 따른 라우팅 코드를 따로 작성해서 import 하는 방식으로 사용하는 것이 보기에 좋다.
- 구동
    * 구동은 외부에서 한다.
    * 구동을 위한 run 파일은, flask-board 뿐 아니라 여러 웹앱을 구동하도록 각각 작성할 수 있다.
    * 구동 모듈을 따로 만드는 이유는, 한 디렉토리 내에서 여러 웹앱을 일목요연하게 구동하기 위함이다.
    * 아니라면 일일히 각 웹앱에 접근해서 따로 구동해야 할 것이다.
- 이 외에도 부가적인 이유가 있을 수 있지만, 차차 생각나는 대로 정리하자.



