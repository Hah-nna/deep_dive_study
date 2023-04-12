**프로그래밍**은 **변수**를 통해 값을 저장하고 참조하며 **연산자로** 값을 연산, 평가하고 **조건문과 반복문**에 의한 흐름제어로 데이터의 흐름을 제어하고 **함수**로 재사용이 가능한 **구문**의 집합을 만들며 객체, 배열 등의 자료를 구조화하는 것이다.

변수는 값의 위치(주소)를 기억하는 저장소이다.
값의 위치: 값이 위치한 메모리 상의 주소(address)

데이터 타입(값의 종류)에 따라메모리의 크기가 다르기 때문에, 메모리에 값을 저장하기 위해서는 메모리 공간을 확보해야 할 메모리의 크기(byte)를 알아야한다.

1byte(8bit) = 256개(2^8)아스키코드(ASCII)
4byte(32bit) = 4,294,967,296ro(2^32) 아스키코드 = -2,147,483,648 ~ 2,147,483,647 정수

C, Java 등의 C-family 언어는 정적타입(static/strong type) 언어로 변수 선언시 변수에 저장할 값의 종류를 타입 지정(type annotation)해야 한다.

자바스크립트는 동적 타입(dynamic/weak type)언어로 변수의 타입 지정(type annotation)없이 값이 할당되는 과정에서 자동으로 변수의 타입이 결정(타입 추론, type interface)된다.
변수는 고정 타입이 없이 여러 타입의 값을 자유롭게 할당할 수 있다.

# 데이터 타입

데이터 타입(data type): 프로그래밍 언어에서 사용할 수 있는 데이터(숫자, 문자열, 불리언 등)의 종류

- 코드에서 사용되는 모든 데이터는 메모리에 저장하고 참조할 수 있어야 한다.
- 데이터 타입은 데이터를 메모리에 저장할 때 확보해야 하는 메모리 공간의 크기, 할당할 수 있는 유효한 값에 대한 정보, 메모리에 저장되어있는 2진수 데이터를 어떻게 해석 할지에 대한 정보를 컴퓨터와 개발자에게 제공한다.

자바스크립트의 모든 값은 데이터 타입을 갖고 ECMAScript 표준(ES6)은 7개의 데이터 타입을 제공한다.

원시타입(primitive data type)

- `boolean`
- `null`
- `undefined`
- `number`
- `string`
- `symbol`(ES6에서 추가)
  객체타입(object/reference type)
- object

숫자 타입 1과 문자열 타입 '1'은 비슷하게 보이지만 다른 타입의 값이다.
개발자는 명확한 의도를 가지고 타입을 구별하여 값을 만들고 자바스크립트 엔진은 타입을 구별하여 값을 취급한다.

## 원시 타입(Primitive Data Type)

원시 타입의 값은 변경 불가능한 값(immutable value)이며 pass-by-value(값에 의한 전달)이다.

### number

C 혹은 Java의 경우, 정수와 실수를 구분하여 int, long, float, dobule 등과 같은 다양한 숫자 타입이 존재하지만 자바스크립트는 하나의 숫자 타입만 존재한다.

숫자 타입의 값은 배정밀도 64비트 부동소수점 형(double-precision 64-bit floating-point format:-(2^53 -1)와 2^53 -1 사이의 숫자값)을 따른다. 모든 수를 실수를 처리하며 정수만을 표현하기 위한 특별한 데이터 타입(integer type)은 없다.

```javascript
var integer = 10; //정수
var doble = 10.12; //실수
var negative = -20; //음의 정수
var binary = 0b01000001; //2진수
var octal = 0o101; //8진수
var hex = 0x41; //16진수
```

2진수, 8진수, 17진수 리터럴은 메모리에 동일한 배정밀도 64비트 부동소수점 형식의 2진수로 저장된다.
자바스크립트는 2진수, 8진수, 16진수 데이터 타입을 제공하지 않기 때문에 이들 값을 참조하면 모두 10진수로 해석된다.

```javascript
console.log(binary); //66
console.log(octal); //65
console.log(hex); //65

//표기법만 다를뿐 같은 값이다.
console.log(binary === octal); //true
console.log(ocatal === hex); //true
```

자바스크립트의 숫자 타입은 정수만을 위한 타입이 없고 모든 수를 실수를 처리한다.

```javascript
console.log(1 === 1.0); //true

var result = 4/2;
console.log(result); //2
result 3/2;
console.log(result); //1.5
```

추가적으로 3가지 특별한 값들도 표현이 가능하다.

- `infinity`: 양의 무한대
- `-infinity`: 음의 무한대
- `NaN`: 산술 연산 불가(not-a-number)

