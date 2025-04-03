---
marp: true
---

## 좋은 컴포넌트란?

---

### 컴포넌트

리액트의 핵심으로 꼽히는 컴포넌트는 리액트로 만들어진 앱을 이루는 최소한의 단위이다.

컴포넌트는 `데이터(props)`를 입력받아 `view(state)`에 따라 상태 DOM 노드를 출력한다.

결국 컴포넌트의 최종 목표는 ‘**재사용**’이다. 이를 위해선 **의존성을 최소화** 시켜야 한다.

---

### 추상화

> 공통된 기능을 가진 컴포넌트를 일반화하여 재사용할 수 있도록 만드는 과정

코드의 중복을 줄이면서 가독성을 높이고, 유지보수를 쉽게 할 수 있다.

---

**고차 컴포넌트(Higher-Order Compoenet, HOC)**

컴포넌트를 인수로 받아 새로운 컴포넌트를 반환하는 함수이다.

HOC를 사용하면 공통된 기능을 가진 컴포넌트를 추상화하여 재사용할 수 있다.

---

```jsx
import React from "react";

// HOC
const withLoading = (Component) => {
  return class WithLoading extends React.Component {
    render() {
      return this.props.isLoading ? (
        <p>Loading...</p>
      ) : (
        <Component {...this.props} />
      );
    }
  };
};

// HOC를 적용한 컴포넌트
const MyComponentWithLoading = withLoading(MyComponent);

export default MyComponentWithLoading;
```

