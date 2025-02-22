# 🤔 CSS의 Cascade는 무엇인가요?

> 여러 스타일 속성 값을 결합하는 방법

<br>

## 💬 CSS란?

`CSS`는 **Cascading Style Sheets**의 약자로, HTML이나 XML로 작성된 문서의 스타일 시트 언어이다.

<br>

## 💬 Cascading이란?

‘cascade’를 직역하면 ‘계단식’이라는 뜻이다. 그러니 CSS를 직역하자면 계단식 스타일 언어이고, 스타일도 단계별로 적용된다고 예상해볼 수 있겠다.

실제로 css에서 의미하는 cascade는 다른 소스에서 유래한 한 속성 값을 결합하는 방법을 정의하는 알고리즘이라고 설명한다. 즉, 여러 개의 출처나 레이어, 스코프 블록에서 동일한 요소의 속성을 결정할 때, 우선 순위를 결정한다.

<br>

## 💬 스타일의 출처 3곳

### 1) User-agent stylesheets : 사용자 에이전트 스타일 시트

사용자 에이전트 또는 브라우저에는 모든 문서에 기본 스타일을 제공하는 기본 스타일 시트가 있다. 이 스타일 시트를 사용자 에이전트 스타일 시트라고 부른다.

대부분의 브라우저에서 수정이 불가한데, CSS 재설정 스타일 시트를 통해 모든 브라우저에 스타일을 초기화한 상태로 시작할 수도 있다.

기본적으로 제공되는 사용자 에이전트 스타일 시트는 개발자가 작성한 CSS 파일보다 낮은 우선순위를 갖는다. (이때 예외로, `!important`는 개발자가 작성한 스타일보다 높은 우선 순위를 갖게 된다.)

### 2) Author stylesheets : 작성자 스타일 시트

작성자 스타일 시트는 가장 일반적인 유형의 스타일 시트로, 쉽게 말해 개발자가 작성한 스타일이다. 하나 이상의 스타일 시트에 연결해 사용할 수 있다.

이때 작성자 스타일 시트를 정의 하는 방법은 크게 3가지이다.

1. **외부 스타일 시트 (linked stylesheet)** : link 태그로 외부 CSS 파일과 연결
2. **내부 스타일 시트 (internal stylesheet)** : HTML 문서 내 작성
3. **인라인 스타일 (inline style)** : 각 HTML 태그에 직접 작성

### 3) User stylesheets : 사용자 스타일 시트

사용자 스타일 시트는 웹 사이트의 사용자가 직접 취향에 맞게 개인적으로 적용한 스타일로, 브라우저 확장 프로그램을 사용하거나 브라우저에서 제공하는 설정으로 적용한다. 예를 들어 글자 크기를 키우고 배경 색을 조정할 수 있고, 개인화된 웹 경험을 위해 설정하기 때문에 실제 사이트까지 영향이 가지는 않는다.

<br>

## 💬 Cascade layers

`cascade layer`는 이전에 알아본 3가지의 스타일 출처를 관리하고 우선순위를 지정하는 방법에 대해 구체적이고 구조적으로 처리하는 개념이다. 즉, CSS에서 스타일 충돌이 발생하면, 어떤 스타일이 최종적으로 적용될지 결정하는 계층 구조를 뜻한다.

<br>

## 💬 Cascade 알고리즘의 우선순위 결정 기준

### 1) 관련성

주어진 요소에 적용되는 규칙만 유지하기 위해 다양한 스타일 규칙들 중에서 요소에 맞는 규칙만 필터링한다.

예를 들어, 선택자(selector)가 요소와 일치하는지, 스타일이 적절한 미디어 쿼리 조건(ex. max-width: 600px)에 해당하는지 결정한다.

### 2) Cascading order

적용되는 스타일의 우선순위를 낮은 순위부터 표로 정리해봤다. 최종적으로 브라우저에는 우선순위가 높은 스타일이 반영된다.

