
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190621-lightgray.svg?style=flat-square)

# [파이썬스러운 코드] Context Manager

## 컨텍스트 관리자를 사용해서 구현한다면?

- 컨텍스트 관리자를 구현함에 있어서,
- **\_\_enter\_\_** 와 **\_\_exit\_\_** 매직 메서드를 구현하는 것만이 유일한 방법은 아니다.
- 표준 라이브러리인 **contextlib** 모듈을 사용하여 보다 간결하고 쉽게 구현 가능하다.

#### @contextlib.contextmanager

- 함수에 contextlib.contextmanager 데코레이터를 적용하면 해당 함수를 컨텍스트 관리자로 변환한다.
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
- \_\_enter\_\_ 로써 yield 이전의 구문이 실행되고, \_\_exit\_\_ 로썬 이후의 구문이 실행된다.
- yield 부분에서 with 구문으로 열린 후 내부 로직인 db_backup() 이 실행된다.
- 이러한 형식의 장점은 매직 메서드를 구현하지 않음으로써 \_\_enter\_\_ 와 \_\_exit\_\_ 부분을 별도의 함수로 관리한다는 것에 있다. (stop_database(), start_database())
- 이는 종속성을 줄이고 불필요한 지원을 하지 않아도 되며, 기존의 함수를 독립적으로 리팩토링하기 쉬운 이점이 있다.
- 많은 상태를 관리할 필요가 없고 다른 클래스와 독립되어 있는 **context manager function** 을 만드는 경우는 이렇게 하는 것이 좋은 방법이다.
- 이것 외에 컨텍스트 관리자를 만드는 여러 방법이 있다. (contextlib 패키지)

#### contextlib.ContextDecorator

- 이 클래스는 context manager 내부에서 실행될 함수에 데코레이터를 적용하기 위한 로직을 제공하는 믹스인 클래스이다.

```python
import contextlib

class dbhandler_decorator(contextlib.ContextDecorator):
    def __enter__(self):
        stop_database()
       
    def __exit__(self, ext_type, ex_value, ex_traceback):
        start_database()

@dbhandler_decorator
def offline_backup():
    run("pg_dump database")
```

- 이 구문에서는 with 문 없이 ```offline_backup()``` 함수만 호출하면 decorator 에 의해 enter, exit 이 자동 실행된다.
- 이 접근법의 유일한 단점은 와전히 독립적이라는 것인데, 컨텍스트 관리자 내부에서 사용하고자 하는 객체를 얻을 수 없기 때문에 그렇다.
- with 구문에서는 진입메서드로부터 객체를 얻어올 수 있다.
- decorator 로써의 이점은, 독립적이므로 재사용성이 굉장히 높다는 것이다.

#### contextlib.suppress

```python
import contextlib

with contextlib.suppress(DataConversionException):
    parse_data(input_json_or_dict)
```

- 이 util 패키지는, 파라미터로 전달한 예외 중 하나가 발생한 경우에는 로직이 실패하지 않도록 한다.
- 위 코드는 DataConversionException 예외를 자체적으로 처리하고 있음을 명시하는 것이다.
    * 입력 데이터가 이미 기대한 것과 같은 포맷이어서 변환할 필요가 없으므로 무시해도 안전하다는 의미.


