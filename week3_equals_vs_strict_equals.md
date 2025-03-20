# 🤔 동등 연산자(==)와 일치 연산자(===)의 차이점은 무엇인가요?

<br />

## 동등 연산자 (==)

동등 연산자는 두 개의 피연산자가 동일한지 여부를 판단해 `Boolean` 값을 반환한다.

<br />

**특징**

서로 다른 자료형인 경우에는 비교 전에 형 변환이 발생하는 특징이 있다.

```jsx
console.log("1" == 1); // true
```

<br />

**동등 연산자(==)의 한계**

앞서 본 특징으로, 동등 연산자는 `0`과 `false`를 구분하지 못한다. (그래서 느슨한 비교라고 불린다.)

```jsx
console.log(0 == false); // true
console.log("" == false); // true
```

⇒ 동등 연산자는 다른 피연산자를 비교할 때 피연산자를 숫자형으로 바꾸기 때문이다.

<br />

**동등 연산자의 비교 과정**

1. 두 피연산자가 동일한 자료형일 경우

   | Object                                                                               | 두 피연산자가 동일한 객체를 참조하는 경우에만 true를 반환           |
   | ------------------------------------------------------------------------------------ | ------------------------------------------------------------------- |
   | String                                                                               | 두 피연산자가 동일한 문자와 동일한 순서를 가질 경우에만 true를 반환 |
   | Number                                                                               | 두 피연산자가 동일한 값을 가질 경우에만 true를 반환                 |
   | (만약 피연산자 중 하나가 NaN이면 false를 반환하므로, NaN == NaN은 항상 false를 반환) |
   | Boolean                                                                              | 두 피연산자가 모두 true이거나 모두 false일 경우에만 true를 반환     |
   | BigInt                                                                               | 두 피연산자가 동일한 값을 가질 경우에만 true를 반환                 |
   | Symbol                                                                               | 두 피연산자가 동일한 심볼을 참조하는 경우에만 true를 반환           |

2. 피연산자 중 하나가 `null` 혹은 `undefined`일 때, 다른 피연산자도 `null` 혹은 `undefined`이라면 true를 반환

   ```jsx
   console.log(null == null); // true
   console.log(undefined == undefined); // true
   console.log(null == undefined); // true
   ```

3. 하나의 피연산자가 객체이고 다른 하나는 원시 값인 경우, 객체를 원시 값으로 변환

   **객체를 원시 값으로 변환하는 과정**

   1. **`valueOf()` 메소드 호출** : 객체를 원시 값으로 변환할 수 있는지 확인

      `valueOf()` 메소드는 객체 자체를 반환하거나 원시 값을 반환하도록 구현하고, 만약 `valueOf()`의 반환 값이 있다면 그 값이 사용된다. `toString()`보다 우선적으로 호출된다.

   2. **`toString()` 메소드 호출** : 객체가 원시 값을 반환하지 않거나 정의되지 않은 경우, 객체를 문자열로 변환

      `toString()`이 원시 값을 반환하면 그 값이 사용된다.

   3. **최종 비교**

      위 2가지 방법으로도 객체가 변환되지 않으면, `toPrimitive()`라는 내장 메서드를 사용한다. `toPrimitive()`는 `valueOf()`와 `toString()`을 호출하는 방식을 조합하여 최종적으로 객체를 원시 값으로 변환한다.

   ```jsx
   const obj = {
     valueOf: function () {
       return 42;
     },
     toString: function () {
       return "This is an object";
     },
   };

   console.log(obj == 42); // true
   console.log(obj == "This is an object"); // false
   ```

   즉, 객체가 원시 값으로 변환되는 과정은 `valueOf()` → `toString()` 순으로 진행된다.

