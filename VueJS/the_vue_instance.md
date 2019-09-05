
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

Vue 인스턴스가 생성되었다면, 그것은 Vue 의 **reactivity system** 에 자신의 **data** 객체에서 찾은 속성들을 추가한다.
이러한 속성들의 값이 바뀌면 view 는 새로운 값에 알맞게 updating 되는 방식으로 "반응(react)" 한다.

```javascript
// Our data object
var data = { a: 1 }

// The object is added to a Vue instance
var vm = new Vue({
    data: data
})

// Getting the property on the instance
// returns the one from the original data
vm.a == data.a // => true

// Setting the property on the instance
// also affects the original data
vm.a = 2
data.a // => 2

// ... and vice-versa
data.a = 3
vm.a // => 3
```

이 data 가 바뀔 때 view 는 다시 렌더링된다.
**data** 의 속성들은 인스턴스가 생성되었을 때 자신들이 존재할 경우에만 반응한다는 사실을 꼭 알아두자.
그러므로 만약 당신이 새로운 속성을 다음과 같이 추가한다면,:

```text
vm.b = 'hi'
```

**b** 로 인한 변화는 어떠한 view updates 도 야기시키지 않는다.
만약 애초부터 비어 있거나 존재하지 않는 속성을 나중에 추가할 필요가 생긴다면, 몇몇 initial value 를 다음과 같이 지정해야 한다.

```text
data: {
    newTodoText: '',
    visitCount: 0,
    hideCompletedTodos: false,
    todos: [],
    error: null
}
```

여기서 단 하나 예외는 이미 존재하는 속성이 바뀌는 것을 사전에 차단하는 **Object.freeze()** 를 사용하는 경우인데,
이는 반응성 시스템(reactivity system)이 변화를 추적하지 못함을 의미한다.

```javascript
var obj = {
    foo: 'bar'
}

Object.freeze(obj)

new Vue({
    el: '#app',
    data: obj
})
```

```html
<div id="app">
    <p>{{ foo }}</p>
    <!-- this will no longer update 'foo'! -->
    <button v-on:click="foo = 'baz'">Chango it</button>
</div>
```

data 속성을 추가하기 위해 Vue 인스턴스는 수많은 유용한 인스턴스 속성과 메서드를 제공한다.
이것들은 사용자 정의 속성들로부터 구별짓기 위해 **$** 로 prefixed 되었다. 다음을 보자:

```javascript
var data = { a: 1 }
var vm = new Vue({
    el: '#example',
    data: data
})

vm.$data === data // => true
vm.$el === document.getElementById('example') // => true

// $watch is an instance method
vm.$watch('a', function (newValue, oldValue) {
    // This callback will be called when 'vm.a' changes
})
```

나중에 인스턴스 속성과 메서드의 전체 목록을 [API reference](https://vuejs.org/v2/api/#Instance-Properties) 에서 찾아볼 수 있을 것이다.

## 인스턴스 수명주기
