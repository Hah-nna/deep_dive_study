함수는 어떤 작업을 수행하기 위해 필요한 문들의 집합을 정의한 코드블록
함수는 이름과 매개변수를 갖으며 필요한 때에 호출하여 코드 블록에 담긴 문들을 일괄적으로 수행할 수 있음

함수는 호출에 의해 실행되는데 한 번만 호출할 수 있는 것이 아니라 여러번 호출할 수 있음

동일한 작업을 반복적으로 수행해야 한다면 미리 정의된 함수를 재사용하는 것이 효율적. 이런 특징은 코드의 재사용이라는 측면에서 유용함

함수의 일반적 기능은 어떤 작업을 수행하는 state들의 집합을 정의하여 코드의 재사용에 목적이 있음. 이런 일반적 기능 이외에 객체 생성, 객체의 행위 정의(메소드), 정보 은닉, 클로저, 모듈화 등의 기능을 수행할 수 있음

자바스크립트 함수는 객체(일급 객체)임. 다른 객체와 구분될 수 있는 특징은 호출할 수 있다는 것임. 함수도 객체이므로 다른 값들처럼 사용할 수 있음. 즉, 변수나 객체, 배열에 저장할 수 있고 다른 함수에 전달되는 인수로도 사용할 수 있으며 함수의 반환값이 될 수도 있음

## 함수 정의

함수를 정의하는 3가지 방식

- 함수 선언문
- 함수 표현식
- Function 생성자 함수

### 함수 선언문

함수 선언문 방식으로 정의한 함수는 Function 키워드와 다음과 같은 내용으로 구성됨

`함수명`
함수의 선언문의 경우, 함수명은 생략할 수 없음. 함수명은 함수 몸체에서 자신을 재귀적 호출하거나 자바스크립트 디버거가 해당 함수를 구분할 수 있는 식별자임

`매개변수 목록`
0개 이상의 목록으로 괄호를 감싸고 콤마로 분리.
다른 언어와의 차이점으로는 매개변수 타입을 기술하지 않는다는 것임(근데 타스를 쓰면 뭐...)

`함수 몸체`
함수가 호출되었을 때 실행되는 문들의 집합. 중괄호로 문들을 감싸고 return문으로 결과값을 반환할 수 있음. 이를 반환값이라 함

```
// 함수 선언문
function square(number) {
  return number * number;
}
```

### 함수 표현식

자바스크립트 함수는 일급 객체이므로 아래와 같은 특징이 있음

- 무명의 리터럴로 표현이 가능함
- 변수나 자료 구조(객체, 배열 등)에 저장할 수 있음
- 함수의 파라미터로 전달할 수 있음
- 반환값으로 사용할 수 있음

함수의 일급객체 특성을 이용하여 함수 리터럴 방식으로 함수를 정의하고 변수에 할당할 수 있는데 이러한 방식을 함수 표현식이라 함
함수 선언문으로 정의한 함수의 예시는 아래와 같음

```
var square = function(number) {
    return number * number;
}
```

함수 표현식으로 정의한 함수는 함수명을 생략할 수 있음. 이러한 함수를 익명 함수라 함. 함수 표현식에서는 함수명을 생략하는 것이 일반적임

```
// 기명 함수 표현식(named function expression)
var foo = function multiply(a, b) {
  return a * b;
};

// 익명 함수 표현식(anonymous function expression)
var bar = function(a, b) {
  return a * b;
};

console.log(foo(10, 5)); // 50
console.log(multiply(10, 5)); // Uncaught ReferenceError: multiply is not defined
```

함수는 일급객체이기 때문에 변수에 할당할 수 있는데 이 변수는 함수명이 아니라 할당된 함수를 가리키는 참조값을 저장하게 됨. 함수 호출시 함수명이 아니라 함수를 가리키는 변수명을 사용하여야 함

```
var foo = function(a, b) {
  return a * b;
};

var bar = foo;

console.log(foo(10, 10)); // 100
console.log(bar(10, 10)); // 100
```

