
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190707-lightgray.svg?style=flat-square)

# Modular Applications with Blueprints

> [Flask 1.0.2 documentation 번역](http://flask.pocoo.org/docs/1.0/blueprints/)

Flask 는 어플리케이션 컴포넌트를 만들고 해당 앱 또는 앱 간의 common patterns 를 지원하기 위해 blueprint 개념을 사용한다.
Blueprint 는 대규모 어플리케이션의 작업과정을 굉장히 단순화시킬 수 있으며, 어플리케이션에 operation 을 등록하기 위한 Flask 확장의 중심 수단을 제공한다.
Blueprint 객체는 Flask 어플리케이션 객체와 유사하게 동작하지만, 그렇다고 해서 그것이 어플리케이션인 것은 아니다.
오히려 그것은 어플리케이션을 어떻게 구조화하거나 확장하는지에 대한 청사진이다.

## Why Blueprints?

Flask 에서의 Blueprint 는 다음의 경우들이 고려되었다.

```text
    - blueprint 세트에 투입할 어플리케이션 인자. 이는 큰 규모의 어플리케이션에 적합한데,
        해당 프로젝트는 어플리케이션 객체로 인스턴스화되며, 여러 확장들로 초기화되고, 청사진의 컬렉션으로 등록된다.
    
    - 어플리케이션에 URL prefix 나 subdomain 으로 청사진 등록. URL prefix/subdomain 의 파라미터는 blueprint 의 모든 뷰 함수에 걸쳐서
        common view 인자(with defaults) 가 된다.
    
    - 어플리케이션에 서로 다른 URL 규칙으로써 여러 번에 걸쳐 청사진을 등록.
    
    - blueprint 를 통해 템플릿 필터, 정적 파일, 템플릿, 그 외 여러 utilities 를 제공한다. blueprint 는 어플리케이션이나 뷰 함수에 구현할 필요가 없다.
    
    - Flask 확장을 초기화할 때 이러한 케이스들을 위해 어플리케이션에 blueprint 를 등록
```

Flask 의 blueprint 는 사실상 어플리케이션이 아니기 때문에 pluggable app 이 아니다 - 여러 경우에서 이것은 어플리케이션에 등록될 수 있는 운영셋이다.
여러 어플리케이션 객체가 존재하지 않는 이유는 무엇인가? 당신은 여러 객체를 만들 수 있지만(see [Application Dispatching](http://flask.pocoo.org/docs/1.0/patterns/appdispatch/#app-dispatch)),
각 app 은 분리된 환경을 가지게 될 것이고, WSGI 계층에 의해 관리될 것이다.

blueprint 는 대신 Flask level 에서 app 환경을 공유하는 separation 을 제공하고, 등록을 필요로 하도록 어플리케이션 객체를 변경할 수 있다.
단점은, 한번 application 이 생성되면 모든 app 객체가 제거되지 않는 한 blueprint 를 등록 해제할 수 없다는것이다.

## The Concept of Blueprints

blueprint 의 기본 개념은, 어플리케이션에 등록될 때 실행할 동작을 기록한다는 것이다. Flask 는 request 를 보내고 하나의 endpoint 로부터 또다른 endpoint 로 URLs 를 생성할 때 blueprint 를 이용해 뷰 함수와 협력한다.

## My First Blueprint

이것은 가장 기본적인 blueprint 의 형태이다. 이 경우 정적 템플릿을 단순히 rendering 하는 blueprint 를 구현한다.

```python
from flask import Blueprint, render_template, abort
from jinja2 import TemplateNotFound

simple_page = Blueprint('simple_page', __name__, template_folder='templates')

@simple_page.route('/', defaults={'page': 'index'})
@simple_page.route('/<page>')
def show(page):
    try:
        return render_template('pages/%s.html'% page)
    except TemplateNotFound:
        abort(404)
```

```@simple_page.route``` 데코레이터의 도움으로 함수를 바인드할 때 blueprint 는 어플리케이션에 ```show``` 함수를 등록하고자 하는 의도를 기록한다.
추가적으로 이것은 **Blueprint** 생성자에 의해 주어진 blueprint 이름을 함수의 endpoint 에 prefix 로써 붙일 것이다. (이 경우, **simple_page**)

## Registering Blueprints

그렇다면 어떻게 blueprint 를 등록하는가? 다음을 보자.

```python
from flask import Flask
from yourapplication.simple_page import simple_page

app = Flask(__name__)
app.register_blueprint(simple_page)
```

만약 당신이 어플리케이션에 등록된 rules 를 확인한다면, 다음의 것들을 찾을 것이다.

```text
[<Rule '/static/<filename>' (HEAD, OPTIONS, GET) -> static>,
 <Rule '/<page>' (HEAD, OPTIONS, GET) -> simple_page.show>,
 <Rule '/' (HEAD, OPTIONS, GET) -> simple_page.show>]
```

첫 번째 것은 정적 파일을 위해 명시적으로 어플리케이션 자체로부터 온 규칙이다. 다른 두 규칙은 ```simple_page``` blueprint 의 show 함수를 위한 것이다.
보다시피, 이것들은 또한 blueprint 의 이름으로 prefixed 되어 있고, 점(.)으로 구분되어 있다.

그러나 blueprint 는 서로 다른 locations 에 mounted 될 수 있다.

```python
app.register_blueprint(simple_page, url_prefix='/pages')
```

그리고 두말할 것 없이 다음의 규칙들이 생성된다.

```text
[<Rule '/static/<filename>' (HEAD, OPTIONS, GET) -> static>,
 <Rule '/pages/<page>' (HEAD, OPTIONS, GET) -> simple_page.show>,
 <Rule '/pages/' (HEAD, OPTIONS, GET) -> simple_page.show>]
```

첫 번째 것은 모든 blueprint 가 적절하게 응답하지 않더라도 당신이 blueprints 를 여러 번 등록할 수 있다는 것을 의미한다.
사실 한 번 이상 마운트될 수 있는지 여부는 blueprint 가 어떻게 구현되었는지에 달려 있다.

## Blueprint Resources

Blueprint 는 게다가 여러 자원들을 제공할 수 있다. 때때로 당신은 오직 리소스 제공 목적으로만 blueprint 를 활용하기를 원할 수도 있다.

## Blueprint Resource Folder

일반적인 어플리케이션들처럼 blueprint 는 folder 안에 포함되도록 고안된다. 복수의 blueprint 들은 같은 폴더로부터 출발하도록 되어 있지만,
그것이 의무적인 case 도 아니고 보통 그렇게 요구되지도 않는다.

그 폴더는 일반적으로 **\_\_name\_\_** 으로 규정된 Blueprint 의 두 번째 인자에 의해 추론된다.
이 인자는 어떤 논리적 파이썬 모듈이나 패키지가 blueprint 에 대응되는지를 특정한다.
만약 이것이 활성화된 파이썬 패키지를 가리킨다면, 그 패키지(filesystem 의 폴더)는 **resource folder** 이다.
만약 이것이 모듈이라면, 해당 모듈이 포함된 패키지가 **resource folder** 가 된다.
당신은 resource folder 가 무엇인지 확인하기 위해 **Blueprint.root_path** 속성으로 접근할 수 있다.

```python
>>> simple_page.root_path
'/Users/username/TestProject/yourapplication'
```

폴더로부터 빠르게 소스를 열기 위해 당신은 **open_resource()** 함수를 사용할 수 있다.

```python
with simple_page.open_resource('static/style.css') as f:
    code = f.read()
```


## Static Files

blueprint 는 ```static_folder``` 인자를 통해 파일시스템의 폴더로 향하는 경로를 제공하는 static files 를 활용함으로써 폴더를 나타낼 수 있다.
이것은 완전한 경로이거나 blueprint's location 에 상대적인 경로이다.

```text
admin = Blueprint('admin', __name__, static_folder='static')
```

기본적으로 path 의 가장 오른쪽 부분이 웹상에 나타난다. 이것은 ```static_url_path``` 인자에 의해 바뀔 수 있다.
왜냐하면 그 폴더는 여기서 ```static``` 으로 불리기 때문에, blueprint + ```/static``` 조합의 ```url_prefix``` 에서 활용 가능할 것이다.
만약 blueprint 의 prefix 가 ```/admin``` 이라면, 정적 URL 은 ```/admin/static``` 이 될 것이다.

엔드포인트는 ```blueprint_name.static``` 으로 명명된다. 어플리케이션의 정적 폴더를 지정하는 것처럼 ```url_for()``` 를 활용해 URL 을 생성할 수 있다.

```python
url_for('admin.static', filename='style.css')
```

그러나, blueprint 가 ```url_prefix``` 를 가지고 있지 않다면, blueprint 의 정적 폴더로 접근하는 것은 가능하지 않다.
왜냐하면 이 경우는 URL 이 ```/static``` 으로 되기 때문인데, 어플리케이션의 ```/static``` 라우트가 우선적으로 선점하고 있기 때문이다.
template folders 와는 다르게, blueprint static folders 는 어플리케이션 정적 폴더에 파일이 존재하지 않는다면 검색되지 않는다.

## Templates

만약 blueprint 가 templates 를 나타내도록 하기를 원한다면, Blueprint 생성자에 **template_folder** 파라미터를 제공함으로써 가능하다.

```python
admin = Blueprint('admin', __name__, template_folder='templates')
```

정적 파일에 관련해서, 경로는 절대적이거나 blueprint resource folder 에 상대적일 수 있다.

템플릿 폴더는 템플릿 검색 경로에 추가되지만, 실제 어플리케이션 템플릿 폴더보다 우선순위가 낮을 것이다.
이런 방식으로 blueprint 가 실제 어플리케이션에 제공하는 템플릿을 쉽게 오버라이드할 수 있다.
이는 또한 blueprint 템플릿이 의도에 관계없이 오버라이드되지 않기를 바란다면, 다른 어떤 blueprint 나 실제 어플리케이션 템플릿도 같은 상대경로를 가지고 있지 않다는 것을 확실하게 해야 한다는 것을 의미한다.
복수의 blueprint 가 같은 상대 템플릿 경로를 제공할 때, 첫 번째로 등록된 blueprint 가 다른 것들에 비해 우선권을 갖는다.

그러므로 ```yourapplication/admin``` 폴더의 blueprint 가 있고, ```'admin/index.html'``` 템플릿을 렌더링하기를 원하면서 ```templates``` 를 **template_folder** 로써 제공받았다면,
당신은 다음과 같은 파일을 생성해야 한다: ```yourapplication/admin/templates/admin/index.html```.
추가적인 ```admin``` 폴더를 생성하는 이유는 실제 어플리케이션 템플릿 폴더의 ```index.html``` 에 나의 템플릿이 오버라이드되는 것을 피하기 위해서이다.

이를 다시 설명하자면, 당신이 ```admin``` 이라는 blueprint 를 가지고 있고 ```index.html``` 로 명명된 템플릿을 렌더링하기 원할 때 가장 좋은 생각은
다음과 같이 템플릿 구조를 짜는 것이다.

```text
yourpackage/
    blueprints/
        admin/
            templates/
                admin/
                    index.html
            __init__.py
```

만약 정확한 템플릿을 로드하는 데 문제가 생긴다면, Flask 로 하여금 모든 ```render_template``` 호출에 지정된 템플릿들을 단계적으로 출력하는 ```EXPLAIN_TEMPLATE_LOADING``` config 변수를 활성화시킬 것.

## Building URLs

만약 하나의 페이지에서부터 다른 페이지를 연결하기 원한다면, URL endpoint 에 blueprint 이름이나 점(.)을 prefix 로 지정하는 것같이 ```url_for()``` 함수를 사용할 수 있다.

```python
url_for('admin.index')
```

추가적으로 blueprint 나 렌더링된 템플릿의 뷰 함수에 있고 같은 blueprint 의 또다른 endpoint 로 연결하기 원한다면,
endpoint 의 prefix 로 오직 점(.)만을 붙임으로써 상대적인 redirects 를 사용할 수 있다.

```python
url_for('.index')
```

이것은, 예를 들자면 최근 요청이 또다른 admin blueprint endpoint 로 dispatched 되는 경우처럼 ```admin.index``` 로 연결될 것이다.

## Error Handlers

Blueprint 는 Flask 어플리케이션 객체가 하는 것처럼 errorhandler decorator 를 지원한다. 그리고 Blueprint-specific custom error pages 를 만드는 것은 쉽다.

다음은 "404 Page Not Found" 예외의 예제이다.

```python
@simple_page.errorhandler(404)
def page_not_found(e):
    return render_template('pages/404.html')
```

대부분의 errorhandlers 는 기대한 대로 단순하게 동작한다. 그러나, 404 와 405 예외 핸들러에 대한 경고가 있다.
이러한 errorhandlers 는 오직 다른 blueprint 의 뷰 함수 내에서 적절한 ```raise``` 구문이나 ```abort``` 호출이 있을 때만 invoked 된다.
이것들은 유효하지 않은 URL 접근과 같이 그 밖의 상황에서는 invoked 되지 않는다.
그 이유는 blueprint 가 특정 URL space 를 "소유"하지 않기 때문인데, 이로 인해 어플리케이션 인스턴스는 유효하지 않은 URL 이 들어왔을 때 어떤 blueprint errorhandler 로 하여금 작동하도록 할지 알 방법이 없게 된다.
만약 당신이 URL prefixes 기반으로 이러한 errors 에 대해서 서로 다른 handling 전략을 실행하고자 한다면, 그것들은 ```request``` 프록시 객체를 사용하여 application level 에서 정의되어야 할 것이다.

```python
@app.errorhandler(404)
@app.errorhandler(405)
def _handle_api_error(ex):
    if request.path.startswith('/api/'):
        return jsonify_error(ex)
    else:
        return ex
```

error handling 에 대하 더 많은 정보는 [Custom Error Pages](http://flask.pocoo.org/docs/1.0/patterns/errorpages/#errorpages) 를 참고할 것.
