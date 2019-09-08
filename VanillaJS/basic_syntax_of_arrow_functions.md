
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190908-lightgray.svg?style=flat-square)

# Basic syntax of arrow functions (ES6, 번역)

참조: [ES6 Arrow Functions: Fat and Concise Syntax in JavaScript](https://www.sitepoint.com/es6-arrow-functions-new-fat-concise-syntax-javascript/)

Arrow Functions 는 JavaScript 함수의 정의를 위한 ES6 의 새로운 문법으로 소개되었다.
이것은 개발자의 시간을 절약시키고 function scope 를 단순화한다.
통계에 따르면 이것은 ES6 형식 중에서 가장 많이 사용된다.

Source: [Axel Rauschmayer survey on favorite ES6 features](http://www.2ality.com/2015/07/favorite-es6-features.html)
Source: [Ponyfoo’s survey on the most commonly used ES6 features](https://ponyfoo.com/articles/javascript-developer-survey-results)

좋은 소식은, 많은 현대적인 브라우저들이 arrow functions 의 사용을 지원한다는 것이다.
이 포스트는 arrow functions 을 어떻게 사용하는지, 일반적인 문법은 무엇인지,
use cases 에는 무엇이 있는지, gotchas/pitfalls 등 자세한 내용을 살펴볼 것이다.

## Arrow FUnctions 는 무엇일까?

Arrow Functions, 또는 "fat arrow" 로도 불리는 이 함수는 함수 표현식을 작성하는 데 더 간결한 문법을 제공하는
[CoffeeScript](http://blogs.msdn.com/b/cdnstudents/archive/2013/09/17/visual-studio-tips-for-javascript-coders-try-coffeescript.aspx?WT.mc_id=16547-DEV-sitepoint-article83) 로부터 차용되었다.
이것은 새로운 token 인 ```=>``` 를 사용하는데, 이것은 마치 fat arrow 같이 보인다.
arrow functions 는 익명이며 ```this``` 가 함수에 bind 되는 방식을 바꾼다.

arrow functions 는 우리의 코드를 더 간결하게 하고, 함수 스코핑과 **this** 키워드의 사용을 단순하게 한다.
이것은 [C#](https://msdn.microsoft.com/en-us/library/bb397687.aspx?WT.mc_id=16547-DEV-sitepoint-article83) 또는 [Python 과 같은 다른 언어에서 사용되는 Lambdas 의 동작](http://www.diveintopython.net/power_of_introspection/lambda_functions.html)과
상당히 유사하게 작동하는 one-line mini functions 이다. (See also lambdas in JavaScript).
arrow functions 를 사용함으로써, 우리는 **function** 이나 **return** 키워드, 중괄호(curly brackets) 의 입력을 피할 수 있다.

## Arrow Functions 사용하기

arrow functions 를 사용 가능하게 할 수 있는 다양한 문법들이 존재하며, [MDN 에는 전체 리스트](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)가 있다.
우리는 이 중에서 당신의 입문을 위해 일반적인 것을 다룰 것이다.
이제 function expressions 를 사용하는 ES5 코드 작성법이 arrow functions 를 사용하는 ES6 에서는 어떻게 작성되는지 비교해 보자.

#### 여러 파라미터를 받는 기본 문법([from MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions))

```javascript
// (param1, param2, paramN) => expresstion

// ES5
var multiplyES5 = function(x, y) {
    return x * y;
};

// ES6
const multiplyES6 = (x, y) => { return x * y };
```

위의 arrow function 예제는 개발자로 하여금 더 적은 라인과 거의 절반의 타이핑으로 같은 결과값을 얻는 데 성공하도록 한다.

오직 하나의 표현식만 제공된다면 중괄호(curly brackets)는 요구되지 않는다.
앞의 예제는 다음과 같이 작성될 수 있다:

```javascript
const multiplyES6 = (x, y) => x * y;
```

#### 하나의 파라미터를 받는 기본 문법

오직 하나의 파라미터만 제공된다면 괄호는 선택적이다.

```javascript
// ES5
var phraseSplitterES5 = function phraseSplitter(phrase) {
    return phrase.split(' ');
}

// ES6
const phraseSplitterES6 = phrase => phrase.split(' ');

console.log(phraseSplitterES6('ES6 Awesomeness')); // ['ES6', 'Awesomeness']
```

#### 파라미터가 없는 경우

파라미터가 없는 경우에는 괄호가 요구된다.

```javascript
// ES5
var docLogES5 = function docLog() {
    console.log(document);
}

// ES6
var docLogES6 = () => { console.log(document); };
docLogES6(); // #document... <html> ...
```

#### 객체 리터럴 문법

함수 표현식과 같은 arrow functions 는 객체 리터럴 표현식을 반환할 수 있도록 활용될 수 있다.
단 하나 유의할 점은 block 과 object 를 구분하기 위해 body 가 괄호로 wrapped 되어야 한다는 것이다.(중괄호가 사용되어야 함)

```javascript
// ES5
var setNameIdsES5 = function setNameIds(id, name) {
    return {
        id: id,
        name: name
    };
};

// ES6
var setNameIdsES6 = (id, name) => ({ id: id, name: name });

console.log(setNameIdsES6 (4, 'kyle')); // Object {id: 4, name: 'kyle'}
```

## Arrow Functions 사용예

지금까지 기본 문법을 살펴봤는데, 이제 어떻게 사용하는지 보도록 하자.

arrow functions 를 사용하는 하나의 일반적인 용례는 배열 조작과 같은 것이다.
이것은 배열로 map 혹은 reduce 할 때 가장 일반적으로 사용된다. 다음의 단순한 배열 객체를 사용해 보자:

```javascript
const smartPhones = [
    { name: 'iphone', price: 649 },
    { name: 'Galaxy S6', price: 576 },
    { name: 'Galaxy Note 5', price: 489 },
];
```

우리는 다음과 같이 ES5 에서 names 나 prices 만을 활용해 배열 객체를 생성할 수 있다.

```javascript
// ES5
var prices = smartPhones.map(function(smartPhone) {
    return smartPhone.price;
});

console.log(prices); // [649, 576, 489]
```

arrow functions 는 더 간결하고 읽기 쉽다.

```javascript
// ES6
const prices = smartPhones.map(smartPhone => smartPhone.price);
console.log(prices); // [649, 576, 489]
```

다음은 [array filter method](https://msdn.microsoft.com/en-us/library/ff679973(v=vs.94).aspx) 를 사용한 또다른 예제이다.

```javascript
const array = [1, 2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15];

// ES5
var divisibleByThreeES5 = array.filter(function (v){
    return v % 3 === 0;
});

// ES6
const divisibleByThreeES6 = array.filter(v => v % 3 === 0);

console.log(divisibleByThreeES6); // [3, 6, 9, 12, 15]
```

#### [Promises](https://medium.com/javascript-scene/master-the-javascript-interview-what-is-a-promise-27fc71e77261) and Callbacks

비동기 callbacks 또는 promises 사용하도록 하는 코드는 종종 매우 많은 **function** 과 **return** 키워드를 포함한다.
promises 를 사용할 때, 이러한 함수 표현식은 줄줄이 이어진다. 여기 [chaining promises from the MSDN docs](https://msdn.microsoft.com/en-us/library/windows/apps/hh700334.aspx?WT.mc_id=16547-DEV-sitepoint-article83) 단순한 예제가 있다:

```javascript
// ES5
aAsync().then(function() {
    returnbAsync();
}).then(function() {
    returncAsync();
}).done(function () {
    finish();
});
```

arrow functions 를 사용한 다음 코드는 보다 단순하고, 틀림없이 읽기 쉬울 것이다.

```javascript
// ES6
aAsync().then(() => bAsync()).then(() => cAsync()).done(() => finish);
```

arrow functions 는 NodeJS 코드의 callback-laden 을 유사한 방식으로 단순화한다. (?)

#### **this** 가 의미하는 바는?

promises/callbacks 에서 arrow functions 를 사용하는 것의 또다른 이점은 **this** 키워드의 혼란성을 줄여준다는 것이다.
복합적으로 중첩된 함수들을 사용하는 코드에서는 정확한 **this** context 를 추적하거나 기억하여 bind 하기 어렵게 될 수 있다.
ES5 에서 당신은 ```.bind``` 메서드([느리다](https://jsperf.com/function-bind-performance/5))나
```var self = this;``` 를 사용하는 closure 를 생성하는 것과 같은 해결 방법을 사용할 수 있으나,

ES6 에서 arrow functions 는 함수 내에서 ```self = this``` closures 를 생성하거나 bind 를 사용할 필요 없이
호출부의 스코프를 유지하도록 할 수 있다.

개발자 [Jack Franklin](https://twitter.com/jack_franklin) 은 훌륭한 
[promise 를 단순화하기 위해 arrow function lexical **this** 를 사용하는 실용적인 예제](http://javascriptplayground.com/blog/2014/04/real-life-es6-arrow-fn/) 를 제공한다.
lexical
arrow functions 없이 promise 코드는 다음과 같이 작성되어야 한다:

```javascript
// ES5
API.prototype.get = function(resource) {
    var self = this;
    return new Promise(function(resolve, reject) {
        http.get(self.uri + resource, function(data) {
            resolve(data);
        });
    });
};
```

arrow function 를 사용하면, 더 간결하고 깔끔하게 동일한 결과를 얻는 데 성공한다.

```javascript
// ES6
API.prototype.get = function(resource) {
    return new Promise((resolve, reject) => {
        http.get(this.uri + resource, function(data) {
            resolve(data);
        });
    });
};
```

만약 a lexical **this** 를 위해 동적인 **this** 와 arrow functions 가 필요하다면 function expressions 를 사용할 수 있다.

## Arrow Functions 의 이해 와 함정 (Gotchas and Pitfalls)

새로운 arrow functions 는 ECMAScript 에 함수 문법의 유용성을 가져왔으나, 새로운 기능들이 그러하듯 이것도 고유의 함정과 gotchas 를 가지고 있다.

JavaScript 개발자이자 작성자인 Kyle Simpson 은 이 flow chart 를 사용하기로 결심할 때 arrow functions 에 충분한 함정이 있다고 느꼈다.
그는 arrow functions 에 너무 많은 혼란스러운 규칙/문법들이 존재한다고 주장했다.
다른 사람들은 arrow functions 를 사용하는 것이 타이핑은 줄이지만 코드의 가독성이 엄청나게 줄어들었다고 말해왔다.
모든 **function** 과 **return** 구문은 일반적으로 복합 중복 함수나 단순 함수 표현식을 읽기 쉽게 만든다.

arrow functions 를 포함할 것인지에 대한 것에만 해도 개발자의 의견들은 다양하다.
이러한 것들을 단순화하여, 여기 arrow functions 를 사용할 때 유의할 점 두 가지를 설명한다.

#### 더 많은 this

이전에 말했듯이 **this** 키워드는 arrow functions 에서 다르게 동작한다.
[call(), apply(), 그리고 bind()](http://javascriptissexy.com/javascript-apply-call-and-bind-methods-are-essential-for-javascript-professionals/) 함수들은
arrow functions 에서 **this** 의 값을 변화시키지 않는다.
(사실, 단순하게 함수 내에서의 **this** 의 값은 바뀔 수 없다; 그것은 함수 호출부와 동일한 값이 되기 때문이다.)
만약 다른 값을 bind 할 필요가 있다면, 함수 표현식을 사용해야 한다.

#### 생성자

arrow functions 는 다른 함수들이 가능한 것과는 다르게 생성자로써 사용될 수 없다.
다른 함수들에서 그랬던 것처럼 같은 객체를 생성하는 데에 이것을 사용하지 말라.
만약 arrow function 에 **new** 를 사용한다면, error 를 발생시킬 것이다.
내장 함수들(메서드)처럼 arrow functions 는 프로토타입 속성이나 다른 내부 메서드를 가지고 있지 않다.
왜냐하면 생성자는 일반적으로 JavaScript 에서 class-like 객체를 생성하는 데 사용되기 때문이다.
대신 새로운 [ES6 classes](https://blogs.msdn.microsoft.com/ie/2014/12/15/classes-in-javascript-exploring-the-implementation-in-chakra/?WT.mc_id=16547-DEV-sitepoint-article83) 를 사용해야 한다.

#### 제너레이터

arrow functions 는 경량으로 디자인 되었으므로 제너레이터로 사용될 수 없다.
ES6 에서 **yield** 키워드를 사용하는 것은 에러를 유발한다. 대신 [ES6 generators](https://davidwalsh.name/es6-generators) 를 사용하라.

#### Arguments object

arrow functions 는 다른 함수에서 그렇듯이 지역 변수 인자를 가지지 않는다.
arguments object 는 개발자로 하여금 동적으로 함수의 인자를 발견하고 접근할 수 있도록 하는 배열과 같은 객체이다.
JavaScript 함수는 무제한 개수의 인자를 받을 수 있기 때문에 이것은 유용하다.
arrow functions 이 객체를 갖지 않는다.