<div align="center">
<img src="https://poiemaweb.com/img/anonymous_function.png" width="50%" height="50%">
</div>

변수 bar와 변수 foo는 동일한 익명 함수의 참조값을 가짐

**<u>함수가 할당된 변수를 사용해 함수를 호출하지 않고 기명 함수의 함수명을 사용해 호출하게 되면 에러가 발생함. 이는 함수 표현식에서 사용한 함수명은 외부 코드에서 접근 불가능하기 때문임</u>**

함수 표현식과 함수 선언문에서 사용한 함수명은 함수 몸체에 자신을 재귀적 호출하거나 자바스크립트 디버거가 해당 함수를 구분할 수 있는 식별자의 역할을 함

함수 선언문으로 정의한 함수 square의 경우 함수명으로 호출할 수 있는데 이는 자바스크립트 엔진에 의해 아래와 같은 함수 표현식으로 형태가 변경되었기 때문임

```
var square = function square(number) {
  return number * number;
};
```

함수명과 함수 참조값을 가진 변수명이 일치하므로 함수명으로 호출되는 듯 보이지만 사실은 변수명으로 호출된 것임

**결국 함수 선언문도 함수 표현식과 동일하게 함수 리터럴 방식으로 정의되는 것임**

### Function 생성자 함수

함수 표현식으로 함수를 정의할 때 함수 리터럴 방식을 사용함
함수 선언문도 내부적으로 자바스크립트 엔진이 기명 함수 표현식으로 변환하므로 결국 함수 리터럴 방식을 사용함

따라서 **함수 선언문과 함수 표현식은 모두 함수 리터럴 방식으로 함수를 정의하는데 이것은 결국 내장 함수 Function 생성자 함수로 함수를 생성하는 것을 단순화 시킨 short-hand임**

Function 생성자 함수는 Function.prototype.constructor 프로퍼티로 접근할 수 있음

Function 생성자 함수로 함수를 생성하는 문법은 아래와 같음

```
new Function(arg1, arg2, ... argN, functionBody)
```

```
var square = new Function('number', 'return number * number');
console.log(square(10)); // 100
```

Function 생성자 함수로 함수를 생성하는 방식은 일반적으로 사용하지 않음

## 함수 호이스팅

위에서 함수 정의방식 3가지를 알아보았음.
정의 방식은 달라도 결국 Function 생성자 함수를 통해 함수를 생성하는 것이었음.
그런데 이 3가지 함수 정의방식은 동작 방식에 약간 차이가 있음

```
var res = square(5);

function square(number) {
  return number * number;
}
```

위의 예시에서 함수 선언문으로 함수가 정의되기 이전에 함수 호출이 가능함. 함수 선언문의 경우 함수 선언위치와는 상관없이 코드 내에 어느 곳이든지 호출이 가능한데 이것을 **함수 호이스팅**이라 함

**자바스크립트는 ES6의 let, const를 포함하여 모든 선언(var, let, const, function, class)를 호이스팅함**

호이스팅이란 var 선언문이나 function 선언문 등 모든 선언문이 해당 스코프의 선두로 옮겨진 것처럼 동작하는 특성을 말함
즉, 자바스크립트는 모든 선언문이 선언되기 이전에 참조 가능함

함수 선언문으로 정의된 함수는 자바스크립트 엔진이 스크립트가 로딩되는 시점에 바로 초기화하고 이를 variable object에 저장함
즉, 함수 선언, 초기화, 할당이 한번에 이루어짐
때문에 함수 선언의 위치와는 상관없이 소스 내 어느 곳에서든지 호출이 가능함

다음은 함수 표현식으로 함수를 정의한 경우임

```
var res = square(5); // TypeError: square is not a function

var square = function(number) {
  return number * number;
}

```

함수 선언문의 경우와는 달리 타입에러가 발생함. 함수 표현식의 경우 함수 호이스팅이 아니라 **변수 호이스팅**이 발생함

