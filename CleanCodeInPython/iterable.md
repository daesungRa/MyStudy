
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190717-lightgray.svg?style=flat-square)

# [파이썬스러운 코드] 이터러블 객체

## 이터러블 객체란?

- 기존의 반복 가능한 내장 반복형 객체가 아닌 반복을 위한 로직을 정의한 일반 객체
- 이터러블
    * **\_\_iter\_\_** 매직 메서드를 구현한 객체
- 이터레이터
    * **\_\_next\_\_** 매직 메서드를 구현한 객체
- 일단은 반복 가능한 객체로 이해

## 이터러블 객체 설명

- 파이썬의 반복은 이터러블 프로토콜이라는 자체 프로토콜을 사용한다.
- ```for e in myobject:``` 형태로 객체를 반복할 수 있는지 확인하기 위해 파이썬은 고수준에서 다음 두 가지를 차례로 검사한다.
    * 객체가 iter, next 매직 메서드 중 하나를 포함하는지 여부
    * 객체가 시퀀스이고 **\_\_len\_\_** 과 **\_\_getitem\_\_** 을 모두 가졌는지 여부
- fallback 메커니즘으로 시퀀스도 반복을 할 수 있으므로, for 루프에서 반복 가능한 객체를 만드는 방법은 두 가지가 있다.

## 이터러블 객체 만들기

- 객체를 반복하려고 하면 파이썬은 해당 객체의 iter() 함수를 호출한다.
- 이 함수는 객체 내에 \_\_iter\_\_ 메서드가 있는지 확인한 후 있으면 메서드를 실행한다.
- 다음은 일정 기간의 날짜를 하루 간격으로 반복하는 객체의 코드이다.
```python
from datetime import timedelta

class DateRangeIterable:
    """자체 이터레이터 메서드를 가지고 있는 이터러블"""
    
    def __init__(self, start_date, end_date):
        self.start_date = start_date
        self.end_date = end_date
        self._present_day = start_date

    def __iter__(self):
        return self
    
    def __next__(self):
        if self._present_day >= self.end_date:
            raise StopIteration
        today = self._present_day
        self._present_day += timedelta(days=1)
        return today
```
- 이 객체는 한 쌍의 날짜를 통해 생성되며 다음과 같이 해당 기간의 날짜를 반복하면서 하루 간격으로 날짜를 표시한다.
```text
>>> for day in DateRangeIterable(date(2019, 1, 1), date(2019, 1, 5)):
...     print(day)
...
2019-01-01
2019-01-02
2019-01-03
2019-01-04
>>>
```
- 동작 과정
    * 파이썬은 iter() 함수를 호출한다.
    * iter() 함수는 \_\_iter\_\_ 매직 메서드를 호출한다.
    * self 를 반환하고 있으므로 객체 자신이 이터러블임을 나타내고 있다.
    * 따라서 for 반복의 각 단계마다 next() 함수를 호출하고, 이는 다시 \_\_next\_\_ 메서드를 호출한다.
    * 이 매직 메서드는 요소를 어떻게 생산하고 하나씩 반환할 것인지 정의하고 있고, 더 이상 생산할 것이 없을 경우 파이썬에게 StopIterable 예외를 발생시켜 끝을 알린다.
    * 즉, StopIterable 예외 발생 시까지 반복해서 next() 를 호출하는 셈이다.
```text
>>> r = DateRangeIterable(date(2019, 1, 1), date(2019, 1, 5))
>>> next(r)
datetime.date(2019, 1, 1)
>>> next(r)
datetime.date(2019, 1, 2)
>>> next(r)
datetime.date(2019, 1, 3)
>>> next(r)
datetime.date(2019, 1, 4)
>>> next(r)
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
    File ... __next__
        raise StopIteration
StopIteration
>>>
```
- 다만, 마지막 도달 이후 반복 호출 시 StopIteration 예외가 계속해서 발생할 수 있는데,
- 이터러블 반복 시에는 두 개 이상의 for 루프에서 이 값을 사용하면 첫 번째 루프만 작동하도록 되어 있으므로 이 문제가 해결된다.
- 그러나, 한번 반복실행하여 마지막 날짜에 도달한 상태인 객체를 다시 한번 호출하면 계속 **StopIteration** 예외가 발생한다.
```text
>>> r1 = DateRangeIterable(date(2019, 1, 1), date(2019, 1, 5))
>>> ", ".join(map(str, r1))
'2019-01-01, 2019-01-02, 2019-01-03, 2019-01-04'
>>> max(r1)
Traceback (most recent call last):
    File "<stdin>", line 1, in <module>
ValueError: max() arg is an empty sequence
```
- \_\_iter\_\_ 에서 제너레이터를 활용하면 이 문제를 해결할 수 있다.
```python
class DateRangeContainerIterable:
    def __init__(self, start_date, end_date):
        self.start_date = start_date
        self.end_date = end_date
    
    def __iter__(self):
        current_day = self.start_date
        while current_day < self.end_date
            yield current_day
            current_day += timedelta(days=1)
```
- 잘 동작한다.
```text
>>> r1 = DateRangeIterable(date(2019, 1, 1), date(2019, 1, 5))
>>> ", ".join(map(str, r1))
'2019-01-01, 2019-01-02, 2019-01-03, 2019-01-04'
>>> max(r1)
datetime.date(2018, 1, 4)
>>>
```
- 이 때는 반복해서 호출해도 매번 새로운 제너레이터 객체를 생성한다.
- 이러한 형태의 객체를 컨테이너 이터러블(container iterable)이라고 한다.
```text
tip: 일반적으로 제너레이터를 사용할 때는 컨테이너 이터러블을 사용하는 것이 좋다.
```


