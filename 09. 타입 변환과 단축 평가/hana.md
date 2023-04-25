# 타입 변환이란?

개발자에 의도적으로 값의 타입을 변환하는 것을 **명시적 타입 변환** 혹은 **타입 캐스팅** 이라 함

```
var x = 2;

// 명시적 타입 변환
var str = x.toString() // 숫자를 문자열로 타입 캐스팅

console.log(typeof str) // string
```

동적 타입 언어인 자바스크립트는 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되기도 함<br>
이를 암묵적 타입 변환 또는 타입 강제 변환이라 함

```
var x = 2

// 암묵적 타입 변환
// 숫자 타입 x의 값을 바탕으로 새로운 문자열 타입의 값을 생성해 표현식을 평가함
var str = x + ''

console.log(typeof str, str); // string 2

// 변수 x의 값이 변경된 것은 아님
console.log(x) // 2
```

명시적 타입 변환도 마찬가지지만 암묵적 타입 변환이 기존 값을 직접 변경하는 것은 아님<br>
변수 값이 원시 타입의 값인 경우 변수 값을 변경하려면 재할당을 통해 새로운 메모리 공간을 확보하고 그 곳에 원시 값을 저장한 후 변수명이 재할당된 원시 값이 저장된 메모리 공간의 주소를 기억하도록 해야함

암묵적 타입 변환은 변수 값을 재할당해서 변경하는 것이 아니라 자바스크립트 엔진이 표현식을 에러없이 평가하기 위해 기존 값을 바탕으로 새로운 타입의 값을 만들어 단 한 번 사용하고 버림. 위의 예제에서 자바스크립트 엔진은 표현식 x + ''를 평가하기 위해 변수 x의 숫자 값을 바탕으로 새로운 문자열 값 '10'을 생성하고 이것으로 표현식 '10'+''을 평가함<br>
이때 자동 생성된 문자열 '10'은 표현식의 평가가 끝나면 아무도 참조하지 않으므로 가바지 컬렉터에 의해 메모리에서 제거됨

명시적 타입 변환은 타입을 변경하겠다는 개발자의 의도가 드러나지만 암묵적 타입 강제변환은 자바스크립트 엔진에 의해서 드러나지 않게 타입이 변경됨

따라서 자신이 작성한 코드에서 암묵적 타입 변환이 일어나는지, 발생한다면 어떤 타입의 어떤 값으로 변환되는지, 타입 변환된 값으로 표현식은 어떻게 평가될 것인지 예측 가능해야함<br>
만약 예측하지 못하거나 예측한 내용이 결과와 일치하지 않는다면 버그를 생산할 가능성이 높아짐

# 암묵적 타입 변환

자바스크립트 엔진은 표현식을 평가할 때 컨텍스트를 고려하여 암묵적 타입 변환을 실행함

```
// 표현식이 모두 문자열 타입이여야 하는 컨텍스트
'10' + 2               // '102'
`1 * 10 = ${ 1 * 10 }` // "1 * 10 = 10"

// 표현식이 모두 숫자 타입이여야 하는 컨텍스트
5 * '10' // 50

// 표현식이 불리언 타입이여야 하는 컨텍스트
!0 // true
if (1) { }
```

자바스크립트는 가급적 에러를 발생시키지 않도록 암묵적 타입 변환을 통해 표현식을 평가함. 암묵적 타입 변환이 발생하면 원시 타입 중 하나로 타입을 자동 변환함함

### 문자열 타입으로 변환

```
1 + '2' // "12"
```

위의 예제에서 `+` 연산자는 피연산자 중 하나 이상이 문자열이므로 문자열 연결 연산자로 동작함. 문자열 연결 연산자의 역할은 문자열 값을 만드는 것임<br>
따라서 문자열 연결 연산자의 피연산자는 컨텍스트 상 문자열 타입이어야 함

자바스크립트 엔진은 문자열 연결 연산자 표현식을 평가하기 위해 문자열 연결.피연산자 중 문자열이 아닌 피연산자를 문자열 타입으로 암묵적 타입 변환함

```
console.log(`1 + 1 = ${1 + 1}`); // "1 + 1 = 2"
```

피연산자만이 암묵적 타입 변환 대상이 되는 것은 아님. 예를 들어 ES6에서 도입된 템플릿 리터럴의 문자열 인터폴레이션은 표현식의 평가 결과를 문자열 타입으로 암묵적 타입 변환함

자바스크립트 엔진은 문자열 타입이 아닌 값을 문자열 타입으로 암묵적 타입 변환을 수행할 때 아래와 같이 동작함

