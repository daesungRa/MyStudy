
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190612-lightgray.svg?style=flat-square)

# SQLAlchemy 사용법 - CRUD 쿼리 수행하기

> 이전 글에서 SQLAlchemy 기반의 DB 모델과 그것을 상속받은 사용자 정의 객체 모델을 만들어 app 을 구동했었다.

> 이제 이 SQLAlchemy 를 활용해 insert, select, update, delete 를 실행하는 방법을 알아본다.

- 어플리케이션 단에서 SQLAlchemy 를 활용해 쿼리를 수행하려면, ```db.session``` 과 ```사용자 정의 model``` 을 사용한다.
- 이하는 [공식 문서 - queries](https://flask-sqlalchemy.palletsprojects.com/en/2.x/queries/) 를 번역하였다.

## Inserting Records

- ... 당신의 모든 models 는 이미 '**a constructor**' 를 가지고 있으므로, 추가적으로 하나만 알면 된다.
- 그것은, Constructor 는 SQLAlchemy 내부적으로가 아닌 오직 당신에 의해 사용되므로, 당신이 어떻게 정의하느냐에 달려 있다는 사실이다.
- Inserting data 는 총 세 가지 step process 로 구성된다.
    1. Create the Python object
    2. Add it to the session
    3. Commit the session
- 여기서 말하는 **session** 은 Flask 의 세션이 아니라 Flask-SQLAlchemy 의 것이다.
- 이것은 **database transaction** 의 본질적으로 강화된 버전이다.
- 동작법은 다음과 같다.
```python
from orm.models.models import db, User

user = User('abc@abc.com', '1111', 'myname', 'mynickname', 'myimg.jpg')
db.session.add(user)
db.session.commit()
```
- 그리 어렵지 않다. 어떤 지점에서 무슨 일이 일어나는가?
- 당신이 session 에 object 를 add 하기 이전에, SQLAlchemy 에는 기본적으로 transaction 에 그것을 추가할 plan 이 존재하지 않는다.
- 이것이 좋은 이유는, 당신이 여전히 변화를 취소할 수 있기 때문이다. > (여기서 변화는 db.session 에 object 를 add 하는 이벤트)
- 예를 들어 새 게시글을 페이지에 추가한다고 생각해볼 때, 당신은 게시글을 database 에 저장하는 것이 아니라 단지 preview rendering 의 이유로 템플릿에 전달만 하기를 원할 수도 있다.
- **add()** 함수는 객체를 추가할 때 호출된다.
- 이것은 database 에 **INSERT** 하는 statement 를 나타낼 것이나, 아직 transaction 이 committed 되지 않은 이유로 **ID** 를 즉시 돌려받지는 못할 것이다.
- 만약 commit 을 한다면, (그제서야) 당신의 user 객체는 ID 를 가지게 될 것이다.
```python
print(user.id)
```
```text
1
```

## Deleting Records

- 레코드 삭제도 매우 유사한데, **add()** 대신 **delete()** 를 사용하면 된다.

```python
db.session.delete(user)
db.session.commit()
```

## Querying Records

- 그렇다면 database 로부터 어떻게 data 를 얻어올 수 있을까?
- 이 목적을 위해 **Flask-SQLAlchemy** 는 당신의 Model 에 '**query**' attribute 를 제공한다.
- 이것을 사용하면, 당신은 모든 레코드에 대한 **새로운 query 객체**를 얻게 된다.
- 그러면 **filter()** 와 같은 메서드를 사용할 수 있게 되는데, 이것은 **all()** 혹은 **first()** 에 의해 select 가 수행되기 이전에 레코드를 필터링 해준다.
- 만약 primary key 를 활용하고 싶다면, **get()** 메서드를 사용하면 된다.
- 다음의 쿼리들은 데이터베이스로부터 아래 table 의 항목들을 가져온다.

|id|username|email|
|:---|:---|:---|
|1|admin|admin@example.com|
|2|peter|peter@example.org|
|3|guest|guest@example.com|

#### username 을 활용한 user 검색:
```python
peter = User.query.filter_by(username='peter').first()
print(peter.id)
print(peter.email)
```
result:
```text
2
u'peter@example.org'
```

- filter 내 입력정보에 맞는 데이터가 없다면 None 이 반환된다.

#### 조금 더 어려운 표현을 통한 그룹 검색:
```python
listData = User.query.filter(User.email.endswith('@example.com')).all()
print(str(listData))
```
result:
```text
[<User u'admin'>, <User u'guest'>]
```

#### username 기반의 order by:
```python
sortData = User.query.order_by(User.username).all()
print(str(sortData))
```
result:
```text
[<User u'admin'>, <User u'guest'>, <User u'peter'>]
```

#### Limiting users:
```python
limitData = User.query.limit(1).all()
print(str(limitData))
```
result:
```text
[<User u'admin'>]
```

#### primary key 를 활용한 user 검색:
```python
pkData = User.query.get(1)
print(pkData)
```
result:
```text
<User u'admin'>
```

## View 함수에서의 Queries

- 만약 Flask view 함수를 작성한다면 missing entries 에 대한 404 에러를 반환하기가 매우 편리해진다.
- 왜냐하면, 이것은 매우 일반적인 관용표현인데, Flask-SQLAlchemy 는 정확히 이것과 같은 목적을 위한 **a helper** 를 제공하기 때문이다.
- **get()** 메서드 대신에 **get_or_404()** 를, **first()** 메서드 대신에 **first_or_404()** 메서드를 사용하면 view 함수는 None 을 반환하는 대신에 404 에러를 발생시킨다.
```python
@app.route('/user/<username>', methods=['GET'])
def show_user(username):
    user = User.query.filter_by(username=username).first_or_404()
    return render_template('show_user.html', user=user)
```
- 또한 **abort()** 와 함께 어떤 설명을 추가하고 싶다면, 다음과 같이 **description** 을 인자로 사용할 수 있다.
```python
User.query.filter_by(username=username).first_or_404(description='There is no data with {}'.format(username))
```


