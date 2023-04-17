# 타입 변환이란?

자바스크립트의 모든 값은 타입이 있다.
명시적 타입 변환(explicit coerscion)/ 타입 캐스팅(type casting)

- 값의 타입은 개발자에 의해 의도적으로 변환이 가능하다.
- 자바스크립트 엔진에 의해 암묵적으로 자동 변환이 가능하다.

```javascript
var x = 10;

//명시적 타입 변환
var str = x.toString(); // 숫자를 문자열로 타입 캐스팅한다.
console.log(typeof str); // string
```

암묵적 타입 변환(implicit coercion) 타입 강제 변환(type coercion)

- 자바스크립트는 동적 타입 언어이지만 자바스크맅브 엔진에 의해 암무적으로 타입이 자동 변환되기도 한다.

```javascript
var x = 10;

//암묵적 타입 변환
//숫자 타입 x의 값을 바탕으로 새로운 문자열 타입의 값을 생성해 표현식을 평가한다.
var str = x + "";

console.log(typeof str, str); //string 10

//변수 x의 값이 변경된 것은 아니다.
console.log(x); //10
```

명시적 타입 변환과 마찬가지로 암묵적 타입 변환이 기족 값을 직접 변경하는 것은 아니다.

- 변수 값이 원시 타입의 값인 경우, 변수 값을 변경하려면 재할당을 통해 새로운 메모리 공간을 확보하고 그 곳에 원시 값을 저장 후 변수명이 재할당 된 원시 값이 저장된 메모리 공간의 주소를 기억하도록 해야한다.

암묵적 타입 변환은 **변수 값을 재할당 해서 변경하는 것이 아니라** 자바스크립트 엔진이 표현식을 에러없이 평가하기 위해, 기존 값을 바탕을 새로운 타입의 값을 만들어 한번 사용 후 버린다.
예시

- 자바스크립트 엔진은 표현식 x+""을 평가학 ㅣ위해 변수 x의 숫자 값을 바탕으로 새로운 문자열 값 '10'을 생성하고 이것으로 표현식 '10'+ ""를 평가한다.
- 이때 자동 생성된 문자열 "10"은 표현식의 평가가끝나면 아무도 참조하지 않기 때문에 **가비지 컬렉터**에 의해 **메모리에서 제거** 된다.

- 자신이 작성한 코드에서 암묵적 타입 변환이 발생하는지, 어떤타입의 어떤 값으로 변환되는지, 타입 변환된 값으로 표현식은 어떻게 평가될 ㅓㅅ인지 예측 가능해아한다.

- 명시적 타입 변환 보다 암묵적 타입 변환이 가독성 면에서 더 좋을 수 있다. `(10).toString()` -> `10 + ""`

# 암묵적 타입 변환

자바스크립트 엔진은 표현식을 평가할 때 문맥(context)에 고려하여 암묵적 타입 변환을 실행한다.

```javascript
//표현식이 모두 문자열 타입이어야 하는 컨텍스트
"10" +
  2 // '102'
  `1*10 = ${1 * 10}`; //"1*10 = 10"

//표현식이 모두 숫자 타입이여야하는 컨텍스트
5 * "10"; //50

//표현식이 불리언 타입이여야 하는 컨텍스트
!0; //true
if (1) {
}
```

표현식을 평가할 떄 켄텍스트에 부합하지 않는 상황이 발생하지만, 자바스크립트는 가급적 에러를 발생시키지 않도로 암묵적 타입 변환을 통해 표현식을 평가한다.

## 문자열 타입으로 변환

```javascript
1 + "2"; //"12"
```

예제의 `+` 연산자는 피연사 중 하나 이상이 문자열이기 때문에 문자열 연결 연산자로 동작한다.

- 자바스크립트 엔진은 표현식을 평가할 때 문맥/컨텍스트에 부합하도록 암묵적 타입 변환을 실행한다.
- ES6에서 도입된 템플릿 리터럴의 문자열 인터폴레이션(string interpolation)의 표현식의 평가 결과를 문자열 타입으로 암묵적 타입 변환한다.

```javascript
console.log(`1+1 = ${1 + 1}`); //"1+1 =2 "
```

```
//숫자 타입
0+"" //"0"
-0+""//"0"
1+"" //"1"
-1+"" //"-1"
NaN + "" //"NaN"
Infinity +"" //"Infinity"
-Infinity + "" // "-infinity"

//불리언 타입
true +"" //"true"
false +"" //"false"

//null 타입
 null +"" //"null"

//undefined 타입
undefined + "" //"undefined"

//심볼 타입
(Symbol())+"" // TypeError:Cannot convert a Symbol value to a string

//객체 타입
({}) + "" //"[object Object]"
Math + "" //"[object Math]:
[]+ "" //""
[10,20]+"" //"10,20"
(function(){}) //function(){}
Array +"" //"function Array(){[native code]}"
```

## 숫자 타입으로 변환

``javascript
1-"1" //0
1\*"0" //10
1/"one" //NaN

