
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190824-lightgray.svg?style=flat-square)

# Vue JS [01]

참조: [Introduction Vue.js](https://vuejs.org/v2/guide/)

> 접근성 : HTML, CSS, JavaScript 만 알면됨!

> 유연성 : 라이브러리에서부터 프레임워크 사이에서 서서히 스케일업하여 적용 가능!

> 고성능 : 20KB min+gzip 컴팩트 런타임! 초고속 Virtual DOM!

> 최소의 노력, 최대의 성과!

## 소개

#### Vue.js 가 무엇인가?

Vue(pronounced /vju:/, like **view**) 는 사용자 인터페이스를 위한 **진보적 프레임워크**이다.
다른 monolithic(단단하게 하나로 짜여져 있는) 프레임워크들과는 다르게,
Vue 는 밑바닥으로부터 점증적으로 적용 가능하도록 디자인되었다.
핵심 라이브러리는 오직 view layer 에 초점을 맞추고 있으며, 다른 라이브러리들 혹은 이미 존재하는 프로젝트들과 통합하기 쉽다.
한편, Vue 는 또한 [modern tooling](https://vuejs.org/v2/guide/single-file-components.html) 및 [supporting libraries](https://github.com/vuejs/awesome-vue#components--libraries) 와 함께 복합적으로 사용될 때 매우 복잡한 단일-페이지 어플리케이션을 강화하는 데 완벽하게 적절하다.

만약 당신이 다음 내용에 들어가기 전에 Vue 에 대해 좀더 배워보고자 한다면, 핵심 원리와 샘플 프로젝트를 접할 수 있는 우리가 만든 영상을 보라.

만약 당신이 경험 있는 프론트엔드 개발자이고, Vue 와 다른 라이브러리/프레임워크 간의 비교를 어떻게 할지 알고자 한다면,
[Comparison with Other Frameworks](https://vuejs.org/v2/guide/comparison.html) 를 보라.

- [Watch a free video course on Vue Mastery](https://www.vuemastery.com/courses/intro-to-vue-js/vue-instance/)

#### 시작하기

```text
들어가기 전에: 이 공식 가이드는 HTML, CSS, 그리고 JavaScript 에 중급 레벨 이상이라고 가정한다. 만약 당신이 프론트엔드 개발에 완전히 처음이라면,
첫 번째 단계로써 바로 프레임워크로 건너뛰는 것은 좋은 생각이 아닐 수 있으므로, 기본을 파악한 뒤 다시 돌아오도록 하라.
다른 프레임워크에 대한 선험적 경험은 도움이 되지만, 꼭 요구되는 것은 아니다.
```

Vue.js 에 도전하는 가장 손쉬운 방법은 [JSFiddle Hello World example](https://jsfiddle.net/chrisvfritz/50wL7mdz/) 을 사용하는 것이다.
다른 탭에 이것을 열고 몇몇 기본적인 샘플에 따라가는 것에 주저하지 말아라. 아니면, 당신은 [**index.html** 파일을 생성](https://gist.githubusercontent.com/chrisvfritz/7f8d7d63000b48493c336e48b3db3e52/raw/ed60c4e5d5c6fec48b0921edaed0cb60be30e87c/index.html)하여 다음과 같이 Vue 를 포함시킬 수 있다.:

```html
<!-- development version, includes helpful console warnings -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
```

또는:

```html
<!-- production version, optimized for size and speed -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

[설치 페이지](https://vuejs.org/v2/guide/installation.html) 는 Vue 를 설치하는 데 더 많은 옵션을 제공한다.:
특히 당신이 만약 Node.js-기반 빌드 툴에 친숙하지 않다면, 우리는 입문자들이 ```vue-cli``` 와 함께 시작하는 것을 **요구하지 않는다.**

만약 좀더 상호적인 것을 선호한다면, 언제든지 일지정지하고 플레이할 수 있는 mix of screencast 와 코드 플레이그라운드를 제공하는 [this tutorial series on Scrimba](https://scrimba.com/playlist/pXKqta) 를 참조할 수 있다.

#### 선언적 렌더링

vue.js 의 코어는 데이터를 DOM 에 간단한 템플릿 문법을 활용해 선언적으로 렌더링할 수 있는 시스템이다.

```html
<div id="app">
    {{ message }}
</div>
```

```html
var app = new Vue({
    el: '#app',
    data: {
        message: 'Hello Vue!'
    }
})
```

result
```text
    Hello Vue!
```

이로써 우리는 첫 Vue 앱을 만들었다.
이것은 문자열 템플릿을 렌더링한 것과 유사해 보이지만, Vue 는 물밑에서 많은 작업을 수행했다.
데이터와 DOM 은 연결되었고, 모든 것은 이제 **reactive**(반응적) 하다. 이것을 어떻게 아는가?
당신의 브라우저에 있는 JavaScript 콘솔을 열고(지금 이 페이지에서) ```app.message``` 에 다른 값을 할당하라.
그러면 업데이트에 따라 렌더링된 예시를 볼 수 있을 것이다.

추가적인 텍스트 interpolation 으로 우리는 element 속성값을 다음과 같이 bind 할 수 있다:

```html
<div id="app-2">
    <span v-bind:title="message">
        Hover your mouse over me for a few seconds
        to see my dynamically bound title!
    </span>
</div>
```

```html
var app2 = new Vue({
    el: '#app-2',
    data: {
        message: 'You loaded this page on ' + new Date().toLocaleString()
    }
})
```

result
```text
    Hover your mouse over me for a few seconds
    to see my dynamically bound title!
```

여기서 우리는 새로운 것을 발견할 수 있다.
보다시피 ```v-bind``` 속성은 **directive**(디렉티브) 로써 호출된다. (여기서 디렉티브는 vue 에서 사용되는 용어인듯)
디렉티브는 자신이 Vue 에 의해 제공된 특별한 속성이라는 것을 지정하기 위해 ```v-``` 로 prefixed 되고, 추측할 수 있듯이,
렌더링된 DOM 에 특별한 반응형 동작으로써 적용된다.
여기서 이것은 기본적으로 "```title``` 속성값을 Vue 인스턴스의 ```message``` 값으로 업데이트 하도록 해라" 라고 명령한다.

#### 조건문과 반복문

또한 엘리먼트의 존재를 toggle 하는 것은 쉽다:

```html
<div id="app-3">
    <span v-if="seen">Now you see me</span>
</div>
```

```html
var app3 = new Vue({
    el: '#app-3',
    data: {
        seen: true
    }
})
```

result
```text
    Now you see me
```

콘솔에 ```app3.seen = false``` 를 입력하라. 그러면 위 메시지가 사라지는 것을 볼 수 있다.

이 예제는 텍스트나 속성값 뿐만 아니라 DOM 구조에도 data 를 bind 할 수 있다고 설명하고 있다.
게다가, Vue 는 또한 elements 가 Vue 에 의해 삽입/수정/삭제될 때 자동적으로 [transition effects](https://vuejs.org/v2/guide/transitions.html) 를 적용할 수 있는 강력한 transition effect system(변이 효과 시스템)을 제공한다.

또다른 몇몇 디렉티브가 존재하는데, 각각은 자신만의 특별한 기능을 가지고 있다.
예를 들어, ```v-for``` 디렉티브는 배열 데이터를 활용해 item 의 리스트를 나타내는데 사용된다:

```html
<div id="app-4">
    <ol>
        <li v-for="todo in todos">
            {{ todo.text }}
        </li>
    </ol>
</div>
```

```html
var app4 = new Vue({
    el: '#app-4',
    data: {
        todos: [
            { text: 'Learn JavaScript' },
            { text: 'Learn Vue' },
            { text: 'Build something awesome' }
        ]
    }
})
```

result
```text
    1. Learn JavaScript
    2. Learn Vue
    3. Build something awesome
```

콘솔에서 ```app4.todos.push({ text: 'New item' })``` 을 입력하라. 그러면 리스트에 새로운 아이템이 추가된 것을 볼 수 있다.

#### 사용자 입력정보 다루기

사용자가 당신의 app 과 상호작용하도록 하기 위해서, 우리는 Vue 인스턴스의 메서드를 invoke 시키는 이벤트를 붙여넣기 위하여 ```v-on``` 디렉티브를 사용할 수 있다.

```html
<div id="app-5">
    <p>{{ message }}</p>
    <button v-on:click="reverseMessage">Reverse Message</button>
</div>
```

```text
var app5 = new Vue({
    el: '#app-5',
    data: {
        message: 'Hello Vue.js!'
    },
    methods: {
        reverseMessage: function () {
            this.message = this.message.split('').reverse().join('')
        }
    }
})
```

result
```text
Hello Vue.js!
(Reverse Message button)
```

이 메서드에서는 DOM 객체를 touch 하지 않고 우리의 app 의 상태를 update 한다는 사실을 숙지하라.
모든 DOM 의 조작은 Vue 에 의해 이루어지고, 당신이 작성한 코드는 숨겨진 로직에 초점이 맞춰진다.

Vue 는 또한 form input 과 app state 사이에서 두 경로로 바인딩하는 ```v-model``` 디렉티브를 제공한다.

```html
<div id="app-6">
    <p>{{ message }}</p>
    <input v-model="message">
</div>
```

```text
var app6 = new Vue({
    el: '#app-6',
    data: {
        message: 'Hello Vue!'
    }
})
```

result
```text
Hello Vue!
(Hello Vue input text box)
```


