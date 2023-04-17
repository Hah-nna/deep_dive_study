# 1. 타입변환?

개발자에 의해 의도적으로 값으리 타입을 변환하는 것을 **명시적 타입 변환(Explicit coercion)또는 타입 캐스팅(Type casting)**이라 한다.
개발자 의도와 상관없이 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것을 **암묵적 타입변환 (Implicit coercon) 또는 강제 변환(Type coercion)**이라한다.

```js

var x = 10;

//명시적 타입 변환
var str = x.toSting()
console.log(type srt) // string


// 암묵적 타입 변환
// 숫자 타입의 x의 값을 바탕으로 새로운 문자열 타입의 값을 생성해 표현식을 평가한다.
var str2 = x + ''
console.log(typeof str2, str2) // string 10
console.log(x) 10
```

암묵적 타입 변환은 변수 값을 재할당해서 변경하는 것이 아니라 자바스크립트 엔진이 표현식을 에러없이 평가하기 위해 기존 값을 바탕으로 새로운 타입의 값을 만들어 단 한번 사용하고 버린다. 위 예제의 경우, 자바스크립트 엔진은 표현식 x + ‘‘을 평가하기 위해 변수 x의 숫자 값을 바탕으로 새로운 문자열 값 ‘10’을 생성하고 이것으로 표현식 ‘10’ + ‘‘를 평가한다. 이때 자동 생성된 문자열 ‘10’은 표현식의 평가가 끝나면 아무도 참조하지 않으므로 가비지 컬렉터에 의해 메모리에서 제거된다.

중요한 것은 코드를 예측할 수 있어야 한다는 것이다(그렇지 않으면 버그를 생산할 가능성이 높아진다.). 동료가 작성한 코드를 정확히 이해할 수 있어야 하고 자신의 코드는 타인에 의해 쉽게 이해될 수 있어야 한다.(가독성 면에서 암묵적타입이 더 좋을 수도 있기 때문에)

# 2. 암묵적 타입변환

자바스크립트 엔진은 표현식을 평가할 때 문맥 즉 컨텍스트(context)에 고려하여 암묵적 타입 변환을 실행한다.

## 2.1 문자열 타입으로 변환

```js
1 + '2';
console.log(`1 + 1 = ${1 + 1}`); // "1 + 1 = 2"
```

- 연산자는 피연산자 중 하나 이상이 문자열이므로 문자열 연결 연산자로 동작한다. 문자열 연결 연산자의 피연산자는 문맥, 즉 컨텍스트 상 문자열 타입이여야 한다.

# 2.2 숫자 타입으로 변환

산술 연산자의 피연산자는 문맥, 즉 컨텍스트 상 숫자 타입이여야 한다. 피연산자를 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 NaN을 반환

```js
1 - '1'; // 0
1 * '10'; // 10
1 / 'one'; // NaN
'1' > //true
  0 + // true
    // 문자열 타입
    '' + // 0
    '0' + // 0
    '1' + // 1
    'string' + // NaN
    // 불리언 타입
    true + // 1
    false + // 0
    // null 타입
    null + // 0
    // undefined 타입
    undefined + // NaN
    // 심볼 타입
    Symbol() + // TypeError: Cannot convert a Symbol value to a number
    // 객체 타입
    {} + // NaN
    [] + // 0
    [10, 20] + // NaN
    function () {}; // NaN
```

피연산자를 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없으므로 NaN을 반환한다.

# 2.3 불리언 타입으로 변환

if 문이나 for 문과 같은 제어문의 조건식(conditional expression)은 불리언 값, 즉 논리적 참, 거짓을 반환해야 하는 표현식이다.
Truthy 값(참으로 인식할 값) 은 true로, Falsy 값(거짓으로 인식할 값)은 false로 변환된다.

### falsy 값

- false
- undefined
- null
- 0, -0
- NaN
- ’’ (빈문자열)

Falsy 값 이외의 값은 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 컨텍스트에서 모두 true로 평가되는 Truthy 값이다.

