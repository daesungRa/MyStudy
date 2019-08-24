
![author](https://img.shields.io/badge/author-daesungRa-lightgray.svg?style=flat-square)
![date](https://img.shields.io/badge/date-190824-lightgray.svg?style=flat-square)

# TypeScript in 5 minutes tutorial

ver : 3.5
url : [typescript tutorial](https://www.typescriptlang.org/docs/handbook/typescript-in-5-minutes.html) 번역

타입스크립트로 간단한 웹 어플리케이션을 빌드해보자.

#### TypeScript 설치

TypeScript 툴을 설치하는 데에는 크게 두 가지 방법이 있다.

- npm 사용 (Node.js 패키지 매니저)
- TypeScript 의 Visual Studio 플러그인 설치

Visual Studio 2017 과 2015 Update 3 에는 TypeScript 가 기본적으로 포함되어 있다. 만약 Visual Studio 와 함께 설치하지 않아도 별도로 다운받을 수 있다.

For NPM users:
```text
> npm install -g typescript
```

#### 첫 TypeScript 파일 빌드하기

당신의 편집기에서 다음의 JavaScript 코드를 입력하고 ```greeter.ts``` 파일을 생성하라.

```text
function greeter(person) {
    return "Hello, " + person;
}

let user = "Jane User";

document.body.textContent = greeter(user);
```

#### 코드 컴파일

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

#### 타입 어노테이션

TypeScript 의 타입 어노테이션은 함수 혹은 변수의 의도된 약속을 기록하는 경량화된 방식이다.
이 경우, 우리는 ```greeter``` 함수가 단일 문자열 파라미터에 의해 호출되도록 의도할 수 있다.
