
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190617-lightgray.svg?style=flat-square)

# [FLASK] Pluggable View 정리

> Flask 는 매력적인 경량 프레임워크임에 분명하지만, app 모듈에서 서버를 구동하면 mapping-function 방식으로
라우팅이 이루어지므로 표현하고자 하는 페이지나 기능이 많아지면 많아질수록 하나의 파일에 모든 view function 이 축적되게 된다.

> 이것은 SW 공학적으로나 장기적인 유지보수 측면에서 그리 좋은 현상이 아니므로, 개선을 위해 'Pluggable View' 기능을 활용한다.

## Concept of Pluggable View

- 축적되는 라우팅 정보를 기준에 따라 분리해 별도의 클래스로 관리한다.
- main app 에서는 각 클래스에 대한 url 을 각 클래스 기반으로 등록하고, HTTP methods 나 parameter 등 필요한 정보를 간략히 전달한다.
- 등록된 클래스 자체는 view function 으로 변환되어 호출된다.

## [번역] Flask 공식 문서 [Pluggable Views](http://flask.pocoo.org/docs/1.0/views/)

- Flask 0.7 은 function 대신 class 기반으로 구성된 **Django 의 the generic views** 로부터 영감을 받은 **pluggable views** 를 소개하고 있다.
- 이것의 메인 컨셉은, ```you can replace parts of the implementations``` 와 ```customizable pluggable views``` 를 제공함에 있다.

#### Basic Principle

- db 로부터 객체 리스트를 로드하여 템플릿에 renders 하는 함수가 있다고 생각해 보자.
```python
@app.route('/users/')
def show_users(page):
    users = User.query.all()
    return render_template('users.html', users=users)
```
- 이것은 (자체로도 충분히) 단순하고 유연하지만,
- 당신은 이 view 를 다른 모델이나 템플릿에 적용될 수 있는 **a generic fashion** (일반화) 에 두고자 하거나, 더 많은 유연성을 원할 수도 있다.
- 이것이 **pluggable class-based views** 가 등장한 배경이다.
- 위 view 를 클래스 베이스 뷰로 바꾸는 첫 step 은 다음과 같다.
```python
from flask.views import View

class ShowUsers(View):
    def dispatch_request(self):
        users = User.query.all()
        return render_template('users.html', objects=users)

app.add_url_rule('/users/', view_func=ShowUsers.as_view('show_users'))
```
- 보다시피 할 일은 **flask.views.View** 의 subclass 를 만들고 **dispatch_request()** 를 구현하는 것이다.
- 그리고 **as_view()** 클래스 메서드를 활용해 이 class 를 **actual view function** 으로 변환해야 한다.
- 이 함수로 넘기는 문자열은 해당 view 가 갖게 될 ```the name of the 'endpoint'``` 이다.
    ```text
    보통 일반적인 view function 에서는 매핑된 함수의 이름이 곧 endpoint 이다.
  reverse_lookup 시에 이 endpoint 를 활용한다.
    ```
- 그럼 코드를 약간 refactor 해보자.
```python
from flask.views import View

class ListView(View):
    def get_template_name(self):
        raise NotImplementedError()
    
    def render_template(self, context):
        return render_template(self.get_template_name(), **context)
    
    def dispatch_request(self):
        context = {'objects': self.get_objects()}
        return self.render_template(context)

class UserView(ListView):
    def get_template_name(self):
        return 'users.html'
    
    def get_objects(self):
        return User.query.all()
```
- 이것은 이같이 작은 규모의 예제에서는 그다지 도움이 되지 않을 수도 있지만, 기본 원리를 설명하는 데는 충분히 좋다.
- 당신이 class-based view 를 가지고 있을 때 ```self``` 가 가리키는 바가 무엇인지 궁금할 수 있다. (When you have a class-based view the question comes up what self points to.)
- 이것이 동작하는 방식은, 클래스 기반으로 새롭게 만들어진 인스턴스로부터 the request 가 전해지는 시점이라면 언제든지 URL 로부터 온 파라미터와 함께 **dispatch_request()** 메서드가 호출되는 과정을 포함한다.
- 클래스 자신은 **as_view()** 함수로 넘어온 파라미터 (name of endpoint, template name 등) 와 함께 인스턴스화 된다.
- 예를 들어 다음과 같이 클래스를 작성할 수 있다.
```python
class RenderTemplateView(View):
    def __init__(self, template_name):
        self.template_name = template_name
    
    def dispatch_request(self):
        return render_template(self.template_name)
```
- 그리고 이것을 다음과 같이 등록할 수 있다.
```python
app.add_url_rule('/about', view_func=RenderTemplateView.as_view('about_page', template_name='about.html'))
```

#### Method Hints