> 변수 호이스팅은 변수 생성 및 초기화와 할당이 분리되어 진행된다. 호이스팅된 변수는 undefined로 초기화 되고 실제값의 할당은 할당문에서 이루어진다.

함수 표현식은 함수 선언문과는 달리 스크립트 로딩 시점에 변수 객체(VO)에 함수를 할당하지 않고 runtime에 해석되고 실행되므로 이 두가지를 구분하는 것은 중요함

JavaScript: The Good Parts의 저자이며 자바스크립트의 권위자인 더글러스 크락포드(Douglas Crockford)는 이와 같은 문제 때문에 함수 표현식만을 사용할 것을 권고하고 있음. 함수 호이스팅이 함수 호출 전 반드시 함수를 선언하여야 한다는 규칙을 무시하므로 코드의 구조를 엉성하게 만들 수 있다고 지적함

또한 함수 선언문으로 함수를 정의하면 사용하기에 쉽지만 대규모 애플리케이션을 개발하는 경우 인터프리터가 너무 많은 코드를 변수 객체(VO)에 저장하므로 애플리케이션의 응답속도는 현저히 떨어질 수 있으므로 주의해야 할 필요가 있음

## 일급 객체(First-class object)

일급 객체란 생성, 대입, 연산, 인자 또는 반환값으로서의 전달 등 프로그래밍 언어의 기본적 조작을 제한없이 사용할 수 있는 대상을 의미함

**일급 객체가 되기 위해 만족시켜야하는 조건**

1. 무명의 리터럴로 표현이 가능함
2. 변수나 자료 구조(객체, 배열)에 저장할 수 있음
3. 함수의 매개변수에 전달할 수 있음
4. 반환값으로 사용할 수 있음

`예시`

```
// 1. 무명의 리터럴로 표현이 가능함
// 2. 변수나 자료 구조에 저장할 수 있음

var increase = function (num) {
  return ++num;
};

var decrease = function (num) {
  return --num;
};

var predicates = { increase, decrease };

// 3. 함수의 매개변수에 전달할 수 있음
// 4. 반환값으로 사용할 수 있음
function makeCounter(predicate) {
  var num = 0;

  return function () {
    num = predicate(num);
    return num;
  };
}

var increaser = makeCounter(predicates.increase);
console.log(increaser()); // 1
console.log(increaser()); // 2

var decreaser = makeCounter(predicates.decrease);
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

자바스크립트 함수는 위의 조건을 모두 만족하므로 자바스크립트 함수는 일급객체임. 따라서 자바스크립트 함수는 흡사 변수와 같이 사용할 수 있으며 코드의 어디에서든지 정의 가능함

**함수와 다른 객체를 구분짓는 특징은 호출할 수 있다는 것임**

## 매개변수(parameter, 인자)

함수의 작업 실행을 위해 추가적인 정보가 필요할 경우 배개변수를 지정함.
매개변수는 함수 내에서 변수와 동일하게 동작함

### 매개변수(parameter, 인자) vs 인수(argument)

매개변수는 함수 내에서 변수와 동일하게 메모리 공간을 확보하며 함수에 전달한 인수는 매개변수에 할당됨. 만약 인수를 전달하지 않으면 매개변수는 undefined로 초기화됨

```
var foo = function (p1, p2) {
  console.log(p1, p2);
};

foo(1); // 1 undefined
```

### call-by-value

원시 타입 인수는 Call-by-value(값에 의한 호출)로 동작함. 이는 함수 호출 원시타입 인수를 매개변수로 전달할 때 매개변수에 값을 복사하여 함수로 전달하는 방식임. 이때 함수 내에서 매개 변수를 통해 값이 변경되어도 전달이 완료된 원시 타입 값은 변경되지 않음

```
function foo(primitive) {
  primitive += 1;
  return primitive;
}

var x = 0;

