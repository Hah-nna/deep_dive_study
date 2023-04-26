# 14 Prototype

# 프로토타입

# 프로토타입 객체

Java, C++ 과 같은 클래스 기반 객체지향 프로그래밍 언어와 달리 **자바스크립트**는 프로토타입 기반 객체지향 프로그래밍 언어이다.

- 자바스크립트의 동작 원리를 이해하기 위해서는 프로토타입의 개념을 잘 이해학 있어야한다.

클래스 기반 객체지향 프로그래밍 언어는 객체 생성 이전에 클래스를 정의하고 이를 통해 객체(인스턴스)를 생성한다. 프로토타입 기반 객체지향 프로그래밍 언어는 클래스 없이(class-less)도 (ECMAScript 6 클래스)객체를 생성하 수 있다.

- 자바스크립트의 객체 생성 바법
  자바스크립트의 모든 객체는 자신의 부모 역할을 담당하는 객체와 연결되어 있다.
- 객체 지향의 상속 개념와 각ㅌ이 부모 객체의 프로퍼티 또는 메소드를 상송받아 사용할 수 있게 한다.
- prototype 객체는 생성자 함수에 의해 생성된 각각의 객체에 공유 프로퍼티를 제공하기 위해 사용한다.

```javascript
var student = {
  name: "Lee",
  score: 90,
};
//studnet에는 hasOwnProperty 메소드가 없지만 아래 구문은 동작한다.
console.log(student.hasOwnProperty("name")); //true

console.dir(studnet);
```

ECMAScript spec에서는 자바스립트의 모든 객체는 [[Prototype]]이라는 인터널 슬롯(internal slot)을 가진다. [[Prototype]]객체의 데이터 프로퍼티는 get 액세스를 위해 상속되어 자식 객체의 프로퍼티처럼 사용할 수 있다. 하지만 set 액세스는 허용되지 않는다. 라고 되어있다.

[[Prototype]]의 값은 Prototype(프로토타입) 객체이며 **proto** accessor property로 접근할 수 있다. **proto** 프로퍼티에 접근하면 내부적으로 Object.getPrototypeOf가 호출되어 프로토타입 객체를 반환한다.

- student 객체는 **proto** 프로퍼티로 자신의 부모 객체(프로퍼파입 객체)인 Object.prototype을 가리키고 있다.

```javascript
var stuent ={
    name:"Lee"
    score:90
}
console.log(student.__proto__ === Object.prototype); //true
```

- 객체를 생성할 때 프로토타입이 결정된다.
- 결정된 프로토타입 객체는 다른 임의으 객체로 변경할 수 있다. **부모 객체인 프로토 타입을 동적으로 변경할수 있다는 것을**의미한다.

# [[Prototype]] vs prototype 프로퍼티

모든 객체는 자신의 프로토타입 객체를 가리키는 [[Prototype]]인터널 슬롯(internal slot)을 갖으며 상속을 위해 사용된다.
함수도 객체이므로[[Prototype]] 인터널 슬롯을 갖는다. 함수 객체는 일반 객체와 달리 prototype 프로퍼티도 소유하게 된다.

- prototype 프로퍼티는 프로토타입 객체를 가리키는 [[Prototype]]인터널 슬롯과 다르다.

```javascript
function Person(name) {
  this.name = name;
}
var foo = new Person("Lee");

console.dir(Person); //prototype 프로퍼티가 있다.
console.dir(foo); //prototype 프로퍼티가 없다.
```

- [[Prototype]]
  - 함수를 포함한 모든 객체가 가지고 있는 인터널 슬롯이다.
  - 객체의 입장에서 자신의 부모 역할을 하는 프로토타입 객체를 가리키며 함수 객체의 경우 `Function.prototype`를 가리킨다.

```javascript
console.log(Person.__proto__ === Function.Prototype);
```

- prototype 프로퍼티
  - 함수 객체만 가지고 있는 프로퍼티이다.
  - 함수 객체가 생성자로 사용될 때 이 함수를 통해 생성될 객체의 부모 역할을 하는 객체(프로토타입 객체)를 가리킨다.

```javascript
console.log(Person.prototype === foo.__proto__);
```

# constructor 프로퍼티

프로토타입 객체는 **constructor 프로퍼티**를 갖는다. 이 constructor 프로퍼티는 객체의 입장에서 자신을 생성한 객체를 가리킨다.

