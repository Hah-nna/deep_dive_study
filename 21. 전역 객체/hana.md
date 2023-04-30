# 전역 객체

전역 객체는 모든 객체의 유일한 최상위 객체를 말함. 일반적으로 브라우저 사이드에서는 window, 서버사이드에서는 global 객체를 의미함

```
// in browser console
this === window // true

// in Terminal
node
this === global // true
```

- 전역 객체는 실행 컨텍스트에 컨트롤이 들어가기 이전에 생성되며 constructor가 없기 때문에 new 연산자를 이용하여 새롭게 생성할 수 있음. 즉, 개발자가 전역 객체를 생성하는 것은 불가능함
- 전역 객체는 전역 스코프를 갖게 됨
- 전역 객체의 자식 객체를 사용할 때 전역 객체의 기술은 생략할 수 있음. 예를 들어 document 객체는 window의 자식 객체로서 window.document...와 같이 기술할 수 있으나 일반적으로 전역 객체의 기술은 생략함

```
document.getElementById('foo').style.display = 'none';
// window.document.getElementById('foo').style.display = 'none';
```

그러나 사용자가 정의한 변수와 전역 객체의 자식 객체 이름이 충돌하는 경우 명확히 전역 객체를 기술하여 혼동을 방지할 수 있음

```
function moveTo(url) {
  var location = {'href':'move to '};
  alert(location.href + url);
  // location.href = url;
  window.location.href = url;
}
moveTo('http://www.google.com');
```

전역 객체는 전역변수를 프로퍼티로 가지게 됨. 즉 전역 변수는 전역 객체의 프로퍼티임

```
var ga = 'Global variable';
console.log(ga);
console.log(window.ga);
```

글로벌 영역에 선언한 함수도 전역 객체의 프로퍼티로 접근할 수 있음. 즉 전역 함수는 전역 객체의 메소드임

```
function foo() {
  console.log('invoked!');
}
window.foo();
```

표준 빌트인 객체(standard built-in object)도 역시 전역 객체의 자식 객체임. 전역 객체의 자식 객체를 이용할 때 전역 객체의 기술은 생략할 수 있으므로 표준 빌트인 객체도 전역 객체의 기술을 생략할 수 있음

```
// window.alert('Hello world!');
alert('Hello world!');
```

## 1. 전역 프로퍼티

전역 프로퍼티는 전역 객체의 프로퍼티를 의미함. 어플리케이션 전역에서 사용하는 값들을 나타내기 위해 사용함. 전역 프로터피는 간단한 값이 대부분이며 다른 프로퍼티나 메소드를 가지고 있지 않음음

### 1.1 infinity

infinity 프로퍼티는 양/음의 무한대를 나타내는 숫자값 infinity를 가짐

```
console.log(window.Infinity); // Infinity

console.log(3/0);  // Infinity
console.log(-3/0); // -Infinity
console.log(Number.MAX_VALUE * 2); // 1.7976931348623157e+308 * 2
console.log(typeof Infinity); // **number**
```

### 1.2 NaN

NaN 프로퍼티는 숫자가 아님ㅇㄹ 나타내는 숫자값 NaN을 가짐. NaN프로퍼티는 Number.NaN 프로퍼티와 같음

```
console.log(window.NaN); // NaN

console.log(Number('xyz')); // NaN
console.log(1 * 'string');  // NaN
console.log(typeof NaN);    // **number**
```

### 1.3 undefined

undefined의 프로퍼티는 원시타입 undefined를 값으로 가짐

```
console.log(window.undefined); // undefined

var foo;
console.log(foo); // undefined
console.log(typeof undefined); // undefined
```

## 2. 전역 함수

전역 함수는 어플리케이션에서 전역으로 호출할 수 있는 함수로서 전역 객체의 메소드임

### 2.1 eval()