console.log(foo(x)); // 1
console.log(x);      // 0
```

### call-by-reference

객체형(참조형) 인수는 call-by-reference(참조에 의한 호출)로 동작함. 이는 함수 호출 시 참조 타입 인수를 함수에 매개변수로 전달할 때 매개변수에 값이 복사되지 않고 객체의 참조값이 매개변수에 저장되어 함수로 전달되는 방식임.
이때 함수 내에서 매개변수의 참조값을 이용하여 객체의 값을 변경했을 때 전달되어진 참조형의 인수값도 같이 변경됨

```
function changeVal(primitive, obj) {
  primitive += 100;
  obj.name = 'Kim';
  obj.gender = 'female';
}

var num = 100;
var obj = {
  name: 'Lee',
  gender: 'male'
};

console.log(num); // 100
console.log(obj); // Object {name: 'Lee', gender: 'male'}

changeVal(num, obj);

console.log(num); // 100
console.log(obj); // Object {name: 'Kim', gender: 'female'}
```

<div align="center">
<img src="https://poiemaweb.com/img/call-by-val&ref.png" width="50%" height="50%">
</div>

changeVal 함수는 원시 타입과 객체 타입 인수를 전달 받아 함수 몸체에서 매개변수의 값을 변경하였음. 이때 원시 타입 인수는 값을 복사하여 매개변수에 전달하기 때문에 함수 몸체에서 그 값을 변경하여도 어떠한 부수 효과(side-effect)도 발생시키지 않음

하지만 객체형 인수는 참조값을 매개변수에 전달하기 때문에 함수 몸체에서 그 값을 변경할 경우 원본 객체가 변경되는 부수 효과(side-effect)가 발생함.
이와 같이 부수 효과를 발생시키는 비순수 함수(Impure function)는 복잡성을 증가시킴. 비순수 함수를 최대한 줄이는 것은 부수 효과를 최대한 억제하는 것과 같음. 이것은 디버깅을 쉽게 만듦.

어떤 외부 상태도 변경하지 않는 함수를 순수함수(Pure function), 외부 상태도 변경시켜는 부수 효과(side-effect)가 발생시키는 함수를 비순수 함수(Impure function)라 함

## 반환값

함수는 자신을 호출한 코드에게 수행한 결과를 리턴할 수 있음
이 때 반환된 값을 반환값이라 함

- return 키워드는 함수를 호출한 코드에게 값을 리턴할 때 사용
- 함수는 배열 등을 이용하여 한 번에 여러 개의 값을 리턴할 수 있음
- 함수는 리턴을 생략할 수 있음. 이떄 함수는 암묵적으로 undefined를 리턴함
- 자바스크립트 해석기는 return 키워드를 만나면 함수의 실행을 중단한 후, 함수를 호출한 코드로 되돌아감. 만약 return 키워드 이후에 다른 구문이 존재하면 그 구문은 실행되지 않음

```
function calculateArea(width, height) {
  var area = width * height;
  return area; // 단일 값의 반환
}
console.log(calculateArea(3, 5)); // 15
console.log(calculateArea(8, 5)); // 40

function getSize(width, height, depth) {
  var area = width * height;
  var volume = width * height * depth;
  return [area, volume]; // 복수 값의 반환
}

console.log('area is ' + getSize(3, 2, 3)[0]);   // area is 6
console.log('volume is ' + getSize(3, 2, 3)[1]); // volume is 18

```

## 함수 객체의 프로퍼티

함수는 객체임. 따라서 함수도 프로퍼티를 가질 수 있음

```
function square(number) {
  return number * number;
}

square.x = 10;
square.y = 20;

console.log(square.x, square.y);
```

함수는 일반 객체와는 다른 함수만의 프로퍼티를 가짐

```
function square(number) {
  return number * number;
}
console.dir(square);
```

<div align="center">
<img src="https://poiemaweb.com/img/function_property.png" width="50%" height="50%">
</div>

### arguments property

arguments객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회가능(iterable)한 유사 배열 객체이며 함수 내부에서 지역변수처럼 사용됨.
즉, 함수 외부에서는 사용할 수 없음

> arguments property는 현재 일부 브라우저에서 지원하고 있지만 ES3부터 표준에서 deprecated 되었음. Function arguments와 같은 사용방법은 권장되지 않으며 함수 내부에서 지역변수처럼 사용할 수 있는 arguments 객체를 참조하도록 함

자바스크립트는 함수 호출 시 함수 정의에 따라 인수를 전달하지 않아도 에러가 발생하지 암ㅎ음

```
function multiply(x, y) {
  console.log(arguments);
  return x * y;
}

