# 객체(Object)란?

자바스크립트는 객체(object)기반의 스크립트 언어이다.

- 자바스크립트를 이루고 있는 거의 **모든 것**(원시 타입을 제외한 나머지)이 객체이다.

**자바스크립트의 객체는 키(key)와 값(value)으로 구성되어 있는 프로퍼티(property)들의 집합이다.**

- 프로퍼티 값으로 자바스크립트에서 사용할 수 있는 모든 값을 사용할 수 있다.
- 자바스크립트의 함수는 일급 객체이므로 값을 취급할 수 있다.
- 프로퍼티 값으로 함수를 이용 할 수 있다. (프로퍼티 값이 함수일 경우 **메소드**라 부른다.)

객체는 데이터를 의미하는 프로퍼티와, 데이터를 참소하고 조작할 수 있는 동작(메소드)로 구성된 집합이다.

자바스크립트의 객체지향의 상송을 구현하기 위해 **"프로토타입(prototype)"** 이라고 불리는 객체의 프로퍼티와 메소드를 상속받을 수 있다.

## 프로퍼티

프로퍼티는 프로퍼티 키(이름)와 프로퍼티 값으로 구성된다.

- 프로퍼티는 프로퍼티 키(식별자)로 유일하게 식별이 가능하다.
  프로퍼티 키: 빈 문자열을 포함하는 모든 문자열 또는 symbol 값
  프로퍼티 값: 모든 값
  프로퍼티 키에 문자열, symbol 값 이외의 값을 지정하면 암묵적으로 타입이 변화되어 문자열이 된다.
- 이미 존재하는 프로퍼티 키를 중복 선언하면 나중에 선언한 프로퍼티가 먼저 선언하는 프로퍼티를 덮어쓴다.

## 메소드

프로퍼티 값이 함수일 경우, 일반 함수와 구분하기 위해 메소드라고 부른다.

- 객체에 제한되어 있는 함수

# 객체 생성 방법

클래스 기반 객체 지향 언어는 클래스를 사전에 정의하고 필요한 시점에 new 연산자를 사용하여 인스턴스를 생성하는 방법으로 객체를 생성한다.

- 자바스크립트는 프로토타입 기반 객체 지향 언어이기 때문에 클래스 개념이 없고 별도의 객체 생성 방법이 존재한다.

## 객체 리터럴

가장 일방적인 자바스크립트의 객체 생성 방식

- 클래스 기반 객체 지향 언어와 비교시 간편하게 객체를 생성할 수 있다.

```javascript
var emptyObject = {};
console.log(typeof emptyObject); //object

var person = {
  name: "Lee",
  gender: "male",
  sayHello: function () {
    console.log("Hi! My name is " + this.name);
  },
};

console.log(typeof person); //object
console.log(person); //{name:"Lee", gender:"male", sayHello:f}

person.sayHello(); //Hi! My name is Lee
```

## Object 생성자 함수

new 연산자와 Object 생성자 함수를 호출하여 빈 객체를 생성할 수 있다.

- 빈 객체 생성 이후 메소드 추가로 객체 완성

생성자(constructor) 함수란 new 키워드와 함께 객체를 생성하고 초기화하는 함수이다.

- 생성자 함수를 통해 생성된 객체를 인스턴스(instance)라고 한다.
- object 외에 String, Number, Boolean, Array, Date, RegExp 등 빌트인 생성자 함수를 제공한다.

```javascript
//빈 객체의 생성
var person = new Object();

//프로퍼티 추가
person.name = "Lee";
person.gender = "male";
person.sayHello = function () {
  console.log("Hi! My name is" + this.name);
};

console.log(typeof person); //object
console.log(person); //{name:"Lee", gender:"male", sayHello:f}

person.sayHello(); //Hi! My name is Lee
```

- 객체 생성 방법에는 객체 리터럴을 사용하는 것이 더 편하다.

- 객체 리처털 방식으로 생성된 객체는 빌트인 함수인 object 생성자 함수로 객체를 생성하는것을 단순화 시킨 축양 표현이다.

## 생성자 함수

객체 리터럴 방식과 생성자 함수 방식으로 객체를 생성하는 것은 **프로퍼티 값만 다른 여러 개의 객체**를 생성할때 불편하다(동일한 프로퍼티를 갖는 객체임에도 불구하고 매번 같은 프로퍼티를 기술해야한다).

