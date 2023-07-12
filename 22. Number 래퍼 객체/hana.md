Number 객체는 원시타입 number를 다룰 때 유용한 프로퍼티와 메소드를 제공하는 wrapper임. 변수 또는 객체의 프로퍼티가 숫자를 값으로 가지고 있다면 Number 객체의 별도 생성 없이 number 객체의 프로퍼티와 메소드를 사용할 수 있음

원시 타입 wrapper 객체의 메소드를 사용할 수 있는 이유는 원시 타입으로 프로퍼티나 메소드를 호출할 때 원시 타입과 관련된 wrapper 객체로 일시적으로 변환되어 프로토타입 객체를 공유하게 되기 때문임.

```
var num = 1.5;
console.log(num.toFixed()); // 2
```

위에서 원시 타입을 담고 있는 변수 num이 Number.prototype.toFixed() 메소드를 호출할 수 있는 것은 변수 num의 값이 일시적으로 wrapper객체로 변환되었기 때문임

## 1. Number Constructor

Number 객체는 Number() 생성자 함수를 통해 생성될 수 있음

```
new Number(value);
```

만약 인자가 숫자로 변환될 수 없다면 NaN을 반환함

```
var x = new Number(123);
var y = new Number('123');
var z = new Number('str');

console.log(x); // 123
console.log(y); // 123
console.log(z); // NaN
```

Number() 생성자 함수를 new 연산자를 붙이지 않아 생성자로 사용하지 않으면 Number 객체를 반환하지 않고 원시 타입 숫자를 반환함. 이때 형 변환히 발생할 수 있음

```
var x = Number('123');

console.log(typeof x, x); // number 123
```

일반적으로 숫자를 사용할 때는 원시 타입 숫자를 사용함

```
var x = 123;
var y = new Number(123);

console.log(x == y);  // true // 123
console.log(x === y); // false // Number {123}

console.log(typeof x); // number
console.log(typeof y); // object
```

## 2. Number Property

static property로 Number 객체를 생성할 필요없이 `Number.prototypeName`의 형태로 사용함

### 2.1 Number.EPSILON(ES6)

Number.EPSILON은 자바스크립트에서 표현할 수 있는 가장 작은 수임. 이는 임의의 수와 그 수보다 큰 수 중 가장 작은 수와의 차이와 같음. Number.EPSILON는 약 2.2204460492503130808472633361816E-16 또는 2<sup>-52</sup>임

부동소수점 산술 연산 비교는 정확한 값을 기대하기 어려움. 정수는 2진법으로 오차없이 저장이 가능하지만 부동소수점을 표현하는 가장 널리 쓰이는 표준인 IEEE754는 2진법으로 변환시 무한소수가 되어 미세한 오차가 발생할 수 밖에 없는 구조적 한계를 가짐.
따라서 이러한 오차를 극복하기 위해 Number.EPSILON를 사용함.

따라서 부동소수점의 비교는 Number.EPSILON을 사용하여 비교 기능을 갖는 함수를 작성해야함

```
console.log(0.1 + 0.2);        // 0.30000000000000004
console.log(0.1 + 0.2 == 0.3); // false!!!

function isEqual(a, b){
  // Math.abs는 절댓값을 반환한함
  // 즉 a와 b의 차이가 JavaScript에서 표현할 수 있는 가장 작은 수인 Number.EPSILON보다 작으면 같은 수로 인정할 수 있음. 즉, Number.EPSILON보다 작은 값은 무시할 수 있는 정도의 미세한 오차로 간주됨
  return Math.abs(a - b) < Number.EPSILON;
}

console.log(isEqual(0.1 + 0.2, 0.3));
```

### 2.2 Number.MAX_VALUE

자바스크립트에서 사용가능한 가장 큰 숫자를 반환함.
MAX_VALUE보다 큰 숫자는 Infinity임

```
Number.MAX_VALUE; // 1.7976931348623157e+308

var num = 10;
num.MAX_VALUE;    // undefined

console.log(Infinity > Number.MAX_VALUE); // true
```

