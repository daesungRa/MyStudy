
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190912-lightgray.svg?style=flat-square)

# 템플릿 문법

참조: [Template Syntax](https://vuejs.org/v2/guide/syntax.html)

Vue.js 는 렌더링된 DOM 을 Vue 인스턴스의 data 내부에 선언적으로 바인딩할 수 있는 HTML-based 템플릿 문법을 사용한다.
모든 Vue.js 템플릿은 규격을 준수하는 브라우저나 HTML parsers 에 의해 파싱될 수 있는 유효한 HTML 이다.

이 안에서 Vue 는 템플릿을 가상 DOM render functions 로 컴파일한다.
반응형 시스템에 포함된 Vue 는 the app 영역이 변경될 때 최소한의 DOM manipulations 에 re-render 하고 apply 할 최소한의 컴포넌트를 지능적으로 파악할 수 있다.

만약 당신이 가상 DOM 개념에 익숙하고 JavaScript 의 raw power 를 선호한다면, 템플릿 대신 선택적 JSX 지원에 따른
[render functions 를 직접 작성](https://vuejs.org/v2/guide/render-function.html)할 수 있다.

## Interpolations (보간)

\# Text

데이터를 바인딩하는 가장 기본적인 form 은 "Mustache" 문법 (이중 중괄호) 을 사용하는 text interpolation (텍스트 보간법) 이다:

```html
<span>Message: {{ msg }}</span>
```

The mustache 태그는 data 객체를 만났을 때 ```msg``` 속성의 값으로 대체된다.
이것은 또한 data 객체의 ```msg``` 속성값이 바뀔 때 수정된다.

**v-once directive** 를 사용하여 데이터 변경에도 업데이트되지 않는 일회적 보간을 할 수 있지만,
같은 노드에 바인딩한 다른 모든 부분에도 영향을 미칠 수 있다는 점을 인지해야 한다.

\# Raw HTML