매개변수에 전달된 문자열 구문 또는 표현식을 평가 또는 실행함. 사용자로부터 입력받은 콘텐츠를 eval()로 실행하는 것은 보안에 매우 취약함. eval()은 가급적 사용하지 말자

```
eval(string)
// string: code 또는 표현식을 나타내는 문자열. 표현식은 존재하는 객체들의 프로퍼티들과 변수들을 포함할 수 있음
```

```
var foo = eval('2 + 2');
console.log(foo); // 4

var x = 5;
var y = 4;
console.log(eval('x * y')); // 20
```

### 2.2 isFinite()

매개변수에 전달된 값이 정상적인 유한수인지 검사하여 그 결과는 Boolean으로 반환함. 매개변수에 전달된 값이 숫자가 아닌 경우 숫자로 변환한 후 검사를 수행함

```
isFinite(testValue) // testValue: 검사 대상 값
```

```
console.log(isFinite(Infinity));  // false
console.log(isFinite(NaN));       // false
console.log(isFinite('Hello'));   // false
console.log(isFinite('2005/12/12'));   // false

console.log(isFinite(0));         // true 왜냐하면 0은 유한한 숫자이기 때문에
console.log(isFinite(2e64));      // true 지수 표기법(Exponential notation)을 사용해 큰 수를 나타냄. 이 수도 유한한 숫자이기 때문에 true
console.log(isFinite('10'));      // true: '10' → 10
console.log(isFinite(null));      // true: null → 0
```

isFinite(null)은 true를 반환하는데 이것은 null을 숫자로 변환하여 검사를 수행했기 때문임

```
// null이 숫자로 암묵적 강제 형변환이 일어난 경우
Number(null)  // 0
// null이 불리언로 암묵적 강제 형변환이 일어난 경우
Boolean(null) // false
```

### 2.3 isNaN()

매개변수에 전달된 값이 NaN인지 검사해 그 결과를 Boolean으로 반환함. 매개변수에 전달된 값이 숫자가 아닌 경우 숫자로 변환 후 검사를 수행함

```
isNaN(testValue) // testValue: 검사 대상 값
```

```
isNaN(NaN)       // true
isNaN(undefined) // true: undefined → NaN
isNaN({})        // true: {} → NaN
isNaN('blabla')  // true: 'blabla' → NaN

isNaN(true)      // false: true → 1
isNaN(null)      // false: null → 0
isNaN(37)        // false

// strings
isNaN('37')      // false: '37' → 37
isNaN('37.37')   // false: '37.37' → 37.37
isNaN('')        // false: '' → 0
isNaN(' ')       // false: ' ' → 0

// dates
isNaN(new Date())             // false: new Date() → Number
isNaN(new Date().toString())  // true:  String → NaN
```

### 2.4 parseFloat()

매개변수에 전달된 문자열을 부동소수점 숫자(floating point number)로 변환하여 반환함

```
parseFloat(string)
// string: 변환 대상 문자열
```

```
parseFloat('3.14');     // 3.14
parseFloat('10.00');    // 10
parseFloat('34 45 66'); // 34
parseFloat(' 60 ');     // 60
parseFloat('40 years'); // 40
parseFloat('He was 40') // NaN
```

parseFloat()는 첫 번째 문자부터 시작해 숫자가 아닌 문자가 나올 때까지의 부분 문자열만 변환함

위의 예시 중 parseFloat('40 years')의 경우 문자열의 앞 부분인 `'40'`을 숫자로 변환하고 `'years'` 부분은 무시함. 그래서 40을 반환함
parseFloat('He was 40')의 경우 문자열의 첫 번째 문자인 `'H'`가 숫자가 아니기 때문에 NaN을 반환함.
주의해야할 점은 부분 문자열이 숫자로 시작하지 않으면 NaN을 반환한다는 것임

### 2.5 parseInt()

매개변수에 전달된 문자열을 정수형 숫자(integer)로 해성하여 반환함.
반환값은 언제나 10진수임

