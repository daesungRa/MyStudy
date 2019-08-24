
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190824-lightgray.svg?style=flat-square)

# Vanilla JS 소개

번역 : [VanillaJS 공식](http://vanilla-js.com/)

> Vanilla JS 는 놀라운 building 능력과 강력한 JavaScript applications 를 위한
빠르고, 경량이고, cross-platform 인 프레임워크이다.

> ..뭐지?

## 소개

[The Vanilla JS team](http://twitter.com/ericwastl) 은 프레임워크의 모든 byte 형식의 코드를 유지보수하며,
그것이 경량이고 직관적일 수 있도록 매일 열심히 일한다. 누가 **Vanilla JS** 를 사용할까? 질문을 환영한다. 다음을 보자.

```text
Facebook	Google	YouTube	Yahoo	Wikipedia	Windows Live	Twitter	Amazon	LinkedIn	MSN
eBay	Microsoft	Tumblr	Apple	Pinterest	PayPal	Reddit	Netflix	Stack Overflow
```

사실, Vanilla JS 는 이미 jQuery, Prototype JS, MooTools, YUI, 그리고 Google Web Toolkit 외에 더 많은 웹사이트에서 사용되고 있다.

## 시작하기

Vanilla JS 팀은 이것이 사용 가능한 그 무엇보다 가장 경량화된 프레임워크라는 사실에 자부심을 갖는다.
우리의 제품-질 배포 전략을 활용함으로써, 당신의 사용자들의 브라우저는 당신의 사이트에 요청을 보내기 이전에 자신의 메모리에 Vanilla JS 를 로드한다.

Vanilla JS 를 사용하기 위해서는, 당신의 앱의 어떤 HTML 에든 다음의 코드를 입력하면 된다.

```html
<script src="path/to/vanilla.js"></script>
```

당신의 어플리케이션을 프로덕션 배포 환경에 보낼 준비가 되었다면, 가장 빠른 방식으로 전환하라.

```text
...??
```

그렇다. 아무런 코드도 없다. Vanilla JS 는 그것을 십 년 이상 자동으로 로딩해 온 브라우저에 매우 popular 하다.

## 속도 비교

... (Vanilla JS 가 가장 빠르다는 측정 통계)

## 코드 예제

다음은 Vanilla JS 와 다른 프레임워크에서 일반적인 작업의 예제이다.

#### 엘리먼트를 fade out 후 삭제

|   |   |
|---|---|
| Vanilla JS  | var s = document.getElementById('thing').style;<br/>s.opacity = 1;<br/>(function fade(){(s.opacity-=.1)<0?s.display="none":setTimeout(fade,40)})();  |
| jQuery  | \<script src="//ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js"\>\</script\><br/>\<script\><br/>$('#thing').fadeOut();<br/>\</script\>  |

#### AJAX call 만들기

|   |   |
|---|---|
| Vanilla JS  | var r = new XMLHttpRequest();<br/>r.open("POST", "path/to/api", true);<br/>r.onreadystatechange = function () {<br/>if (r.readyState != 4 or r.status != 200) return;<br/>alert("Success: " + r.responseText);<br/>};<br/>r.send("banana=yellow");  |
| jQuery  | \<script src="//ajax.googleapis.com/ajax/libs/jquery/1/jquery.min.js"\>\</script\><br/>\<script\><br/>$.ajax({<br/>type: 'POST',<br/>url: "path/to/api",<br/>data: "banana=yellow",<br/>success: function (data) {<br/>alert("Success: " + data);<br/>},<br/>});<br/>\</script\>  |

## 부가설명 - 왜 jQuery 대신 Vanilla JS 인가?

다음의 내용은 [jaeyoon blog](https://blog.jaeyoon.io/2017/12/jquery-free.html) 의 포스팅을 참조한다.

요약하자면, Vanilla JS 는 너무나도 무거운 jQuery 를 비꼬기 위한 개그 사이트이다.
유용한 수많은 기능들이 포함되었는데도 다운받으면 용량이 0 byte 라던지, 샘플 코드가 오리지날 JS 와 완전히 동일하다든지,
이것이 십 년 전부터 브라우저에 적용되어져 왔다던지, 그리고 유수한 많은 웹사이트에서 활용되고 있다는 설명내용을 보면 유추할 수 있다.
(사실 'vanilla' 라는 단어에는 흔한, 어디서나 볼 수 있는 등의 수식적 의미가 존재한다.)

#### 왜?

문법적으로 상당히 간략화되고 자동화된 jQuery 가 이제는 jQuery free! 구호의 대상이 되고 있는 것에는 스마트폰과 모듈화 패러다임의 영향이 크다.

자동화와 이벤트를 위해 너무나도 무거워진 jQuery 는 CPU 처리능력과 메모리 용량이 상대적으로 부족한 스마트폰에서는 자원과 데이터 잡아먹는 하마가 되었다.
필요한 기능에 따라 경량으로 모듈화되는 현재 프레임워크의 패러다임 속에서 jQuery 를 부담스러운 대상으로 여겨지게 되었으며,
나아가 JS 는 Node JS 등 서버 사이드에서도 활발하게 사용되고 있으므로 클라이언트 브라우저에서만 동작하는 jQuery 는 이제 애매한 위치에 처했다.

#### 결정적 이유

결정적 이유는 React, Angular, Vue 등 보다 직관적이고 가벼운 프론트엔트 프레임워크/라이브러리가 등장한 것이다.

#### 정말 jQuery free 가 답인가?

결론적으로 꼭 그렇지만은 않다고 말할 수 있다.

일단 'free' 의 입장에서, **$** 대신 **querySelector** 의 사용은 그렇게까지 비효율적이지는 않다.
어느 정도 숙련되기만 하면 AJAX 를 JS 로 직접 사용하는 것도 아주 복잡한 일까지는 아니다.

'free' 이외의 입장에서(꼭 jQuery 를 옹호하는 것은 아니고), jQuery free 의 의미는 상황에 따라 달라진다.
여전히 웹 기반의 서비스에서는 jQuery 가 상당히 편리하게 활용되고 있기 때문이다. (현재 내가 그렇다..)

그렇다면, 지금까지 모든 것에 jQuery 만을 적용해왔던 흐름으로부터 벗어나, jQuery 도 수많은 옵션 중의 하나로 간주하고 필요에 따라
유용하게 활용할 수 있으면 그것으로 족할 것이다. 빠르게 변화하는 기술 패러다임 속에서, 어느 한 쪽에 치우치거나 고여버리면 변화를 따라가기 어렵게 되니, 모든 것을 자유롭게,
하지만 무분별하지는 않게 수용할 수 있는 열린 자세를 갖도록 하자.