```javascript
var pInf = 10 / 0; //양의 무한대
console.log(pInf); //Infinity

var nInf = 10 / -0; //음의 무한대
console.log(nInf); //-Infinity

var nan = 1 * "string"; // 산술 연산 불가
console.log(nan); //NaN
```

수학적 의미의 실수(real number)는 허수(imaginary number)가 아닌 유리수, 무리수를 말하지만, 프로그래밍 언어에서 실수는 일반적으로 소수를 말한다.

### string

문자열(string) 타입은 텍트스 데이터를 나타낸다.
문자열은 0개 이상의 16bit 유티코드 문자(UTF-16)들의 집합으로 대부분의 전세계의 문자를 표현할 수 있다.
문자열은 작은 따옴표(')와 큰 따옴표(") 안에 텍스트를 넣어 생성한다.

```javascript
var str = "string"; //큰 따옴표
str = "string"; //작은 따옴표
str = `string`; //백틱 (ES6 템플릿 리터럴)

str = "큰 따옴표로 감싼 문자열 내의 '작은따옴표'는 문자열이다.";
str = '작은 따옴표로 감싼 문자열 내의 "큰 따옴표"는 문자열이다';
```

다른 언어와 다르게 자바스크립트의 문자열은 원시 타입이며 변경 불가능(immutable)하다.
문자열이 생성된 후에는 그 문자열을 변경할 수 없다.

```javascript
var str = "hello";
str = "world";
```

- 첫번재 구문 실행 후 메모리에 문자열 "hello" 생성 -> 식별자 str은 메모리에 생성된 문자열 "hello"의 메모리 주소
- 두번째 구문이 실행되면 "hello"를 수정하는 것이 아닌, 새로운 문자열 "world"를 메모리에 생성 -> 식별자 str은 "world"를 가리킴
- 문자열 "hello"와 "world"는 모두 메모리에 존재하지만 변수 str이 참조하는 주소만 변경된다.

```javascript
var str = "string";
//문자열은 유사배열이다.
for (var i = 0; i < str.length; i++) {
  console.log(str[i]);
}
//문자열을 변경할 수 없다.

str[0] = "S";
console.log(str); //string
```

문자열은 배열처럼 인덱스를 통해 접근 가능하며 이런 특성을 갖는 데이터를 **유사 배열** 이라한다.
`str[0] = "S"`처럼 이미 생성된 문자열의 일부를 변경해도 반영되지 않는다.
한번 생성된 문자열은 read only로 변경이 불가능(immutable)하다.

- 새로운 문자열을 재할당하는것은 가능하다.

```javascript
var str = "string";
console.log(str); // string

str = "String";
console.log(str); //String;

str += "test";
console.log(str); //String test

str = str.substring(0, 3);
console.log(str); //Str

str = str.toUpperCase();
cosole.log(str); // STR
```

### boolean

불리언(boolean) 타입의 값은 논리적 참, 거짓을 나타내는 `true` 와 `false` 이다.

```javascript
var foo = ture;
var bar = false;

//typeof 연산자는 타입을 나타내는 문자열을 반환한다.
console.log(typeof foo); //boolean
console.log(typeof bar); //boolean
```

불리언 타입의 값은 참과 거짓으로 구분되는 조건에 의해 프로그램의 흐름을 제어하는 조건문에서 자주 사용된다.

비어있는 문자열과 `null`, `undefined`, 숫자 0은 `false`로 간주된다.

### undefined

undefined의 값은 `undefined`가 유일하다.

- 선언 이후 값을 할당하지 않은 변수는 `undefined` 값을 가진다.
- 선언 되었지만 값을 할당할지 않은 변수에 접근하거나 존재하지 않는 객체 프로퍼티에 접근할 경우 `undefined`가 반환된다.
- 변수 선언에 의해 확보된 메모리 공간을 처음 할당이 이루어질 때까지 빈 상태(대부분 비어있지 않고 쓰레기 값(garbage value)이 들어있다)로 두지 않고 자바스크립트 엔진이 undefined로 초기화 한다,

```javascript
var foo;
console.log(foo); //undefined
```

- 개발자가 의도적으로 할당한 값이 아니라 자바스크립트 엔진에 의해 초기된 값
- 변수를 참조했을 때 undefined가 반환되면, 참조한 변수가 선언 이후에 값이 할당된 적이 없는 변수라는 것을 개발자가 간파할 수 있다.
- 변수의 값이 없다는것을 명시하고 싶은 경우에는 `undefined` 대신 `null`을 할당하는 것이 좋다.

### null

null 타입의 값은 `null`이 유일하다.

- 자바스크립트는 대소문자를 구별(case-sensitive)하기 때문에 `null`은 Null, NULL 등과 다르다.
  프로그래밍 언어에서 `null`은 의도적으로 변수에 값이 없다는 것을 명시할 경우 사용된다.
- 변수가 기억하는 메모리 어드레스의 참조 정보를 제거한다.
- 자바스크립트 엔진은 누구도 참조하지 않는 메모리 영역에 대해 카비지 콜렉션을 수행한다.

```javascript
var foo = "Lee";
foo = null; //참조 정보가 제거 됨
```

함수가 호출되었으나 유효한 값을 반환할 수 없는 경우, 명시적으로 null을 반환하기도 한다.
ex) 조건에 부합하는 HTML 요소를 검색해 반환하는 Documnet.querySelector()는 조건에 부합하는 HTML 요소를 검색할 수 없는 경우, null 반환

