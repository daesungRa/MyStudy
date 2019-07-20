
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190720-lightgray.svg?style=flat-square)

# Clean Code Python

> 참조 및 번역: [파이썬으로 클린 코드 베이스의 소프트웨어 만드는 방법](https://github.com/zedr/clean-code-python#introduction)

## 소개

**Robert C. Martin** 의 책 **Clean Code** 를 파이썬에 적용한 소프트웨어 공학 원리에 대한 문서이다.
이것은 코드 스타일 가이드가 아니라, 파이썬으로 읽기 쉽고, 재사용에 용이하며, 리펙토링이 가능한 소프트웨어를 만들어내기 위한 가이드이다.

이곳에 기록된 모든 원리를 엄격하게 따라야 하는 것은 아니다. 이것들은 가이드라인 그 이상이 아니지만, **Clean Code** 에 관해 수 년간 경험적으로 축적된 저자의 
코드 작성법이라는 것을 상기하자.

(Target Python3.7+)

## 변수

#### Use meaningful and pronounceable variable names (의미 있고 발음이 쉬운 변수명 사용하기)

나쁜 예:
```text
ymdstr = datetime.date.today().strftime("%y-%m-%d")
```

좋은 예:
```text
current_date: str = datetime.date.today().strftime("%y-%m-%d")
```

#### Use the same vocabulary for the same type of variable (같은 타입의 변수에 같은 어휘를 사용하기)

나쁜 예: 같은 엔티티 기반 변수에 서로 다른 세 이름 사용
```text
get_user_info()
get_client_data()
get_customer_record()
```

좋은 예: 엔티티가 같을 때, 당신의 함수는 그것을 참조하는 데 일관성이 있어야 한다.
```text
get_user_info()
get_user_data()
get_user_record()
```

조금 더 살펴보기: 파이썬은 또한 객체 지향적 프로그래밍 언어이다. 그것이 이해된다면, 함수들의 집합체인 패키지는 인스턴스 속성, 속성 메서드, 혹은 일반 메서드로 구성된
통합적 구현체로써 당신의 코드에 존재해야 한다.
```python
class User:
    info: str
    
    @property
    def data(self) -> dict:
        # ...
    
    def get_record(self) -> Union[Record, None]:
        # ...
```

#### Use searchable names (검색 가능한 이름을 사용하기)

우리는 코드를 작성하는 것보다 읽은 일이 더 많은 것이다. 그것은 우리가 작성하는 코드가 읽기 좋고 검색하기 용이하도록 하는 데 중요하다.
프로그램을 이해하기 위해 의미 있는 변수명을 네이밍하지 않음으로써, 읽는 사람들로 하여금 힘들게 할 수도 있다.

이름을 검색이 쉽도록 만들자.

나쁜 예:
```text
# What the heck is 86400 for?
time.sleep(86400);
```

좋은 예:
```text
# Declare them in the global namespace for the module.
SECONDS_IN_A_DAY = 60 * 60 * 24

time.sleep(SECONDS_IN_A_DAY)
```

#### Use explanatory variables (설명적인 변수명을 사용하기)




