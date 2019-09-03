
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

```javascript
// Define a new component called todo-item
Vue.component('todo-item', {
    template: '<li>This is a todo</li>'
})
```

이제 이것을 다른 컴포넌트의 템플릿에 조립할 수 이따:

```html
<ol>
    <!-- Create an instance of the todo-item component -->
    <todo-item></todo-item>
</ol>
```

그러나 아쉽게도 이것은 모든 todo 에 같은 텍스트를 렌더링할 것이다.
우리는 부모 scope 로부터 자손 컴포넌트에 data 를 전달할 수 있어야만 한다.
그러면 ```prop``` 값을 전달받을 수 있도록 상단에 정의된 컴포넌트를 수정해 보자.

```javascript
Vue.component('todo-item', {
    // The todo-item component now accepts a
    // "prop", which is like a custom attribute.
    // This prop is called todo.
    props: ['todo'],
    template: '<li>{{ todo.text }}</li>'
})
```

이제 우리는 ```v-bind``` 를 사용해 반복된 각각의 컴포넌트에 todo 정보를 전달할 수 있다.

```html
<div id="app-7">
    <ol>
        <!--
            Now we provide each todo-item with the todo object
            it's representing, so that its content can be dynamic.
            We also need to provide each component with a "key",
            which will be explained later.
        -->
        <todo-item
            v-for="item in groceryList"
            v-bind:todo="item"
            v-bind:key="item.id"
        ></todo-item>
    </ol>
</div>
```

```javascript
Vue.component('todo-item', {
    props: ['todo'],
    template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
    el: '#app-7',
    data: {
        groceryList: [
            { id: 0, text: 'Vegetables' },
            { id: 1, text: 'Cheese' },
            { id: 2, text: 'Whatever else humans are supposed to eat' },
        ]
    }
})
```

```text
    1. Vegetables
    2. Cheese
    3. Whatever else humans are supposed to eat
```

이것은 고안된 예제이지만 보다 작은 두 유닛으로 분리된 app 에 의해 관리되는데,
이것들은 props 인터페이스를 경유하여 합리적인 이유에 의해 분리된 것이다.
우리는 이제 부모 app 에 영향을 미치지 않고서 보다 복잡한 템플릿과 로직을 활용해 우리의 ```<todo-item>``` 컴포넌트를 더 발전시킬 것이다.

큰 어플리케이션에서는 전체 app 을 개발적으로 관리 가능한 컴포넌트들로 분리하는 것이 필수적이다.
우리는 [later in the guide](https://vuejs.org/v2/guide/components.html) 에서 컴포넌트에 대해 더 많은 이야기를 나눌 것이지만,
여기 app 의 템플릿이 컴포넌트와 함께 어떤게 나타날지에 관한 (상상의) 예제가 있다.

```html
<div id="app">
    <app-nav></app-nav>
    <app-view>
        <app-sidebar></app-sidebar>
        <app-content></app-content>
    </app-view>
</div>
```
