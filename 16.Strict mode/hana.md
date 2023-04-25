## 1. strict mode란?

```
function foo() {
  x = 10;
}

console.log(x); // ?
```

foo 함수 내에서 선언하지 않은 변수 x에 값 10을 할당함. 이떄 변수 x를 찾아야 x에 값을 할당할 수 있기 때문에 자바스크립트 엔진은 변수x가 어디서 선언되었는지 스코프 체인을 통해 검색하기 시작함

자바스크립트 엔진은 먼저 함수 스코프에서 변수 x를 검색함. foo 함수에는 변수 x의 선언이 없으므로 자바스크립트 엔진은 foo 함수의 상위 스코프에서 변수 x의 선언을 검색함. 위의 예제의 경우 전역스코프가 될 것임

전역 스코프에도 변수x의 선언이 존재하지 않기 때문에 에러가 날 것 같지만 자바스크립트 엔진은 암묵적으로 전역 객체에 프토퍼티 x를 동적으로 생성함. 결국 식별자 x는 전역변수가 됨. 이렇게 전역변수가 된 변수를 **암묵적 변수**라 함

개발자의 의도와는 상관없이 자바스크립트 엔진이 생성한 암묵적 전역 변수는 오류를 발생시키는 원인이 될 가능성이 큼. 따라서 반드시 var. let, const 키워드를 사용하여 변수를 선언한 다음 변수를 사용해야함.

하지만 실수는 언제나 발생하기에 잠재적인 오유를 발생시키기 어려운 개발 환경을 만들고 그 환경에서 개발을 하는 것이 근본적인 해결책임

이를 지원하기 위해 ES5부터 **strict mode**가 추가되었음. strict mode는 자바스크립트 언어의 문법을 보다 엄격히 적용하여 기존에는 무시되던 오류를 발생시킬 가능성이 높거나 자바스크립트 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킴
ESlint와 같은 린트 도구를 사용해도 strict mode와 유사한 효과를 얻을 수 있음
(필자는 strict mode모드보다 린트 도구 사용을 선호한다고 함)

## strict mode의 적용

strict mode를 적용하려면 전역의 선두 또는 함수 몸체 선두에 `"use strict";`를 추가함
전역의 선두에 추가하면 스크립트 전체에 strict mode가 적용됨
하지만 전역에 strict mode를 적용하는 것은 바람직하지 않음

```
// 전역의 맨 처음에 추가해 스크립트 전체에 strict mode적용
// 하지만 바람직하지 않음

'use strict';

function foo() {
  x = 10; // ReferenceError: x is not defined
}
foo();
```

함수 몸체의 선두에 추가하면 해당 함수와 중첩된 내부 함수에 strict mode가 적용됨

```
// 함수 단위로 strict mode 적용

function foo() {
  'use strict';

  x = 10; // ReferenceError: x is not defined
}
foo();
```

코드의 선두에 strict mode를 위치시키지 않으면 제대로 동작하지 않음

```
function foo() {
  x = 10; // 에러를 발생시키지 않음
  'use strict';
}
foo();
```

## 3. 전역에 strict mode 적용 피하기

전역에 적용한 strict mode는 스크립트 단위로 적용됨

```
<!DOCTYPE html>
<html>
<body>
  <script>
    'use strict';
  </script>
  <script>
    x = 1; // 에러가 발생하지 않는다.
    console.log(x); // 1
  </script>
  <script>
    'use strict';

    y = 1; // ReferenceError: y is not defined
    console.log(y);
  </script>
</body>
</html>
```

위 예제와 같이 스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 자신의 스크립트에 한정되어 적용됨

하지만 strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있음. 특히 외부 서드 파티 라이브러리를 사용하는 경우 라이브러리가 non-strict mode일 경우도 있기 때문에 **전역에 strict mode를 적용하는 것은 바람직하지 않음**
이러한 경우 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 적용함

```
// 즉시실행 함수에 strict mode 적용
(function () {
  'use strict';

  // Do something...
}());
```

## 4. 함수 단위로 strict mode를 적용하는 것도 피하기

앞서 말한 바와 같이 함수 단위로 strict mode를 적용할 수도 있음. 하지만 어떤 함수는 strict mode를 적용하고 어떤 함수는 strict mode를 적용하지 않는 것은 바람직하지 않으며, 모든 함수에 일일이 strict mode를 적용하는 것도 번거로운 일임. 그리고 strict mode가 적용된 함수가 참조할 외부의 컨텍스트에 strict mode를 적용하지 않는다면 이 또한 문제가 발생할 수 있음

```
(function () {
  // non-strict mode
  var lеt = 10; // 에러가 발생하지 않는다.

  function foo() {
    'use strict';

    let = 20; // SyntaxError: Unexpected strict mode reserved word
  }
  foo();
}());
```

따라서 strict mode는 즉시실행함수로 감싼 스크립트 단위로 적용하는 것이 바람직함

## 5. strict mode가 발생시키는 에러

### 5.1 암묵적 전역 변수

선언하지 않은 변수를 참조하면 레퍼런스 에러 발생

```
(function () {
  'use strict';

  x = 1;
  console.log(x); // ReferenceError: x is not defined
}());
```

### 5.2 변수, 함수, 매개변수의 삭제

```
(function () {
  'use strict';

  var x = 1;
  delete x;
  // SyntaxError: Delete of an unqualified identifier in strict mode.

  function foo(a) {
    delete a;
    // SyntaxError: Delete of an unqualified identifier in strict mode.
  }
  delete foo;
  // SyntaxError: Delete of an unqualified identifier in strict mode.
}());
```

### 5.3 매개변수 이름의 중복

중복된 함수 파라미터 이름을 사용하면 syntaxError 발생

```
(function () {
  'use strict';

  //SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
}());
```

### 5.4 with 문의 사용

with문을 사용하면 syntaxError 발생함

```
(function () {
  'use strict';

  // SyntaxError: Strict mode code may not include a with statement
  with({ x: 1 }) {
    console.log(x);
  }
}());
```

**with문이란?**

- 객체의 속성에 빠르게 접근하기 위한 문법
- 객체 속성의 이름을 반복해서 작성하지 않아도 됨 -> 코드 간결
- with문은 코드의 가독성을 떨어뜨릴 수 있음
- 객체의 속성이 존재하지 않는 경우 런타임 에러
- 오류를 발생시키기 쉬움 -> 사용을 권장하지 않음

**without문이란?**

- 객체의 속성에 직접 접근하는 방식
- 객체의 속성 이름을 직접 입력 -> 가독성 좋음
- 객체의 속성이 존재하지 않는 경우 undefined 반환

```
const obj = {
  name: 'John',
  age: 30,
  address: {
    city: 'New York',
    street: '123 Main St',
    zipCode: '10001'
  }
};

// with문을 사용하여 obj.address.city에 접근
with (obj.address) {
  console.log(city); // 'New York'
}

// without문을 사용하여 obj.address.city에 접근
console.log(obj.address.city); // 'New York'

```

### 5.5 일반 함수의 this

strict mode에서 함수를 일반 함수로서 호출하면 this 에 undefined가 바인딩됨
생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문임. 이때 에러는 발생하지 않음

```
(function () {
  'use strict';

  function foo() {
    console.log(this); // undefined
  }
  foo();

  function Foo() {
    console.log(this); // Foo
  }
  new Foo();
  // 생성자 함수로 호출하면 this에 새로운 객체 할당
  // new Foo()를 실행 -> this에 Foo라는 새로운 객체 생성
}());
```

## 6. 브라우저 호환성

IE 9 이하는 지원하지 않음