```javascript
var person1 = {
  name: "Lee",
  gender: "male",
  sayHello: function () {
    console.log("Hi! My name is" + this.name);
  },
};

var person2 = {
  name: "Kim",
  gender: "female",
  sayHello: function () {
    console.log("Hi! My name is" + this.name);
  },
};
```

- 생성자 함수를 사용하면 객체를 생성하기 위한 템플릿(클래스)처럼 사용하여 프로퍼티가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

```javascript
//생성자 함수
function Person(name, gender) {
  this.name = name;
  this.gender = gender;
  this.sayHello = function () {
    console.log("Hi! My name is" + this.name);
  };
}

//인스턴스 생성
var person1 = new Person("Lee", "male");
var person2 = new Person("Kim", "femaile");

console.log("person1:", typeof person1);
console.log("person2:", typeof person2);
console.log("person1:", person1);
console.log("person2:", person2);

person1.sayHello();
person2.sayHello();
```

- 생성자 함수 이름은 일반적으로 대문자로 시작한다. (생성자 함수임을 인식할 수 있다.)
- 프로퍼티 또는 메소드명 앞에 기술한 `this` 생성자 함수가 생성할 인스턴스(instance)를 가리킨다.
- this에 연결(바인딩)되어 있는 프로퍼티와 메소드는 `public`(외부에서 참조 가능)하다.
- 생성자 함수 내에서 선언된 일반 변수는 `private`(외부에서 참조 불가능)하다.
- 생성자 함수 내부에서는 자유롭게 접근이 가능하지만 외부에서는 접근할 수 없다.

```javascript
function Person(name, gender) {
  var married = true; //private
  this.name = name; //public
  this.gender = gender; //public
  this.sayHello = function () {
    //public
    console.log("Hi! My name is" + this.name);
  };
}

var person = new Person("Lee", "male");

console.log(typeof person); //object
console.log(person); //Person { name:"Lee", gender:"male", sayHello:[Function]}

console.log(person.gender); //"male"
console.log(person.married); //undefined
```

자바스크립트의 생성자 함수는 **객체를 생성하는 함수**이다.(자바와 같은 클래스 기반 객체 지향 언어의 생성자(constructor)와는 다르게 형식이 정해져있지 않고 기존 함수와 동일한 방법으로 생성자 함수를 선언하고 new 연산자를 붙여서 호출하면 해당 함수는 생성자 함수로 동작한다.)

- 생성자 함수가 아닌 일반 함수에 new 연산자를 붙여 호출하면 생성자 함수처럼 동잘할 수 있다.
- new 연산자와 함께 함수를 호출하면 `this`바인딩이 다르게 동작한다.

# 객체 프로퍼티 접근

## 프로퍼티 키

프로퍼티 키는 일반적으로 문자열(빈 문자열 포함)을 지정한다.

- 프로퍼티 키에 문자열이나 symbol값 이외의 값을 지정하면 암묵적으로 타입이 변환되어 문자열이 된다.
- **프로퍼티 키는 문자열이므로 따옴표 (""또는 '')를 사용한다.**
- 자바스크립트에서 사용한 유효한 이름이 아닌 경우, 따옴표를 사용하고 **유효한 이름**인 경우 따옴표를 생략한다.

** 프로퍼티 값이 함수인 경우 메소드라고 한다.**

```javascript
var person = {
  "first-name": "Ji-Young",
  "last-name": "Yoon",
  gender: "male",
  1: 10,
  function: 1, // OK, 하지만 예약어는 사용하지 말아야한다.
};

console.log(person);
```

프로퍼티 키 first-name에는 따옴표를 사용해야하지만 first_name에는 생략이 가능하다. \*`-` 는 사용 가능한 유효한 이름이 아닌 `'-'` 연산자가 있는 표현식이다.

```javascript
var person = {
    first-name:"Ji-Young", //SyntaxError: Unexpected token-
};
```

표현식을 프로퍼티 키로 사용하려면 대괄호로 묶어야한다.

- 자바스크립트 엔진은 표현식을 평가하기 위해 식별자 frist를 찾을 것이고 이때 ReferenceError가 발생한다.

