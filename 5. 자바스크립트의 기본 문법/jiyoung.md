# 변수

- 변수(varibale)은 값(value)를 저장(할당)하고 그 저장된 값을 참조하기 위해 사용한다.
- 캐싱할 필요가 있는 값은 변수에 담아 사용한다.
- 위치(주소)를 기억하는 저장소이다.
- 변수란, **메모리 주소(memory address)**에 접근하기 위해 사람이 이해할 수 있는 언어로 지정한 식별자(identifier)이다.
- 변수 선언에는 `var`, `const`를 사용하고 값을 할당하기 위해 `=`(할당연산자)를 사용한다.

```javascript
var x; //변수의 선언
x = 1; //값의 할당
```

# 값

```javascript
var str = "Hello World";
```

예제설명

- str 이라는 이름의 **변수 선언**
- 문자열 **리터럴** "Hello World" **값 할당**
- 문자열 리터럴 "Hello World"는 **문자열 타입의 값**

| 용어                   | 의미                                                                 |
| ---------------------- | -------------------------------------------------------------------- |
| 데이터 타입(Data Type) | 프로그래밍 언어에서 사용할 수 있는 값의 종류                         |
| 변수(variale)          | 값이 저장된 메모리 공간의 주소를 가리키는 식별자(identifier)         |
| 리터럴(literal)        | 소스코드 안에서 직접 만들어 낸 상수 값 자체, 값을 구성하는 최소 단위 |

값은 프로그래밍에 의해 조작될 수 있고 다양한 방법으로 생성 가능하다.
가장 간단한 방법은 **리터럴 표기법(literal notation)**을 사용하는 것이다.

```javascript
//숫자 리터럴
10.50
1001

//문자열 리터럴
'hello'
"world"

//불리언 리터럴
false
true

//null 리터럴
null

//undefiend 리터럴
undifiend

//객체 리터럴
{name: "yoon", gender:"female"}

//배열 리터럴
[1,2,3]

//정규표현식 리터럴
/ab+c/

//함수 리터럴
function(){}
```

숫자, 문자열, 불리언과 같은 원시 타입의 리터럴은 다양한 연산자가 피연산자가 되어 하나의 값으로 평가될 수 있다.

```javascript
//산술 연산
10.5 + 1001;
```

자바스크립트의 모든 값은 데이터 타입을 갖는다.

7개의 데이터 타입
**원시타입 (primitive data type)**

- `number`
- `string`
- `boolean`
- `null`
- `undefined`
- `symbole` (ECMAScript 6)
  **객체 타입(object data type)**
- `object`

자바스크립트는 다른 언어와 다르게 변수를 선언할 때 데이터 타입을 미리 지정하지 않는다.
다시 말해, 변수에 할당된 값의 타입에 의해 동적으로 변수의 타입이 결정된다(동적 타이핑).

```javascript
//number
var num1 = 1001;
var num2 = 10.5;

//string
var string1 = "Hello";
var string2 = "World";

//boolean
var bool = true;

//null
var foo = null;

//nudefined
var bar;

//object
var obj = { name: "yoon", gender: "female" };

//array
var arr = [1, 2, 3];

//function
var foo = function () {};
```

# 연산자

연산자(operator)는 하나이상의 표현식을 대상으로 산술, 할당, 비교, 논리, 타입 연산 등을 수행해 하나의 값을 만든다. 연산의 대상은 피연산자(operand)이다.

```javascript
//산술 연산자
var area = 5 * 4; //20

//문자열 연산자
var str = "My name is" + "Lee"; //My name is Lee

//할당 연산자
var color = "red"; //"red"

//비교 연산자
var foo = 3 > 5; // false

//논리 연산자
var bar = 5 > 3 && 2 < 4; // true

//타입 연산자
var type = typeof "Hi"; //"string"

//인턴스 생성 연산자
var today = new Date(); // Tue Apr 11 2023 00:10:19 GMT+0900 (한국 표준시)
```

자바스크립트는 암묵적 타입 강제 변환을 통해 연산을 수행하기 때문에 피연산자의 타입은 반드시 일치할 필요는 없다.

```javascript
var foo = 1 + "10"; //"110"
var bar = 1 * "10"; //10
```

# 키워드

키워드는 수행할 동작을 규정한 것이다.

# 주석

주석(comment)은 작성된 코드의 의미를 설명하기 위해 사용한다.

- 한줄 주석은 `//` 다음에 작성한다.
- 여러 줄 주석은 `/* 과 */의` 사이에 작성한다.

