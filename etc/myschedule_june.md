
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190531-lightgray.svg?style=flat-square)

# 스케줄

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