[관련 공식 문서](https://ko.legacy.reactjs.org/docs/higher-order-components.html)

---

### 재사용성

> 컴포넌트를 여러 곳에서 사용할 수 있도록 만드는 것

재사용 가능한 컴포넌트를 만들면 개발 속도가 빨라지고, 코드의 일관성을 유지할 수 있다.

이를 위해선 컴포넌트의 역할을 명확히 하고, 필요한 매개변수를 통해 동작을 제어한다.

**재사용 원칙**

1. 컴포넌트는 **하나의 역할**을 수행한다
2. 필요한 **매개변수**를 통해 동작을 제어한다.
3. 가능한 한 **독립적**으로 동작해야 한다.
4. 컴포넌트의 **상태는 최소한으로 유지**해야 한다.
5. **스타일을 외부에서 제어**할 수 있도록 해야 한다.

---

```jsx
import React from "react";

const Button = ({ onClick, children, style }) => (
  <button onClick={onClick} style={style}>
    {children}
  </button>
);

const App = () => (
  <div>
    <Button
      onClick={() => alert("Button 1 clicked")}
      style={{ backgroundColor: "blue", color: "white" }}
    >
      Button 1
    </Button>

    <Button
      onClick={() => alert("Button 2 clicked")}
      style={{ backgroundColor: "green", color: "white" }}
    >
      Button 2
    </Button>
  </div>
);

export default App;
```

---

## 내가 생각하는 좋은 컴포넌트의 핵심, “재사용성”

비슷한 UI 이지만 사소한 디테일이 달라 새로운 컴포넌트를 만드는 경우가 있었다.

⇒ `Compound Component 패턴` 이면 해결할 수 있겠다!

---

### Compound Component 패턴

> 핵심은 Context API

`Headless` 컴포넌트를 만들자. 즉, 관심사(UI와 데이터)를 분리하자.

---

**간단한 흐름**

1. 데이터에 해당하는 부분을 Context API로 정의한다. ⇒ 컴포넌트 내부에서 공유될 상태이다.
2. 부모 컴포넌트를 정의한다. ⇒ Context API를 공유한다.
3. 자식 컴포넌트를 정의한다. ⇒ Context API를 통해 필요한 상태를 공유받는다.
4. (선택) 부모 컴포넌트의 프로퍼티로 자식 컴포넌트를 등록한다.

---

`Context API`로 외부에서는 자식 컴포넌트가 부모 컴포넌트와 데이터를 공유하고 있는지 모른다.

= 암묵적으로 자식 컴포넌트에서 부모 컴포넌트에 접근한다.

⇒ 사용할 때 하위 컴포넌트를 자유롭게 볼 수 있고 위치나 마크업을 수정할 수 있어서 좋다.

---

## 컴포넌트 분리하기

- 예시 코드

  ```jsx
  import { useState } from "react";

  const SignUp = () => {
    const [form, setForm] = useState({
      name: "",
      email: "",
      password: "",
      confirmPassword: "",
      agreeTerms: false,
    });
    const [error, setError] = useState("");

    const handleChange = (e) => {
      const { name, value, type, checked } = e.target;
      setForm((prev) => ({
        ...prev,
        [name]: type === "checkbox" ? checked : value,
      }));
    };

    const handleSubmit = (e) => {
      e.preventDefault();
      if (form.password !== form.confirmPassword) {
        setError("Passwords do not match");
        return;
      }
      if (!form.agreeTerms) {
        setError("You must agree to the terms and conditions");
        return;
      }
      console.log("Form Submitted:", form);
      setError("");
    };

    return (
      <div style={{ maxWidth: 400, margin: "auto" }}>
        <h2>Sign Up</h2>
        {error && <p style={{ color: "red" }}>{error}</p>}
        <form onSubmit={handleSubmit}>
          <div>
            <label>Name:</label>
            <input
              type="text"
              name="name"
              value={form.name}
              onChange={handleChange}
            />
          </div>
          <div>
            <label>Email:</label>
            <input
              type="email"
              name="email"
              value={form.email}
              onChange={handleChange}
            />
          </div>
          <div>
            <label>Password:</label>
            <input
              type="password"
              name="password"
              value={form.password}
              onChange={handleChange}
            />
          </div>
          <div>
            <label>Confirm Password:</label>
            <input
              type="password"
              name="confirmPassword"
              value={form.confirmPassword}
              onChange={handleChange}
            />
          </div>
          <div>
            <label>
              <input
                type="checkbox"
                name="agreeTerms"
                checked={form.agreeTerms}
                onChange={handleChange}
              />{" "}
              I agree to the terms
            </label>
          </div>
          <button type="submit">Sign Up</button>
        </form>
      </div>
    );
  };

  export default SignUp;
  ```

---

### 1. Compound Component 패턴 적용

compound component를 통해 Input을 분리했다.

- 관련 코드

  ```jsx
  import { useContext } from "react";
  import { createContext } from "react";

  const InputContext = createContext({
    name: "",
    value: "",
    type: "",
    onChange: () => {},
  });

  const InputWrapper = ({ name, value, type, onChange, children }) => {
    const contextValue = { name, value, type, onChange };

    return (
      <InputContext.Provider value={contextValue}>
        <div>{children}</div>
      </InputContext.Provider>
    );
  };

  const Input = ({ ...props }) => {
    const { name, value, onChange, type } = useContext(InputContext);

    return (
      <input
        name={name}
        value={value}
        type={type}
        onChange={onChange}
        {...props}
      />
    );
  };

  const Label = ({ children, ...props }) => {
    const { name } = useContext(InputContext);

    return (
      <label htmlFor={name} {...props}>
        {children}
      </label>
    );
  };

  InputWrapper.Input = Input;
  InputWrapper.Label = Label;

  export default InputWrapper;
  ```

- 관련 코드
  ```jsx
  <InputWrapper
    name="name"
    type="text"
    value={form.name}
    onChange={handleChange}
  >
    <InputWrapper.Label>Name:</InputWrapper.Label>
    <InputWrapper.Input />
  </InputWrapper>
  ```

---

### 2. check box는 어떻게 할까?

문제는 약관 동의 체크 박스이다.

input 속성 중 value가 없고 checked가 있어서 어떻게 처리할지 고민된다.

⇒ 체크 박스는 또 다른 컴포넌트에서 기본으로 사용할 수 있으므로 새로 만들었다.

1차 리팩토링을 끝냈다.

- 관련 코드

  ```jsx
  import { useState } from "react";
  import InputWrapper from "./InputWrapper";
  import CheckboxWrapper from "./CheckboxWrapper";

  const SignUp = () => {
    const [form, setForm] = useState({
      name: "",
      email: "",
      password: "",
      confirmPassword: "",
      agreeTerms: false,
    });
    const [error, setError] = useState("");

    const handleChange = (e) => {
      const { name, value, type, checked } = e.target;
      setForm((prev) => ({
        ...prev,
        [name]: type === "checkbox" ? checked : value,
      }));
    };

    const handleSubmit = (e) => {
      e.preventDefault();
      if (form.password !== form.confirmPassword) {
        setError("Passwords do not match");
        return;
      }
      if (!form.agreeTerms) {
        setError("You must agree to the terms and conditions");
        return;
      }
      console.log("Form Submitted:", form);
      setError("");
    };

    return (
      <div style={{ maxWidth: 400, margin: "auto" }}>
        <h2>Sign Up</h2>
        {error && <p style={{ color: "red" }}>{error}</p>}
        <form onSubmit={handleSubmit}>
          <InputWrapper
            name="name"
            type="text"
            value={form.name}
            onChange={handleChange}
          >
            <InputWrapper.Label>Name:</InputWrapper.Label>
            <InputWrapper.Input />
          </InputWrapper>

          <InputWrapper
            name="email"
            type="email"
            value={form.email}
            onChange={handleChange}
          >
            <InputWrapper.Label>Email:</InputWrapper.Label>
            <InputWrapper.Input />
          </InputWrapper>

          <InputWrapper
            name="password"
            type="password"
            value={form.password}
            onChange={handleChange}
          >
            <InputWrapper.Label>Password:</InputWrapper.Label>
            <InputWrapper.Input />
          </InputWrapper>

          <InputWrapper
            name="confirmPassword"
            type="password"
            value={form.confirmPassword}
            onChange={handleChange}
          >
            <InputWrapper.Label>Confirm Password:</InputWrapper.Label>
            <InputWrapper.Input />
          </InputWrapper>

          <CheckboxWrapper
            name="agreeTerms"
            type="checkbox"
            checked={form.agreeTerms}
            onChange={handleChange}
          >
            <CheckboxWrapper.Input />
            <CheckboxWrapper.Label>I agree to the terms</CheckboxWrapper.Label>
          </CheckboxWrapper>

          <button type="submit">Sign Up</button>
        </form>
      </div>
    );
  };

  export default SignUp;
  ```

---

### 3. 이게 최선인가?

여기에서 문득 “이게 최선인가?”하는 고민이 들었다.

지금 코드는 처음 코드에서 비교했을때 재사용성이 높아졌는가? 오히려 불편해진 것은 아닌가? 하는 생각이 들었기 때문이다.

그리고 전보다 재사용성이 높아지지 않았다고 생각했다.

그 이유는 코드가 짧기 때문이다. 지금은 한 페이지에서, 하나의 예제만 쓰고 있지만 이 컴포넌트를 재사용하는 일이 많아질 수록 유용할거라 생각한다.

⇒ 그래서 이때 얻은 생각 = 프로젝트의 규모도 영향을 미치는구나!

물론 지금은 짧은 파일 하나이니까 더 짧게 줄일 수 있겠지만, 재사용을 가장 큰 초점으로 두고 시작했고, 실제 프로젝트를 염두하고 했기 때문에 여기까지 수정하기로 결론 내렸다.

---

### 4. 단일 책임 원칙을 따르자

- 기존 코드
  ```jsx
  const handleSubmit = (e) => {
    e.preventDefault();
    if (form.password !== form.confirmPassword) {
      setError("Passwords do not match");
      return;
    }
    if (!form.agreeTerms) {
      setError("You must agree to the terms and conditions");
      return;
    }
    console.log("Form Submitted:", form);
    setError("");
  };
  ```

현재 코드는 폼 오류 검증과 에러 메시지 출력의 2가지를 담당하고 있다.

그래서 폼 오류 검증을 새로운 함수로 분리했다.

- 변경 코드

  ```jsx
  const validateForm = (form) => {
    if (form.password !== form.confirmPassword) {
      return "Passwords do not match";
    }
    if (!form.agreeTerms) {
      return "You must agree to the terms and conditions";
    }
  };

  const handleSubmit = (e) => {
    e.preventDefault();

    const errorMessage = validateForm(form);
    if (errorMessage) {
      setError(errorMessage);
      return;
    }

    console.log("Form Submitted:", form);
    setError("");
  };
  ```

---

### 5. 입력 타입을 구분하자

- 기존 코드
  ```jsx
  const handleChange = (e) => {
    const { name, value, type, checked } = e.target;
    setForm((prev) => ({
      ...prev,
      [name]: type === "checkbox" ? checked : value,
    }));
  };
  ```

현재 이 코드는 input type이 text일 때와 checkbox일 때 2가지 상황을 함께 다루고 있다.

향후 더 많은 input 타입이 추가되면 복잡해질 수 있으므로 분리했다.

- 변경 코드

  ```jsx
  const handleInputChange = (e) => {
    const { name, value } = e.target;
    setForm((prev) => ({
      ...prev,
      [name]: value,
    }));
  };

  const handleCheckboxChange = (e) => {
    const { name, checked } = e.target;
    setForm((prev) => ({
      ...prev,
      [name]: checked,
    }));
  };
  ```

---

[리액트에서 컴포넌트 추상화와 재사용성](https://f-lab.kr/insight/react-component-abstraction-reusability)

[[10분 테코톡] 호프의 프론트엔드에서 컴포넌트](https://www.youtube.com/watch?v=aAs36UeLnTg&t=88s)