주석은 해석기(parser)가 무시하며 실행되지 않는다.

# 문

프로그램(스크립트)은 컴퓨터(client-side JavaScript, 웹 브라우저)에 의해 단계별로 수행될 명령들의 집합이다.
문은 리터럴, 연산자(operator), 표현식(expression),키워드(keyword) 등으로 구성되며 세미콜(;)으로 끝나야 한다.

```javascript
var x = 5;
var y = 6;
var z = x + y;

console.log(z);
```

문은 코드 블록 (code block, {...})으로 그룹화할 수 있고 그룹화의 목적은 함께 실행되어져야 하는 문을 정의하기 위함이다.

```javascript
//함수
function myFunction(x, y) {
  return x + y;
}

//if 문
if (x > 0) {
  //do something
}

//for 문
for (var i = 0; i < 10; i++) {
  //do something
}
```

문들은 일반적으로 위에서 아래 순서로 실행된다. 실행 순서는 조건문(if, switch), 반복문(while, for)의 사용으로 제어할 수 있다 이를 흐름제어(control flow)라 한다.

```javascript
var time = 10;
var greeting;

if (time < 10) {
  greeting = "Good morning";
} else if (time < 20) {
  greeting = "Good day";
} else {
  greeting = "Good evening";
}

console.log(greeting);
```

다른 언어와 달리 자바스크립트에서는 블록 유효범위(block-level scope)를 생성하지 않는다. 함수 단위의 유효범위(function-level scope)만이 생성된다.

# 표현식

표션식(expression)은 하나의 값으로 평가(evaluation)된다. 값(리터럴), 변수, 객체의 프로퍼티, 배열의 요소, 함수 호출, 메소드 호출, 피연산자와 연산자의 조합은 모두 표현식이며 하나의 값으로 평가(evaluation)된다.

```javascript
//표현식

5; //5
5 * 10; //50
5 * 10 >
  10(
    //ture
    5 * 10 > 10
  ) && 5 * 10 < 100; //true
```

# 문과 표현식의 비교

자연어에서 문(statement)이 마침표로 끝나는 하나의 완전한 문장(sentence)이라고 한다면 표현식은 문을 구성하는 요소이다.

```javascript
//선언문(declaration statement)
var x = 5 * 10; // 표현식 x = 5 * 10를 포함하는 문이다.

//할당문(assignment statement)
x = 100; // 이 자체가 표현식이지만 완전한 문이기도 하다.
```

# 함수

함수란 어떤 작업을 수행하기 위해서 필요한 문(statement)들의 집합을 정의한 코드 블록이다.
함수는 이름과 매개변수를 갖으며 필요한 때에 호출하여 코드 블록에 담긴 문들을 일괄적으로 실행할 수 있다.

```javascript
//함수의 정의 (함수 선언문)
function square(number) {
  return number * number;
}
```

함수는 호출에 의해 실행되고 여러번 호출이 가능하다.

```javascript
//함수의 정의(함수 선언문)
function square(number) {
  number * number;
}

//함수 호출
square(2); //4
```

# 객체

자바스크립트는 객체(object) 기반의 스크립트 언어이며 자바스크립트를 이루고 있는 거의 **모든 것**이 객체이다 (원시 타입을 제외한 나머지 값들).

객체는 키와 값으로 구성된 프로퍼티(property)의 집합이다.
함수는 일급 객체로 값을 취급할 수 있다. 따라서 프로퍼티 값으로 함수를 사용할 수 있으며 프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 메소드라 부른다.

```javascript
var person = {
  name: "Lee",
  genter: "male",
  sayHello: function () {
    console.log("Hi! My name is" + this.name);
  },
};

console.log(typeof person); //object
console.log(person) //{name:"Lee", genter:"male", sayHello:[Function:sayHello]}

person.sayHello(); Hi! May name is Lee
```

이와 같이 객체는 데이터를 의미하는 프로퍼티와 데이터를 참조하고 조작할 수 있는 동작(behaviour)을 의미하는 메소드(method)로 구성된 집합이다.
자바스크립트의 객체는 객체지향의 상속을 구현하기 위해 "프로토타입"이라고 불리는 객체의 프로퍼티와 메소드를 상속받을 수 있다.

# 배열

배열(array)은 1개의 변수에 여러 개의 값을 순차적으로 저장할 때 사용된다.

```javascript
var arr = [1, 2, 3, 4, 5];

console.log(arr[1]); //2
```