4. 위 조건에 모두 해당하지 않은 두 피연산자는 원시 값으로 변환되어 비교

   **변환되는 원시 값** : 문자열, 숫자, 불리언, 심볼, 빅인트 중 하나

   - 원시 값 변환 후 동일 ⇒ 1단계로 비교한다.
   - 피연산자 중 하나가 Symbol이라면 `false` 반환한다.
   - 피연산자 중 하나만 `Boolean`형인 경우, `Boolean`형을 숫자로 변환해 비교한다.
     (`True == 1`, `False == 0`)
   - 문자열을 숫자로 변환한다. 이때 변환에 실패하면 `NaN`으로 `false`가 반환된다.
   - 숫자를 빅인트(`BigInt`)로 변환하고, 수학적 값에 따라 비교한다. 단, 숫자가 `±Infinity`나 `NaN`일 경우엔 `false`를 반환한다.
   - 문자열을 빅인트(`BigInt`)로 변환하고, 변환에 실패하면 `false`를 반환한다.

<br />
<br />

## 일치 연산자 (===)

일치 연산자는 엄격한 동등 연산자로, 두 개의 피연산자를 자료형까지 고려해 검사하여 `Boolean` 값을 반환한다.

<br />

**동등 연산자 vs 일치 연산자**

가장 큰 차이점은 일치 연산자는 비교 전에 형 변환이 일어나지 않는다. (엄격한 비교라고 불린다.)

<br />

**일치 연산자의 비교 과정**

일치 연산자는 아래와 같은 과정에 따라 비교한다.

1. 피연산자가 다른 유형인 경우 `False`를 반환
2. 두 피연산자가 모두 객체인 경우, 동일한 객체를 참조하는 경우에만 `True`를 반환
3. 두 피연산자가 모두 `null` 혹은 `undefined`라면 `True`를 반환
4. 피연산자 중 하나라도 `NaN`가 있다면 `False` 반환
5. 1~4의 조건에 해당하지 않으면 두 피연산자의 값을 비교

   1. 숫자형 : 동일한 값을 가진다. `+0`과 `-0`은 동일한 값으로 간주된다.
   2. 문자열 : 같은 순서로 동일한 문자를 가져야 한다.
   3. 불린형 : 모두 `True`이거나 모두 `False`여야 한다.

   <br />

## 부등호 연산자 (!=, !==)

동등 연산자와 일치 연산자는 ‘동일한지’를 비교하는 연산자라면,

부등호 연산자는 ‘다른지’를 비교한다.

느슨하고, 엄격하게 비교하는 것은 기존 동등 연산자와 일치 연산자의 특징을 따른다.

<br />

## 참조형을 비교하는 방법

기본 자료형은 변수에 ‘값’이 저장되기 때문에 간단하게 비교할 수 있다.

하지만 참조형(객체, 배열)은 변수에 ‘주소 값’이 저장되기 때문에, 이때는 동등 연산자와 일치 연산자가 동일하게 동작한다.

```jsx
let a = {};
let b = a; // 참조에 의한 복사

console.log(a == b); // true
console.log(a === b); // true
```

```jsx
let a = [1, 2, 3];
let b = [2, 3, 4];
let c = a;
let d = [1, 2, 3];

console.log(a == b); // false
console.log(a === b); // false

console.log(a == c); // true
console.log(a === c); // true

console.log(a == d); // false
console.log(a === d); // false
```

따라서 참조형의 값(내용)을 비교하고 싶다면 다른 방법을 사용해야 한다.

<br />
<br />

## 결론

- 동등 연산자(==)는 두 피연산자의 ‘값’을 느슨하게 비교한다. (의도적 형 변환이 발생한다.)
- 일치 연산자(===)는 두 피연산자의 ‘값’과 ‘자료형’을 엄격하게 비교한다.
- 참조형을 비교할 때는 동등 연산자와 일치 연산자가 주소 값을 비교하므로, 동일하게 작동한다.

<br />
<br />

---

https://developer.mozilla.org/ko/docs/Web/JavaScript/Reference/Operators/Equality

https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Strict_equality

https://ko.javascript.info/object-copy#ref-2138
