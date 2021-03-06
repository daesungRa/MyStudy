﻿![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190109-lightgray.svg?style=flat-square)

# Spring Framework 개요 - IoC 와 DI

> 스프링의 주요 컨셉은 Inversion of Control (IoC) 이다. 제어의 반전이라는 이 용어는, 프로그래머가 작성한 프로그램의 여러 컴포넌트들이 재사용 라이브러리의 흐름 제어를 받게 되는 소프트웨어 디자인 패턴을 의미한다.

IoC 의 한 방식인 Dependency Injection (DI), 즉 의존성 주입을 설명하자면,
- 기존 코드에서는 외부 라이브러리를 직접 import 해 내부에서 객체를 생성해 사용했다면
- DI 흐름에서는 **필요한 외부의 객체를 가져오는 것이 아니라 외부로부터 주입받아 사용**한다.
- 즉, 코드 내부에서 객체를 일일히 생성하는 것이 아니라 외부에 이미 생성된 필요 객체를 전달받아 재사용하게 되는 것이다.

## DI 비유적 설명

- 비유하자면 자동차 조립 시 지금까지는 공장에서 직접 바퀴를 자체생산해 사용했다면,
- DI 패턴에서는 공장 외부에서 규격에 맞게 이미 생산된 바퀴를 가져와서 조립한다.
- 이것의 장점은,
	- 부품화가 진행되므로 생산성이 높아지고
	- 규격에 따라 여러 종류의 인터페이스 구현 제품을 사용할 수 있다는 것이다 > 인터페이스만 맞다면 바퀴를 종류별로 골라 사용 가능함
	- 또한, 제어의 흐름을 XML 파일로 미리 지정해 놓고 그 흐름에 따라 재사용하므로 개발자가 고려할 사항이 줄어들었다
- 결과적으로 기존의 직접 import 방식은 의존성이 높다. import 한 특정 외부 라이브러리에 대한 의존도가 높다는 의미다.
- 그러나 DI 패턴은 필요한 클래스의 객체 변수만 선언하고, 실제 객체는 외부로부터 주입받기만 하면 되므로 (의존성 주입), 특정 라이브러리에 상대적으로 의존적이지 않다
- 의존성이 낮다는 것은 외부 로직 흐름을  고려하지 않고 자기 자신의 로직에만 집중할 수 있음을 의미한다. 코드 변경 시, 외부에 영항을 미치지는 않을 지 고민할 필요가 없기 때문이다
- 다음 IoC 의 설계 목적을 보면 DI 패턴의 의미를 더 확실히 알 수 있다

## 제어 반전의 설계 목적

- 작업을 구현하는 방식과 작업 수행 자체를 분리
- 모듈을 제작할 때, 모듈과 외부 프로그램의 결합에 대해 고민할 필요 없이 모듈의 목적에 집중할 수 있다
- 다른 시스템이 어떻게 동작할지에 대해 고민할 필요 없이, 미리 정해진 협약대로만 동작하게 하면 된다
- 모듈을 바꾸어도 다른 시스템에 부작용을 일으키지 않는다

<[위키백과 참조](https://ko.wikipedia.org/wiki/%EC%A0%9C%EC%96%B4_%EB%B0%98%EC%A0%84)>

## 결론
### DI 는 프로그램 각 모듈의 결합도를 낮추고 응집도(부품화, 모듈화)를 높이며 제어 반전 (IoC) 원리에 의해 생산성을 높인다


