### 2.3 Number.MIN_VALUE

자바스크립트에서 사용 가능한 가장 작은 숫자를 반환함. MIN_VALUE는 0에 가장 가까운 양수 값임. MIN_VALUE보다 작은 숫자는 0으로 변환됨

```
Number.MIN_VALUE; // 5e-324

var num = 10;
num.MIN_VALUE;    // undefined

console.log(Number.MIN_VALUE > 0); // true
```

문득 보다가 Number.MIN_VALUE와 Number.EPSILON 중 무엇이 더 클까?
라는 생각이 듦.

Number.MIN_VALUE는 JavaScript에서 표현 가능한 가장 작은 양수 값임. 즉, 0과 -Number MAX_VALUE 사이의 가장 작은 양수 값을 말함.

반면, Number.EPSILON은 JavaScript에서 1과 1보다 큰 다음 가장 작은 수 사이의 차이임. 이 값은 부동 소수점 연산에서 반올림 오차를 줄이기 위해 사용됨.

따라서, Number.MIN_VALUE와 Number.EPSILON 중에서는 Number.MIN_VALUE가 더 작은 값임

chatGPT가 알려줌 ㅎ...

### 2.4 Number.POSITIVE_INFINITY

양의 무한대 `Infinity`를 반환함

```
Number.POSITIVE_INFINITY // Infinity

var num = 10;
num.POSITIVE_INFINITY;   // undefined
```

### 2.5 Number.NEGATIVE_INFINITY

음의 무한대 `-Infinity`를 반환함

```
Number.NEGATIVE_INFINITY // -Infinity

var num = 10;
num.NEGATIVE_INFINITY;   // undefined
```

### 2.6 Number.NaN

숫자가 나임을 나타내는 숫자값. Number.NaN 프로퍼티는 window.NaN 프로퍼티와 같음

```
console.log(Number('xyz')); // NaN
console.log(1 * 'string');  // NaN
console.log(typeof NaN);    // number
```

## 3. Number.Method

### 3.1 Number.isInfinite(testValue: number) : Boolean

매개변수에 전달된 값이 정상적인 유한수인지를 검사하여 그 결과를 불린으로 반환함

```
/**
 * @param {any} testValue - 검사 대상 값. 암묵적 형변환되지 않는다.
 * @return {boolean}
 */
Number.isFinite(testValue)
```

Number.isFinite()는 전역함수 isFinite()와 차이가 있음. 전역 함수 isFinite()는 인수를 숫자로 변환하여 검사를 수행하지만 Number.isFinite()는 인수를 변환하지 않음. 따라서 숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false가 됨

```
Number.isFinite(Infinity)  // false
Number.isFinite(NaN)       // false
Number.isFinite('Hello')   // false
Number.isFinite('2005/12/12')   // false

Number.isFinite(0)         // true
Number.isFinite(2e64)      // true
Number.isFinite(null)      // false. isFinite(null) => true
```

### 3.2 Number.isInteger(testValue: number) : Boolean

매개변수에 전달된 값이 정수인지 검사하여 그 결과를 불린으로 반환함.
검사 전에 인수를 숫자로 변환하지 않음

```
/**
 * @param {any} testValue - 검사 대상 값. 암묵적 형변환되지 않는다.
 * @return {boolean}
 */
Number.isInteger(testValue)
```

```
Number.isInteger(123)   //true
Number.isInteger(-123)  //true
Number.isInteger(5-2)   //true
Number.isInteger(0)     //true
Number.isInteger(0.5)   //false
Number.isInteger('123') //false
Number.isInteger(false) //false
Number.isInteger(Infinity)  //false
Number.isInteger(-Infinity) //false
Number.isInteger(0 / 0) //false
```

### 3.3 Number.isNaN(testValue: number) : Boolean

매개변수에 전달된 값이 NaN인지를 검사하여 그 결과를 불린으로 반환함

