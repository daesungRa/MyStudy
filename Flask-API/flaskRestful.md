
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190610-lightgray.svg?style=flat-square)

# REST API 와 Flask-Restful 공부하기

> Flask 에서는 어떻게 Restful API 를 구현할까?

## Rest? Rest API? Restful?

[heejeong Kwon blog](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)

#### Rest

- Representational State Transfer
- 자원을 이름(자원의 표현)으로 구분하여 해당 자원의 상태(정보)를 주고 받는 모든 것을 의미
- **resource** 의 **representation** 에 의한 **상태 정보 전달**
- 구체적으로, HTTP URI 를 통해 resource 를 명시하고, HTTP Methods 를 통해 해당 자원에 대한 CRUD Operation 을 실현한다.
- 다양한 멀티플랫폼이 존재하는 현재 상황에서 가장 간결하면서도 명료한 통신이 가능하도록 한다.
- 장점
    * HTTP Protocol 을 그대로 사용하므로 별도의 인프라를 구축할 필요가 없다.
    * HTTP 표준 활용, 이것을 활용하는 모든 플랫폼에서 사용 가능
    * Hypermedia API 의 기본을 충실히 지키면서 범용성을 보장한다.
    * REST API 메시지가 의도하는 바를 명확하게 나타내므로 이를 쉽게 파악할 수 있다.
    * 여러 가지 서비스 디자인에서 생길 수 있는 문제를 최소화한다.
    * server 와 client 의 역할을 명확하게 분리한다.
- 단점
    * 표준이 존재하지 않음
    * GET, POST, PUT, DELETE 네 가지 메서드만 사용가능
    * 구형 브라우저 지원 미흡

#### Rest 의 구성 요소

- **Resource** : URI
    * 모든 자원에 고유한 ID 가 존재하고, 이것은 Server 에 저장된다
    * 자원을 구별하는 ID 는 **'/groups/:group_id** 와 같은 방식의 HTTP URI 이다.
    * 이러한 URI 자원은 클라이언트에서 서버로 특정 정보에 대한 handling 을 요청할 때 이용된다.
- **Verb** : HTTP Method
    * HTTP Protocol 의 Method 를 그대로 사용.
    * GET, POST, PUT, DELETE
- **Representation** : Representation of Resource
    * 클라이언트가 서버로 자원의 상태에 대한 핸들링 요청을 하면 Server 는 적절한 Representation 을 보낸다.
    * REST 에서 하나의 자원은 **JSON**, **XML**, **TEXT**, **RSS** 등 여러 형태로 Representation 된다.

#### REST 의 특징

- Server-Client 구조
- Stateless
- Cacheable
- Layered System
- Code-On-Demand(optional)
- Uniform Interface - 인터페이스 일관성

#### REST API 의 개념

- REST 를 기반으로 API 서비스를 구현한 것을 의미한다.
- 최근 Open API 나 Micro Service 등을 제공하는 벤더 업체 대부분은 REST API 를 사용한다.

#### REST API 의 특징

- REST 기반으로 시스템을 분산해 확장성과 재사용성이 제고됨 > 유지보수 및 운용에 편리
- HTTP 기반 Client-Server 구조 API 에 쉽게 적용 가능 > 각종 웹 언어로 제작 가능

## Flask-RESTful

- **Flask-RESTful** 은 **REST APIs** 를 빠르게 building 하기 위한 지원을 추가한 **Flask** 의 확장 모듈이다.
- 



