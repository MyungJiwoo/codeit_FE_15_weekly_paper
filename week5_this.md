# 🤔 자바스크립트의 this는 무엇인가요?

`this`는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 `자기 참조 변수(self-referencing variable)`이다. this를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메소드를 참조할 수 있다.

`this`는 자바스크립트 엔진에 의해 암묵적으로 생성되고 코드 전역에서 참조할 수 있다

하지만 `this`가 가리키는 값인 `this 바인딩`은 함수 호출 방식에 의해 동적으로 결정된다.

즉, 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 this에 바인딩할 객체가 동적으로 결정된다.

<br />

### 바인딩

`바인딩`이란 쉽게 말해 식별자와 값을 연결하는 과정을 말한다.

예를 들어, 변수 선언은 변수 이름과 확보된 메모리 공간의 주소를 연결하는 과정이다.

<br />

### this가 필요한 이유

```jsx
const circle = {
	radius: 5;
	getDiameter() {
		return 2 * circle.radius;
	}
};
```

이런 `circle` 객체가 있다고 했을 때, 만약 `radius`를 파라미터로 바꿔서 전달 받고, 그 값으로 객체를 생성한다면 `getDiameter()` 내의 `radius`는 어떤 값을 갖고 있어야 할까?

예를 들어 `circle` 객체가 실행되는 순간에 객체가 생기기 때문에 `radius`는 어떤 값이 될지 모르니 `circle`이라고 지칭하는 것은 옳지 않다.

즉, 생성자 함수를 정의하는 시점에는 아직 인스턴스가 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다. 이때 자신이 속한 객체나 자신이 생성할 인스턴스를 가리키는 식별자가 `this`이다.

따라서 아래와 같이 `this`를 사용한 코드로 바꿀 수 있다.

```jsx
const circle = {
	radius: 5;
	getDiameter() {
		return 2 * this.radius;
	}
};
```

<br />
<br />

## 함수 호출 방식과 this 바인딩

함수 호출 방식은 크게 4가지로 나뉜다.

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. `Function.prototype.apply / call / bind` 메서드에 의한 간접 호출

<br />

### 일반 함수 호출

```jsx
const foo = function () {
  console.dir(this);
};

foo();
```

기본적으로 `this`에는 전역 객체가 바인딩된다.

전역 함수와 중첩 함수를 일반 함수로 호출한 경우 함수 내부의 `this`에는 전역 객체가 바인딩된다. 메서드 내에서 정의한 중첩 함수, 콜백 함수 등 어떤 함수라도 일반 함수로 호출되면 `this`에 전역 객체가 바인딩 된다.

하지만 `this`의 역할은 객체의 프로퍼티나 메서드를 참조하기 위한 용도이기 때문에, 일반 함수에서 `this`는 의미가 없다. 따라서 `strict mode`에서 일반 함수의 this에는 `undefined`가 바인딩 된다.

결론적으로, 일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 this에는 전역 객체가 바인딩된다.

**내 맘대로 this를 바꿀 수는 없나요?**

하지만 명시적으로 `this`를 바인딩 할 수 있는 `Function.prototype.apply`, `Function.prototype.call`, `Function.prototype.bind` 메소드를 이용하면 원하는 객체를 바인딩할 수도 있다.

혹은 `화살표 함수`를 통해 상위 스코프의 `this`를 가리키게 할 수도 있다.

```jsx
var value = 1;

const obj = {
  value: 100,
  foo() {
    seetTimeout(() => console.log(this.value), 100); // 100
  },
};
```

<br />

### 화살표 함수 호출

화살표 함수는 `this`를 바인딩 하지 않고, 상위 스코프의 `this`를 사용한다. 따라서 객체 내부에서 화살표 함수를 사용하면, `this`는 전역 객체(`window` 혹은 `global`)을 가리킨다.

```jsx
var value = 1;

const obj = {
	value: 100,
	foo: () => console.log(this.value); // undefined
};
```

위 코드에서 `foo()`는 `obj` 안에 선언되어 있지만, 화살표 함수라서 `obj` 스코프를 따르고, `obj`는 전역 스코프이기 때문에 `foo()`도 전역 스코프이다.

<br />

### 메서드 호출

메서드 호출은, 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(`.`) 연산자 앞의 객체가 바인딩된다. 하지만 메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다는 것이다.

메서드는 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체로, 프로퍼티가 함수 객체를 가리키고 있을 뿐이다. 따라서 메서드는 다른 객체의 메서드가 될 수도 있고, 일반 변수에 할당하여 일반 변수로 호출될 수도 있다.

```jsx
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function () {
  return this.name;
};

// 1) 인스턴스에서 메서드를 호출한 경우
const me = new Person("Lee");
console.log(me.getName()); // Lee

// 2) Person 객체(Person.prototype)에서 직접 메서드 호출
Person.prototype.name = "Kim";
console.log(Person.prototype.getName()); // Kim
```