````
* 산술 연산자의 역할은 숫자 값을 만드는 것이다.
* 산술 연산자의 피연산자는 문맥/컨텍스트 상 숫자 타입이어야 한다.
* 피연산자를 숫자 타입으로 변환할 수 없는 경우 NaN으 ㄹ반환한다.

```javascript
'1' > 0 //true
````

비교 연산자의 역할은 불리언 값을 만드는 것이다 `>` 비교 연산자는 피연산자의 크기를 비교하므로 컨텍스트 상 숫자 타입이어야한다.

- 자바스크립트 엔진은 비교 연산자 표현식 평가를 위해 숫자 타입으로 암묵적 타입 변환을 한다.

`+` 단항 연산자는 피연산자가 숫잡 타입의 값이 아니면 암묵적으로 타입 변환을 수행한다.

```javascript
//문자열 타입
+"" //0
+'0' //0
+'1' //1
+'string //NaN

//불리언 타입
+true //1
+false //0

//null 타입
+null //0

//undefined 타입
+undefined //NaN

//심볼 타입
+Symbol() //TypeError: Cannot conver a Symbol value to a number

//객체 타입
+{} //NaN
+[] //0
+[10,20] //NaN
+(function(){}) //NaN
```

- 빈문자열, 빈 배열, null, false는 0으로, true는 1로 변환된다.
- 객체와 빈 배열이 아닌 배열, undefined는 변환도지 않고 NaN이 된다.

## 불리언 타입으로 변환

```javascript
if(") console.og(x);
```

if문이나 for문과 같은 제어문의 조건식(conditional expression)은 불리언 값, 논리적 참과 거짓을 빈환해야 하는 표현식 이다.

```javascript
if ("") console.log("1");
if (true) console.log("2");
if (0) console.log("3");
if ("str") console.log("4");
if (null) console.log("5");

//2 4
```

- 자바스크립트 엔진은 불리언 타입이 아닌 값을 **Truthy 값(참으로 인식할 값) 또는 Falsy 값(거짓으로 인식할 값)** 으로 구분한다.
- turthy 는 true, falsy 는 false로 반환한다.

false로 평가되는 falsy 값

- false
- undefined
- null
- 0, -0
- NaN
- "(빈문자열)"

Falsy값 이외의 값은 true로 평가되는 truthy값이다.

```javascript
//주어진 인자가 falsy 값이면 true, turthy 값이면 false를 반환한다.
function isFalsy(v) {
  return !v;
}

//주어진 인자가 truthy값이면 true, falsy값이면 false를 반환한다.
function isTruthy(v) {
  return !!v;
}

//모두 true를 반환한다.
console.log(isFalsy(false));
console.log(isFalsy(undefined));
console.log(isFalsy(null));
console.log(isFalsy(0));
console.log(isFalsy(NaN));
console.log(isFalsy(""));

console.log(isTruthy(true));
// 빈 문자열이 아닌 문자열은 Truthy 값이다.
console.log(isTruthy("0"));
console.log(isTruthy({}));
console.log(isTruthy([]));
```

# 명시적 타입 변환

개발자에 의도에 의해 명시적으로 타입 변경을 하는 방법은 다양한다.

- 래퍼 객체 생상자 함수를 new 연산자 없이 호출
- 자바스크립트에서 저공하는 빌트인 메소드 사용
- 암묵적 타입 변환

## 문자열 타입으로 변환

문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법

1. String 생성자 함수를 new 연산자 없이 호출
2. Object.prototype.toString 메소드 사용
3. 문자열 연결 연산자 이용

```javascript
//1. String 생성자 함수 new 연산자 없이 호출
//숫자 타입 -> 문자열 타입
console.log(String(1)); //"1"
console.log(String(NaN)); //"NaN"
console.log(String(Infinity)); //"Infinity"

//불리언 타입 -> 문자열 타입
console.log(String(true)); //"true"
console.log(String(false))); //"false"

//2. Object.prototype.toString 메소드 사용
//숫자 타입 -> 문자열 타입
console.log((1).toString()); //"1"
console.log((NaN).toString()); //"NaN"
console.log((Infinity).toString()); //"Infinity"

//불리언 타입 -> 문자열 타입
console.log((true).toString()); //"true"
console.log((false).toString()); //"false"

//3. 문자열 연결 연산자 이용
//숫자 타입-> 문자열 타입
console.log(1 + '');        // "1"
console.log(NaN + '');      // "NaN"
console.log(Infinity + ''); // "Infinity"
// 불리언 타입 => 문자열 타입
console.log(true + '');     // "true"
console.log(false + '');    // "false"
```

## 숫자 타입으로 변환

숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법

1. Number 생성자 함수를 new 연산자 없이 호출
2. parseInt, parseFloat 함수 사용( 문자열만 가능)
3. `+`단항 연결 연산자 이용
4. `*`산술 연산자 이용

```javascript
//1. Number 생성자 함수 new 연산자 없이 호출
//문자열 타입 -> 숫자 타입
console.log(Number("0")); // 0
console.log(Number("-1")); // -1
console.log(Number("10.53")); // 10.53

// 불리언 타입 => 숫자 타입
console.log(Number(true)); // 1
console.log(Number(false)); // 0

