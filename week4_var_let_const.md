# 🤔 var, let, const를 중복 선언 허용, 스코프, 호이스팅 관점에서 서로 비교해 주세요.

<br>

### 미리 보기

| 키워드  | 호이스팅            | TDZ | 스코프      | 중복 선언 | 재할당 |
| ------- | ------------------- | --- | ----------- | --------- | ------ |
| `var`   | 선언부만 호이스팅됨 | X   | 함수 스코프 | O         | O      |
| `let`   | 선언부만 호이스팅됨 | O   | 블록 스코프 | X         | O      |
| `const` | 선언부만 호이스팅됨 | O   | 블록 스코프 | X         | X      |

<br>

## 📌 스코프 (scope)

스코프란 값과 표현식이 표현되거나 참조될 수 있는, 현재 실행되는 컨텍스트를 의미한다. 사용하려는 변수 또는 표현식이 해당 스코프내에 있지 않다면 사용할 수 없다.

스코프는 계층 구조를 가지는데, 상위 스코프는 하위 스코프에 접근할 수 있지만 그 반대 경우는 불가능하다.

**스코프의 종류**

1. 전역 범위 : 스크립트 모드에서 실행되는 모든 코드의 기본 범위
2. 모듈 범위 : 모듈 모드에서 실행되는 코드의 범위
3. 함수 범위 : `function`으로 생성된 범위
4. 블록 범위 : 중괄호 쌍(블록)으로 생성된 범위

<br>

## 📌 호이스팅 (hoisting)

호이스팅은 인터프리터가 코드를 실행하기 전에 함수, 변수, 클래스 또는 import의 선언문을 해당 범위의 맨 위로 끌어올리는 것처럼 보이는 현상이다. 쉽게 말해 자바스크립트에서 변수 및 함수 선언이 코드 실행 전에 메모리에 할당되는 현상이다.

변수 호이스팅의 경우, 선언과 할당이 동시에 작성되어도 ‘선언’만 호이스팅 되며, 값의 할당은 실제로 할당 코드가 동작할 때 이뤄진다.

이처럼 호이스팅은 변수 선언이 실제 선언 위치와 다르게 동작할 수 있어서, 코드의 예측 가능성을 낮추는 요인 중 하나로 꼽힌다.

<br>

## 📌 TDZ (Temporal Dead Zone)

일시적 사각지대라고 불리는 TDZ는 스코프의 시작 지점부터 초기화 시작 지점까지의 구간을 말한다.

즉, 코드가 처음 실행되고 변수가 선언되기 전까지의 구간을 말한다. 이는 변수가 선언되기 전에 접근하는 것을 방지하기 때문에 코드의 예측 가능성을 높이고, 변수 선언과 초기화를 구분하면서 코드의 안정성을 높일 수 있다.

TDZ 안에 있는 변수를 접근하려고 하면 `ReferenceError`가 발생한다.

`let`과 `const`가 TDZ의 영향을 받고, 그렇기 때문에 `var`보다 권장되는 변수 초기화 방식이다.

<br>

## 1️⃣ var

**호이스팅**

`var`는 호이스팅이 발생한다. 따라서 코드 상단에 변수를 선언하는 것을 권장한다.

호이스팅은 선언만 가능하므로 `undefined`가 출력된다.

```jsx
console.log(a); // undefined

var a = 10;
console.log(a); // 10
```

**스코프**

`var`는 함수 스코프, 블록 내부에서 선언 했더라도 바깥에서 접근할 수 있다.

```jsx
if (true) {
  var b = 20;
}
console.log(b); // 20
```

다만 함수 스코프이므로 함수 안에 선언한 변수는 외부에서 접근할 수 없다.

```jsx
function test() {
  var b = 20;
}
console.log(b); // ❌ ReferenceError
```

**중복 선언**

`var`는 중복 선언이 가능하다. 하지만 기존의 변수를 덮어쓰는 개념으로, 재할당과는 다르다.

```jsx
var c = 30;
var c = 40;
console.log(c); // 40
```

<br>

## 2️⃣ let

**호이스팅**

정확히 말하면 `let`도 호이스팅이 발생한다. 하지만 일시적 사각지대인 TDZ로 인해 변수가 초기화되기 전까지는 접근할 수 없고, 선언 전 호출시 `ReferenceError`가 발생한다.

```jsx
console.log(a); // ❌ ReferenceError: Cannot access 'a' before initialization
let a = 10;
console.log(a);
```

**스코프**

`let`은 블록 스코프로 함수, 조건문을 가리지 않고 코드 블록 내부에서만 접근할 수 있다.

```jsx
// 조건문 (블록 스코프)
if (true) {
  let b = 20;
}
console.log(b); // ❌ ReferenceError: b is not defined
```

```jsx
// 함수 (함수 스코프)
function test() {
  let c = 30;
}
console.log(c); // ❌ ReferenceError: c is not defined
```

**중복 선언**

`let`은 같은 스코프 안에서 중복 선언시 `SyntaxError`가 발생한다.

```jsx
let d = 40;
let d = 50; // ❌ SyntaxError: Identifier 'd' has already been declared
```

하지만 `let`은 재할당이 가능하다.

```jsx
let e = 6;
e = 60;
console.log(e); // 60
```

<br>

## 3️⃣ const

**호이스팅**

`const`도 `let`과 동일하게 호이스팅은 발생하지만 TDZ로 인해 초기화되기 전까지 접근할 수 없다.

```jsx
console.log(a); // ❌ ReferenceError: Cannot access 'a' before initialization
const a = 10;
console.log(a);
```

그리고 `const`는 선언과 동시에 값을 할당해야 한다. 만약 그렇지 않으면 `SyntaxError`가 발생한다.

```jsx
const b; // ❌ SyntaxError: Missing initializer in const declaration
b = 20;
```

**스코프**

`const`도 블록 스코프를 가진다.

```jsx
// 조건문 (블록 스코프)
if (true) {
  const c = 30;
}
console.log(c); // ❌ ReferenceError: c is not defined
```

```jsx
// 함수 (함수 스코프)
function test() {
  const d = 40;
}
console.log(d); // ❌ ReferenceError: d is not defined
```

**중복 선언**

`const`도 중복 선언이 불가능하다. 그리고 재할당도 불가능하기 때문에 상수를 선언할 때 주로 사용된다.

만약 재할당을 시도하면 `TypeError`가 발생한다.

장점으로는 코드의 안정성을 높일 수 있다.

```jsx
const e = 50;
const e = 50; // ❌ SyntaxError: Identifier 'e' has already been declared
```

```jsx
const d = 6;
d = 60; // ❌ TypeError: Assignment to constant variable.
```

<br>

---

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Statements/var

https://f-lab.kr/insight/understanding-javascript-variable-declaration-and-tdz-20240828