<br />

### 생성자 함수 호출

생성자 함수는 객체의 이름과 같은 함수 이름을 갖고 인스턴스를 생성하는 함수로, 내부의 `this`에는 생성자 함수가 미래에 생성할 인스턴스가 바인딩된다.

그래서 생성자 함수를 정의했더라도 `new` 연산자와 함께 사용하지 않으면 일반 함수로 동작한다.

```jsx
function Circle(radius) {
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 1) 생성자 함수로 호출한 경우
const circle1 = new Circle(5);
console.log(circle1.getDiameter()); // 10

// 2) 생성자 함수를 일반 함수로 호출한 경우
const circle2 = Circle(15);
console.log(circle2); // undefined (일반 함수인 Circle에는 return이 없다.)
console.log(radius); // 15 (일반 함수인 Circle의 this는 전역 객체이다.)
```

**2번 경우에서 radius가 15로 출력되는 이유**

`Circle(15)`가 일반 함수로 호출되면서, `this.radius`는 전역 객체로 분류된다. 따라서 `radius`를 출력하면 `15`가 출력되는 것이다.

<br />

### Function.prototype.apply / call / bind 메서드에 의한 간접 호출

`Function.prototype.apply`, `Function.prototype.call`, `Function.prototype.bind`는 자바스크립트 함수에서 `this` 값을 설정하고, 함수 실행을 제어할 수 있다.

3가지 모두 `Funcion.prototype`의 메서드로, 모든 함수가 상속받아서 사용할 수 있다.

| 메서드    | 설명                                                 | 인수 전달 방식                | 반환값      |
| --------- | ---------------------------------------------------- | ----------------------------- | ----------- |
| `call()`  | 즉시 실행, this를 설정하고 인수를 개별적으로 전달    | 개별 인수                     | undefined   |
| `apply()` | 즉시 실행, this를 설정하고 인수를 배열 형식으로 전달 | 배열 형태의 인수              | undefined   |
| `bind()`  | 새로운 함수 반환, this를 설정하고 나중에 실행        | 개별 인수 또는 배열 형태 인수 | 새로운 함수 |

`Function.prototype.apply`와 `Function.prototype.call` 메서드는 `this`로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다. 그래서 `apply`와 `call`의 본질은 함수를 호출하는 것이다.

```jsx
/*
* @param thisArg : this로 사용할 객체
* @param argsArray : 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
* @returns : 호출된 함수의 반환값
*/
Function.prototype.apply(thisArg[, argsArray]);
```

```jsx
/*
* @param thisArg : this로 사용할 객체
* @param arg1, arg2, ... : 함수에게 전달할 인수 리스트
* @returns : 호출된 함수의 반환값
*/
Function.prototype.call(thisArg[, arg1[, arg2[, ...]]]);
```

이런 `apply`와 `call` 메서드는 보통 유사 배열 객체에 배열 메서드를 사용하고 싶을 때 사용된다.

- `arguments`와 같은 상황이 대표적이다.

  ```jsx
  function convertArgsToArray() {
    console.log(arguments);
    const arr = Array.prototype.slice.call(arguments);
    return arr;
  }

  converArgsToArray(1, 2, 3);
  ```

하지만 `Function.prototype.bind` 메서드는 함수를 호출하지 않는다. 다만 첫 번째 인수로 전달한 값으로 `this 바인딩`이 교체된 함수를 새롭게 생성해 반환한다.

- `bind` 메서드는 함수를 새롭게 생성해 반환하지, 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.

  ```jsx
  function getThisBinding() {
  	return this;
  }

  coinst thisArg = { a : 1 };

  console.log(getThisBinding.bind(thisArg));  // getThisBinding
  console.log(getThisBinding.bind(thisArg)());  // {a: 1}
  ```

`bind` 메서드는 메서드의 `this`와 메서드 내부의 중첩 함수 또는 콜백 함수의 `this`가 불일치하는 문제를 해결하기 위해 많이 사용된다.

<br />
<br />

## 결론

| 함수 호출 방식                                                 | this 바인딩                                                                |
| -------------------------------------------------------------- | -------------------------------------------------------------------------- |
| 일반 함수 호출                                                 | 전역 객체                                                                  |
| 메서드 호출                                                    | 메서드를 호출한 객체                                                       |
| 생성자 함수 호출                                               | 생성자 함수가 (미래에) 생성할 인스턴스                                     |
| Function.prototype.apply / call / bind 메서드에 의한 간접 호출 | Function.prototype.apply / call / bind 메서드에 첫 번째 인수로 전달한 객체 |

<br />
<br />

---

<모던 자바스크립트 Deep Dive>