```
parseInt(string, radix);
// string: 파싱 대상 문자열
// radix: 진법을 나타내는 기수(2 ~ 36, 기본값 10)
```

첫 번째 매개변수에 전달된 값이 문자열이 아니면 문자열로 변환한 후 숫자로 해석하여 반환함

```
parseInt(10);     // 10
parseInt(10.123); // 10
```

두 번쨰 매개변수에는 진법을 나타내는 기수(2~36)을 지정할 수 있음. 기수를 생략하면 첫 번째 매개변수에 전달된 문자열을 10진수로 해석하여 반환함

```
parseInt('10');     // 10
parseInt('10.123'); // 10
```

두 번째 매개변수에 진법을 나타내는 기수를 지정하면 첫 번째 매개변수에 전달된 문자열을 해당 기수의 숫자로 해석하여 반환함. 이때 **반환값은 언제나 10진수**임

```
parseInt('10', 2);  // 2진수 10 → 10진수 2
parseInt('10', 8);  // 8진수 10 → 10진수 8
parseInt('10', 16); // 16진수 10 → 10진수 16
```

> 기수를 지정하여 10진수를 해당 기수의 문자열로 반환하고 싶을 때는 Number.prototype.toString 메소드를 사용함

두 번째 매개변수에 진법을 나타내는 기수를 지정하지 않더라도 첫 번째 매개변수에 전달된 문자열이 '0x' 또는 '0X'로 시작하면 16진수로 해석해 반환함

```
parseInt('0x10'); // 16진수 10 → 10진수 16
```

두 번째 매개변수에 진법을 나타내는 기수를 지정하지 않더라도 첫 번째 매개변수에 전달된 문자열이 '0'으로 시작한다면 8진수로 해석하지 않고 10진수로 해석함

> ES5 이전까지는 비록 사용을 금지하고는 있었지만 "0"로 시작하는 숫자를 8진수로 해석했음. ES6부터는 "0"로 시작하는 숫자를 8진수로 해석하지 않고 10진수로 해석함.

따라서 문자열을 8진수로 해석하려면 반드시 지수를 지정해야함

```
parseInt('010'); //10 //8진수 10으로 인식하지 않음
parseInt('010', 8); // 8진수 10 → 10진수 8
parseInt('10', 8); // 8진수 10 → 10진수 8
```

parseInt는 첫 번째 매개변수에 전달된 문자열의 첫 번째 문자가 해당 지수의 숫자로 변환될 수 없다면 NaN을 반환함

```
parseInt('A0'));   // NaN
parseInt('20', 2); // NaN
```

하지만 첫번째 매개변수에 전달된 문자열의 두번째 문자부터 해당 진수를 나타내는 숫자가 아닌 문자(예를 들어 2진수의 경우, 2)와 마주치면 이 문자와 계속되는 문자들은 전부 무시되며 해석된 정수값만을 반환함

```
parseInt('1A0'));    // 1 // 10빈수로 해석되어 정수 1 반환. A는 숫자로 해석이 안 되므로 무시
parseInt('102', 2)); // 2 // 2진수로 해석해 정수 2 반환. 문자 2는 2진수에서 사용하지 않는 숫자이므로 무시
parseInt('58', 8);   // 5 // 8진수로 해석해 정수 5반환. 문자열 58에서 문자 8은 8진수에서 사용하지 않는 숫자이므로 무시됨
parseInt('FG', 16);  // 15 // 16진수에서는 0부터 9까지의 숫자와 A부터 F까지의 알파벳이 사용됨. 따라서 'F'까지는 유효한 16진수이지만, 'G'는 유효한 16진수가 아니므로, 해당 문자열에서 'F'까지만을 16진수로 해석하고 그 값인 15를 반환함
```

첫번째 매개변수에 전달된 문자열에 공백이 있다면 첫번째 문자열만 해석하여 반환하며 전후 공백은 무시됨. 만일 첫번째 문자열을 숫자로 파싱할 수 없는 경우, NaN을 반환함