//2. parseInt, parseFloat 함수 사용(문자열만 가능)
//문자열 타입 => 숫자 타입
console.log(parseInt("0")); // 0
console.log(parseInt("-1")); // -1
console.log(parseFloat("10.53")); // 10.53

//3. + 단항 연결 연산자 이용
console.log(+"0"); // 0
console.log(+"-1"); // -1
console.log(+"10.53"); // 10.53

// 불리언 타입 => 숫자 타입
console.log(+true); // 1
console.log(+false); // 0

//4. * 산술 연산자 이용
//문자열 타입 => 숫자 타입
console.log("0" * 1); // 0
console.log("-1" * 1); // -1
console.log("10.53" * 1); // 10.53

// 불리언 타입 => 숫자 타입
console.log(true * 1); // 1
console.log(false * 1); // 0
```

## 불리언 타입으로 변환

불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법

1. Boolean 생성자 함수를 new 연산자 없이 호출
2. ! 부정 논리 연산자를 두번 사용

```javascript
// 1. Boolean 생성자 함수를 new 연산자 없이 호출
// 문자열 타입 => 불리언 타입
console.log(Boolean("x")); // true
console.log(Boolean("")); // false
console.log(Boolean("false")); // true

// 숫자 타입 => 불리언 타입
console.log(Boolean(0)); // false
console.log(Boolean(1)); // true
console.log(Boolean(NaN)); // false
console.log(Boolean(Infinity)); // true

// null 타입 => 불리언 타입
console.log(Boolean(null)); // false

// undefined 타입 => 불리언 타입
console.log(Boolean(undefined)); // false

// 객체 타입 => 불리언 타입
console.log(Boolean({})); // true
console.log(Boolean([])); // true

// 2. ! 부정 논리 연산자를 두번 사용
// 문자열 타입 => 불리언 타입
console.log(!!"x"); // true
console.log(!!""); // false
console.log(!!"false"); // true

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

# 단축 평가

```javscript
"Cat" && "Dog" //"Dog"
```

논리곱 연산자`&&`는 두개의 피연산자 모두가 `true`로 평가될 때 `true`를 반환한다.

- 오른쪽에서 왼쪽으로 평가가 진행된다.

1. "cat"은 truthy 값으로 `true`로 평가된다.
2. "dog"는 truthy 값으로 `true`로 형가된다. 두개의 피연산자가 모두 true로 평가되고 논리곱 연산자의 결과를 결정한 것은 두번째 피연산자 'Dog"dlek.
3. 논리고 연산자 `&&`는 **논리 연산의 결과를 결정한 두번째 피연산자 문자열"Dog"를 반환한다.**

- 논리합 `||`도 동일하게 동작한다.

```javascript
"Cat" || "Dog"; //"Cat"
```

논리합 연산자 `||`는 두개의 연산자 중 하나만 `true`로 평가 되어도 `true`를 반환한다. \*오른쪽에서 왼쪽으로 평가된다.

1. "Cat"은 truthy 값으로 `true`로 평가된다. **두번째 피연산자까지 평가하지 않아도 표현식을 평가할 수 있다.**
2. 논리합 연산자 `||`는 **논리 연산의 결과를 결정한 첫번째 피연산자 "Cat"을 반환한다.**

단축평가(short-circuit evaluation)은 논리곱 연산자와 논리합 연산자와 같이 **논리 평가를 결정한 피연산자의 평가 결과를 그대로 반환**하는 것을 말한다.
|단축평가 표현식|평가결과|
|---|---|
|true`||`anything|true|
|false`||`anything|anything|
|true&&anything|anything|
|false&&anything|false|

```javascript
//논리합(||) 연산자
"cat" || "dog"; //"cat"
false || "dog"; //"dog"
"cat " || false; //"cat "

//논리곱(&&) 연산자
"cat" && "dog"; //"dog"
false && "dog"; //false
"cat" && false; //false
```

단축 평가가 유용한 경우

- 객체가 null인지 확인하고 프로퍼티를 참조할 때

```javascript
var elem = null;
console.log(elem.value); //TypeError: Cannto read property "value" of null
console.log(elem && elem.value); //null
```

객체는 key와 value로 구성된 프로퍼티의 집합이다.

- 개체가 null인 경우, 객체의 프로퍼티를 참조하면 타입 에러가 발생지만 단축평가를 사용하면 에러를 발생하지 않는다.

- 함수의 인수(argument)를 초기화 할 때

```javascript
//단축 평가를 사용한 매개 변수의 기본값 설정
function getStringLength(str) {
  str = str || "";
  return str.length;
}

getStringLength(); //0
getStringLength("hi"); //2

//ES6의 매개변수의 기본값 설정
function getStringLength(str = "") {
  return str.length;
}

getStringLength(); //0
getStringLenght("hi"); //2
```

- 함수를 호출할 떄 인수를 전달하지 않으면 매개변수는 undefined를 갖는다.
- 단축 평가를 사용하여 매개변수의 기본값을 설정하면 Undefined 로 인해 발생할 수 있는 에러를 방지할 수 있다.

15/April/2023