- Person() 생성자 함수에 의해 생성된 객체 foo, foo 객체를 생성한 객체는 Person() 생성자 함수이다. foo 객체입장에서 자신을 생성한 객체는 Person() 생성자 함수이며, foo 객체의 프로토타입 객체는 Person.prototypedlek.
- 프로토타입 객체 Person.prototype의 constructor 프로퍼티는 Person() 생성자 함수를 가리킨다.

```javascript
function Person(name) {
  this.name = name;
}
var foo = new Person("Lee");

//Person() 생성자 함수에 의해 생성된 객체를 생성한 객체는 Person() 생성자 함수이다.
console.log(Personprototype.constructor === person);

//foo 객체를 생성한 객체는 Person() 생성자 함수이다.
console.log(foo.constructor === person);

//Person() 생성자 함수를 생성한 객체는 Function() 생성자 함수이다.
console.log(Person.constructor === Function);
```

# Prototype chain

**프로토타입 체인**이란 자바스크립트는 특정 객체의 프로퍼티나 메소드에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티 또는 메소드가 없으면 [[Prototype]]이 가리키는 링크를 따라 자신의 부모 역할을 하는 프로토타입 객체의 프로퍼티나 메소드를 차례대로 검색하는 것을 말한다.

```javascript
var stduent = {
  name: "Lee",
  score: 90,
};
//Object.prototype.hasOwnProperty()
console.log(student.hasOwnProperty("name")); //true
```

- student 객체는 hasOwnProperty 메소드를 가지고 있지 않아 에러가 발생하여야 하나 정상적으로 결과가 출력되었다.
- stduent 객체의 [[Prototype]]이 가리키는 링크를 따라 student 객체의 부모 역할을 하는 프로토타입 객체(Object.prototype)의 메소드 hasOwnProperty를 호출했기 때문이다.

```javascript
var student = {
  name: "Lee",
  score: 90,
};
console.dir(student);
console.log(student.hasOwnProperty("name")); //true
console.log(stduent.__proto__ === Object.prototype); //true
console.log(Obejct.prototype.hasOwnProperty("hasOwnProperty")); //true
```

## 객체 리터럴 방식으로 생성된 객체의 프로토타입 체인

**객체 생성 방법** 3가지

- 객체 리터럴
- 생성자 함수
- Object()생성자 함수

객체 리터럴 방식으로 생성된 객체는 내장 함수 (Built-in)인 Object() 생성자 함수로 객체를 생성하는 것을 단순화 시킨 것이다.
자바스크립트 엔진은 **객체 리터럴**로 객체를 생성하는 코드를 만나면 내부적으로 Object() 생성자 함수를 사용하여 객체를 생성한다.

Object() 생성자는 함수이다. 함수 객체인 Object() 생성자 함수는 일반 객체와 달리 Prototype 프로퍼티가 있다.

- prototype 프로퍼티는 함수 객체가 생성자로 사용될 때 이 삼수를 통해 생성된 객체의 부모 역하릉ㄹ 하는 객체, 프로토타입 객체를 가리킨다.
- [[prototpye]]은 객체의 입장에서 자신의 부모 역할을 하는 객체, 프로토타입 객체를 가리킨다.

```javascript
var person = {
  name: "Lee",
  gender: "male",
  sayHello: function () {
    console.log("Hi! my name is " + this.name);
  },
};

console.dir(person);

console.log(person.__proto__ === Object.prototype); //1 true
console.log(Object.prototype.constructor === Object); //2 true
console.log(Object.__proto__ === Function.prototype); //3 true
console.log(Function.prototype.__proto__ === Object.prototype); //4 true
```

- 객체 리터럴을 사용하여 객체를 생성한 경우, 그 객체의 프로토타입 객체는 Object.prototype이다.

## 생성자 함수로 생성된 객체의 프로토타입 체인

생성자 함수로 객체를 생성하기 위해서 생성자 함수를 정의해야 한다.
**함수를 정의하는 방식** 3가지

- 함수 선언식(Function declaration)
- 함수 표현식(Function expression)
- Function() 생성자 함수

함수 표현식으로 함수를 정의할 때 함수 리터럴 방식을 사용한다.

```javascript
var square = function (number) {
  return number * number;
};
```

함수 선언식의 경우 자바스크립트 엔진이 내부적으로 기명 함수표현식으로 변환한다