```javascript
var element = document.querySelector(".myElem");
//HTML 문저에 myElem 클래스를 갖는 요소가 없다면 null 반환
consol.log(element); //null
```

- `typeof` 연산자로 null 값을 연산하면 null이 아닌 object가 나오는데, 자바스크립트의 설계상의 오류이다.

```javascript
var foo = null;
console.log(typeof foo); //object
```

- null 타입을 확인할 때는 typeof를 사용하지 말고 일치연산자(===)을 사용해야 한다.

```javascript
var foo = null;
console.log(typeof foo === null); //false
console.log(foo === null); //true
```

### symbol

심볼(symbol)은 ES6에서 새롭게 추가된 7번째 타입으로 변경이 불가능한 원시 타입의 값이다.
심볼은 이름의 충돌 위험이 없는 유일한 객체의 프로퍼티 키(property key)를 만들기 위해 사용한다.

- 심볼은 symbol 함수를 호출해 생성한다.
- 생성된 신볼 값은 다른 심볼 값과 다른 유일한 심볼 값이다.

```javascript
//심볼 key는 이름의 충돌 위험이 없는 유일한 객체의 프로퍼티 키
var key = Symbol("key");
console.log(typeof key); //symbol

var obj = {};
obj[key] = "value";
console.log(obj[key]); //value
```

## 객체 타입 (object type, reference type)

객체는 데이터와 그 데이터에 관련한 동작(절차, 방법, 기능)을 모두 포함할 수 있는 개념적 존재이다.

- 이름과 값을 가지는 데이터를 의미하는 프로퍼티(property)와 동작을 의미하는 메소드(method)를 포함할 수 있는 독립적 주체
- 자바스크립트는 객체(object) 기반의 스크립트 언어이다.
- 자바스크립트를 이루고 있는 거의 **모든 것**
- 원시 타입(primitives)을 제외한 나머지 값(배열, 함수, 정규표현식)
- pass-by-reference(참조에 의한 절달)방식으로 전달

# 변수

변수(valiable)는 프로그램에서 사용되는 데이터를 일정 기간 동안 기억하고 필요할 때 다시 사용하기 위해 데이터에 고유 이름인 식별자(identifier)를 명시한 것이다.

- 변수에 명시한 고유한 식별자 = 변수명 / 변수로 참조할 수 있는 데이터 = 변수값
- 식별자: 변수명, 함수명, 프로퍼티명, 클래스 명 등등

변수는 `var`, `var`, `const` 키워드를 사용하여 **선언**, 할당 연산자 `=`를 사용해 값을 **할당**, 식별자인 변수명을 사용해 변수에 저장된 값을 **참조**한다.

```javascript
var score; //변수의 선언
score = 80; //값의 할당
score = 90; //값의 재할당
score; //변수의 참조

//변수의 선언과 할당
var average = (50 + 100) / 2;
```

- 변수는 고유한 식별자(변수명)에 의해 구별하여 참조할 수 있다.

* 메모리에 저장된 데이터를 참조하려면 데이터가 저장된 메모리 상의 주소(address)를 알아야 한다.

## 변수의 선언

변하지 않는 상수는 변수 이름을 대문자로 하여 상수임을 암시한다.

- 변수는 한번쓰고 버리는 값이 아닌 일정 기간 유지할 필요가 있는 값에 사용된다.
- 이해하기 쉽게 의미있는 변수 명을 지정해아한다.

변수명은 식별자(identifier)로 불리기도 하며 명명 규칙이 존재한다.

- 반드시 영문자(특수문자 제외), underscore(\_), 또는 달러기호($)로 시작되어야 한다.
- 이어지는 문자에는 순자(0~9)도 사용 가능하다.
- 자바스크립트는 대/소문자를 구별하기 때문에 사용할 수 있는 문자는 "A" ~ "Z"(대문자)와 "a" ~ "z"(소문자)이다.
  변수를 선언할 때는 `var` `var` 키워드를 사용한다. 등호 (=, equl sign)는 변수에 값을 할당하는 할당 연산자 이다.

```javascript
var name; //선언
name = "Lee"; //할당

var age = 30; //선언과 할당

var person = "Lee";
address = "seoul";
price = 200;

var price = 10;
var tax = 1;
var total = price + tax;
```