````javascript
var person = {
    [first-name]: "Ji-Young", //ReferenceError:first is no defiend
    };
    ```
    예약어를 프로퍼티 키로 사용해도 에러가 발생하지 않지만, 예상치 못한 에러가 발생할 수 있어 프로퍼티 키로 사용하지 않아야한다.

```code
//자바스크립트 예약어
abstract  arguments boolean break byte
case  catch char  class*  const
continue  debugger  default delete  do
double  else  enum* eval  export*
extends*  false final finally float
for function  goto  if  implements
import* in  instanceof  int interface
let long  native  new null
package private protected public  return
short static  super*  switch  synchronized
this  throw throws  transient true
try typeof  var void  volatile
while with  yield
// *는 ES6에서 추가된 예약어
````

## 프로퍼티 값 읽기

객체의 프로퍼티 값에 접근하는 방법은 `마침표(.)`와 `대괄호([])`표기법이 있다.

```javascript
var person = {
    "first-name":"ji-young",
    "last-name":"Yoon",
    gender:"female",
    1:10
};

consoel.log(person);
console.log(person.first-name); //NaN: undefined-undefined
console.log(person[first-name]); //ReferenceError:first is no defined
console.log(person["first-name"]); //"ji-young"

console.log(person.gender); // "female"
console.log(person[gender]); //  ReferenceError:gender is not defined
console.log(person["gender"]); // "female"

console.log(person["1"]); // 10
console.log(person[1]); // 10: person[1]->person["1"]
cnosoole.log(person.1); // SyntaxError
```

프로퍼티 키가 유효한 자바스크립트 이름이고 예약어가 아닌 경우, 프로퍼티 값은 **마침표, 대괄호** 표기법을 모두 사용할 수 있다.

프로퍼티 이름이 유효하지 않은 경우 프로퍼티 값은 **대괄포** 표기법으로 읽어야한다.

- 대괄호 내에 프로퍼티 이름은 문자열이어야한다.

객체에 존재하지 않는 프로퍼티를 참조하면 `undefined`를 반환한다.

```javascript
var person = {
  "first-name": "ji-young",
  "last-name": "Yoon",
  gender: "female",
};

console.log(person.age); //undefined
```

## 프로퍼티 값 갱신

객체가 소유하고 있는 프로퍼티에 새로운 값을 할당하면 값이 갱신된다.

```javascript
var person = {
  "first-name": "ji-young",
  "last-name": "Yoon",
  gender: "female",
};

person["first-name"] = "Kim";
console.log(person["first-name"]); //"Kim"
```

## 프로퍼티 동적 생성

객체가 소유하지 않고 있지 않은 프로퍼티 키에 값을 할당하면 주어진 키와 값으로 프로퍼티를 생성하여 객체에 추가한다.

```javascript
var person = {
  "first-name": "jiyoung",
  "last-name": "Yoon",
  gender: "female",
};

person.age = 20;
console.log(person.age); //20
```

## 프로퍼티 삭제

`delete` 연산자를 사용하면 객체의 프로퍼티를 삭제할 수 있다.

- 피연산자는 프로퍼티 키 이어야한다.

```javascript
var person = {
  "first-name": "jiyoung",
  "last-name": "Yoon",
  gender: "female",
};

delete person.gender;
console.log(person.gender); //undefined

delete person;
console.log(person); //Object {first-name:"jiyoung", last-name:"Yoon"}
```

## for-in문

for-in 문을 사용하면 객체(배열 포함)에 포함된 모든 프로퍼티에 대해 루프를 수행할 수 있다.

```javascript
var person = {
    "first-name":"jiyoung",
    "last-name":"Yoon",
    gender:"female"
};

//prop에 객체의 프로퍼티 이름이 반환된다 * 순서를 보장하지 않음 *
for (var prop in person){
    console.log(prop +":"+person[prop]);
}
/*
first-name:jiyoung
last-name:Yoon
gender:female
*/

var array = ["one","two"];

//index에 배열의 경우 인덱스가 반환된다.
for (var index in array){
    console.log(index:":"+array[index]);
    }
/*
0:one
1:two
*/
```

for-in문은 객체의 문자열 키(key)를 순회하기 위한 문법이다.

- 배열에는 사용하지 않는 것이 좋다.

1. 객체의 경우, 프로퍼티의 순서가 보장되지 않는다(객체의 프로퍼티에는 순서가 없기 때문). 배열운 순서를 보장하는 데이터 구조이지만 **순서를 보장하지 않는다**.
2. 배열 요소들만을 순회하지 않는다.

```javascript
//배열 요소들만 순회하지 않는다.
var array = ["one", "two"];
array.name = "my array";

for (var index in array) {
  console.log(index + ":" + array[index]);
}
/*
0:one
1:two
name:my name
*/
```

