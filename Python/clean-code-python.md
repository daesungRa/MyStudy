
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

나쁜 예:
```text
address = 'One Infinite Loop, Cupertino 95014'
city_zip_code_regex = r'^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$'
matches = re.match(city_zip_code_regex, address)

save_city_zip_code(matches[1], matches[2])
```

그리 나쁘지만은 않은 예: 이것은 조금 더 낫지만, 그래도 여전히 regex 에 깊이 의존적이다.
```text
address = 'One Infinite Loop, Cupertino 95014'
city_zip_code_regex = r'^[^,\\]+[,\\\s]+(.+?)\s*(\d{5})?$'
matches = re.match(city_zip_code_regex, address)

city, zip_code = matches.groups()
save_city_zip_code(city, zip_code)
```

좋은 예: naming subpatterns 로 regex 에 대한 의존성을 줄였다.
```text
address = 'One Infinite Loop, Cupertino 95014'
city_zip_code_regex = r'^[^,\\]+[,\\\s]+(?P<city>.+?)\s*(?P<zip_code>\d{5})?$'
matches = re.match(city_zip_code_regex, address)

save_city_zip_code(matches['city'], matches['zip_code'])
```

#### Avoid Mental Mapping (상상하도록 하는 것을 피하라)

당신의 코드를 읽는 사람으로 하여금 변수들이 무엇을 의미하는지 번역하도록 하지 말 것. 명료한 것이 암시적인 것보다 낫다.

Explicit is better than implicit.

나쁜 예:
```text
seq = ('Austin', 'New York', 'San Francisco')

for item in seq:
    do_stuff()
    do_some_other_stuff()
    # ...
    # Wait, what's 'item' for again?
    dispatch(item)
```

좋은 예:
```text
locations = ('Austin', 'New York', 'San Francisco')

for location in locations:
    do_stuff()
    do_some_other_stuff()
    # ...
    dispatch(location)
```

#### Don't add unneeded context (불필요한 문맥을 추가하지 말 것)

만약 당신의 클래스나 객체 이름이 무언가를 설명한다면, 그것을 변수명에서 반복하지 말 것.

나쁜 예:
```text
class Car:
    car_make: str
    car_model: str
    car_color: str
```

좋은 예:
```text
class Car:
    make: str
    model: str
    color: str
```

#### Use default arguments instead of short circuiting or conditionals (짧은 구문이나 조건문 대신 디폴트 인자를 사용할 것)

Tricky:
```text
def create_micro_brewery(name):
    name = "Hipster Brew Co." if name is None else name
    slug = hashlib.sha1(name.encode()).hexdigest()
    # etc.
```

기본 인자를 지정할 수 있음에도 왜 이렇게 쓰는가? 기본 인자를 사용하면 또한 인자로 문자열을 기대한다는 사실을 명백히 하게 된다.

좋은 예:
```text
def create_micro_brewery(name: str = "Hipster Brew Co.":
    slug = hashlib.sha1(name.encode()).hexdigest()
```

## 함수

#### Function arguments (2 or fewer ideally) (함수의 인자는 2개 혹은 그 이하가 이상적이다.)

함수 파라미터의 총량을 제한하는 것은 함수 테스트를 쉽게 하기 때문에 엄청나게 중요하다.
세 개 이상의 파라미터를 갖는 것은 당신이 각각의 추가된 인자에 따른 tons of different cases 에 대한 테스트를 (일일히) 수행하도록 하는 가중된 부담을 안겨준다.

인자가 없는 것이 가장 이상적인 경우이다. 하나 혹은 두 개의 인자도 괜찮지만, 세 개는 피해야 한다.
그 이상의 모든 것은 통합되어야만 한다. 일반적으로, 두 개보다 많은 인자를 가진다면 함수는 너무 많은 일을 수행하게 된다.
그렇지 않은 경우라도, 대부분은 상위 객체라면 인자로써 충분할 것이다.

나쁜 예:
```text
def create_menu(title, body, button_text, cancellable):
    # ...
```

좋은 예:
```text
class Menu:
    def __init__(self, config: dict):
        title = config["title"]
        body = config["body"]
        # ...

menu = Menu(
    {
        "title": "My Menu",
        "body": "Something about my menu",
        "button_text": "OK",
        "cancellable": False
    }
)
```
 또 다른 좋은 예:
 ```text
 
 ```

