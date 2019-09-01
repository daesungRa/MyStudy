
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190901-lightgray.svg?style=flat-square)

# Vue JS [02]

참조: [Introduction Vue.js](https://vuejs.org/v2/guide/)

#### 부품 조립하기

The component 시스템은 Vue 의 또다른 주요한 컨셉트인데,
이것(The component system)이 우리로 하여금 작은 부품들, self-contained, 재사용 가능한 부품들로 조립된 큰 규모의 어플리케이션을
구축할 수 있도록 하는 abstraction(추상) 이기 때문에 그러하다.
이에 대해 생각해 보면, 거의 모든 타입의 어플리케이션 인터페이스는 component 들의 tree 로써 추상화될 수 있다. (tree 구조)

![tree of components](https://vuejs.org/images/components.png)

Vue 에서 하나의 component 는 본질적으로 사전에 정의된 옵션을 포함하는 Vue 인스턴스이다.
Vue 에 component 를 등록하는 것은 간단하다.