```
// 숫자 타입
0 + ''              // "0"
-0 + ''             // "0", 자바스크립트에서는 -0과 0을 동일하게 간주함.
                    // 자바스크립트에서는 덧셈 연산자 중 문자열이 포함되어 있으면 다른 피연산자를 문자열로 변환함
                    // 그래서 "0"로 변환됨
                    // 만약 "-0"을 문자열로 변환하려면 `String(-0)`게 하면 됨

1 + ''              // "1"
-1 + ''             // "-1"
NaN + ''            // "NaN"
Infinity + ''       // "Infinity"
-Infinity + ''      // "-Infinity"
// 불리언 타입
true + ''           // "true"
false + ''          // "false"
// null 타입
null + ''           // "null"
// undefined 타입
undefined + ''      // "undefined"
// 심볼 타입
(Symbol()) + ''     // TypeError: Cannot convert a Symbol value to a string
// 객체 타입
({}) + ''           // "[object Object]"
Math + ''           // "[object Math]"
[] + ''             // ""
[10, 20] + ''       // "10,20"
(function(){}) + '' // "function(){}"
Array + ''          // "function Array() { [native code] }"
```

### 숫자 타입으로 변환

```
1 - '1'    // 0
1 * '10'   // 10
1 / 'one'  // NaN
```

위의 예시에서 연산자는 모두 산술연산자임. 산술 연산자의 역할은 숫자 값을 만드는 것임. 따라서 산술 연산자의 피연산자는 문맥, 즉 컨텍스트 상 숫자 타입이어야 함

자바스크립트 엔진은 산술 연산자 표현식을 평가하기 위해 산술 연산자의 피연산자중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환함<br>
이때 피연산자를 숫자 타입으로 변환할 수 없는 경우는 산술 연산을 수행할 수 없음 -> NaN을 리턴함

산술 연산자 뿐만 아니라 비교 연산자도 피연산자를 숫자 타입으로 변환함

```
'1' > 0 // true
```

비교 연산자는 불리언 값을 만드는 것임<br>
`>` 비교 연산자는 피연산자의 크기를 비교하므로 피연산자는 컨텍스트 상 숫자 타입이어야 함. 자바스크립트 엔진은 비교 연산자 표현식을 평가하기 위해 비교 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환함

자바스크립트 엔진은 숫자 타입이 아닌 값을 숫자 타입으로 암묵적 타입 변환을 수행할 때 다음과 같이 동작함<br>
`+` 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행함

```
// 문자열 타입
+''             // 0
+'0'            // 0
+'1'            // 1
+'string'       // NaN
// 불리언 타입
+true           // 1
+false          // 0
// null 타입
+null           // 0
// undefined 타입
+undefined      // NaN
// 심볼 타입
+Symbol()       // TypeError: Cannot convert a Symbol value to a number
// 객체 타입
+{}             // NaN
+[]             // 0
+[10, 20]       // NaN
+(function(){}) // NaN
```

빈 문자열, 빈 배열, null, false는 falsy한 값으로 0, true는 1로 변환됨
이 때 객체와 빈 배열이 아닌 배열, undefined는 변환되지 않아 NaN이 되는 것에 주의하자

### 불리언 타입으로 변환

if, for문과 같은 제어문의 조건식은 불리언 값, 즉 논리적 참과 거짓을 리턴해야하는 표현식임. 자바스크립트 엔진은 제어문의 조건식을 평가한 결과를 불리언 타입으로 암묵적 타입 변환함

```
if ('')    console.log('1'); // 빈 문자열은 falsy한 값이라 콘솔x
if (true)  console.log('2');
if (0)     console.log('3'); // 0도 falsy한 값이라 콘솔x
if ('str') console.log('4'); // falsy한 값을 제외하곤 모두 truthy한 값이기 ㅐ문에 콘솔o
if (null)  console.log('5'); // null은 falsy한 값에 속하기 때문에 콘솙x

// 2 4
```

자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 값(참으로 인식할 값) 또는 Falsy 값(거짓으로 인식할 값)으로 구분함. 즉, Truthy 값은 true로, Falsy 값은 false로 변환됨

아래 값들은 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 컨텍스트에서 false로 평가되는 Falsy 값들임

- false
- undefined
- null
- 0, -0
- NaN
- "" (빈문자열)

# 명시적 타입 변환

### 문자열 타입으로 변환

문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법

- String 생성자 함수를 new연산자 없이 호출하는 방법
- Object.prototype.toString 메소드를 사용하는 방법
- 문자열 연결 연산자를 이용하는 방법

```
// 1. String 생성자 함수를 new 연산자 없이 호출하는 방법

// 숫자 타입 -> 문자열 타입
console.log(String(1));        // "1"
console.log(String(NaN));      // "NaN"
console.log(String(Infinity)); // "Infinity"

// 불리언 타입 -> 문자열 타입
console.log(String(true));     // "true"
console.log(String(false));    // "false"

// 2. Object.prototype.toString 메소드를 사용하는 방법

// 숫자 타입 -> 문자열 타입
console.log((1).toString());        // "1"
console.log((NaN).toString());      // "NaN"
console.log((Infinity).toString()); // "Infinity"

// 불리언 타입 -> 문자열 타입
console.log((true).toString());     // "true"
console.log((false).toString());    // "false"

// 3. 문자열 연결 연산자를 이용하는 방법

// 숫자 타입 => 문자열 타입
console.log(1 + '');        // "1"
console.log(NaN + '');      // "NaN"
console.log(Infinity + ''); // "Infinity"

// 불리언 타입 => 문자열 타입
console.log(true + '');     // "true"
console.log(false + '');    // "false"
```