```js
// 주어진 인자가 Falsy 값이면 true, Truthy 값이면 false를 반환한다.
function isFalsy(v) {
  return !v;
}

// 주어진 인자가 Truthy 값이면 true, Falsy 값이면 false를 반환한다.
function isTruthy(v) {
  return !!v;
}

// 모두 true를 반환한다.
console.log(isFalsy(false));
console.log(isFalsy(undefined));
console.log(isFalsy(null));
console.log(isFalsy(0));
console.log(isFalsy(NaN));
console.log(isFalsy(''));

console.log(isTruthy(true));
// 빈 문자열이 아닌 문자열은 Truthy 값이다.
console.log(isTruthy('0'));
console.log(isTruthy({}));
console.log(isTruthy([]));
```

# 3. 명시적 타입 변환

## 3.1 문자열 타입으로 변환

### 문자열 타입이 아닌 값을 문자열 타입으로 변환하는 경우

- String 생성자 함수를 new 연산자 없이 호출하는 방법
- Objec.prototype.toString 메소드를 사용하는 방법
- 문자열 연결 연산자를 이용하는 방법

```js
// 숫자 타입 => 문자열 타입
console.log(String(1));
console.log(String(NaN)); // "NaN"
console.log(String(Infinity)); // "Infinity"
// 불리언 타입 => 문자열 타입
console.log(String(true)); // "true"
console.log(String(false)); // "false"

// 2. Object.prototype.toString 메소드를 사용하는 방법
// 숫자 타입 => 문자열 타입
console.log((1).toString()); // "1"
console.log(NaN.toString()); // "NaN"
console.log(Infinity.toString()); // "Infinity"
// 불리언 타입 => 문자열 타입
console.log(true.toString()); // "true"
console.log(false.toString()); // "false"

// 3. 문자열 연결 연산자를 이용하는 방법
// 숫자 타입 => 문자열 타입
console.log(1 + ''); // "1"
console.log(NaN + ''); // "NaN"
console.log(Infinity + ''); // "Infinity"
// 불리언 타입 => 문자열 타입
console.log(true + ''); // "true"
console.log(false + ''); // "false"
```

## 3.2 숫자 타입으로 변환

### 숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법

- Number 생성자 함수를 new 연산자 없이 호출하는 방법
- parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
- 단한 연결 연산자를 이용하는 방법
- 산술 연산자를 이용하는 방법

```js
// 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 숫자 타입
console.log(Number('0')); // 0
console.log(Number('-1')); // -1
console.log(Number('10.53')); // 10.53
// 불리언 타입 => 숫자 타입
console.log(Number(true)); // 1
console.log(Number(false)); // 0

// 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
// 문자열 타입 => 숫자 타입
console.log(parseInt('0')); // 0
console.log(parseInt('-1')); // -1
console.log(parseFloat('10.53')); // 10.53

// 3. + 단항 연결 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
console.log(+'0'); // 0
console.log(+'-1'); // -1
console.log(+'10.53'); // 10.53
// 불리언 타입 => 숫자 타입
console.log(+true); // 1
console.log(+false); // 0

// 4. _ 산술 연산자를 이용하는 방법
// 문자열 타입 => 숫자 타입
console.log('0' _ 1); // 0
console.log('-1' _ 1); // -1
console.log('10.53' _ 1); // 10.53
// 불리언 타입 => 숫자 타입
console.log(true _ 1); // 1
console.log(false _ 1); // 0
```

## 3.3 불리언 타입으로 변환

### 불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법

- Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
- ! 부정 논리 연산자를 두번 사용하는 방법

```js
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 불리언 타입
console.log(Boolean('x')); // true
console.log(Boolean('')); // false
console.log(Boolean('false')); // true
// 숫자 타입 => 불리언 타입
console.log(Boolean(0)); // false
console.log(Boolean(1)); // true
console.log(Boolean(NaN)); // false
console.log(Boolean(Infinity)); // true
// null 타입 => 불리언 타입
console.log(Boolean(null)); // false
// undefined 타입 => 불리언 타 입
console.log(Boolean(undefined)); // false
// 객체 타입 => 불리언 타입
console.log(Boolean({})); // true
console.log(Boolean([])); // true

// 2. ! 부정 논리 연산자를 두번 사용하는 방법
// 문자열 타입 => 불리언 타입
console.log(!!'x'); // true
console.log(!!''); // false
console.log(!!'false'); // true
// 숫자 타입 => 불리언 타입
console.log(!!0); // false
console.log(!!1); // true
console.log(!!NaN); // false
console.log(!!Infinity); // true
// null 타입 => 불리언 타입
console.log(!!null); // false
// undefined 타입 => 불리언 타입
console.log(!!undefined); // false
// 객체 타입 => 불리언 타입
console.log(!!{}); // true
console.log(!![]); // true
```