```javascript
var square = function square(number) {
  return number * number;
};
```

함수석언식, 표현식 모두 함수 리터럴 방식을 사용한다. 함수 리터럴 방식은 Function() 생성자 함수로 함수를 생성하는 것을 단순화 시킨것이다.

- 3가지 함수 정싀 방식은 Function() 생성자 함수를 통해 함수 객체를 생걱한다.
- 어떤 방식으로 함수 객체를 생성해도 모든 함수 객체의 prototype 객체는 function.prototyped이다.
- 생성자 함수도 객체이므로 생성자 함수의 prototype의 객체는 Function.prototype이다.

객체 생성 방식 3가지
|객체 생성 방식|엔진의 객체 생성|인스턴스 prototype 객체|
|---|---|---|
|객체 리터럴|Object() 생성자 함수|Object.prototype|
|Object()생성자 함수|Object() 생성자 함수|Object.prototype|
|생성자 함수|생성자 함수|생성자 함수 이름.prototype|

```javascript
function Person(name, gender){
    this.name = name;
    this.gender = gender;
    this.sayHello = fuction(){
        console.log("Hi! my name is" + this.name);
    };
}

var foo = new Person("Lee","male");

console.dir(Person);
console.dir(foo);

console.log(foo.__proto__ === Person.prototype); //1 true
console.log(Person.prototype.__proto__ === Object.prototype); //2 true
console.log(Person.prototype.constructor === Person); //3 true
console.log(Person.__proto__ === Function.prototype); //4 true
console.log(Function.prototype.__proto__ === Object.prototype); //5 true
```

- foo 객체의 프로토타입 객체 Person.prototype 객체와 Person() 생성자 함수 프로토타입 객체인 Functino.prototype의 프로토타입 객체는 Object.prototype 객체이다.
- 객체 리터럴 방식이나 생성자 함수 방식이나 모든 객체의 부모 객체인 Object.prototype 객체에서 프로토타입 체인이 끝나기 때문이다.
- Object.prototype 객체를 프로토타입 체인의 종점(end of prototype chain)이라한다.

# 프로토타입 객체의 확장

프로토타입 객체도 객체이기 떄문에 일반 객체와 같이 프로퍼티를 추가/ 삭제할 수 있다.

- 추가/삭제된 프로퍼티는 즉시 프로토타입 체인에 반영된다.

```javascript
function Person(name) {
  this.name = name;
}

var foo = new Person("Lee");

Person.prototype.sayHello = function () {
  console.log("Hi! my name is" + this.name);
};

foo.sayHello();
```

생성자 함수 Person은 프로토타입 객체 Person.prototype와 prototype 프로퍼티에 의해 바인딩 되어있다. Person.prototype 객체는 일반 객체와 같이 프로퍼티를 추가/삭제가 가능하다.

- 예시의 Person.prototype 객체에 메소드 sayHello를 추가하면 sayHello 메소드는 프로토 타입 체인에 반영된다. 생성자 함수 Person에 의해 생성된 모든 객체는 프로토타입 체인에 의해 부모객체인 Person.prototype의 메소드를 사용할 수 있다.

# 원시 타입(Primitive data type)의 확장

자바스크립트에서 원시 타입(숫자,문자열, boolean, null, undefined)을 제외한 모든 것은 객치이다.

- 원시타입인 문자열은 객체와 유사하게 동장한다.

```javascript
var str = "test";
console.log(typeof str); //string
console.log(str.constructor === String); //true
console.dir(str); //test

var strObj = new String("test");
console.log(typeof strObj); //object
console.log(strObj.constructor === String); //true
console.log(strObj); // {0:"t", 1:"e", 2:"s", 3:"t", length:4, __proto__:String, [[PrimitiveValue]]:"test"}

console.log(str.toUpperCase()); //TEST
console.log(strObj.toUpperCase()); //TEST
```

원시 타입 문자열과 String() 생성자 함수로 생성한 문자열 객체의 타입은 다르다.
원시 타입은 객체가 아니므로 프로퍼티나 메소드를 가질수 없지만, 원시 타입으로 프로퍼티나 메소드를 호출할 때 **원시 타입과 연관된 객체로 일시적으로 변환되어 프토로타입 객체를 공유하게 된다.**

- 원시타입은 객체가 아니므로 프로퍼티나 메소드를 직접 추가할 수 ㅇ벗다.