multiply();        // {}
multiply(1);       // { '0': 1 }
multiply(1, 2);    // { '0': 1, '1': 2 }
multiply(1, 2, 3); // { '0': 1, '1': 2, '2': 3 }

```

매개변수(parameter)는 인수(argument)로 초기화됨

- 매개변수의 갯수보다 인수를 적게 전달했을 때 인수가 전달되지 않은 매개변수는 undefined로 초기화됨
- 매개변수의 갯수보다 인수를 더 많이 전달한 경우, 초과된 인수는 무시됨

이런 자바스크립트의 특성 때문에 런타임시에 호출된 함수의 인자 갯수를 확인하고 이에 따라 동작을 달리 정의할 필요가 있을 수 있음
이때 유용하게 사용되는 것이 arguments 객체임

**arguments 객체는 매개변수 갯수가 확정되지 않은 가변인자 함수를 구현할 때 유용하게 사용됨**

```
function sum() {
  var res = 0;

  for (var i = 0; i < arguments.length; i++) {
    res += arguments[i];
  }

  return res;
}

console.log(sum());        // 0
console.log(sum(1, 2));    // 3
console.log(sum(1, 2, 3)); // 6
```

자바스크립트는 함수를 호출할 때 인수들과 함께 암묵적으로 arguments 객체가 함수 내부로 전달됨. arguments 객체는 배열의 형태로 인자값 정보를 담고 있지만 실제 배열이 아닌 유사배열객체임

**유사배열객체란 length 프로퍼티를 가진 객체를 말함** 유사배열객체는 배열이 아니므로 애별 메소드를 사용하는 경우 에러가 발생하게 됨. 따라서 배열 메소드를 사용하려면 `Function.prototype.call, Function.prototype.apply`를 사용하여야 하는 번거로움이 있음

```
function sum() {
  if (!arguments.length) return 0;

  // arguments 객체를 배열로 변환
  var array = Array.prototype.slice.call(arguments);
  return array.reduce(function (pre, cur) {
    return pre + cur;
  });
}

// ES6
// function sum(...args) {
//   if (!args.length) return 0;
//   return args.reduce((pre, cur) => pre + cur);
// }

console.log(sum(1, 2, 3, 4, 5)); // 15
```

### caller property

caller property는 자신을 호출한 함수를 의미함

```
function foo(func) {
  var res = func();
  return res;
}

function bar() {
  return 'caller : ' + bar.caller;
}

console.log(foo(bar)); // caller : function foo(func) {...}
console.log(bar());    // null (browser에서의 실행 결과)

```

### length property

length property는 함수 정의 시 작성된 매개변수 갯수를 의미함

```
function foo() {}
console.log(foo.length); // 0

function bar(x) {
  return x;
}
console.log(bar.length); // 1

function baz(x, y) {
  return x * y;
}
console.log(baz.length); // 2
```

arguments.length의 값과는 다를 수 있으므로 주의하여야함 arguments.length는 함수 호출시 인자의 갯수임

```
function bas(a, b, c) {
  console.lo(bas.length);
  console.log(arguments.length);
}

