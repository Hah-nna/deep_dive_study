자바스크립트 객체는 다음 이미지와 같이 크게 세 가지로 나눌 수 있음

<div align="center">
<img src="https://poiemaweb.com/img/objects.png" width="50%" height="50%">
</div>

## 1. 네이티브 객체

네이티브 객체는 ECMAScript 명세에 정의된 객체를 말하며 어플리케이션 전역의 공통 기능을 제공함. 네이티브 객체는 환경과 관계없이 언제나 사용할 수 있음

Object, String, Number, Function, Array, RegExp, Date, Math와 같은 객체 생성에 관계가 있는 함수 객체와 메소드로 구성됨

네이티브 객체를 Global Objects라고 부르기도 하는데 이것은 전역 객체와 다른 의미로 사용되므로 주의해야 함

전역객체는(Global Object)는 모든 객체의 상위 객체를 말하고 일반적으로 브라우저 사이드에서는 window, 서버사이드에서는 global 객체를 의미함

따라서 전역 객체는 네이티브 객체를 포함한 모든 객체를 다루는 것이 가능하지만 네이티브 객체는 자바스크립트 엔진이 기본적으로 제공하는 객체만을 가리킴

### 1.1 Object

Object() 생성자 함수는 객체를 생성함. 만약 생성자 인수 값이 null이거나 undefined면 빈 객체를 반환함

```
// 변수 o에 빈 객체를 저장
var o = new Object();
console.log(typeof o + ': ', o); // object

o = new Object(undefined);
console.log(typeof o + ': ', o); // object

o = new Object(null);
console.log(typeof o + ': ', o); // object
```

그 외의 경우 생성자 함수의 인수값에 따라 강제 형변환된 객체가 반환됨. 이때 반환된 객체의 [[Prototype]] 프로퍼티에 바인딩된 객체는 Object.prototype이 아님

```
// String 객체를 반환
// var obj = new String('String');과 동치
var obj = new Object('String');
console.log(typeof obj + ': ', obj);
console.dir(obj);

var strObj = new String('String');
console.log(typeof strObj + ': ', strObj);

// Number 객체를 반환
// var obj = new Number(123);과 동치
var obj = new Object(123);
console.log(typeof obj + ': ', obj);

var numObj = new Number(123);
console.log(typeof numObj + ': ', numObj);

// Boolean 객체를 반환
// var obj = new Boolean(true);과 동치
var obj = new Object(true);
console.log(typeof obj + ': ', obj);

var boolObj = new Boolean(123);
console.log(typeof boolObj + ': ', boolObj);
```

객체를 생성할 경우 특수한 상황이 아니면 객체리터럴 방식을 사용하는 것이 일반적임

```
var o = {};
```

### 1.2 Function

자바스크립트의 모든 함수는 Function 객체임. 다른 모든 객체들처럼 Function 객체는 new 연산자를 사용해 생성할 수 있음

```
var adder = new Function('a', 'b', 'return a + b');

adder(2, 6);  // 8
```

### 1.3 Boolean

Boolean 객체는 원시타입 boolean을 위한 래퍼 객체임. Boolean 생성자 함수로 Boolean 객체를 생성할 수 있음

```
var foo = new Boolean(true);    // true
var foo = new Boolean('false'); // true

var foo = new Boolean(false); // false
var foo = new Boolean();      // false
var foo = new Boolean('');    // false
var foo = new Boolean(0);     // false
var foo = new Boolean(null);  // false
```

Boolean 객체와 원시타입 boolean을 혼동하기 쉬움. Boolean 객체는 true와 false를 포함하고 있는 객체임

```
var booleanIsMine = true; // boolean 형태의 원시타입 할당

var x = new Boolean(false);
console.log(x) // Boolean {false}
console.log(x.valueOf) // false;

```

### 1.4 Number ~ 1.9 Array는 뒷 챕터에서 계속 정리할 예정임

### 1.10 Error

Error 생성자는 error 객체를 생성함. error 객체의 인스턴스는 런타임 에러가 발생하였을 때 throw됨

```
try {
  // foo();
  throw new Error('Whoops!');
} catch (e) {
  console.log(e.name + ': ' + e.message);
}
```

Error 이외에 Error에 관련한 객체는 아래와 같음

- EvalError
- InternalError
- RangeError
- ReferenceError
- SyntaxError
- TypeError
- URIError

### 1.11 Symbol

symbol은 ECMAScript 6에서 추가된 유일하고 변경 불가능한 원시타입으로 symbol 객체는 원시타입 symbol 값을 생성함

### 1.12 원시 타입과 래퍼 객체(wrapper object)

각 네이티브 객체는 각자의 프로퍼티와 메소드를 가짐. 정적 프로퍼티, 메소드는 해당 인스턴스를 생성하지 않아도 사용할 수 있고 prototype에 속해있는 메소드는 해당 prototype을 상속받은 인스턴스가 있어야만 사용할 수 있음

그런데 원시타입 값에 대해 표준 빌트인 객체의 메소드를 호출하면 정상적으로 작동함

```
var str = 'Hello world!';
var res = str.toUpperCase();
console.log(res); // 'HELLO WORLD!'

var num = 1.5;
console.log(num.toFixed()); // 2
```

원시 타입 값에 대해 표준 빌트인 메소드를 호출할 때 **원시타입 값은 연관된 객체(wrapper 객체)로 일시 변환** 되기 때문에 가능함. 그리고 메소드 호출이 종료되면 객체로 변환된 원시타입 값은 다시 원시 타입 값으로 복귀함

wrapper 객체는 String, Number, Boolean이 있음

## 2. 호스트 객체

호스트 객체는 브라우저 환경에서 제공하는 window, XmlHttpRequest, HTMLElement 등의 DOM 노드 객체와 호스트 환경에 정의된 객체를 말함. 예를 들어 브라우저에서 동작하는 환경과 브라우저 외부에서 동작하는 환경의 자바스크립트(node.js)는 다른 호스트 객체를 사용할 수 있음

브라웆가 동작하는 환경의 호스트 객체는 전역 객체인 window, BOM, DOM 및 XmlHttpRequest 객체 등을 제공함

### 2.1 전역 객체

전역 객체는 모든 객체의 유일한 최상위 객체를 의미함
일반적으로 브라우저 사이드에서는 window, 서버사이드에서는 global 객체를 의미함

### 2.2 BOM(Browser Object Model)

브라우저 객체 모델은 브라우저 탭 또는 브라우저 창의 모델을 생성함.
즉, 브라우저 창, 프레임 등과 같은 브라우저 자체를 제어하는 객체모델임,
최상위 객체는 window 객체로 현재 브라우저 창 또는 탭을 표현하는 객체임. 또한 이 객체의 자식 객체들은 브라우저의 다른 기능들을 표현함. 이 객체들은 standard built-in object가 구성된 이후에 구성됨

<div align="center"> 
<img src="https://poiemaweb.com/img/BOM.png" width="50%" height="50%">
</div>

### 2.3 DOM(Document Object Model)

문서 객체 모델은 현재 웹페이지 모델을 생성함.
즉, HTML, XML 등의 문서의 요소와 속성, 스타일 등을 다루는 객체 모델임
최상위 객체는 document 객체로 전체 문서를 표현함.
또한 이 객체의 다른 자식 객체들은 문서의 다른 요소들을 표현함. 이 객체들은 standard built-in object가 구성된 이후에 구성됨

<div align="center"> 
<img src="https://poiemaweb.com/img/DOM.png" width="50%" height="50%">
</div>