```javascript
var str = "test";

//에러가 발생하지 않는다.
str.myMethod = function () {
  console.log("str.myMethod");
};

str.myMethod(); //Uncaught TypeError: str.myMethod is not a function
```

string 객체의 프로토타입 객체 string.prototype에 메소드를 추가하면 원시 타입, 객체 모두 메소드를 사용할 수 있다.

```javascript
var str = "test";

String.prototype.myMethod = function () {
  return "myMethod";
};

console.log(str.myMethod()); //myMethod
console.log("string".myMethod()); //myMethod
console.dir(String.prototype);
```

- 모든 객체는 프로토타입 체인에 의해 Object.porotpye 객체의 메소드를 사용할 수 있다.
  Object.prototype 객체는 프로토타입 체인의 종점으로 모든 객체가 사용할 수 있는 메소드를 갖는다.

- 자바스크립트는 표준 내장 객체의 프로토타입 객체에 개발자가 정의한 메소드의 추가를 허용한다.

```javascript
var str = "test";

String.prototype.myMethod = function () {
  return "myMethod";
};

console.log(str.myMethod());
console.dir(String.prototype);

console.log(str.__ptoro__ === String.prototype); // 1 true
console.log(String.prototype.__proto__ === Object.prototype); // ② true
console.log(String.prototype.constructor === String); // ③ true
console.log(String.__proto__ === Function.prototype); // ④ true
console.log(Function.prototype.__proto__ === Object.prototype); // ⑤ true
```

# 프로포타입 객체의 변경

객체를 생성할 때 프로토타입이 결정되고 결정된 프로토타입 객체는 임의로 변경할 수 있다.
부모 객체인 프로토타입을 동적으로 변경할 수 있다는 것을 의미하고 이를 활용해 객체의 상속을 구현할 수 있다.

프로토타입 객체 변경시 주의 할 점

- 프로토타입 객체 변경 시점 이전에 생성된 객체
  기존 프로토타입 객체를[[prototype]]에 바인딩한다.
- 프로토타입 객체 변경 시점 이후에 생성된 객체
  변경된 프로토타입 객체를 [[prototype]]에 바인딩한다.

```javascript
function Person(name) {
  this.name = name;
}
var foo = new Person("Lee");

//프로토타입 객체의 변경
Person.prototype = { gender: "male" };

var bar = new Person("Kim");

console.log(foo.gender); //undefined
console.log(bar.gender); //"male"

console.log(foo.constructor); // 1 Person(name)
console.log(bar.constructor); // 2 Object()
```

1. constructor 프로퍼티는 person() 생성자 함수를 가리킨다.
2. 프로토타입 객체 변경 후, Person() 생성자 함수의 Prototype 프로퍼티가 가리키는 프로토타입 객체를 일반 객체로 변경하면서 Person.prototype.constructor 프로퍼티도 삭제되었다. 프로토타입 체이닝에 의해 Object.prototype.constructor, Object() 생성자 함수가 된다.

# 프로토타입 체인 동작 조건

객체의 프로퍼티를 참조하는 경우, 해당 객체에 프로퍼티가 없는 경우, 프로토타입 체인이 동작한다.

객체의 프로퍼티에 값을 할당하는 경우, 프로토타입 체인이 동장하지 않는다.

- 객체에 해당 프로퍼티가 잇는 경우, 값을 재할당하고 해당 프로퍼티가 없는 경우는 해당 객체에 프로퍼티를 동적을 추가한다.

```javascript
function Person(name) {
  this.name = name;
}

Person.prototype.gender = "male"; //1

var foo = new Person("Lee");
var bar = new Person("Kim");

console.log(foo.gender); // 1 "male"
console.log(bar.gender); // 1 "male"

//1. foo 객체에 gender 프로퍼티가 없으면 프로퍼티 동적 추가
//2. foo 객체에 gender 프로퍼티가 있으면 해당 프로퍼티에 값 할당
foo.gender = "female"; //2

console.log(foo.gender); //2 "female"
console.log(bar.gender); //1 "male"
```

- foo 객체의 gender 프로퍼티에 값을 할당 하려면, 프로토타입 체인이 발생하여 Person.prototype 객체의 gender 프로퍼티에 값을 할당하는 것이 아니라 foo 객체에 동적으로 추가한다.

25/April/2023
