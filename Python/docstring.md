
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190602-lightgray.svg?style=flat-square)

# 파이썬 Docstring

참조 : [datacamp docs](https://www.datacamp.com/community/tutorials/docstrings-python)

> docstring 은 일반적인 주석(comments)와는 다르다. comments 는 지양해야하는 반면 docstring 은 가급적 자세하고 명료하게 작성해야 한다.

> docstring 은 쉽게 말해서 해당 함수나 클래스와 같은 로직의 '일부'이다. 반면 주석은 부가적인 '설명'이다.

> docstring 은 해당 component 에 대한 '문서화'이다. 문서는 그것을 접한 다른 사람의 원활한 이해를 돕는다.

## Docstrings in Python (번역)

- One-line Docstrings, Multi-line Docstring, popular Docstring formats 의 특징과 상호간의 차이를 각각의 용례와 함께 배운다
- 이하의 규칙은 only a convention 이며, **PEP 기준**에 철저히 기반한다. (파이썬의 문법적, 필수적 규칙까지는 아니라는 의미)
- 개요
    * **파이썬 Docstring** 은 class, module, function 또는 method 의 정의이다.
    * any 파이썬 객체에나 존재하는 **\_\_doc\_\_** 속성으로써 접근 가능하며, 기본적으로 존재하는 **help()** 함수에서 편리하게 나타난다.
    * 특정 code 의 더 넓은 기능적 부분을 이해하기에 좋다.
    * 주로 코드의 각 라인이나 표현들이 어떻게 동작할지 자체적으로 알 수 있도록 프로그래머에 의해 작성된 설명적인 텍스트이다.
    * 당신의 코드에 대한 문서화는 **클린 코드** 작성과 **잘 읽히는** 프로그램을 만드는 데 있어서 근본적으로 충분히 좋은 기반을 제공한다.
    * 그러나 이미 언급했듯 어떤 특정한 기준이나 규칙이 존재한다는 것은 아니다.

#### One-line Docstrings

- 한 줄에 맞춰진 docstring 을 의미한다.
- 삼중의 단일 또는 이중 따옴표를 여닫는 양쪽 부분에서 일치시켜야만 하며, 모두 한 줄에 위치해야 한다.
- 일반적으로 삼중의 이중 따옴표롤 여닫는다.
```python
def square(a):
    '''Returns argument a is squared.'''
    return a**a

print (square.__doc__)


help(square)
```
출력 결과
```text
Returns argument a is squared.
Help on function square in module __main__:

square(a)
    Returns argument a is squared.
```

#### Multi-line Docstrings

- 이것도 동일하게 정적 문자열 line 을 포함하나, 설명적 텍스트와 함께 단일 공백(line)이 뒤따른다.
- 그 예시는 다음과 같다.
```python
def some_function(argument1):
    """Summary or Description of the Function

    Parameters:
    argument1 (int): Description of arg1

    Returns:
    int:Returning value

   """

    return argument1

print(some_function.__doc__)
```
출력 결과
```text
Summary or Description of the Function

    Parameters:
    argument1 (int): Description of arg1

    Returns:
    int:Returning value
```
- 이러한 convention 은 자동 인덱싱 도구를 위한 쓰임새가 뒤따라야 한다. (자동 인덱싱 도구에서 유용하게 쓰여야 한다는 의미)

#### Popular Docstring Formats

- Docstring 을 더 사용하기 좋고 이해하기 쉽게 해주는 **Docstring Format** 들이 존재한다.
- 정해진 규칙은 없지만, 한 프로젝트에서는 docstring 작성에 일관성을 유지해야만 한다.
- **Sphinx** 에 의해 제공되는 포맷 타입을 사용하는 것이 권장된다.
- 가장 일반적인 포맷은 다음과 같다
    * Numpy/SciPy docstrings : Sphinx 에 의해 제공되는 reStructured 와 GoogleDocstrings 의 combination
    * Pydoc : Sphinx 에 의해 제공되는 파이썬을 위한 standard documentation module
    * Epydoc : HTML documents 와 API documentation 을 생성하는 도구의 시리즈인 Epytext 를 렌더링한다.
    * Google Docstrings : 구글 스타일
- 위에서 본것 같이 여러 종류의 documentation strings 가 존재하지만, 대체로 유사하기 때문에 크게 걱정할 필요는 없다.
- 기반이 되는 **Sphinx Style** 을 자세히 살펴본다면 다른 포맷들에 대한 doc 도 쉽게 따라갈 수 있을 것이다.

#### Sphinx Style

- 이것은 쉽고 전통적인 스타일이며 최초에 파이썬 documentation 을 위해 특별히 만들어졌다.
- Sphinx 는 reStructuredText 를 사용하는데, 이것은 Markdown 에서의 사용법과 유사하다.
- Sphinx 를 실행하면 프로젝트 문서화를 위한 기본 골격을 만들어준다.
```python
class Vehicle(object):
    '''
    The Vehicle object contains lots of vehicles
    :param arg: The arg is used for ...
    :type arg: str
    :param `*args`: The variable arguments are used for ...
    :param `**kwargs`: The keyword arguments are used for ...
    :ivar arg: This is where we store arg
    :vartype arg: str
    '''


    def __init__(self, arg, *args, **kwargs):
        self.arg = arg

    def cars(self, distance, destination):
        '''We can't travel a certain distance in vehicles without fuels, so here's the fuels

        :param distance: The amount of distance traveled
        :type amount: int
        :param bool destinationReached: Should the fuels be refilled to cover required distance?
        :raises: :class:`RuntimeError`: Out of fuel

        :returns: A Car mileage
        :rtype: Cars
        '''  
        pass
```
- Sphinx 는 대다수의 프로그래밍 언어에서 사용되는 **keyword**를 사용한다.
- 그러나 이것은 특별히 **role** 이라고 불린다.
- 위 코드에서 Sphinx 는 **param** 을 역할(role)로, **type**을 **param** 의 Sphinx data type 으로 규정했다.
- type 의 역할은 optional 이지만, param 은 필수이다.
- **return** document 부분은 객체를 반환하도록 규정되는데, 이것은 param 의 role 과는 다르다.
- the return role 은 **rtype** 에 의존하지 않으며, 그 역도 마찬가지다.
- **rtype** 은 주어진 함수로부터 반환되는 객체의 타입이다.

#### Google Style

#### Numpy Style

#### 파이썬 내장 Docstrings

- 파이썬의 모든 내장 함수, 클래스, 메서드는 그것에 포함된 실제 인간 언어 description 을 가지고 있다.
- 당신은 그것에 두 가지 방식으로 접근할 수 있는데,
    1. **__doc\_\_** 속성
    2. **help()** 함수 가 그것이다.
- 예를 들어,
```python
import time
print(time.__doc__)
```
출력 결과는,
```text
This module provides various functions to manipulate time values.

There are two standard representations of time.  One is the number
of seconds since the Epoch, in UTC (a.k.a. GMT).  It may be an integer
or a floating point number (to represent fractions of seconds).
The Epoch is system-defined; on Unix, it is generally January 1st, 1970.
The actual value can be retrieved by calling gmtime(0).

The other representation is a tuple of 9 integers giving local time.
The tuple items are:
  year (including century, e.g. 1998)
  month (1-12)
  day (1-31)
  hours (0-23)
  minutes (0-59)
  seconds (0-59)
  weekday (0-6, Monday is 0)
  Julian day (day in the year, 1-366)
  DST (Daylight Savings Time) flag (-1, 0 or 1)
If the DST flag is 0, the time is given in the regular time zone;
if it is 1, the time is given in the DST time zone;
if it is -1, mktime() should guess based on the date and time.

............................................
.............................................
more description..........................
```
- 이렇게 쓰면 된다!

#### Docstring 의 단점

- 모든 문서화가 그렇듯이 docstring 에도 단점이 존재하는데,
1. 지속적으로 수작업 및 코드 변경 시 수동 업데이트를 해야 한다.
2. 유용한 docstring 을 위해서는 여러 줄에 걸쳐 상세하게 작성해야 한다.