- 값을 할당하지 않은 변수는 `undefined`로 초기값을 갖고 선언하지 않은 변수에 접근하면 `ReferenceError`가 발생한다.

```javascript
var x;
console.log(x); //undefeind
console.log(y); //ReferenceError
```

## 변수의 중복 선언

var, var 키워드로 선언한 변수는 중복 선언이 가능하다.

```javascript
var x = 1;
console.log(x); //1

//변수의 중복 선언
var x = 100;
console.log(x); //100
```

중복 선언된 변수는 에러없이 이전 변수의 값을 덮어쓰지만 의도치 않게 중복 선언해 변수의 값을 변경하는 부작용이 생길 수 있기 때문에 사용하지 않는 것이 좋다.

## 동적 타이핑 (Dynamic Typing)

자바스크립트는 동적 타입(dynamic/weak type) 언어이다.

- 타입 지정(type annotation)없이 값이 할당되는 과정에서 값의 타입에 의해 자동으로 타입이 결정(type inference) 될 것이라는 뜻이다.
- 같은 변수에 여러 타입의 값 할당이 가능하다.

```javascript
var foo;

console.log(typeof foo); //undefined

foo = null;
console.log(typeof foo); //object

foo = {};
console.log(typeof foo); //object

foo = 3;
console.log(typeof foo); //number

foo = 3.14;
console.log(typeof foo); //number

foo = "Hi";
console.log(typeof foo); //string

foo = true;
console.log(typeof foo); //boolean
```

## 변수 호이스팅(Variable Hoisting)

```javascript
console.log(foo); //1 undefined
var foo = 123;
console.log(foo); //2 123
{
  var foo = 456;
}
console.log(foo); //3 456
```

var 키워드를 사용하여 선언한 변수는 중복 선언이 가능하기 때문에 위의 코드는 문법적으로 문제가 없다.
`Hoisting` 때문에 1에서 변수 foo는 아직 선언되지 않아서 `ReferenceError: foo is not defined`가 발생해야하지만 콘솔에는 undefined가 출력된다.

- 다른 C-familiy와 차별되는 자바스크립트의 특징으로 **모든 선언문은 호이스팅**된다.

호이스팅이란 var 선언문이나 function 선언문 등 모든 선언문이 해당 scope의 선두로 옮겨진 것처럼 동작하는 특성을 말한다.

자바스크립트는 모든 선언문(var, let, const, function,class)이 선언되기 이전에 참조 가능하다.

- 선언 단계(declaration phase)
  - 변수 객체(variable object)에 변수를 등록. (스코프가 참조하는 대상)
- 초기화 단계(initialisation phase)
  - 변수 객체(variable object)에 등록된 변수를 메모리에 할당. (변수가 undefined로 초기화)
- 할당 단계(assignment phase)
  - undefined로 초기화된 변수에 실제값을 할당

var 키워드로 선언된 변수는 선언 단계와 초기화 단계가 한번에 이루어진다.

자바스크립트는 다른 C-family와 달리 블록 레벨 스코프를 가지지 않고 함수 레벨 스코프를 갖는다.

- ECMAScript 6에서 도입된 let, const 키워드를 사용하면 블록 레벨 스코프를 사용할 수 있다.

함수 레벨 스코프(function-level scope):
함수 내에서 선언된 변수는 함수 내에서만 유효하며 함수 외부에서는 참조할 수 없다.

- 함수 내부에서 선언한 변수는 지역변수이고 함수 외부에서 선언한 변수는 모두 전역 변수이다.

블록 레벨 스코프(block-level scope):
코드 블록 내에서 선언된 변수는 코드 블록 내에서만 유효하며 코드 블록 외부에서는 참조할 수 없다.

## var 키워드로 선언된 변수의 문제점

ES5에서 변수를 선언할 수 있는 유일한 방법은 var 키워드를 사용하는 것이다.
하지만 다른 C-family 언어와 차별되는 특징(설계상 오류)으로 심각한 문제를 발생할 수 있다.

1. 함수 레벨 스코프
   - 전역 변수의 남발
   - for loop 초기화식에서 사용한 변수를 for loop 외부 또는 전역에서 참조할 수있다.
2. var 키워드 생략 허용
   - 의도하지 않은 변수의 전역화
3. 중복 선언 허용
   - 의도하지 않은 변수값 변경
4. 변수 호이스팅
   - 변수를 선언하기 전에 참조가 가능하다.

- 대부분의 문제는 전역 변수로 인해 발생하기 때문에 불가피한 상황을 제외하고는 사용을 자제해야한다.
- ES6는 이러한 var의 단점을 보완하기 위해 **let 과 const** 키워드를 도입했다.