| 우선순위 | 종류                         | 중요도         | 설명                                |
| -------- | ---------------------------- | -------------- | ----------------------------------- |
| 8 (low)  | **user-agent (brower)**      | normal         | 브라우저 기본 스타일                |
| 7        | **user**                     | normal         | 사용자가 설정한 스타일              |
| 6        | **author (developer)**       | normal         | 개발자가 작성한 스타일              |
| 5        | **CSS @keyframe animations** |                | 애니메이션 스타일                   |
| 4        | **author (developer)**       | **!important** | 개발자가 작성한 스타일 + !important |
| 3        | **user**                     | **!important** | 사용자가 설정한 스타일 + !important |
| 2        | **user-agent (browser)**     | **!important** | 브라우저 기본 스타일 + !important   |
| 1 (high) | **CSS transitions**          |                | 전환 스타일                         |

### 3) 특이성

만약 충돌한 스타일이 동일한 origin(기원)이라면, 스타일 규칙의 특이성을 비교해서 더 높은 특이성을 가진 규칙이 우선 적용된다.

이때 특이성은 3가지 주요 요소로 결정되는데, 각각 특이성 점수를 가지며 점수가 높을 수록 우선순위가 높다.

1. **ID 선택자** : 100점
2. **클래스 선택자 및 속성 선택자** : 10점
3. **태그 선택자** : 1점

예를 들어, 아래와 같은 코드가 있다.

```html
<p class="text">Hello World</p>
```

```css
p {
  color: red;
}
.text {
  color: blue;
}
```

`p`는 태그 선택자로 특이성 점수가 1점이고, `.text`는 10점이다. 따라서 `“Hello World”`는 파란색 글씨로 표시된다.

### 4) 스코핑 근접성

```html
<div class="container">
  <div class="box">
    <p class="text">Hello World</p>
  </div>
</div>
```

```css
.container .text {
  color: red;
}
.box .text {
  color: blue;
}
```

위의 코드에서 `.container .text`와 `.box .text`는 각각 특이성 점수가 20점으로 동일하다. 이럴때는 스타일이 적용될 대상과 스코프와 가까울수록 우선순위가 높아진다. (가장 가까운 부모 요소가 스코프의 루트가 되어, 그 범위 내에서 스타일이 적용된다.)

그러니까, 예시에서는 `.box와 .container` 중 `.box가 .text`와 더 가까운 스코프를 갖고 있기 때문에 `“Hello World”`는 파란색 글씨로 표시된다.

### 5) 코드 순서

```html
<div class="container">
  <p class="text">Hello World</p>
</div>
```

```css
.container .text {
  color: red;
}
.container .text {
  color: blue;
}
```

만약에 같은 스코프 근접성과 특이성, 우선순위라면, 가장 마지막에 작성된 스타일이 적용된다. 따라서 `“Hello World`”는 파란색으로 표시된다.

<br>

## 💬 인라인 스타일과 레이어

### 1) 인라인 스타일

```html
<p class="text" style="color: blue">Hello World</p>
```

```css
.text {
  color: red;
}
```

스타일을 적용하는 3가지 방법(외부 스타일 시트 / 내부 스타일 시트 / 인라인 시트) 중에, 인라인 스타일의 우선 순위가 가장 높다. 따라서 위의 경우에는 `“Hello World”`가 파란색 글씨로 표현된다.

### 2) 레이어

```css
@import unlayeredStyles.css;
@import AStyles.css layer(A);
@import moreUnlayeredStyles.css;
@import BStyles.css layer(B);
@import CStyles.css layer(C);
```

코드 순서에 따라 우선순위가 적용된다. 따라서 레이어 A, B, C에서 A는 가장 낮은 우선순위를 갖고, C는 가장 높은 우선순위를 갖는다.

그리고 인라인 스타일과 레이어의 개념은 cascade 알고리즘과 복합적으로 적용된다. 쉽게, 레이어마다 casecade 알고리즘이 적용된다고 생각하면 된다.

### 스타일 적용 우선순위

