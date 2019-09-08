
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190908-lightgray.svg?style=flat-square)

# MVC, MVP, MVVM 비교정리

## 위키 개요

#### [MVC](https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%EC%BB%A8%ED%8A%B8%EB%A1%A4%EB%9F%AC)

Model-View-Controller, 소프트웨어 공학 디자인 패턴의 하나.

이 패턴을 성공적으로 사용하면, 사용자 인터페이스로부터 비즈니스 로직을 분리하여 애플리케이션의 시각적 요소나
그 이면에서 실행되는 비즈니스 로직을 서로 영향 없이 쉽게 고칠 수 있는 애플리케이션을 만들 수 있다.

여기서 모델은 애플리케이션의 정보(데이터), 뷰는 사용자 인터페이스, 컨트롤러는 데이터와 비즈니스 로직 사이의 상호동작을 관리한다.

#### [MVP](https://ko.wikipedia.org/wiki/%EB%AA%A8%EB%8D%B8-%EB%B7%B0-%ED%94%84%EB%A6%AC%EC%A0%A0%ED%84%B0)

Model-View-Presenter, MVC 아키텍처 패턴의 파생 패턴으로, 사용자 인터페이스 개발을 위해 대부분 사용된다.
주된 목적은 View 와 Model 을 완전히 분리하기 위함이며, Model 의 역할인 비즈니스 로직을 독립적으로 테스트할 수 있다.

여기서 프리젠터는 "middle-man" 의 기능을 담당하는데, 모든 프레젠테이션 로직은 프리젠터로 넘어간다.

![MVP pattern](https://t1.daumcdn.net/cfile/tistory/99CE114B5A7E74F02B)

본질적으로 MVC 와 같지만, 데이터의 결과가 View 에 연결되는 것이 아니라 인터페이스로 연결된다는 점이 다르다.
이에 따라 MVC 가 가진 테스트 가능성 문제와 함께 모듈화/유연성 문제를 해결한다.
결과적으로 Presenter 는 View 와 Model 사이에서 데이터 전달 역할을 한다.

#### [MVVM](https://ko.wikipedia.org/wiki/모델-뷰-뷰모델)

Model-View-View-Model, 소프트웨어 아키텍처 패턴의 하나.

MVVM 은 마크업 언어나 GUI 코드를 통해 [그래픽 사용자 인터페이스](https://ko.wikipedia.org/wiki/%EA%B7%B8%EB%9E%98%ED%94%BD_%EC%82%AC%EC%9A%A9%EC%9E%90_%EC%9D%B8%ED%84%B0%ED%8E%98%EC%9D%B4%EC%8A%A4)
개발을 비즈니스 로직이나 프론트엔드 로직과 분리를 용이케 한다.
여기서 View 모델은 값 변환기이며, 이는 뷰 모델이 모델로부터 데이터 객체를 노출(변환)시키는 일을 맡음으로써 객체들이 쉽게 표현될 수 있게 한다는 의미이다.
이러한 관점에서 뷰 모델은 단순한 뷰 이상의 모델이며 뷰의 디스플레이 로직 대부분을 관리한다.
뷰 모델은 [중재자 패턴](https://ko.wikipedia.org/wiki/%EC%A4%91%EC%9E%AC%EC%9E%90_%ED%8C%A8%ED%84%B4)을 구현할 수 있으며 뷰가 지원하는 유즈 케이스들의 집합 주변 벡엔드 로직에 대한 접근 권한을 정리한다.

## MVC, MVP, MVVM 비교

참조: [마기의 개발 블로그](https://magi82.github.io/android-mvc-mvp-mvvm/)