```
/**
 * @param {any} testValue - 검사 대상 값. 암묵적 형변환되지 않는다.
 * @return {boolean}
 */
Number.isNaN(testValue)
```

Number.isNaN()는 전역 함수 isNaN()와 차이가 있음. 전역 함수 isNaN()는 인수를 숫자로 변환하여 검사를 수행하지만 Number.isNaN()는 인수를 변환하지 않음. 따라서 숫자가 아닌 인수가 주어졌을 때 반환값은 언제나 false가 됨.

```

Number.isNaN(NaN)       // true
Number.isNaN(undefined) // false. undefined → NaN. isNaN(undefined) → true.
Number.isNaN({})        // false. {} → NaN.        isNaN({}) → true.
Number.isNaN('blabla')  // false. 'blabla' → NaN.  isNaN('blabla') → true.

Number.isNaN(true)      // false
Number.isNaN(null)      // false
Number.isNaN(37)        // false
Number.isNaN('37');     // false
Number.isNaN('37.37');  // false
Number.isNaN('');       // false
Number.isNaN(' ');      // false
Number.isNaN(new Date())             // false
Number.isNaN(new Date().toString())  // false. String → NaN. isNaN(String) → true.
```

### 3.4 Number.isSafeInteger(testValue: number) : Boolean

매개변수로 전달된 값이 안전한 정수값인지 검사하여 그 결과를 불린으로 반환함. 안전한 정수값은 -(2<sup>53</sup> -1) 와 2<sup>53</sup>-1 사이의 정수값임. 검사 전에 인수를 숫자로 변환하지 않음

```
/**
 * @param {any} testValue - 검사 대상 값. 암묵적 형변환되지 않는다.
 * @return {boolean}
 */
Number.isSafeInteger(testValue)
```

```
Number.isSafeInteger(123)   //true
Number.isSafeInteger(-123)  //true
Number.isSafeInteger(5-2)   //true
Number.isSafeInteger(0)     //true
Number.isSafeInteger(1000000000000000)  // true
Number.isSafeInteger(10000000000000001) // false
Number.isSafeInteger(0.5)   //false
Number.isSafeInteger('123') //false
Number.isSafeInteger(false) //false
Number.isSafeInteger(Infinity)  //false
Number.isSafeInteger(-Infinity) //false
Number.isSafeInteger(0 / 0) //false
```

### 3.5 Number.prototype.toExponential(fractionDigits?: number): string

대상을 지수 표기법으로 변환하여 문자열로 반환함. 지수 표기법이란 매우 큰 숫자를 표기할 때 주로 사용하며 e(Exponent) 앞에 있는 숫자에 10의 n승이 곱하는 형식으로 수를 나타내는 방식임

```
1234 = 1.234e+3
```

```
/**
 * @param {number} [fractionDigits] - 0~20 사이의 정수값으로 소숫점 이하의 자릿수를 나타낸다. 옵션으로 생략 가능하다.
 * @return {string}
 */
numObj.toExponential([fractionDigits])
```

```
var numObj = 77.1234;

console.log(numObj.toExponential());  // logs 7.71234e+1
console.log(numObj.toExponential(4)); // logs 7.7123e+1
console.log(numObj.toExponential(2)); // logs 7.71e+1
console.log(77.1234.toExponential()); // logs 7.71234e+1
console.log(77.toExponential());      // SyntaxError: Invalid or unexpected token
console.log(77 .toExponential());     // logs 7.7e+1
```

정수리터럴과 함께 메소드를 사용할 경우에는 아래와 같이 에러가 발생함

```
77.toString(); // SyntaxError
```

숫자 뒤에 `.`은 의미가 모호함. 소수 구분 기호일 수도 있고 객체 프로퍼티에 접근하기 위한 마침표 표기법일 수도 있음. 따라서 자바스크립트엔진은 숫자 뒤의 `.`을 부동소수점 숫자의 일부로 해석함. 그러나 `77.toString()`은 숫자로 해석할 수 없으므로 에러가 발생함

