
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
class MenuConfig:
    """A configuration for the Menu.
    
    Attributes:
        title: The title of the Menu.
        body: The body of the Menu.
        button_text: The text for the button label.
        cancellable: Can it be cancelled?
    """
    title: str
    body: str
    button_text: str
    cancellable: bool = False
    
def create_menu(config: MenuConfig):
    title = config.title
    body = config.body
    # ...
    
config = MenuConfig
config.title = "My delicious menu"
config.body = "A description of the various items on the menu"
config.button_text = "Order now!"
# The instance attribute overrides the default class attribute.
config.cancellable = True

create_menu(config)
```

Fancy:
```text
from typing import NamedTuple

class MenuConfig(NamedTuple):
    """A configuration for the Menu.

    Attributes:
        title: The title of the Menu.
        body: The body of the Menu.
        button_text: The text for the button label.
        cancellable: Can it be cancelled?
    """
    title: str
    body: str
    button_text: str
    cancellable: bool = False


def create_menu(config: MenuConfig):
    title, body, button_text, cancellable = config
    # ...

create_menu(
    MenuConfig(
        title="My delicious menu",
        body="A description of the various items on the menu",
        button_text="Order now!"
    )
)
```

Even fancier:
```text
from dataclasses import astuple, dataclass

@dataclass
class MenuConfig:
    """A configuration for the Menu.

    Attributes:
        title: The title of the Menu.
        body: The body of the Menu.
        button_text: The text for the button label.
        cancellable: Can it be cancelled?
    """
    title: str
    body: str
    button_text: str
    cancellable: bool = False

def create_menu(config: MenuConfig):
    title, body, button_text, cancellable = astuple(config)
    # ...


create_menu(
    MenuConfig(
        title="My delicious menu",
        body="A description of the various items on the menu",
        button_text="Order now!"
    )
)
```

#### Functions should do one thing (함수는 하나의 작업만 해야 한다)

이것은 소프트웨어 공학에서 가장 중요한 규칙이다. 함수들이 하나 이상의 작업을 한다면, 그것들은 조립하거나, 테스트하거나, 추론하기 어려워진다.
함수를 단지 하나의 동작으로 독립시킬 수 있다면, 그것들은 리페토링이 쉬워지고 당신의 코드는 읽기 더 깔끔해질 것이다.
이 규칙을 잘 지킨다면, 당신은 많은 개발자들보다 앞설 수 있을 것이다.

나쁜 예:
```python
def email_clients(clients: List[Client]):
    """Filter active clients and send them an email.
    """
    for client in clients:
        if client.active:
            email(client)
```

좋은 예:
```python
def get_active_clients(clients: List[Client]) -> List[Client]:
    """Filter active clients.
    """
    return [client for client in clients if client.active]

def email_clients(clients: List[Client, ...]) -> None:
    """Send an email to a given list of clients.
    """
    for client in clients:
        email(client)
```

제너레이터를 사용할 수 있는 기회가 보이는가?

더 좋은 예:
```python
def active_clients(clients: List[Client]) -> Generator[Client]:
    """Only active clients.
    """
    return (client for client in clients if client.active)

def email_client(clients: Iterator[Client]) -> None:
    """Send an email to a given list of clients.
    """
    for client in clients:
        email(client)
```

#### Function names should say what they do (함수 이름은 함수가 어떤 작업을 하는지 나타내야 한다.)

나쁜 예:
```python
class Email:
    def handle(self):
        # Do something...

message = Email()
message.send()
```

좋은 예:
```python
class Email:
    def send(self) -> None:
        """Send this message.
        """

message = Email()
message.send()
```

#### Functions should only be one level of abstraction (함수는 추상화의 한 레벨이어야만 한다.)

하나의 추상 레벨 이상이라면, 당신의 함수는 일반적으로 너무 많은 일을 수행하게 된다.
재사용성과 쉬운 테스팅을 고려하여 함수를 분리하라!

나쁜 예:
```python
def parse_better_js_alternative(code: str) -> None:
    regexes = [
        # ...
    ]
    
    statements = regexes.split()
    tokens = []
    for regex in regexes:
        for statement in statements:
            # ...
    
    ast = []
    for token in tokens:
        # Lex.
    
    for node in ast:
        # Parse.