bas(1,2) // 3,2
bas(1,2,3) // 3,3
bas(1,2,3,4) // 3,4
```

### **proto**접근자 프로퍼티

모든 객체는 `[[Prototype]]`이라는 내부 슬롯이 있음. `[[Prototype]]` 내부 슬롯은 프로토타입 객체를 가리킴. **프로토타입 객체**란 프로토타입 기반 객체 지향 프로그래밍의 근간을 이루는 객체로서 객체간의 상속(Inheritance)을 구현하기 위해 사용됨. 즉, 프로토타입 객체는 **다른 객체에 공유 프로퍼티를 제공하는 객체를 말함**

`__proto__ 프로퍼티`는 `[[Prototype]]` 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티임. 내부 슬롯에는 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한하여 접근할 수 있음. `[[Prototype]]` 내부 슬롯에도 직접 접근할 수 없으며 `__proto__ 접근자 프로퍼티`를 통해 간접적으로 프로토타입 객체에 접근할 수 있음.

```
// __proto__ 접근자 프로퍼티를 통해 자신의 프로토타입 객체에 접근할 수 있음
// 객체 리터럴로 셍성한 객체의 프로토타입 객체는 Object.prototype.
console.log({}.__proto__ === Object.prototype); // true
```

`__proto__` 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 모든 객체의 프로토타입 객체인 Object.prototype 객체의 프로퍼티임. 모든 객체는 상속을 통해 `__proto__` 접근자 프로퍼티를 사용할 수 있음

```
// 객체는 __proto__ 프로퍼티를 소유하지 않음
console.log(Object.getOwnPropertyDescriptor({}, '__proto__'));
// undefined

// __proto__ 프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티임
console.log(Object.getOwnPropertyDescriptor(Object.prototype, '__proto__'));
// {get: ƒ, set: ƒ, enumerable: false, configurable: true}

// 모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있음
console.log({}.__proto__ === Object.prototype); // true
```

함수도 객체이므로 `__proto__` 접근자 프로퍼티를 통해 프로토타입 객체에 접근할 수 있음

```
// 함수 객체의 프로토타입 객체는 Function.prototype임
console.log((function() {}).__proto__ === Function.prototype); // true
```

### prototype property

**prototype property 함수 객체만이 소유하는 프로퍼티**임.
즉, 일반 객체에는 prototype property가 없음

```
// 함수 객체는 prototype 프로퍼티를 소유한다.
console.log(Object.getOwnPropertyDescriptor(function() {}, 'prototype'));
// {value: {…}, writable: true, enumerable: false, configurable: false}

// 일반 객체는 prototype 프로퍼티를 소유하지 않는다.
console.log(Object.getOwnPropertyDescriptor({}, 'prototype'));
// undefined
```

**prototype property는 함수가 객체를 생성하는 생성자 함수로 사용될 때, 생성자 함수가 생성한 인스턴스의 프로토타입 객체를 가리킴**

위의 말이 이해가 안 가서 chatGPT한테 물어봄

JavaScript에서 모든 객체는 [[Prototype]]이라는 내부 슬롯을 가집니다. 이 [[Prototype]]은 해당 객체의 부모 역할을 하는 객체를 가리킵니다. 객체를 생성할 때, 생성자 함수를 사용하면 해당 객체의 [[Prototype]]은 생성자 함수의 prototype 프로퍼티가 가리키는 객체로 설정됩니다.

즉, 생성자 함수 Foo를 사용해 객체 obj를 생성할 때, obj의 [[Prototype]]은 Foo.prototype 객체를 가리키게 됩니다. 이렇게 [[Prototype]] 체인을 따라 부모 객체를 찾아가면서 프로퍼티를 찾는 것이 프로토타입 상속의 원리입니다.

따라서 생성자 함수가 생성한 인스턴스의 [[Prototype]]을 가리키기 위해, 생성자 함수에는 prototype 프로퍼티가 존재합니다. 이 prototype 프로퍼티는 생성자 함수로부터 생성된 인스턴스의 [[Prototype]] 객체를 가리키는 역할을 합니다. 즉, Foo.prototype은 Foo 생성자 함수로부터 생성된 인스턴스의 [[Prototype]]을 가리키는 것입니다.

다음은 생성자 함수 Person과 person1 객체를 생성하는 코드 예시입니다.

javascript
Copy code
function Person(name) {
this.name = name;
}

const person1 = new Person('John');
위 코드에서 Person.prototype은 person1 객체의 [[Prototype]]을 가리키게 됩니다. 따라서 person1 객체는 Person.prototype 객체의 프로퍼티와 메서드를 상속받게 됩니다. 예를 들어, Person.prototype에 greet 메서드를 추가하면 다음과 같이 person1 객체에서도 greet 메서드를 호출할 수 있습니다.

javascript
Copy code
Person.prototype.greet = function() {
console.log('Hello, my name is ' + this.name);
};

person1.greet(); // Hello, my name is John
이렇게 생성자 함수의 prototype 프로퍼티를 사용하면, 생성자 함수로부터 생성된 모든 인스턴스가 공통적으로 상속받을 수 있는 프로퍼티와 메서드를 추가할 수 있습니다.

예시를 보니까 좀 이해가 됨
역시 chatGPT...

## 함수의 다양한 형태

### 즉시 실행 함수

함수의 정의와 동시에 실행되는 함수를 즉시 실행 함수(IIFE)라고 함
최초 한 번만 호출되며 다시 호출할 수는 없음.
이런 특징을 이용하여 최초 한 번만 실행이 필요한 초기화 처리 등에 사용할 수 있음

```
// 기명 즉시 실행 함수(named immediately-invoked function expression)
(function myFunction() {
  var a = 3;
  var b = 5;
  return a * b;
}());

