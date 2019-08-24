
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190824-lightgray.svg?style=flat-square)

# TypeScript in 5 minutes tutorial

ver : 3.5
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
