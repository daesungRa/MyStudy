
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190824-lightgray.svg?style=flat-square)

# Vue JS

> 접근성 : HTML, CSS, JavaScript 만 알면됨!

> 유연성 : 라이브러리에서부터 프레임워크 사이에서 서서히 스케일업하여 적용 가능!

> 고성능 : 20KB min+gzip 컴팩트 런타임! 초고속 Virtual DOM!

> == 최소의 노력, 최대의 성과!

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

#### 