// 익명 즉시 실행 함수(immediately-invoked function expression)
(function () {
  var a = 3;
  var b = 5;
  return a * b;
}());

// SyntaxError: Unexpected token (
// 함수선언문은 자바스크립트 엔진에 의해 함수 몸체를 닫는 중괄호 뒤에 ;가 자동 추가됨
function () {
  // ...
}(); // => };();

// 따라서 즉시 실행 함수는 소괄호로 감싸줌
(function () {
  // ...
}());

(function () {
  // ...
})();
```

자바스크립트에서 가장 큰 문제점 중의 하나는 파일이 분리되어 있다하더라도 글로벌 스코프가 하나이며 글로벌 스코프에 선언된 변수나 함수는 코드 내의 어디서든지 접근이 가능하다는 것임.

따라서 다른 스크립트 파일 내에서 동일한 이름으로 명명된 변수나 함수가 같은 스코프 내에 존재할 경우 원치 않는 결과를 가져올 수 있음

즉시 실행 함수 내에 처리 로직을 모아두면 혹시 있을 수도 있는 변수명 또는 함수명의 충돌을 방지할 수 있어 이를 위한 목적으로 즉시실행함수가 사용되기도 함

특히 jQuery와 같은 라이브러리의 경우, 코드를 즉시 실행 함수 내에 정의해 두면 라이브러리의 변수들이 독립된 영역 내에 있게 되므로 여러 라이브러리들은 동시에 사용하더라도 변수명 충돌과 같은 문제를 방지할 수 있음

```
(function () {
  var foo = 1;
  console.log(foo);
}());

var foo = 100;
console.log(foo);

// 1
// 100
```

### 내부함수

함수 내부에 정의된 함수를 내부함수라 함
아래의 예제에 내부함수 child는 자신을 포함하고 있는 부모함수 parent의 변수에 접근할 수 있음.
하지만 부모함수는 자식함수(내부함수)의 변수에 접근할 수 없음

```
function parent(param) {
  var parentVar = param;
  function child() {
    var childVar = 'lee';
    console.log(parentVar + ' ' + childVar); // Hello lee
  }
  child();
  console.log(parentVar + ' ' + childVar);
  // Uncaught ReferenceError: childVar is not defined
}
parent('Hello');
```

또한 내부함수는 부모함수의 외부에서 접근할 수 없음

```
function sayHello(name){
  var text = 'Hello ' + name;
  var logHello = function(){ console.log(text); }
  logHello();
}

sayHello('lee');  // Hello lee
logHello('lee');  // logHello is not defined
```

### 재귀 함수

재귀함수는 자기 자신을 호출하는 함수를 말함

```
// 피보나치 수열
// 피보나치 수는 0과 1로 시작하며, 다음 피보나치 수는 바로 앞의 두 피보나치 수의 합이 됨
// 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, 55, 89, 144, 233, 377, 610, ...
function fibonacci(n) {
  if (n < 2) return n;
  return fibonacci(n - 1) + fibonacci(n - 2);
}