우선, 스타일 적용 우선순위는 cascade 알고리즘과 별개의 개념이다.

순서로 따지면 cascade 알고리즘가 먼저 스타일을 계산하고, 요소에 적용될 스타일을 결정한다. 그 다음에 스타일 적용 우선순위로 최종적으로 브라우저에 스타일이 반영된다.

스타일 적용 우선순위에 포함되는 규칙은 레이어, 인라인 스타일, !important 등이 있고, 표로 정리해보면 다음과 같다.

| 우선순위 | 종류                                 | 중요도         |
| -------- | ------------------------------------ | -------------- |
| 8 (low)  | **user-agent - 첫 번째 선언 레이어** | normal         |
|          | **user-agent - 마지막 선언 레이어**  | normal         |
|          | **user-agent - 무인 스타일**         | normal         |
| 7        | **user - 첫 번째 선언 레이어**       | normal         |
|          | **user - 마지막 선언 레이어**        | normal         |
|          | **user - 무인 스타일**               | normal         |
| 6        | **author - 첫 번째 선언 레이어**     | normal         |
|          | **author - 마지막 선언 레이어**      | normal         |
|          | **author - 무인 스타일**             | normal         |
|          | **inline style**                     | normal         |
| 5        | **animations**                       |                |
| 4        | **author - 무인 스타일**             | **!important** |
|          | **author - 마지막 선언 레이어**      | **!important** |
|          | **author - 첫 번째 선언 레이어**     | **!important** |
|          | **inline style**                     | **!important** |
| 3        | **user - 무인 스타일**               | **!important** |
|          | **user - 마지막 선언 레이어**        | **!important** |
|          | **user - 첫 번째 선언 레이어**       | **!important** |
| 2        | **user-agent - 무인 스타일**         | **!important** |
|          | **user-agent - 마지막 선언 레이어**  | **!important** |
|          | **user-agent - 첫 번째 선언 레이어** | **!important** |
| 1 (high) | **transitions**                      |                |

_이때 좀 헷갈릴 수 있는건, cascade 알고리즘에서는 user style보다 author style의 우선순위가 높은데, 스타일 적용 우선순위에서는 user style의 우선순위가 더 높다는 것이다. 그래서 내가 이해하고 결론 내린건, 애초에 cascade 알고리즘보다 스타일 적용 우선순위의 우선순위가 높기 때문에, author style과 user style이 동시에 적용됐다면, 스타일 적용 우선순위에 따라 user style이 최종적으로 반영된다고 결론냈다._

<br>

## 💬 cascade에 포함되는 속성

CSS cascade에 참여하는 요소는 `property: value;` 형태의 선언들 뿐이다. 이외 일부 규칙은 포함되지 않는다.

- **cascade에 참여하지 않는 경우**
  - `@font-face` : 여러 폰트 규칙 중 가장 적절한 하나만 채택. 충돌시 cascade 알고리즘의 1, 2, 4기준만 사용
  - `@keyframes` : 애니메이션 개별 속성의 영향이 아닌, 전체적으로 @keyframes 규칙이 적용.
  - `@charest` : 문자 인코딩을 저장하는 경우에만 사용
  - `HTML 표현 속성` : 브라우저가 CSS 규칙으로 변환하지만, 특이도가 0이라 가장 쉽게 덮어씌인다.
    ```html
    // HTML 표현 속성 : HTML 속성처럼 사용되는 스타일. HTML5부터는 권장되지
    않는다.
    <p align="center">Hello World</p>
    // text-align: center;와 같다.
    ```
- **cascade에 참여하는 경우**
  - `@media`, `@document`, `@supports` : 이 규칙들에 포함되는 선언은 cascade에 참여하지만, 조건이 맞지 않으면 해당 선택자 자체가 적용되지 않을 수도 있음
  - `@import` : 키워드 자체는 cascade에 참여하지 않지만, 불러온 스타일은 cascade에 적용됨.

<br>

## 참고 URL

https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_cascade/Cascade