- 이런 단점을 극복하기 위해 ES6에서 for-of문이 추가되었다.

```javascript
const array = [1, 2, 3];
array.name = "my array";

for (const value of array) {
  console.log(value);
}

/*
1
2
3
*/

for (const [indx, value] of array.entries()) {
  console.log(index, value);
}

/*
0 1
1 2
2 3
*/
```

- for-in문은 객체의 프로퍼티를 순회하기 위해 사용
- for-of문은 배열의 요소를 순회하기 위해 사용

# Passy-by reference

object type을 객체 타입 또는 참조 타입이라고 한다.
참조 타입은 객체의 모든 연산이 실제 값이 아닌 참조값으로 처리 된다.

- 원시 타입은 값이 한번 정해지면 변경할 수 없지만(immutable), 객체는 프로퍼티를 변경, 추가, 삭제가 가능한 (mutable) 값이다.

객체 타입은 동적으로 변화할 수 있어 메모리 공간을 확보 예측이 불가능하기 때문에 런타임에 메모리 공간을 확보하고 메모리의 힙 영역(heap segment)에 저장된다.

- 원시 타입은 값(value)으로 전달된다. (pass-by-value)

````javascript
//pass-by-reference
var foo = {
    val:10
    }
    var bar = foo;
    console.log( foo.val, bar.val); //10 10
    console.log(foo===var); //true

    bar.val =20;
    console.log(foo.val, bar.val); //20 20
    console.log(foo===bar); //true;
    ```
    * 객체 리터럴 방식으로 생성된 변수 foo는 객체 자체를 저장하고 있는 것이 아닌, 생성된 객체의 참조값(address)를 저장하고 있다.

```javascript
var foo = {val:10};
var bar = {val:10};

console.log(foo.val, bar.val); //10 10
console.log(foo === bar); //false

var baz = bar;
console.log(baz.val, bar.val); //10 10
console.log(baz === bar); //true
````

- 변수 foo와 bar는 내용은 같지만 별개의 객체를 생성하여 참조값을 할당하기 때문에 참조값(address)는 같지 않다.
- 변수 baz에는 bar의 값을 할당하여 같은 객체를 가리키는 참조값을 저장하고 있어 참조값이 동일하다.

```javascript
var a = {},
  b = {},
  c = {}; // 각각 다른 빈 객체를 참조
console.log(a === b, a === c, b === c); // false false false

a = b = c = {}; //a,b,c는 모두 같은 빈 객체를 참조
console.log(a === b, a === c, b === c); //true true true
```

# Pass-by-value

원시 타입은 값으로 전달된다.

- 값이 복사 되어 잔달 되는 것을 pass-by-value(값에 의한 전달)라 한다.
- 원시 타입은 값이 정해지면 변경할 수 없고(immutable), 런타입(변수 할당 시점)에 메모리의 스택 영역(stack segment)에 고정된 메모리 영역을 점유하고 저장된다.

```javascript
// pass-by-value
var a = 1;
var b = a;

console.log(a, b); //1 1
console.log(a === b); //true

a = 10;
console.log(a, b); //1 10
console.log(a === b); //false
```

a는 원시 타입인 숫자 1을 저장하고 있다.

- 원시 타입의 경우 값이 복사되어 변수에 저장된다. (참조 타입이 아닌, 값 자체가 저장된다.)

# 객체의 분류

- Object

  - built-in object
    - standard built-in object
    - native object
      - BOM(Browser Object Model)
      - DOM(Document Object Model)
  - host object

- Built-in Object(내장 객체)는 웹페이지 등을 표현하기 위해 표현하기 위한 공통의 기능을 제공한다. 웹페이지가 브라우저에 의해 로드되자마자 별다른 행위없이 바로 사용가능하다.
  - standart built-in objects( or global objects)
  - BOM (Browser Object Model)
  - DOM (Document Object Model)

**Standard Built-in Objects(표준 빌트인 객체)**를 제외한 BOM과 DOM을 Native Object라고 분류하기도 한다.
사용자가 생성한 객체를 **Host Object(사용자 정의 객체)**라고 한다.

- Host Object(사용자 정의 객체)
  사용자가 생성한 객체이다. constructor혹은 객체 리터럴을 통해 사용자가 객체를 정의하고 확장시키기 때문에 built-in object와 native object가 구성된 이후에 구성된다.

16/April/2023
