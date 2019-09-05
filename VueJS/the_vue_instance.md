
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190905-lightgray.svg?style=flat-square)

# The Vue Instance

참조: [The Vue Instance](https://vuejs.org/v2/guide/instance.html)

## Vue 인스턴스 만들기

모든 Vue 어플리케이션은 ```Vue``` 함수로 새로운 Vue 인스턴스를 생성함으로써 시작된다.

```javascript
var vm = new Vue({
    // options
})
```

비록 [MVVM pattern](https://en.wikipedia.org/wiki/Model_View_ViewModel) 과 엄밀하게 관련되지는 않지만, Vue 의 디자인은 부분적으로 그것으로부터 영감을 받았다.
컨벤션으로써, 우리는 종종 Vue 인스턴스를 참조하기 위해 **vm** 변수를 사용한다.

Vue 인스턴스를 생성했다면, **option object** 를 전달하라.
이 가이드의 대다수는 당신이 원하는 대로 동작하도록 하기 위해 이러한 옵션들을 어떻게 사용해야 하는지 설명한다.
참고로 당신은 [API reference](https://vuejs.org/v2/api/#Options-Data) 에서 옵션의 전체 목록을 검색할 수 있다.

Vue 어플리케이션은 ```new Vue``` 를 통해 생성된 **root Vue instance** 로 구성되어 있는데,
선택적으로 중첩된 tree 와 재사용 가능한 부품들로 조직되어 있다.
예를 들어 todo 앱의 컴포넌트 트리는 다음과 같이 보여질 수 있다:

```text
Root Instance
ㄴ TodoList
    ㅏ TodoItem
    ㅣ ㅏ DeleteTodoButton
    ㅣ ㄴ EditTodoButton
    ㄴ TodoListFooter
       ㅏ ClearTodoButton
       ㄴ TodoListStatistics
```

[the component system]() 에 대한 자세한 것은 차후에 이야기할 것이다.
지금은 단지 모든 Vue 컴포넌트가 Vue 인스턴스라는 사실과, 이로 인해 서로 같은 옵션을 공유한다는 것을 알고 있으면 된다.
(몇몇 root-specific options 제외)

## Data 와 Methods
