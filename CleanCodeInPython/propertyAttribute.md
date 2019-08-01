
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190630-lightgray.svg?style=flat-square)

# [파이썬스러운 코드] 프로퍼티, 속성과 객체 메서드의 다른 타입들

> 파이썬 객체의 모든 프로퍼티와 함수는 public 이다. 무조건!

## 파이썬에서의 밑줄

- 다만, 밑줄을 사용하는 몇 가지 규칙을 통해 public, private, protected 등의 속성으로 호출되기를 **기대**할 수 있다.
- 이는 각각의 특성을 **강제하는 것이 아니라 단순시 명시**하는 것이다.
- 다시 말하지만, 파이썬의 모든 프로퍼티와 함수는 public 이다.
- 예를 들어 어두에 밑줄 하나를 포함시키면 해당 객체는 private 임을 의미한다. 그러나 실제로는 public 이기 때문에 외부에서 접근 가능하다.
- 그러므로, 코드 작성자는 밑줄 규칙을 숙지하고 준수하여 잘못된 사용에 따른 파급 효과를 걱정하지 않을 수 있다.
```text
tip: 외부 호출과 관련된 객체가 아니라면 모든 멤버의 접두사로 하나의 밑줄을 사용하는 것이 좋다.
```

## 프로퍼티

- 객체에 특정 값을 저장하거나 상태를 나타내고자 할 때 속성(attribute)을 사용한다.
- 이러한 속성에 대한 접근을 제어하기 위해 자바에서는 getter, setter 를 사용하지만, 파이썬에서는 프로퍼티를 사용한다.

이메일 입력 예제
```python
import re

EMAIL_FORMAT = re.compile(r"[^@]+@[^@]+[^@]+")

def is_valid_email(potentially_valid_email: str):
    return re.match(EMAIL_FORMAT, potentially_valid_email) is not None

class User:
    def __init__(self, username):
        self.username = username
        self._email = None

    @property
    def email(self):
        return self._email
    
    @email.setter
    def email(self, new_email):
        if not is_valid_email(new_email):
            raise ValueError(f"유효한 이메일이 아니므로 {new_email} 값을 사용할 수 없음")
        self._email = new_email
```

- 여기서 ```@property``` 로 감싸진 함수( == getter)는 private 속성의 이메일을 반환하고,
- ```@email.setter``` 로 감싸진 함수( == setter)는 새로운 이메일 대입 시 자동으로 호출되어
- 유효성을 검증한 뒤 해당 값을 저장하거나 예외를 발생시킨다.

```text
>>> u1 = User("jsmith")
>>> u1.email = "jsmith@"
Traceback (most recent call last):
...
유효한 이메일이 아니므로 jsmith@ 값을 사용할 수 없음
>>> u1.email = "jsmith@g.co"
>>>u1.email
'jsmith@g.co'
```

- **email** 키워드를 직접적으로 사용하여 get, set 작업을 수행할 수 있으므로 이해가 쉽고 간편하다.
- 또한 클래스 메서드 명명이 일관적이므로, 서로 다른 이름과 기능으로 인한 혼동을 줄일 수 있다.

```text
tip: 객체의 모든 속성에 프로퍼티를 적용할 필요는 없다. 대부분의 경우 일반 속성을 사용하고, 속성 값을 가져오거나 수정할 가능성이 있는 경우에만 프로퍼티를 사용한다.
```

#### 프로퍼티는 명령-쿼리 분리 원칙(command and query separation - CC08)을 따르기 위한 좋은 방법이다.

- 명령-쿼리 분리 원칙이란, 객체의 메서드가 상태를 변경하는 메서드이거나 값을 반환하는 쿼리 둘 중의 하나여야만 한다는 것이다.



