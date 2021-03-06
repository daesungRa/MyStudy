
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190610-lightgray.svg?style=flat-square)

# REST API 와 Flask-Restful 공부하기

> Flask 에서는 어떻게 Restful API 를 구현할까?

## Rest? Rest API? Restful?

> 참조 : [heejeong Kwon blog](https://gmlwjd9405.github.io/2018/09/21/rest-and-restful.html)

#### REST

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

#### Elements of REST

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

#### Feature of REST

- Server-Client 구조
- Stateless
- Cacheable
- Layered System
- Code-On-Demand(optional)
- Uniform Interface - 인터페이스 일관성

#### Concept of REST API

- REST 를 기반으로 API 서비스를 구현한 것을 의미한다.
- 최근 Open API 나 Micro Service 등을 제공하는 벤더 업체 대부분은 REST API 를 사용한다.

#### Feature of REST API

- REST 기반으로 시스템을 분산해 확장성과 재사용성이 제고됨 > 유지보수 및 운용에 편리
- HTTP 기반 Client-Server 구조 API 에 쉽게 적용 가능 > 각종 웹 언어로 제작 가능

#### Basic principle of REST API Architecture

- 리소스의 표현 용어
    * **Document** : 하나의 독립적 자원. 객체 인스턴스나 DB 레코드와 유사한 개념
    * **Collection** : 그러한 독립 자원들의 집합. 서버 단의 각 디렉토리 정도를 의미한다.
    * **Store** : 클라이언트 단에서 관리하는 리소스 저장소
- URI 는 정보의 자원을 표현해야 한다.
    * Resource 는 동사보다는 명사를, 대문자보다는 소문자를 사용한다.
    * Resource 의 도큐먼트 이름은 단수 명사를 사용해야 한다.
    * Resource 의 컬렉션 이름은 복수 명사를 사용해야 한다.
    * Resource 의 스토어 이름은 복수 명사를 사용해야 한다.
        - ex) GET /Member/1 >> GET /members/1
- 자원에 대한 행위는 HTTP Method 로 표현한다.
    * URI 에 HTTP Method 가 들어가면 안 된다.
        - ex) GET /members/delete/1 >> DELETE /members/1
    * URI 에 행위에 대한 동사 표현이 들어가면 안 된다.
        - ex) GET /members/show/1 >> GET /members/1
        - ex) GET /members/insert/2 >> POST /members/2
    * 경로 부분 중 변하는 부분은 **id** 등 유일한 값으로 대체한다( :id 는 하나의 특정 resource 를 나타냄 )
- 슬래시(/) 는 계층 관계를 나타냄
- URI 마지막 문자로 슬래시(/) 를 포함하지 않는다
    * 고유한 특정 리소스를 나타내기 위해 명료한 식별자를 사용해야 한다.
        - ex) ~/house/apartments/ >> ~/house/apartments
- 하이픈(-) 은 URI 가독성을 높이는데 사용한다.
- 언더바(_) 는 URI 에 사용하지 않는다. (가독성의 이유)
- URI 경로에는 소문자를 사용한다. 대문자 사용은 피한다.
- 파일 확장자는 URI 에 포함시키지 않는다.
    * 메시지 바디의 포맷을 나타내기 위해서는 **Accept header** 를 사용한다.
    * ex) http://restapi.example.com/members/soccer/345/photo.jpg (x)
    * ex) http://restapi.example.com/members/soccer/345/photo HTTP/1.1 Host: restapi.example.com Accept: image/jpg (o)
- 리소스 간 연관 관계가 있는 경우 표현 방식
    * /리소스명/리소스 ID/관계가 있는 다른 리소스명
    * ex) GET : /users/{userid}/devices (일반적으로 'has a' 관계를 표현할 때 사용됨)
- REST API 설계 예시

