
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190612-lightgray.svg?style=flat-square)

# SQLAlchemy 사용법!

참조 : [건프의 소소한 개발 이야기](https://ljs93kr.tistory.com/59) 에서 주로 참조함.

> Object Relational Mapping

> SQLAlchemy 는 다양한 파이썬 프로젝트에서 관계형 DB 와 논리적으로 연동하여 편리하게 객체지향적 개념을 구현할 수 있는 모듈이다. 이로써 Database 와 웹앱 프로젝트가 따로 놀지 않고 한 몸처럼 운용될 수 있다!

## 간단정리

- **Flask** 프로젝트에서 사용하는 **flask_sqlalchemy** 과 **MySql** 접속 드라이버인 **pymysql** 을 기준으로 설명한다.
- pymysql 은 SQLAlchemy 의 database uri 설정 시 자동으로 사용되는것 같다

#### 1. 설치
```text
> pipenv install flask_sqlalchemy, pymysql
```
- flask_sqlalchemy 모듈을 설치하면, sqlalchemy 모듈도 같이 설치된다.

#### 2. 어플리케이션에서 import 할 model 정의

- 먼저 데이터베이스 테이블에 매핑될 모델 객체를 정의한다. 정의는 class 기반으로 한다.
- SQLAlchemy 는 자신이 포함한 모듈들을 활용해 database table 내 모든 컬럼 속성 및 제약조건 등을 모델 객체에 지정할 수 있도록 한다.
- 모델 코드를 보면 이해가 쉽다.

/orm/models/models.py
```python
from flask_sqlalchemy import SQLAlchemy
from datetime import datetime

db = SQLAlchemy()

class User(db.Model):
    __tablename__ = 'users'
    __table_args__ = {'mysql_collate': 'utf8_general_ci'}
    
    email = db.Column(db.String(100), primary_key=True, unique=True)
    username = db.Column(db.String(50))
    pwd = db.Column(db.String(200))
    nickname = db.Column(db.String(50))
    profile = db.Column(db.String(200))
    auth = db.Column(db.Integer(1))
    created = db.Column(db.Datetime)
    
    def __init__(self, email, pwd, username, nickname, profile=None):
        self.email = email
        self.username = username
        self.pwd = pwd
        self.nickname = nickname
        self.profile = profile
        self.auth = 1
        self.created = datetime.utcnow()
    
    def __repr__(self):
        return ''
    
    def as_dict(self):
        return {x.name: getattr(self, x.name) for x in self.__table__.columns}
```

- 모델 객체인 User 클래스가 상속받은 db.Model 은 SQLAlchemy 에 설정된 기본적인 db 테이블 환경정보를 가지고 있다.
- 내가 구현하고자 하는 객체는 이것을 상속함으로써 테이블을 위한 기초 틀을 갖추게 된다.
- tablename : 생성하거나 사용할 테이블 이름
- table_args : 인코딩을 위한 인자
- db.Column 등 모든 속성들 : 컬럼, 컬럼의 속성을 의미한다.
- init, repr 메서드 : 특별한 이유가 없다면 굳이 구현하지 않아도 된다. 알다시피 전자는 테이블 컬럼 초기화를 위해 호출되고, 후자는 해당 클래스를 요약하는 문자열을 반환한다.
- as_dict : 쿼리의 결과는 QueryObject 인데, 이것을 dict 타입으로 자동 변환해준다고 한다..

#### 3. 어플리케이션 단에서의 구현

- 메인 패키지명은 **orm** 으로써, 이것 안에 flask application 과 sqlalchemy 테스트를 위한 코드가 포함되어 있다. 외부에서 호출하여 사용.

/orm/\_\_init\_\_.py
```python
from flask import Flask
from orm.models import models

db = models.db

def create_app():
    app = Flask(__name__)
    app.config['SECRET_KEY'] = 'secret key'
    app.config['SQLALCHEMY_DATABASE_URI'] = 'mysql+pymysql://flask_user:0000@localhost/flaskdb' # mysql://username:password@DB_IP/DB_NAME
    app.config['SQLALCHEMY_TRACK_MODIFICATIONS'] = False
    db.init_app(app)
    return app
```

- 따로 정의된 models 로부터 원하는 객체 모델(User 등)을 import 하는 방식.
- 이 예제의 경우 모든 모델은 models.py 내에 만들어져 있고, ```db = SQLAlchemy()``` 객체 또한 이 파일 내부에 있으므로,
- import 한 models 로부터 db 객체를 얻어와 현재 flask application (== app) 을 기반으로 ```init_app(app)``` 한다.
- ```create_all()``` 함수는 데이터베이스 내에 명시한 테이블이 존재하지 않으면 모두 생성한다. (CREATE TABLE)
- 그러나 여기서 문제점은 ```init_app(app)``` 과 ```create_all()``` 이 함께 있으면 후자에서 에러가 발생한다는 것인데,

    ```text
    create_all() : No application found. Either work inside a view function or push an application context.
    ```
- 테이블을 생성하는 이 로직을 **view function** 내부에서 수행하거나, **application context** 에서 수행하라는 메시지이다.
- 즉, 근본없이 디비 초기화를 하지 말고, SQLAlchemy 가 적용된 application 내부에서 디비 관련 작업을 행하라는 것이다.
- 아마 이것 외에 다른 모든 작업(CRUD)들도 application context 내에서 수행하지 않으면 같은 에러를 내지 않을까 싶다.
- 그럼 이제 create_app 을 호출하여 앱을 생성 후 구동시키면 된다

#### 4. run for application

- 외부 구동 파일에서 orm 패키지 모듈을 import 하여 앱을 구동한다.

orm_run.py
```python
from orm import db, create_app

app = create_app()

if __name__ == '__main__':
    with app.app_context():
        db.create_all()
    app.config['DEBUG'] = True
    app.run()
```

- 여기서 주목할 점은 with 구문 내부이다.
- 앞서 설명했듯이, SQLAlchemy 기반 db 관련 작업은 근본없는 장소가 아닌 application 이 구동중이 장소 내부여야 한다고 했다.
- create_all() 은 연결된 db 에 자신에게 정의된 테이블이 이미 있으면 사용하고 없으면 새로 생성하는 작업을 수행하므로,
- **application context** 를 with 구문을 통해 열고 해당 로직을 수행했다.
    * mariadb 에 접속된 heidisql 에서 이 작업이 수행 된 후 리로드를 해보면, 여러 컬럼 속성 및 제약조건과 함께 users 테이블이 생성된 것을 확인할 수 있다.
- 이후 context manager 에 의해 컨텍스트가 닫히고, 최종적으로 app 이 구동된다.

#### SQLAlchemy 를 활용한 CRUD 는 다음 게시글에서 설명한다.



