
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

#### 이중 밑줄은 파이썬스러운 코드가 아니다!


