# 🤔 자바스크립트에서 얕은 복사(Shallow Copy)와 깊은 복사(Deep Copy)는 무엇인가요?

<br />

## 얕은 복사 (Shallow Copy)

객체의 참조 주소만 복사하여 새로운 객체를 생성되므로, 원본 객체와 복사된 객체가 동일한 데이터를 참조한다.

따라서 원본이나 복사본을 변경하면, 다른 객체도 변경될 수 있다.

얕은 복사는 메모리를 적게 사용하고, 과정이 간단하면서, 객체의 상태를 부분적으로 복사할 수 있기 때문에 성능이 좋다. 하지만 객체의 모든 상태를 완전하게 복제할 수 없기 때문에 원본이 변경될 수 있어 안전하지 않다.

<br>

**코드로 확인해보자**

얕은 복사로, 같은 참조 값을 공유하는 객체끼리는 값이 변경되면 원본 객체의 값도 변경된다.

```jsx
let array = [1, 2, 3, 4, 5];
console.log(array); // [1, 2, 3, 4, 5]

let arrayCopy1 = array; // 얕은 복사
console.log(arrayCopy1); // [1, 2, 3, 4, 5]

arrayCopy1[0] = 10;
console.log(array, arrayCopy1); // [10, 2, 3, 4, 5] [10, 2, 3, 4, 5]
```

<br>

**중첩된 객체**

중첩된 객체의 경우, 값이 아닌 최상위 속성만 복사한다. 따라서 복사본의 최상위 속성을 재할당해도 원본 객체에는 영향이 가지 않지만, 복사본의 중첩 객체 속성을 재할당하면 원본 객체에 영향을 끼친다.

<br>

**코드로 확인해보자**

- 1번 : 복사본의 중첩 객체의 **속성**을 재할당 했기 때문에 원본도 함께 수정된다.
- 2번 : 최상위 속성의 `[3, 4, 5]`가 담긴 주소 값을 `3`의 값으로 변경했기 때문에 원본에 영향이 없다.

```
let array = [1, 2, [3, 4, 5]];
console.log(array); // [1, 2, [3, 4, 5]]

let arrayCopy2 = [...array]; // 얕은 복사
console.log(arrayCopy2); // [1, 2, [3, 4, 5]]

/* 1번 */
arrayCopy2[2][0] = 300;
console.log(array, arrayCopy2); // [1, 2, [300, 4, 5]] [1, 2, [300, 4, 5]]

/* 2번 */
arrayCopy2[2] = 3;
console.log(array, arrayCopy2); // [1, 2, [300, 4, 5]] [1, 2, 3]
```

<br>

**장단점**

| 장점                                                      | 단점                                          |
| --------------------------------------------------------- | --------------------------------------------- |
| 메모리를 적게 사용하고, 복사 과정이 간단하여 성능이 좋다. | 객체를 완전히 복제할 수 없어서 안전하지 않다. |

<br>
<br>

### 얕은 복사 구현 방식

1. `Array.from()` 메소드
2. 스프레드 연산자
3. `Array.prototype.concat()` 메소드
4. `Array.prototype.slice()` 메소드
5. `Object.assign()` 메소드
6. `Object.create()` 메소드

<br>
<br>

## 깊은 복사 (Deep Copy)

객체의 모든 필드를 복사해 새로운 객체를 생성하므로, 원본 객체와 복사된 객체가 독립적으로 존재한다.

깊은 복사는 객체의 모든 필드를 재귀적으로 복사하기 때문에 메모리를 많이 사용하고, 과정이 복잡해 성능이 저하될 수 있다. 그러나 모든 필드가 독립적으로 완전히 복제되기 때문에 원본을 안전하게 사용할 수 있다.

<br>

**원시 값과 객체의 복사**

- **원시 값의 경우**
  원시 값은 값 자체를 비교하기 때문에, 같은 값을 비교하면 무조건 `true`가 반환된다.

  ```jsx
  let a = 10;
  let b = a; // 값 복사
  let c = 10;

  console.log(a === b); // true
  console.log(a === c); // true
  ```

- **객체의 경우**
  객체는 메모리 주소로 비교하므로, 두 객체의 내용이 같더라도 `false`가 반환될 수 있다.
  (다른 객체일 수 있다.)

  ```jsx
  let o1 = { name: "Alice" };
  let o2 = o1; // 참조 복사
  let o3 = { name: "Alice" }; // 새로운 객체 생성

  console.log(o1 === o2); // true (같은 객체)
  console.log(o1 === o3); // false (구조적으로 같아도 다른 객체)
  ```

<br>

**공식적인 깊은 복사의 정의**

위에서 봤듯이, 객체는 같은 내용이어도 다른 객체일 수 있다. 따라서 공식적인 깊은 복사의 정의는 아래와 같다.

- 두 객체는 같은 객체가 아니다. (`o1` !== `o2`)
- `o1`과 `o2`의 속성은 같은 이름과 순서이다.
- 두 객체의 속성 값은 서로의 깊은 복사 값이다.
- 두 객체의 프로토타입 체인은 구조적으로 동일하다.

<br>

**중첩된 속성이 없는 객체의 복사**

얕은 복사는 최상위 속성만 복사한다. 즉, 중첩된 속성이 없는 객체는 깊은 복사와 얕은 복사가 동일하다.

<br>

**장단점**

| 장점                                      | 단점                                         |
| ----------------------------------------- | -------------------------------------------- |
| 독립된 원본과 복사본이 생성되어 안전하다. | 복사 과정이 복잡하고 메모리를 많이 사용한다. |
| 얕은 복사에 비해 성능이 떨어진다.         |

<br>
<br>

### 깊은 복사 구현 방식

1. `JSON.parse(JSON.stringify())` : 직렬화를 통한 깊은 복사

   간단한 객체의 경우 직렬화가 가능하지만, 함수 (클로저 포함), 심볼, HTML 요소를 나타내는 객체 등 복잡한 객체의 경우 `JSON.stringify()` 호출을 실패할 수 있다. 이때는 깊은 복사본을 만드는 방법이 없다.

2. 재귀 함수

<br>
<br>

---

https://f-lab.kr/insight/understanding-deep-and-shallow-copy-20240717

https://developer.mozilla.org/ko/docs/Glossary/Shallow_copy

https://developer.mozilla.org/ko/docs/Glossary/Deep_copy