# 5. 단축 평가

```js
'Cat' && 'Dog';
```

논리 연산자 &&는 두 개의 피연사자가 모두 모두 `true`로 평가될 때 `true`를 반환

> 1. 첫번째 피연산자 ‘Cat’은 Truthy 값이므로 true로 평가된다. 하지만 이 시점까지는 위 표현식을 평가할 수 없다. 두번째 피연산자까지 평가해 보아야 위 표현식을 평가할 수 있다.
> 2. 두번째 피연산자 ‘Dog’은 Truthy 값이므로 true로 평가된다. 이때 두개의 피연산자가 모두 true로 평가되었다. 이때 논리곱 연산의 결과를 결정한 것은 두번째 피연산자 ‘Dog’다.
> 3. 논리곱 연산자 &&는 논리 연산의 결과를 결정한 두번째 피연산자의 평가 결과 즉, 문자열 ‘Dog’를 그대로 반환한다.

```js
'Cat' || 'Dog'; // 'Cat'
```

논리합 연산자 ||는 두개의 피연산자 중 하나만 true로 평가되어도 true를 반환한다. 대부분의 연산자가 그렇듯이 논리합 연산자도 오른쪽에서 왼쪽으로 평가가 진행된다.

> 1.첫번째 피연산자 ‘Cat’은 Truthy 값이므로 true로 평가된다. 이 시점에 두번째 피연산자까지 평가해 보지 않아도 위 표현식을 평가할 수 있다. 2.논리합 연산자 ||는 논리 연산의 결과를 결정한 첫번째 피연산자의 평가 결과 즉, 문자열 ‘Cat’를 그대로 반환한다.

논리곱 연산자 &&와 논리합 연산자 ||는 이와 같이 논리 평가를 결정한 피연산자의 평가 결과를 그대로 반환한다. 이를 단축 평가(Short-Circuit evaluation)라 부른다. 단축 평가는 아래의 규칙을 따른다.

| 단축 평가 표현식    | 평가결과 |
| ------------------- | -------- |
| true \*or anything  | true     |
| false \*or anything | anything |
| true && anything    | anything |
| false && anything   | false    |

*or => ||
```js
// 논리합(||) 연산자
'Cat' || 'Dog'; // 'Cat'
false || 'Dog'; // 'Dog'
'Cat' || false; // 'Cat'

// 논리곱(&&) 연산자
'Cat' && 'Dog'; // Dog
false && 'Dog'; // false
'Cat' && false; // false
```

단축 평가는 아래와 같은 상황에서 유용하게 사용된다.

- 객체가 null 인지 확인하고 프로퍼트를 참조할 때

```js
var elem = null;
console.log(elem.value);
console.log(elem && elem.value); // null
```

객체는 키(key)과 값(value) 구성된 프로퍼티(Property)들의 집합.
만약 객체가 null인 경우, 객체의 프로퍼티를 참조하면 타입 에러(TypeError)가 발생한다. 이때 단축 평가를 사용하면 에러를 발생시키지 않는다.

- 함수의 인수(argument)를 초기화할 때

```js
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || '';
  return str.length;
}

getStringLength(); // 0
getStringLength('hi'); // 2

// ES6의 매개변수의 기본값 설정
function getStringLength(str = '') {
  return str.length;
}

getStringLength(); // 0
getStringLength('hi'); // 2
```

함수를 호출할 때 인수를 전달하지 않으면 매개변수는 undefined를 갖는다. 이때 단축 평가를 사용하여 매개변수의 기본값을 설정하면 undefined로 인해 발생할 수 있는 에러를 방지할 수 있다.