### 숫자 타입으로 변환

숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법

- Number 생성자 함수를 new 연산자 없이 호출하는 방법
- parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
- 단항 연결 연산자를 이용하는 방법
- 산술 연산자를 이용하는 방법

```
// 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법

// 문자열 타입 -> 숫자 타입
console.log(Number('0'));     // 0
console.log(Number('-1'));    // -1
console.log(Number('10.53')); // 10.53

// 불리언 타입 -> 숫자 타입
console.log(Number(true));    // 1
console.log(Number(false));   // 0

// 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)

// 문자열 타입 -> 숫자 타입
console.log(parseInt('0'));       // 0
console.log(parseInt('-1'));      // -1
console.log(parseFloat('10.53')); // 10.53

// 3. + 단항 연결 연산자를 이용하는 방법
// 문자열 타입 -> 숫자 타입
console.log(+'0');     // 0
console.log(+'-1');    // -1
console.log(+'10.53'); // 10.53

// 불리언 타입 => 숫자 타입
console.log(+true);    // 1
console.log(+false);   // 0

// 4. * 산술 연산자를 이용하는 방법
// 문자열 타입 -> 숫자 타입
console.log('0' * 1);     // 0
console.log('-1' * 1);    // -1
console.log('10.53' * 1); // 10.53

// 불리언 타입 -> 숫자 타입
console.log(true * 1);    // 1
console.log(false * 1);   // 0
```

### 불리언 타입으로 변환

불리언 타입이 아닌 불리언 타입으로 변환하는 방법

- boolean 셍성자 함수를 new 연산자 없이 호출하는 방법
- !부정 논리 연산자를 두 번 사용하는 방법

```
// 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
// 문자열 타입 => 불리언 타입
console.log(Boolean('x'));       // true
console.log(Boolean(''));        // false
console.log(Boolean('false'));   // true

// 숫자 타입 => 불리언 타입
console.log(Boolean(0));         // false
console.log(Boolean(1));         // true
console.log(Boolean(NaN));       // false
console.log(Boolean(Infinity));  // true

// null 타입 => 불리언 타입
console.log(Boolean(null));      // false

// undefined 타입 => 불리언 타 입
console.log(Boolean(undefined)); // false

// 객체 타입 => 불리언 타입
console.log(Boolean({}));        // true
console.log(Boolean([]));        // true

// 2. ! 부정 논리 연산자를 두번 사용하는 방법
// 문자열 타입 => 불리언 타입
console.log(!!'x');       // true
console.log(!!'');        // false
console.log(!!'false');   // true

// 숫자 타입 => 불리언 타입
console.log(!!0);         // false
console.log(!!1);         // true
console.log(!!NaN);       // false
console.log(!!Infinity);  // true

// null 타입 => 불리언 타입
console.log(!!null);      // false

// undefined 타입 => 불리언 타입
console.log(!!undefined); // false

// 객체 타입 => 불리언 타입
console.log(!!{});        // true
console.log(!![]);        // true
```

# 단축 평가

논리곱 연산자 `&&`와 논리합 연산자`||` 는 이와 같이 **논리 평가를 결정한 피연산자의 평가 결과를 그대로 반환**<br>
이를 **단축 평가**라 부름

단축 평가는 아래의 규칙을 따름

| 단축 평가 표현식    | 평가 결과 |
| ------------------- | --------- | ---------- | -------- |
| true `              |           | ` anything | true     |
| false `             |           | ` anything | anything |
| true `&&` anything  | anything  |
| false `&&` anything | false     |

```
// 논리합(||) 연산자
'Cat' || 'Dog'  // 'Cat'
false || 'Dog'  // 'Dog'
'Cat' || false  // 'Cat'

// 논리곱(&&) 연산자
'Cat' && 'Dog'  // Dog
false && 'Dog'  // false
'Cat' && false  // false
```

단축 평가는 아래와 같은 상황에서 유용하게 사용됨 (다음 장에서 더 자세하게 다룰 예정)

- 객체가 null인지 확인하고 프로퍼티를 참조할 때

```
var elem = null;

console.log(elem.value); // TypeError: Cannot read property 'value' of null
console.log(elem && elem.value); // null
```

객체는 key와 value로 구성된 프로퍼티들의 집합임. 만약 객체가 null인 경우 객체의 프로퍼티를 참조하면 타입 에러가 발생함<br>
이때 단축 평가를 사용하면 에러를 발생시키지 않음

- 함수의 아규먼트를 초기화할 때

```
// 단축 평가를 사용한 매개변수의 기본값 설정
function getStringLength(str) {
  str = str || '';
  return str.length;
}

getStringLength();     // 0
getStringLength('hi'); // 2

// ES6의 매개변수의 기본값 설정
function getStringLength(str = '') {
  return str.length;
}

getStringLength();     // 0
getStringLength('hi'); // 2
```

함수를 호출할 때 인수를 전달하지 않으면 매개변수는 undefined를 가짐<br>
이때 단축 평가를 사용하여 매개변수의 기본값을 설정하면 undefined로 인해 발생할 수 있는 에러를 방지할 수 있음
