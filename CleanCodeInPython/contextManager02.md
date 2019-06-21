
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190611-lightgray.svg?style=flat-square)

# [파이썬스러운 코드] Context Manager

> 컨텍스트 관리자가 유용한 이유는, 사실상 모든 코드에 적용될 수 있는 패턴이기 때문이다. 이것은 사전조건과 사후조건을 가지고 있다.

> 일반적으로 열거나 닫아야 하는 Resource(리소스) 관리를 위해 사용된다.

## try-finally 와의 비교

- 나에게 익숙한 try-finally 구문으로 파일을 여닫는 예제를 작성하면 다음과 같다.
```python
...

file = open(filename)
try:
    process_file(file)
finally:
    file.close()

...
```
- 먼저 파일을 열고 특정 process 를 수행하는 위 예제는, finally 구문을 통해 필수적으로 파일을 close 하도록 강제하고 있다.
- 파이썬만의 Context Manager 패턴을 이용하여 같은 로직을 구현하면 다음과 같다.
```python
...

with open(filename) as file:
    process_file(file)

...
```
- 뭔가 확 줄어들었다!
- Context Manager 가 보여주는 마법(magic) 의 핵심에는 **\_\_enter\_\_()** 와 **\_\_exit\_\_()** 메서드가 존재한다. (실제로 매직 메서드이다)
- 코드가 interpreter 방식으로 진행되다 **with** 구문을 만나면 지정된 리소스를 사용하기 위한 새로운 Context 로 진입한다.
- 이 때, 이름 그대로 **\_\_enter\_\_()** 는 본 프로세스에 진입하기 '이전'에 호출되고, **\_\_exit\_\_()** 는 프로세스의 마지막 라인이 실행되고 난 다음에 호출된다.
- **\_\_enter\_\_()** 메서드로부터 반환된 값은 그것이 무엇이든 as 이후의 변수에 할당된다.
- **\_\_exit\_\_()** 메서드는 프로세스 중간에 예외가 발생할지라도 **finally** 구문 처럼 필수적으로 호출되기 때문에 안정적이다.
- 게다가 예외에 대해서는, 해당 예외 객체를 파라미터로 받기 때문에 임의의 방법으로 에러 처리도 가능하다.

#### * Context Manage 는 물론 리소스 핸들링 외에도 사전/사후 처리를 요하는 모든 로직에서 활용 가능하다.

## 데이터베이스 백업 예제

- 컨텍스트 관리자는 관심사를 분리하고 독립적으로 유지되어야 하는 코드를 분리하는 좋은 방법이다.
- 예를 들어 오프라인 상태에서 데이터베이스 백업을 수행하는 스크립트를 작성한다고 가정하면, (백업 시에는 서비스를 중지해야만 함)
- 백업 작업이 끝난 이후 성공 여부에 관계 없이 서비스 프로세스를 다시 시작해야 한다.
- 이 때 백업이 실패하거나 예외가 발생하더라도 서비스는 그냥 시작된다.
- 이것을 구현하기 위한 첫 번째 방법은 **컨텍스트 관리자를 사용하지 않고** ```서비스중지-백업-예외처리-서비스재시작``` 의 전 과정을 핸들링하는 거대 단일 함수를 만드는 것이다.
```python
...

def stop_database():
    run("systemctl stop postgresql.service")

def start_database():
    run("systemctl start postgresql.service")

class DBHandler:
    def __enter__(self):
        stop_database()
        return self
    
    def __exit__(self, exc_type, ex_value, ex_traceback):
        start_database()

def db_backup():
    run("pg_dump database")

def main():
    with DBHandler():
        db_backup()

...
```

- with 구문의 DBHandler 를 사용한 블럭 내부에서 Context Manager 를 사용하지 않았다는 의미는,
- \_\_enter\_\_ 의 반환값이 사용되지 않으며, 유지보수 작업이나 예외에 대한 처리에 상관 없이 \_\_exit\_\_ 이 호출된다는 데 있다.
- 일반적으로 \_\_enter\_\_ 에서 무언가를 반환하고 그것을 기반으로 핸들링하는 것과, 에러를 무시하지 않고 적절히 처리하는 것은 좋은 습관이다.
- \_\_exit\_\_ 에서는 예외에 대한 정보를 파라미터로 받고(없으면 None) 서비스 start 작업을 수행하는데,
- 적절한 예외처리 없이 무시하거나 True 를 반환하는 것은 좋지 못하다. (그렇게 하는 특별한 이유가 있지 않는 한.)

## 컨텍스트 관리자를 사용해서 구현한다면?