```

좋은 예:
```python
REGEXES = (
    # ...
)

def parse_better_js_alternative(code: str) -> None:
    tokens = tokenize(code)
    syntax_tree = parse(tokens)
    
    for node in syntax_tree:
        # Parse.

def tokenize(code: str) -> list:
    statements = code.split()
    tokens = []
    for regex in REGEXES:
        for statement in statements:
            # Append the statement to tokens.
    
    return tokens

def parse(tokens: list) -> list:
    syntax_tree = []
    for token in tokens:
        # Append the parsed token to the syntax tree.
    
    return syntax_tree
```

#### Don't use flags as function parameters (함수 파라미터로 flags 를 사용하지 말 것.)

Flags 는 이 함수가 하나 이상의 일을 한다는 것을 나타낸다. 함수는 하나의 작업만 해야 한다.
만약 boolean 기반으로 서로 다른 코드 진행을 수행한다면, 함수를 분리할 것.

나쁜 예:
```python
from pathlib import Path

def create_file(name: str, temp: bool) -> None:
    if temp:
        Path('./temp/' + name).touch()
    else:
        Path(name).touch()
```

좋은 예:
```python
from pathlib import Path

def create_file(name: str) -> None:
    Path(name).touch()

def create_temp_file(name: str) -> None:
    Path('./temp/' + name).touch()
```

#### Avoid side effects (사이드 이펙트를 피할 것, 부작용)

함수는 값을 넘겨받고 또다른 값을 반환하는 것 이외의 일을 수행하면 사이드 이펙트를 만들어낸다.
예를 들어, 사이드 이펙트는 파일을 쓰거나, 몇몇 global 변수를 수정하거나, 우연히 당신의 모든 돈을 낯선 사람에게 연결시킬 수 있다.

때로 프로그램에 사이드 이펙트가 필요할 수 있다. 예를 들어, 앞선 예제처럼 파일 쓰기가 필요하기도 하다.
이러한 경우에는, 사이드 이펙트들을 집중시켜 통합할 장소를 지정해야 한다.
여러 함수나 클래스들이 각각의 분리된 파일에 기록하지 않도록 하라. 오히려, 오직 하나의 서비스만이 그 작업을 수행하는 것이 낫다.

핵심은, 어떠한 구조도 없이 객체 간 공동의 상태를 공유하는 것이나, 무엇으로부터도 변화할 수 있는 가변 데이터 타입을 사용하는 것,
클래스의 인스턴스를 사용하거나 사이드 이펙트들이 발생하는 장소를 집중화시키지 않는 것과 같이 일반적으로 빠져들기 쉬운 함정을 피하는 것이다.
만약 이렇게 할 수 있다면, 당신은 수많은 다른 개발자들보다 더 행복해질 것이다.

나쁜 예:
```python
# This is a module-level name.
# It's good practice to define these as immutable values, such as a string.
# However...
name = 'Daesung Ra'

def split_into_first_and_last_name() -> None:
    # The use of the global keyword here is changing the meaning of the
    # the following line. This function is now mutating the module-level
    # state and introducing a side-effect!
    global name
    name = name.split()

split_into_first_and_last_name()

print(name) # ['Daesung', 'Ra']

# OK. It worked the first time, but what will happen if we call the
# function again?
```

좋은 예:
```python
def split_into_first_and_last_name(name: str) -> None:
    return name.split()

name = 'Daesung Ra'
new_name = split_into_first_and_last_name(name)

print(name) # 'Daesung Ra'
print(new_name) # ['Daesung', 'Ra']
```
(매개변수로 넘어가는 값은 같은 객체를 참조하지는 않는다, shallow copy?)

또 다른 좋은 예:
```python
from dataclasses import dataclass

@dataclass
class Person:
    name: str
    
    @property
    def name_as_first_and_last(self) -> list:
        return self.name.split()

# The reason why we create instances of classes is to manage state!
# 우리가 클래스의 인스턴스를 생성하는 이유는 상태(state)를 관리하기 위해서임!
person = Person('Daesung Ra')
print(person.name)
print(person.name_as_first_and_last) # ['Daesung', 'Ra']
```



