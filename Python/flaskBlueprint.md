
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190527-lightgray.svg?style=flat-square)

# Modular Applications with Blueprints

> [Flask 1.0.2 documentation 번역](http://flask.pocoo.org/docs/1.0/blueprints/)

- Flask 는 어플리케이션 컴포넌트를 만들고 해당 앱 또는 앱 간의 common patterns 를 지원하기 위해 blueprint 개념을 사용한다.
- Blueprint 는 대규모 어플리케이션의 작업과정을 굉장히 단순화시킬 수 있으며, 어플리케이션에 operation 을 등록하기 위한 Flask 확장의 중심 수단을 제공한다.
- Blueprint 객체는 Flask 어플리케이션 객체와 유사하게 동작하지만, 그렇다고 해서 그것이 어플리케이션인 것은 아니다.
- 오히려 그것은 어플리케이션을 어떻게 구조화하거나 확장하는지에 대한 청사진이다.

## Why Blueprints?

- Flask 에서의 Blueprint 는 다음의 경우들이 고려되었다.
```text
    - blueprint 세트에 투입할 어플리케이션 인자. 이는 큰 규모의 어플리케이션에 적합한데,
        해당 프로젝트는 어플리케이션 객체로 인스턴스화되며, 여러 확장들로 초기화되고, 청사진의 컬렉션으로 등록된다.
    
    - 어플리케이션에 URL prefix 나 subdomain 으로 청사진 등록. URL prefix/subdomain 의 파라미터는 blueprint 의 모든 뷰 함수에 걸쳐서
        common view 인자(with defaults) 가 된다.
    
    - 어플리케이션에 서로 다른 URL 규칙으로써 여러 번에 걸쳐 청사진을 등록.
    
    - blueprint 를 통해 템플릿 필터, 정적 파일, 템플릿, 그 외 여러 utilities 를 제공한다. blueprint 는 어플리케이션이나 뷰 함수에 구현할 필요가 없다.
    
    - Flask 확장을 초기화할 때 이러한 케이스들을 위해 어플리케이션에 blueprint 를 등록
```
- Flask 의 blueprint 는 사실상 어플리케이션이 아니기 때문에 pluggable app 이 아니다 - 여러 경우에서 이것은 어플리케이션에 등록될 수 있는 운영셋이다.
- 여러 어플리케이션 객체가 존재하지 않는 이유는 무엇인가? 당신은 여러 객체를 만들 수 있지만(see [Application Dispatching](http://flask.pocoo.org/docs/1.0/patterns/appdispatch/#app-dispatch)),
- 각 app 은 분리된 환경을 가지게 될 것이고, WSGI 계층에 의해 관리될 것이다.
- blueprint 는 대신 Flask level 에서 app 환경을 공유하는 separation 을 제공하고, 등록을 필요로 하도록 어플리케이션 객체를 변경할 수 있다.
- 단점은, 한번 application 이 생성되면 모든 app 객체가 제거되지 않는 한 blueprint 를 등록 해제할 수 없다는것이다.



