
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190621-lightgray.svg?style=flat-square)

# [파이썬스러운 코드] Context Manager

## 컨텍스트 관리자를 사용해서 구현한다면?

- 컨텍스트 관리자를 구현함에 있어서,
- **\_\_enter\_\_** 와 **\_\_exit\_\_** 매직 메서드를 구현하는 것만이 유일한 방법은 아니다.
- 표준 라이브러리인 **contextlib** 모듈을 사용하여 보다 간결하고 쉽게 구현 가능하다.

#### contextmanager decorator

- 함수에 contextlib.contextmanager 데코레이트를 적용하면 해당 함수를 컨텍스트 관리자로 변환한다.
- 함수는 generator 형태로 작성되어야 한다.

```python
import contextlib

@contextlib.contextmanager
def db_handler():
    stop_database()
    yield
    start_database()

with db_handler():
    db_backup()
```

- contextlib.contextmanager 어노테이션으로 작성된 db_handler() 함수는 컨텍스트 관리자이자 제너레이터 형태로써,
- \_\_enter\_\_ 로썬 yield 이전의 구문이 실행되고, \_\_exit\_\_ 로썬 이후의 구문이 실행된다.
- yield 부분에서 with 구문으로 열린 후 내부 로직인 db_backup() 이 실행된다.
- 이러한 형식의 장점은 매직 메서드를 구현하지 않음으로써 \_\_enter\_\_ 와 \_\_exit\_\_ 부분을 별도의 함수로 관리한다는 것에 있다. (stop_database(), start_database())
- 이는 종속성을 줄이고 불필요한 지원을 하지 않아도 되며, 기존의 함수를 독립적으로 리팩토링하기 쉬운 이점이 있다.
- 많은 상태를 관리할 필요가 없고 다른 클래스와 독립되어 있는 **context manager function** 을 만드는 경우는 이렇게 하는 것이 좋은 방법이다.
- 이것 외에 컨텍스트 관리자를 만드는 여러 방법이 있다. (contextlib 패키지)