```
parseInt('34 45 66'); // 34
parseInt(' 60 ');     // 60
parseInt('40 years'); // 40
parseInt('He was 40') // NaN
```

### 2.6 encodeURI() / decodeURI()

encodeURI()는 매개변수로 전달된 URI(uniform resource identifier)를 인코딩함

<div align="center">
<img src="https://poiemaweb.com/img/uri.png" height="50%" width="50%">
</div>

여기서 인코딩이란 URI 문자들을 이스케이프 처리하는 것을 의미함

**이스케이프 처리**
네트워크를 통해 정보를 공유할 때 어떤 시스템에서도 읽을 수 있는 ASCII Character-set로 변환하는 것임. UTF-8 특수문자의 경우 1문자당 1~3byte, UTF-8 한글 표현의 경우, 1문자당 3btye임. 예를 들어 특수문자 공백(space)은 %20, 한글 ‘가’는 %EC%9E%90으로 인코딩됨

**이스케이프 처리 이유**
URI 문법 형식 표준 RFC3986에 따르면 URL은 ASCII Character-set으로만 구성되어야 하며 한글을 포함한 대부분의 외국어나 ASCII에 정의되지 않은 특수문자의 경우 URL에 포함될 수 없음. 따라서 URL 내에서 의미를 갖고 있는 문자(%, ?, #)나 URL에 올 수 없는 문자(한글, 공백 등) 또는 시스템에 의해 해석될 수 있는 문자(<, >)를 이스케이프 처리하여 야기될 수 있는 문제를 예방하기 위함임

단 아래의 문자는 이스케이프 처리에서 제외됨

- 알파벳, 숫자 0~9, -\_.!~\*'()

decodeURI()은 매개변수로 전달된 URI을 디코딩함

```
encodeURI(URI)
// URI: 완전한 URI
decodeURI(encodedURI)
// encodedURI: 인코딩된 완전한 URI
```

```
var uri = 'http://example.com?name=이웅모&job=programmer&teacher';
var enc = encodeURI(uri);
var dec = decodeURI(enc);
console.log(enc);
// http://example.com?name=%EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher
console.log(dec);
// http://example.com?name=이웅모&job=programmer&teacher
```

### 2.7 encodeURIComponent() / decodeURIComponent()

encodeURIComponent()은 매개변수로 전달된 URI(Uniform Resource Identifier) component(구성 요소)를 인코딩함. 여기서 인코딩이란 URI의 문자들을 이스케이프 처리하는 것을 의미함. 단 아래의 문자는 이스케이프 처리에서 제외됨

- 알파벳, 숫자 0~9, - \_ . ! ~ \* ‘ ( )

decodeURIComponent()은 매개변수로 전달된 URI component(구성 요소)를 디코딩함

encodeURIComponent()는 인수를 쿼리스트링의 일부라고 간주함. 따라서 =, ?, &를 인코딩함. 반면 encodeURI()는 인수를 URI 전체라고 간주하며 파라미터 구분자인 =, ?, &를 인코딩하지 않음

```
encodeURIComponent(URI)
// URI: URI component(구성 요소)
decodeURIComponent(encodedURI)
// encodedURI: 인코딩된 URI component(구성 요소)
```

```
var uriComp = '이웅모&job=programmer&teacher';

// encodeURI / decodeURI
var enc = encodeURI(uriComp);
var dec = decodeURI(enc);
console.log(enc);
// %EC%9D%B4%EC%9B%85%EB%AA%A8&job=programmer&teacher
console.log(dec);
// 이웅모&job=programmer&teacher

// encodeURIComponent / decodeURIComponent
enc = encodeURIComponent(uriComp);
dec = decodeURIComponent(enc);
console.log(enc);
// %EC%9D%B4%EC%9B%85%EB%AA%A8%26job%3Dprogrammer%26teacher
console.log(dec);
// 이웅모&job=programmer&teacher
```