![REST API 설계 예시](https://github.com/daesungRa/MyStudy/blob/master/imgs/flask/restapi-example.png)

#### Concept of RESTful

- RESTful 이란
    * RESTful 은 일반적으로 REST Architecture 를 구현하는 웹 서비스를 나타내기 위해 사용되는 용어이다.
    * 예를 들어, REST API 를 제공하는 웹 서비스를 **RESTful** 하다고 할 수 있다.
    * 엄격한 standard 가 존재하는 것이 아니고, REST 의 원리를 따르는 시스템을 RESTful 하다고 지칭하는 것.
- RESTful 의 목적
    * 이해와 사용이 쉬은 REST API 를 만드는 것
    * 근본적으로 서비스의 성능 향상보다는, 일관적 컨벤션을 통한 이해도 및 호환성을 제고하는 데 목적이 있다.
    * 그러므로 성능이 중요한 곳에서는 RESTful 하게 구현하지 않을 수 있다.
- RESTful 하지 못한 대표적인 경우
    * CRUD 모두를 POST 로만 처리하는 API
    * route 에 resource, id 외의 정보가 들어가는 경우 (/students/updateName)

## Flask-RESTful

> 드디어 flask 다.. 후..

> 참조 : [Flask-RESTful 공식 문서](https://flask-restful.readthedocs.io/en/latest/) 번역

- **Flask-RESTful** 은 **REST APIs** 를 빠르게 building 하기 위한 지원을 추가한 **Flask** 의 확장 모듈이다.
- 이것은 당신의 ORM/libraries 와 함께 동작하는 경량화된 개념이다.
- Flask-RESTful 은 최소한의 setup 으로 최상의 결과를 낼 수 있도록 한다.
- 만약 Flask 에 친숙하다면, Flask-RESTful 도 습득하기 쉬울 것이다.

#### 분류

- [빠른 시작](#Quickstart)

#### Quickstart

- A Minimal API (최소한의 경량 API)
    * 최소한의 API 로써 Flask-RESTful 의 모습은 다음과 같다.
    ```python
    from flask import Flask
    from flask_restful import Resource, Api
    
    app = Flask(__name__)
    api = Api(app)
    
    class HelloWorld(Resource):
        def get(self):
            return {'hello': 'world'}
    
    api.add_resource(HelloWorld, '/')
    
    if __name__ == '__main__':
        app.run(debug=True)
    ```
    * 이것을 저장한 후 실행하면 된다.
    * ```Flask debugging mode``` 를 활성화하면 코드 리로딩과 더 나은 에러 메시지를 보여 준다.
    * 다만, 실제 운영 환경에서 디버그 모드는 절대 활성화되면 안 된다!
- Resourceful Routing (리소스 기반 라우팅)
    * Flask-RESTful 에 의해 제공되는 the main building block 은 Resource(리소스, 자원) 이다.
    * Resource 는 최상단 [Flask pluggable views](http://flask.pocoo.org/docs/views/) 에 build 되어 해당 리소스에 메서드를 정의하는 것만으로 다양한 HTTP 행위(메서드)에 쉽게 접근하도록 한다.
    ```python
    from flask import Flask, request
    from flask_restful import Resource, Api
    
    app = Flask(__name__)
    api = Api(app)
    
    todos = {}
    
    class TodoSimple(Resource):
        def get(self, todo_id):
            return {todo_id: todos[todo_id]}
    
        def put(self, todo_id):
            todos[todo_id] = request.form['data']
            return {todo_id: todos[todo_id]}
    
    api.add_resource(TodoSimple, '/<string:todo_id>')
    
    if __name__ == '__main__':
        app.run(debug=True)
    ```
    * ```add_resource``` 부분에 추가한 리소스 정의에 따라 URI 에는 str 타입의 고유한 id 가 포함되어야 한다.
    * REST 원리에 따라 각 HTTP 메서드 동작에 맞는 메서드가 호출된다.
    * Flask-RESTful 은 view 메서드로부터 반환되는 여러 종류의 return 값을 이해한다.
    * Flask 와 유사하게, iterable 하고 response 객체로 변환 가능한 모든 결과값을 Flask response object 에 담아서 있는 그대로 반환할 수 있다.
    * 또한, 다중 return 값을 활용해서 response code 와 response headers 를 세팅할 수 있다. 아래 코드를 보자.
    ```python
    class Todo1(Resource):
        def get(self):
            # Default to 200 OK
            return {'task': 'Hello world'}
    
    class Todo2(Resource):
        def get(self):
            # Set the response code to 201
            return {'task': 'Hello world'}, 201
    
    class Todo3(Resource):
        def get(self):
            # Set the response code to 201 and return custom headers
            return {'task': 'Hello world'}, 201, {'Etag': 'some-opaque-string'}
    ```
- Endpoints (엔드포인트)
    * 여러 URL 을 API object 의 ```add_resource()``` 메서드에 투입함으로써 당신의 API 는 여러 종류의 URL 을 가질 수 있다.
    * 각각의 URL 은 당신의 Resource 로 라우팅된다.
    ```python
    api.add_resource(HelloWorld,
        '/',
        '/hello')
    ```
    * 또한 당신의 resource methods 에 특정 변수를 경로의 endpoint 부분으로 매치시킬 수 있다.
    ```python
    api.add_resource(Todo,
        '/todo/<int:todo_id>', endpoint='todo_ep')
    ```
- Argument Parsing (인자(매개변수) 파싱)
    * Flask 가 request data 에 대한 쉬운 access 를 제공하는 동안, 전송된 form data 를 validate 하는 것은 여전히 고통스러운 일이었다.
    * Flask-RESTful 은 [argparse](http://docs.python.org/dev/library/argparse.html) 라이브러리와 유사한 [reqparse](https://flask-restful.readthedocs.io/en/latest/api.html#module-reqparse) 모듈을 사용함으로써 request data validation 을 위한 내부적인 지원을 제공한다.
    ```python
    from flask_restful import reqparse
    
    parser = reqparse.RequestParser()
    parser.add_argument('rate', type=int, help='Rate to charge for this resource')
    args = parser.parse_args()
    ```
    * 여기서 **reqparse.RequestParser.parse_args()** 는 파이썬 dict 타입 값을 반환한다.
    * reqparse 모듈을 사용하면 정상적인 에러 메시지가 제공된다.
    * 인자(매개변수)가 유효성 검증에 실패하면, Flask-RESTful 은 400 Bad Request 와 함께 error에 대한 response highlighting 를 respond 할 것이다.
    * **[inputs]()** 모듈은 **inputs.date()**, **inputs.url()** 과 같이 여러 종류의 common conversion 함수를 제공한다.
    * 또한, ```strict=True``` 인자와 함께 **parse_args()** 메서드가 호출되면 당신의 parser 에 정의되지 않은 arguments 가 request 에 포함된 경우 에러가 thrown 된다.
    ```python
    args = parser.parse_args(strict=True)
    ```
- Data Formatting (데이터 포매팅)
    * 기본적으로, 당신의 iterable 반환값에 속한 모든 fields 는 **'as-is'**(현재 상태?) 로 렌더링될 것이다.
    * 이러한 작업은 단지 파이썬 자료구조만을 다루는 동안에는 훌륭하겠지만, objects 와 함께 작업한다면 매우 좌절스럽게 될 것이다.
    * 이러한 문제를 해결하기 위해, Flask-RESTful 은 **[fields](https://flask-restful.readthedocs.io/en/latest/api.html#module-fields) 모듈**과 **[marshal_with()](https://flask-restful.readthedocs.io/en/latest/api.html#flask_restful.marshal_with) decorator**를 제공한다.
    * Django ORM(Object Relational Mapping) 이나 WTForm 과 유사하게, 당신은 response(응답) 에 대한 구조를 describe 하기 위해 **fields 모듈**을 사용할 것이다.
    ```python
    from flask_restful import fields, marshal_with
    
    resource_fields = {
        'task':   fields.String,
        'uri':    fields.Url('todo_ep')
    }
    
    class TodoDao(object):
        def __init__(self, todo_id, task):
            self.todo_id = todo_id
            self.task = task
    
            # This field will not be sent in the response
            self.status = 'active'
    
    class Todo(Resource):
        @marshal_with(resource_fields)
        def get(self, **kwargs):
            return TodoDao(todo_id='my_todo', task='Remember the milk')
    ```
    * 위 예제는 파이썬 객체와 그것의 preparses 가 직렬화되는 과정을 보여준다.
    * **marshal_with()** decorator 는 **resource_fields** 에 의해 describe 된 변환 결과를 apply 할 것이다.
    * 객체로부터 추출된 유일한 field 는 'task' 이다.
    * **[fields.Url](https://flask-restful.readthedocs.io/en/latest/api.html#fields.Url)** field 는 endpoint 이름을 포함하고, response 에서 그 endpoint 를 위한 URL 을 생성하는 특별한 field 이다.
    * 당신이 필요한 수많은 field 타입들은 이미 포함되어 있다. 이를 위해 **[fields](https://flask-restful.readthedocs.io/en/latest/api.html#module-fields)** 가이드를 참조할 것.
- 전체 예제
    * 다음 예제를 저장 후 실행하고, CRUD 에 맞게 테스트해볼 것.
```python
from flask import Flask
from flask_restful import reqparse, abort, Api, Resource

app = Flask(__name__)
api = Api(app)

TODOS = {
    'todo1': {'task': 'build an API'},
    'todo2': {'task': '?????'},
    'todo3': {'task': 'profit!'},
}


def abort_if_todo_doesnt_exist(todo_id):
    if todo_id not in TODOS:
        abort(404, message="Todo {} doesn't exist".format(todo_id))

parser = reqparse.RequestParser()
parser.add_argument('task')


# Todo
# shows a single todo item and lets you delete a todo item
class Todo(Resource):
    def get(self, todo_id):
        abort_if_todo_doesnt_exist(todo_id)
        return TODOS[todo_id]

    def delete(self, todo_id):
        abort_if_todo_doesnt_exist(todo_id)
        del TODOS[todo_id]
        return '', 204

    def put(self, todo_id):
        args = parser.parse_args()
        task = {'task': args['task']}
        TODOS[todo_id] = task
        return task, 201


# TodoList
# shows a list of all todos, and lets you POST to add new tasks
class TodoList(Resource):
    def get(self):
        return TODOS

    def post(self):
        args = parser.parse_args()
        todo_id = int(max(TODOS.keys()).lstrip('todo')) + 1
        todo_id = 'todo%i' % todo_id
        TODOS[todo_id] = {'task': args['task']}
        return TODOS[todo_id], 201

##
## Actually setup the Api resource routing here
##
api.add_resource(TodoList, '/todos')
api.add_resource(Todo, '/todos/<todo_id>')


if __name__ == '__main__':
    app.run(debug=True)
```