```
1.23.toString (); // '1.23'
```

위의 예시의 경우 숫자 뒤에 `.`뒤에는 숫자가 이어지므로 `.`은 명백하게 부동소수점 숫자의 일부임. 숫자에 소수점은 하나만 존재하므로 두 번째 `.`은 마침표 표기법으로 해석됨

따라서 정수 리터럴과 함께 메소드를 사용할 경우 아래와 같은 방법을 권장함

```
(77).toString(); // '77'
```

아래의 방법도 허용되기는 함. 자바스크립트 숫자는 정수 부분과 소수 부분사이의 공백을 포함할 수 없음. 따라서 숫자 뒤의 `.` 뒤에 공백이 오면 `.`을 마침표 표기법으로 해석하기 때문임

```
77 .toString(); // '77'
```

### 3.6 Number.prototype.toFixed(fractionDigits?: number): string

매개변수로 지정된 소숫점자리를 반올림하여 문자열로 반환함

```
/**
 * @param {number} [fractionDigits] - 0~20 사이의 정수값으로 소숫점 이하 자릿수를 나타냄. 기본값은 0이며 옵션으로 생략 가능함

 * @return {string}
 */
numObj.toFixed([fractionDigits])
```

```
var numObj = 12345.6789;

// 소숫점 이하 반올림
console.log(numObj.toFixed());   // '12346'

// 소숫점 이하 1자리수 유효, 나머지 반올림
console.log(numObj.toFixed(1));  // '12345.7'

// 소숫점 이하 2자리수 유효, 나머지 반올림
console.log(numObj.toFixed(2));  // '12345.68'

// 소숫점 이하 3자리수 유효, 나머지 반올림
console.log(numObj.toFixed(3));  // '12345.679'

// 소숫점 이하 6자리수 유효, 나머지 반올림
console.log(numObj.toFixed(6));  // '12345.678900'
// 소수점 이하의 자리가 모자랄 경우 0으로 채움
```

### 3.7 Number.prototype.toPrecision(precision?: number): string

매개변수로 지정된 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환함. 지정된 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과를 반환함

```
/**
 * @param {number} [precision] - 1~21 사이의 정수값으로 전체 자릿수를 나타낸다. 옵션으로 생략 가능함

 * @return {string}
 */
numObj.toPrecision([precision])
```

```
var numObj = 15345.6789;

// 전체자리수 유효
console.log(numObj.toPrecision());   // '12345.6789'

// 전체 1자리수 유효, 나머지 반올림
console.log(numObj.toPrecision(1));  // '2e+4'

// 전체 2자리수 유효, 나머지 반올림
console.log(numObj.toPrecision(2));  // '1.5e+4'

// 전체 3자리수 유효, 나머지 반올림
console.log(numObj.toPrecision(3));  // '1.53e+4'

// 전체 6자리수 유효, 나머지 반올림
console.log(numObj.toPrecision(6));  // '12345.7'
```

### 3.8 Number.prototype.toString(radix?: number): string

숫자를 문자열로 변환해 반환함

```
/**
 * @param {number} [radix] - 2~36 사이의 정수값으로 진법을 나타냄. 옵션으로 생략 가능함

 * @return {string}
 */
numObj.toString([radix])
```

```
var count = 10;
console.log(count.toString());   // '10'
console.log((17).toString());    // '17'
console.log(17 .toString());     // '17'
console.log((17.2).toString());  // '17.2'

var x = 16;
console.log(x.toString(2));       // '10000'
console.log(x.toString(8));       // '20'
console.log(x.toString(16));      // '10'

console.log((254).toString(16));  // 'fe'
console.log((-10).toString(2));   // '-1010'
console.log((-0xff).toString(2)); // '-11111111'
```

### 3.9 Number.prototype.valueOf(): number

Number 객체의 원시타입을 반환함

```
var numObj = new Number(10);

console.log(typeof numObj); // object

var num = numObj.valueOf();
console.log(num);           // 10
console.log(typeof num);    // number
```