console.log(fibonacci(0)); // 0
console.log(fibonacci(1)); // 1
console.log(fibonacci(2)); // 1
console.log(fibonacci(3)); // 2
console.log(fibonacci(4)); // 3
console.log(fibonacci(5)); // 5
console.log(fibonacci(6)); // 8

// 팩토리얼
// 팩토리얼(계승)은 1부터 자신까지의 모든 양의 정수의 곱임
// n! = 1 * 2 * ... * (n-1) * n
function factorial(n) {
  if (n < 2) return 1;
  return factorial(n - 1) * n;
}

console.log(factorial(0)); // 1
console.log(factorial(1)); // 1
console.log(factorial(2)); // 2
console.log(factorial(3)); // 6
console.log(factorial(4)); // 24
console.log(factorial(5)); // 120
console.log(factorial(6)); // 720
```

재귀 함수는 자신을 무한히 연쇄 호출하므로 호출을 멈출 수 있는 탈출 조건을 반드시 만들어야함. 탈출 조건이 없는 경우 함수가 무한 호출되어 stackoverflow 에러가 발생함. 위의 두개의 예제 모두 조건식을 통해 재귀 호출을 중지하고 있음

<div align="center">
<img src="https://poiemaweb.com/img/recursive-call.png" width="50%" height="50%">
</div>

**재귀 함수는 반복 연산을 간단히 구현할 수 있다는 장점이 있지만 무한 반복에 빠질 수 있고, stackoverflow 에러를 발생시킬 수 있으므로 주의해야함**

<div align="center">
<img src="https://poiemaweb.com/img/stackoverflow.png" width="50%" height="50%">
</div>

대부분의 재귀함수는 for나 while문으로 구현이 가능함. 반복문보다 재귀함수를 통해 보다 직관적으로 이해하기 쉬운 구현이 가능한 경우에만 한정적으로 적용하는 것이 바람직함.

### 콜백함수

콜백함수는 함수를 명시적으로 호출하는 방식이 아니라 특정 이벤트가 발생했을 시 시스템에 의해 호출되는 함수를 말함

콜백 함수가 자주 사용되는 대표적인 예는 이벤트 핸들러 처리임

```
<!DOCTYPE html>
<html>
<body>
  <button id="myButton">Click me</button>
  <script>
    var button = document.getElementById('myButton');
    button.addEventListener('click', function() {
      console.log('button clicked!');
    });
  </script>
</body>
</html>
```

자바스크립트 함수는 일급 객체임. 따라서 자바스크립트 함수는 흡사 변수와 같이 사용될 수 있음
콜백 함수는 매개변수를 통해 전달되고 전달받은 함수 내부에서 **어느 특정 시점에 실행됨**

```
setTimeout(function () {
  console.log('1초 후 출력된다.');
}, 1000);
```

위의 setTimeout()의 콜백 함수를 살펴보자. 두번째 매개변수에 전달된 시간이 경과되면 첫번째 매개변수에 전달한 콜백 함수가 호출됨

<div align="center">
<img src="https://poiemaweb.com/img/callback.png" width="50%" height="50%">
</div>

콜백함수는 주로 비동기식 처리모델에 사용됨. 비동기식 처리모델이란 처리가 종료하면 호출될 함수(콜백함수)를 미리 매개변수에 전달하고 처리가 종료되면 콜백함수를 호출하는 것임

콜백함수는 콜백 큐에 들어가 있다가 해당 이벤트가 발생하면 호출됨. 콜백 함수는 클로저이므로 콜백 큐에 단독으로 존재하다가 호출되어도 콜백함수를 전달받은 함수의 변수에 접근할 수 있음

```
function doSomething() {
  var name = 'Lee';

  setTimeout(function () {
    console.log('My name is ' + name);
  }, 100);
}

doSomething(); // My name is Lee
```