- Pluggable views 는 ```route() 를 사용하는 것``` 또는 ```(더 나은) add_url_rule()``` 중 하나에 의해서 application 에 일반 함수처럼 attached 되어 있다.
- 그러나 그것은 또한 당신이 HTTP methods 의 name 을 제공해야 한다는 것을 의미한다.
- 그 정보를 클래스 내부로 이동시키기 위해서, 다음의 information 을 가진 **methods** 속성을 제공할 수 있다.
```python
class MyView(View):
    method = ['GET', 'POST']
    
    def dispatch_request(self):
        if request.method == 'POST':
            ...
        ...

app.add_url_rule('/myview', view_func=MyView.as_view('myview'))
```

#### Method Based Dispatching (메서드 기반 dispatching)

- **RESTful API** 를 위해서, 각각의 HTTP method 에 따른 서로 다른 함수를 실행하는 것은 특히 유용하다.
- **flask.views.MethodView** 를 활용함으로써 이것을 쉽게 할 수 있다.
- 각 HTTP method 는 같은 이름을 가진 함수에 매핑된다 (just in lowercase)
```python
from flask.views import MethodView

class UserAPI(MethodView):
    def get(self):
        users = User.query.all()
        ...
    
    def post(self):
        user = User.from_form_data(request.form)
        ...

app.add_url_rule('/users/', view_func=UserAPI.as_view('users'))
```
- 이 때는 별도의 **methods** 속성값을 전달하지 않아도 된다. 클래스 내부 함수 정의에 따라 자동으로 세팅된다.

#### Decorating Views

- 라우팅 시스템에 추가된 view class 자체로서는 view function 은 아니므로, 이 클래스를 decorate 한다는 것은 전혀 말이 되지 않는다.
- 대신 당신은 **as_view()** 의 return 값을 직접 decorate 해야 한다.
```python
def user_required(f):
    """Checks whether user is logged in or raises error 401."""
    def decorator(*args, **kwargs):
        if not g.user:
            abort(401)
        return f(*args, **kwargs)
    return decorator

view = user_required(UserAPI.as_view('users'))
app.add_url_rule('/users/', view_func=view)
```
- Flask 0.8 버전으로 시작한다면 클래스를 정의할 때 적용할 decorator 리스트를 특정하는 대체적인 방법이 있다.
```python
class UserAPI(MethodView):
    decorators = [user_required]
```
- 그러나 caller's perspective 로부터 온 함축적인 **self** 로 인해 당신은 개별적인 메서드에서 평범한 view decorators 를 사용할 수 없다는 점을 명심하라.

#### Method Views for APIs (API 를 위한 method views)

- 웹 API 는 종종 HTTP 동작과 매우 밀접하게 작동하기 때문에 **MethodView** 와 같은 API 기반의 구현체를 구현하는 데 많은 아이디어를 제공했다.
- 이는 해당 API 가 대부분의 경우 같은 view 메서드로 향하도록 하는 서로 다른 URL rules 을 요구할 것이라는 사실을 당신이 깨닫게 된다는 것을 말한다.
- 예를 들어 웹에 user object 를 노출시킨다고 생각해 보자.

|URL|Method|Description|
|:---|:---|:---|
|/users/|GET|Gives a list of all users|
|/users/|POST|Creates a new user|
|/users/<id>|GET|Shows a single user|
|/users/<id>|PUT|Updates a single user|
|/users/<id>|DELETE|Deletes a single user|

- 그럼, **MethodView** 를 활용해서 어떻게 (위 표와 같은) 각 행위가 동작하도록 할 수 있는가?
- 정답은, 동일한 view 에 대해 복수의 url rule 을 제공할 수 있다는 사실을 십분 활용하는 것이다.
- the view 가 다음과 같이 생겼다고 잠시 가정해보자.
```python
class UserAPI(MethodView):

    def get(self, user_id):
        if user_id is None:
            # return a list of users
            pass
        else:
            # expose a single user
            pass

    def post(self):
        # create a new user
        pass

    def delete(self, user_id):
        # delete a single user
        pass

    def put(self, user_id):
        # update a single user
        pass
```
- 어떻게 라우팅 시스템에서 이것들을 각각 가져올 수 있을까?
- 바로 각각의 메서드에 두 개의 url rules 와 명시적인 mentioning 을 하는 것이다.
```python
user_view = UserAPI.as_view('user_api')
app.add_url_rule('/users/', defaults={'user_id': None}, view_func=user_view, methods=['GET',])
app.add_url_rule('/users/', view_func=user_view, methods=['POST',])
app.add_url_rule('/users/<int:user_id>', view_func=user_view, methods=['GET', 'PUT', 'DELETE'])
```
- 만약 비슷한 형태의 많은 API 가 있다면 다음 '등록 코드' 와 같이 수정할 수 있다.
```python
def register_api(view, endpoint, url, pk='id', pk_type='int'):
    view_func = view.as_view(endpoint)
    app.add_url_rule(url, defaults={pk: None}, view_func=view_func, methods=['GET',])
    app.add_url_rule(url, view_func=view_func, methods=['POST',])
    app.add_url_rule('%s<%s:%s>' % (url, pk_type, pk), view_func=view_func, methods=['GET', 'PUT', 'DELETE',])

register_api(UserAPI, 'user_api', '/users/', pk='user_id')
```


