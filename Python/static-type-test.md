
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190817-lightgray.svg?style=flat-square)

# 정적 타입 검사로 더 나은 파이썬 코드 작성

발표자: 이창희

- 싫어하는 이유?
    * 동적 타입 언어 - run 시에 타입이 정해짐
    * 유연하지만, 확인하기 어려움
- 동적 타입은 대규모 협업 시 어려움이 있으므로, 대부분의 동적 타입 언어는 정적 타입 검사기를 제공한다
- 이점 : 가독성을 올리고, 버그를 '예방'

## 파이썬에서는?

- variable annotation

```python
age: int = 1234

# ERROR!
x: list[str] = ['A', 'B', 'C']

# solution
from typing import List, Tuple, Dict, Union

x: List[str] = ['A', 'B', 'C']

my_list: List[Union[str, int]] = []
my_list: List[Union[str, None]] = []
```

- 이 외에 TypeVar를 활용해 제네릭 타입을 구현할 수 있다.
- Generic 클래스를 상속받아 구현할 수 있다.
- Callable 클래스도 있다.
- 비동기 함수도 지원
    * asynciio
- 타입 힌팅은, 이것일 것이라고 제안하는 것 뿐 실제로 제한하지는 않는다

## 제한하려면? -> mypy
- mypy 모듈을 사용해 작성한 코드를 테스트한다.
- 예상치 못한 버그를 예방한다.
- 특정 객체가 할당되면, 처음 결정된 타입 객체로써 지정되므로, 재할당이 아니고서는 변경이 불가.
- 각 IDE 에서 mypy 플러그인을 지원한다.
