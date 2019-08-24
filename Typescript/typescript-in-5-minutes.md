
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190824-lightgray.svg?style=flat-square)

# TypeScript in 5 minutes tutorial

> ver : 3.5<br/>
url : [typescript tutorial](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html) 번역

타입스크립트로 간단한 웹 어플리케이션을 빌드해보자.

## TypeScript 설치

TypeScript 툴을 설치하는 데에는 크게 두 가지 방법이 있다.

- npm 사용 (Node.js 패키지 매니저)
- TypeScript 의 Visual Studio 플러그인 설치

Visual Studio 2017 과 2015 Update 3 에는 TypeScript 가 기본적으로 포함되어 있다. 만약 Visual Studio 와 함께 설치하지 않아도 별도로 다운받을 수 있다.

For NPM users:
```text
> npm install -g typescript
```

## 첫 TypeScript 파일 빌드하기

당신의 편집기에서 다음의 JavaScript 코드를 입력하고 ```greeter.ts``` 파일을 생성하라.

```text
function greeter(person) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.textContent = greeter(user);
```

## 코드 컴파일

우리는 ```.ts``` 확장자를 사용했지만, 이것은 아직은 단지 JavaScript 이다.
당신은 이것을 JavaScript app 에 곧바로 **복사/붙여넣기**할 수 있다.

command line 에서 TypeScript 컴파일러를 실행하면,

```text
tsc greeter.ts
```

당신이 입력한 것과 동일한 JavaScript 코드를 포함한 ```greeter.js``` 파일이 생성될 것이다.
이제 JavaScript app 에서 TypeScript 를 사용함으로써 이것을 실행한다.

그러면 우리는 TypeScript 가 제공하는 새로운 몇몇 도구들의 이점을 얻게 된다.
다음과 같이 'person' 함수의 매개변수에 ```: string``` type annotation 을 추가하라.
```text
function greeter(person: string) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.textContent = greeter(user);
```

## 타입 어노테이션

TypeScript 의 타입 어노테이션은 함수 혹은 변수의 의도된 약속을 기록하는 경량화된 방식이다.
이 경우, 우리는 ```greeter``` 함수가 단일 문자열 파라미터에 의해 호출되도록 의도할 수 있다.
이제 배열을 대신 전달하도록 ```greeter``` 함수의 호출 부분을 변경해보자.

```text
function greeter(person: string) {
    return "Hello, " + person;
}

let user = [0, 1, 2];

document.body.textContent = greeter(user);
```

재컴파일을 하면, 다음과 같은 에러 메시지를 보게 된다.

```text
error TS2345: Argument of type 'number[]' is not assignable to parameter of type 'string'.
```

유사하게, ```greeter``` 함수 호출부에 모든 인자를 제거해볼 수 있다.
TypeScript 는 당신이 기대되지 않는 개수의 파라미터와 함께 이 함수를 호출했다는 사실을 알려준다.
이때 TypeScript 는 당신이 작성한 두 가지 구조의 코드와 타입 어노테이션에 기반한 정적 분석 결과를 제공할 수 있다.

에러가 있다고 하더라도 ```greeter.js``` 파일은 여전히 생성된다는 사실을 숙지하라.
당신의 코드에 에러가 있어도 TypeScript 를 사용할 수 있다.
그러나 이 경우 TypeScript 는 당신의 코드가 기대하지 않는 방식으로 실행될 수도 있다는 사실을 경고한다.

## 인터페이스

우리의 예제를 더 발전시켜보자.
여기 firstName 과 lastName 필드를 가지고 있는 객체를 묘사하는 인터페이스가 있다.
TypeScript 에서는, 내부 구조가 서로 호환 가능하다면 위 두 타입도 호환 가능하다.
이것은 명시적인 **implements** 구문 없이 인터페이스가 요구하는 형태를 포함함으로써 해당 인터페이스를 구현할 수 있도록 한다.

```text
interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = { firstName: "Jane", lastName: "User" };

document.body.textContent = greeter(user);
```

## 클래스

마지막으로, 클래스를 활용해 예제를 확장해보자.
TypeScript 는 클래스 기반 객체지향 프로그래밍을 지원하는 것과 같이 JavaScript 의 새로운 형식을 지원한다.

다음은 생성자와 몇몇 public fields 를 포함한 **Student** 클래스를 생성한다.
클래스와 인터페이스는 함께 작 동작하지만, 프로그래머가 올바른 레벨의 추상화를 결정할 수 있도록 해야만 한다는 사실을 인지해야 한다.

또 숙지할 것은, 생성자의 인자로 ```public``` 을 사용하는 것은
우리가 name 을 전달함으로써 자동으로 클래스의 속성값(properties)이 지정되도록 하는 _shorthand_(역주: 속기) 방식이다.

```text
class Student {
    fullName: string;
    constructor(public firstName: string, public middleInitial: string, public lastName: string) {
        this.fullName = firstName + " " + middleInitial + " " + lastName;
    }
}

interface Person {
    firstName: string;
    lastName: string;
}

function greeter(person: Person) {
    return "Hello, " + person.firstName + " " + person.lastName;
}

let user = new Student("Jane", "M.", "User");

document.body.textContent = greeter(user);
```

```tsc greeter.ts``` 를 재실행하면 생성된 JavaScript 는 이전 코드의 결과와 같다는 사실을 알 수 있다.
TypeScript 의 클래스는 단지 JavaScript 에서 빈번히 사용되는 프로토타입 기반 객체지향 기법을 위한 간략한 방법일 뿐이다.

## TypeScript 웹앱 실행하기

이제 다음의 ```greeter.html``` 코드를 입력하자.

```html
<!DOCTYPE HTML>
<html>
    <head>
        <title>TypeScript Greeter</title>
    </head>
    <body>
        <script src="greeter.js"></script>
    </body>
</html>
```

당신의 첫 TypeScript 웹앱을 실행하기 위해 브라우저에서 ```greeter.html``` 을 열어보자!

Optional: Visual Studio 에서 ```greeter.ts``` 를 열거나 TypeScript playground 에 코드를 붙여넣어라.
그럼 식별자들이 자신들의 타입에 따라 hover(마우스를 올리면 옆에 타입이 나타남)할 수 있다.
몇몇 경우에 이러한 타입들은 자동으로 유추된다는 사실을 숙지하라.
... 후략
