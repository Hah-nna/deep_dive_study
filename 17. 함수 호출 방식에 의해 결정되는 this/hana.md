자바스크립트의 함수는 호출될 때 매개변수로 전달되는 인자값 이외에 `arguments 객체`와 `this`를 암묵적으로 전달 받음

```
function square(number) {

  console.log(arguments);
  // Arguments(1)
0: 2
callee: ƒ square(number)
length: 1
Symbol(Symbol.iterator): ƒ values()
[[Prototype]]: Object


  console.log(this);
// window
  return number * number;
}

square(2);
```

자바스크립트의 this는 자바와 같은 익숙한 언어의 개념과 달라 개발자에게 혼란을 줌

java에서는 this 인스턴스는 자기자신을 가리키는 참조변수임. this 자체가 객체 자신에 대한 참조 값을 가지고 있다는 뜻임. 주로 매개변수와 객체 자신이 가지고 있는 멤버 변수명이 같을 경우 이를 구분하기 위해서 사용됨. 아래 java 코드의 생성자 함수 내의 this.name은 멤버 변수를 의미하며 name은 생성자 함수가 전달받은 매개변수를 의미함

```
public Class Person {

  private String name;

  public Person(String name) {
    this.name = name;
  }
}
```

하지만 자바스크립트의 경우 java와 같이 this에 바인딩되는 객체는 한 가지가 아니라 해당 함수 호출 방식에 따라 this에 바인딩되는 객체가 달라짐

# 함수 호출 방식과 this 바인딩

자바스크립트의 경우 함수 호출 방식에 의해 this에 바인딩할 어떤 객체가 동적으로 결정됨. 즉, 함수를 선언할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, **함수를 호출할 때 함수가 어떻게 호출되었는지에 따라** this에 바인딩할 객체가 동적으로 결정됨

> 함수의 상위 스코프를 결정하는 방식인 **렉시컬 스코프**는 함수를 선언할 때 결정됨. this 바인딩과 혼동하지 말자

함수의 호출 방식은 다음과 같음

- 함수 호출
- 메소드 호출
- 생성자 함수 호출
- apply/call/bind 호출

```
var foo = function () {
  console.dir(this);
};

// 1. 함수 호출
foo(); // window
// window.foo();

// 2. 메소드 호출
var obj = { foo: foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
var instance = new foo(); // instance

// 4. apply/call/bind 호출
var bar = { name: 'bar' };
foo.call(bar);   // bar
foo.apply(bar);  // bar
foo.bind(bar)(); // bar
```

## 1. 함수 호출

전역 객체는 모든 객체의 유일한 최상위 객체를 의미하며 일반적으로 브라우저 사이드에서는 `window`, 서버사이드에서는 `global` 객체를 의미함

```
// in browser console
this === window // true

// in Terminal
node
this === global // true
```

전역객체는 전역 스코프를 갖는 전역변수를 프로퍼티로 소유함. 글로벌 영역에 선언한 함수는 전역객체의 프로퍼티로 접근할 수 있는 전역 변수의 메소드임

```
var ga = 'Global variable';

console.log(ga); // Global variable
console.log(window.ga); // Global variable

function foo() {
  console.log('invoked!');
}
window.foo(); // invoked!
```

기본적으로 this는 전역객체에 바인딩됨. 전역함수는 물론이고 내부함수의 경우도 this는 외부함수가 아닌 전역객체에 바인딩됨

```
function foo() {
  console.log("foo's this: ",  this);  // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

또한 메소드의 내부함수일 경우에도 this는 전역객체에 바인딩됨

```
var value = 1;

var obj = {
  value: 100,
  foo: function() {
    console.log("foo's this: ",  this);  // obj
    console.log("foo's this.value: ",  this.value); // 100
    function bar() {
      console.log("bar's this: ",  this); // window
      console.log("bar's this.value: ", this.value); // 1
    }
    bar();
  }
};

obj.foo();
```

콜백함수의 경우도 this는 전역객체에 바인딩됨

```
var value = 1;

var obj = {
  value: 100,
  foo: function() {
    setTimeout(function() {
      console.log("callback's this: ",  this);  // window
      console.log("callback's this.value: ",  this.value); // 1
    }, 100);
  }
};

obj.foo();
```

**내부함수는 일반함수, 메소드, 콜백함수 어디에서 선언되었든 관계없이 this는 전역객체를 바인딩함**
내부함수의 this가 전역객체를 참조하는 것을 회피하는 방법은 아래와 같음

```
var value = 1;

var obj = {
  value: 100,
  foo: function() {
    var that = this;  // Workaround : this === obj

    console.log("foo's this: ",  this);  // obj
    console.log("foo's this.value: ",  this.value); // 100
    function bar() {
      console.log("bar's this: ",  this); // window
      console.log("bar's this.value: ", this.value); // 1

      console.log("bar's that: ",  that); // obj
      console.log("bar's that.value: ", that.value); // 100
    }
    bar();
  }
};

obj.foo();
```

잘 이해가 안 가서 chatGPT에게 물어봄

> 내부 함수는 외부 함수에 의해 호출되는 함수이며, 일반적으로 외부 함수의 변수에 접근할 수 있습니다. 그러나 내부 함수에서 'this' 키워드를 사용하면 'this'가 전역 객체를 참조하는 문제가 발생합니다.

예를 들어, 위의 예시 코드에서 'bar' 함수 내에서 'this'를 사용하면 'this'는 전역 객체를 참조합니다. 이는 'bar' 함수가 내부 함수이기 때문에, 이 함수를 호출하는 객체가 없기 때문입니다.

하지만, 이러한 문제를 해결하는 방법 중 하나는 'that'과 같은 변수를 생성하여 내부 함수에서 사용하는 것입니다. 위의 코드에서 'foo' 함수 안에서 'that' 변수를 생성하고 'this'를 할당했습니다. 그리고 'bar' 함수에서 'that' 변수를 사용하여 원하는 값을 가져올 수 있습니다.

즉, 이 방법은 내부 함수에서 'this'가 전역 객체를 참조하는 문제를 회피하기 위해, 외부 함수에서 'this'를 다른 변수에 할당하여 사용하는 것입니다. 이를 통해 내부 함수에서 'this'가 참조하는 값이 외부 함수의 'this'와 같아지므로 원하는 값을 가져올 수 있습니다.

즉, foo 함수 안에서 변수를 만들고 this(obj)를 할당함
그리고 내부함수인 bar함수에서 변수를 사용해 원하는 값을 가져온거임

위의 방법 이외에도 자바스크립트는 this를 명시적으로 바인딩할 수 있는 apply, call, bind 메소드를 제공함

```
var value = 1;

var obj = {
  value: 100,
  foo: function() {
    console.log("foo's this: ",  this);  // obj
    console.log("foo's this.value: ",  this.value); // 100
    function bar(a, b) {
      console.log("bar's this: ",  this); // obj
      console.log("bar's this.value: ", this.value); // 100
      console.log("bar's arguments: ", arguments);
    }
    bar.apply(obj, [1, 2]);
    bar.call(obj, 1, 2);
    bar.bind(obj)(1, 2);
  }
};

obj.foo();
```

만약 apply, call, bind를 사용하지 않으면 내부함수 bar는 전역객체인 window에 바인딩될것임

## 2. 메소드 호출

함수가 객체의 프로퍼티 값이면 메소드로서 호출됨.
이떄 내부의 this는 해당 메소드를 소유한 객체, 즉 해당 메소드를 호출한 객체에 바인딩됨

```
var obj1 = {
  name: 'Lee',
  sayName: function() {
    console.log(this.name);
  }
}

var obj2 = {
  name: 'Kim'
}

obj2.sayName = obj1.sayName;

// obj2 = {
    name: 'Kim',
    sayName: function() {
    console.log(this.name);
  }
}

obj1.sayName(); //Lee
obj2.sayName(); // Kim
```

프로토타입 객체도 메소드를 가질 수 있음.
프로토타입 객체 메소드 내부에서 사용된 this도 일반 메소드 방식과 마찬가지로 해당 메소드를 호출한 객체에 바인딩됨

```
function Person(name) {
  this.name = name;
}

Person.prototype.getName = function() {
  return this.name;
}

var me = new Person('Lee');
console.log(me.getName());

Person.prototype.name = 'Kim';
console.log(Person.prototype.getName());
```

## 3. 생성자 함수 호출

자바스크립트의 생성자 함수는 말 그대로 객체를 생성하는 역할을 함.
하지만 자바와 같은 객체지향 언어는 생성자 함수와는 다르게 그 형식이 정해져있는 것이 아니라
**기존 함수에 new 연산자를 붙여서 호출하면 해당 함수는 생성자 함수로 동작함**

반대로 생각하면 생성자 함수가 아닌 일반 함수에 new 연산자를 붙여 호출하면 생성자 함수처럼 동작할 수 있음. 따라서 일반적으로 생성자 함수명은 첫문자를 대문자로 기술하여 혼란을 방지하려는 노력을 함

```
// 생성자 함수
function Person(name) {
  this.name = name;
}

var me = new Person('Lee');
console.log(me); // {name: "Lee"}

// new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수로 동작하지 않음

var you = Person('Kim');
console.log(you); // undefined
```

new 연산자와 함께 생성자 함수를 호출하면 this 바인딩이 메소드나 함수 호출 때와는 다르게 동작함

### 3.1 생성자 함수 동작 방식

new 연산자와 함께 생성자 함수를 호출하면 다음과 같은 수순으로 동작함

> **1. 빈 객체 생성 및 this 바인딩**
> 생성자 함수의 코드가 실행되기 전 빈 객체가 생성됨. 이 빈 객체가 생성자 함수가 새로 생성하는 객체임. 이후 **생성자 함수 내에서 사용되는 this는 이 빈 객체를 가리킴** 그리고 생성된 빈 객체는 생성자 함수의 프로토타입 프로퍼티가 가리키는 객체를 자신의 프로토타입 객체로 설정함

> **2. this를 통한 프로퍼티 생성**
> 생성된 빈 객체에 this를 사용하여 동적으로 프로퍼티나 메소드를 사용할 수 있음. this는 새로 생성된 객체를 가리키므로 this를 통해 생성된 프로퍼티와 메소드는 새로 생성된 객체에 추가됨

> **3. 생성된 객체 반환**

- 반환문이 없는 경우 this에 바인딩된 새로 생성한 객체가 반환됨. 명시적으로 this를 반환해도 결과는 같음
- 반환문이 this가 아닌 다른 객체를 명시적으로 반환하는 경우 this가 아닌 해당 객체가 반환됨. 이떄 this를 반환하지 않은 함수는 생성자 함수로서의 역할을 수행하지 못함. 따라서 생성자 함수는 반환문을 명시적으로 사용하지 않음

```
function Person(name) {
  // 생성자 함수 코드 실행 전 -------- 1
  this.name = name;  // --------- 2
  // 생성된 함수 반환 -------------- 3
}

var me = new Person('Lee');
console.log(me.name); // Lee
```

<div align="center">
<img src="https://poiemaweb.com/img/constructor.png" height="50%" width="50%">
</div>

### 3.2 객체 리터럴 방식과 생성자 함수 방식의 차이

객체 리터럴 방식과 생성자 함수 방식의 차이를 비교해보자

```
// 객체 리터럴 방식
var foo = {
  name: 'foo',
  gender: 'male'
}

console.dir(foo);

// 생성자 함수 방식
function Person(name, gender) {
  this.name = name;
  this.gender = gender;
}

var me  = new Person('Lee', 'male');
console.dir(me);

var you = new Person('Kim', 'female');
console.dir(you);
```

객체리터럴 방식과 생성자 함수 방식의 차이는 프로토타입 객체([[Prototype]])에 있음

- 객체 리터럴 방식의 경우 생성된 객체의 프로토타입 객체는 Object.prototype임
- 생성자 함수 방식의 경우 생성된 객체의 프로토타입 객체는 Person.prototype임

### 3.3 생성자 함수 new 연산자를 붙이지 않고 호출할 경우

**일반 함수와 생성자 함수에 특별한 형식적 차이는 없으며 함수에 new 연산자를 붙여서 호출하면 해당 함수는 생성자 함수로 동작함**

그러나 객체 생성의 목적으로 작성한 생성자 함수를 new 없이 호출하거나 일반함수에 new를 붙여 호출하면 오류가 발생할 수 있음. 일반함수와 생성자 함수의 호출시 this바인딩 방식이 다르기 때문임

일반 함수를 호출하면 this는 전역 객체에 바인딩되지만 new 연산자와 함께 생성자 함수를 호출하면 this는 생성자 함수가 암묵적으로 생성한 빈 객체에 바인딩됨

```
function Person(name) {
  // new없이 호출하는 경우, 전역객체에 name 프로퍼티를 추가
  this.name = name;
};

// 일반 함수로서 호출되었기 때문에 객체를 암묵적으로 생성하여 반환하지 않음
// 일반 함수의 this는 전역객체를 가리킴
var me = Person('Lee');

console.log(me); // undefined
console.log(window.name); // Lee
```

생성자 함수를 new 없이 호출한 경우 함수 Person 내부의 this는 전역객체를 가리키므로 name은 전역변수(window)에 바인딩됨. 또한 new와 함께 생성자 함수를 호출하는 경우에 암묵적으로 반환하던 this도 반환하지 않으며 반환문이 없으므로 undefined를 반환하게 됨

일반함수와 생성자 함수에 특별한 형식적 차이는 없기 때문에 일반적으로 생성자 함수명은 첫문자를 대문자로 기술하여 혼란을 방지하려는 노력을 한다. 그러나 이러한 규칙을 사용한다 하더라도 실수는 발생할 수 있다.

이러한 위험성을 회피하기 위해 사용되는 패턴(Scope-Safe Constructor)은 다음과 같음. 이 패턴은 대부분의 라이브러리에서 광범위하게 사용함

참고로 대부분의 빌트인 생성자(Object, Regex, Array 등)는 new 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환함

다시 말하지만 new 연산자와 함께 생성자 함수를 호출하는 경우, 생성자 함수 내부의 this는 생성자 함수에 의해 생성된 인스턴스를 가리킴. 따라서 아래 A 함수가 new 연산자와 함께 생성자 함수로써 호출되면 A 함수 내부의 this는 A 생성자 함수에 의해 생성된 인스턴스를 가리킴

```
// Scope-Safe Constructor Pattern
function A(arg) {
  // 생성자 함수가 new 연산자와 함께 호출되면 함수의 선두에서 빈객체를 생성하고 this에 바인딩함

  /*
  this가 호출된 함수(arguments.callee, 본 예제의 경우 A)의 인스턴스가 아니면 new 연산자를 사용하지 않은 것이므로 이 경우 new와 함께 생성자 함수를 호출하여 인스턴스를 반환함
  arguments.callee는 호출된 함수의 이름을 나타냄. 이 예제의 경우 A로 표기하여도 문제없이 동작하지만 특정함수의 이름과 의존성을 없애기 위해서 arguments.callee를 사용하는 것이 좋음
  */
  if (!(this instanceof arguments.callee)) {
    return new arguments.callee(arg);
  }

  // 프로퍼티 생성과 값의 할당
  this.value = arg ? arg : 0;
}

var a = new A(100);
var b = A(10);

console.log(a.value); //100
console.log(b.value); // 10
```

> callee는 arguments 객체의 프로퍼티로서 함수 바디 내에서 현재 실행 중인 함수를 참조할 때 사용함. 다시 말해, 함수 바디 내에서 현재 실행 중인 함수의 이름을 반환함

## 4. apply/call/bind 호출

this에 바인딩될 객체는 함수 호출 패턴에 의해 결정됨. 이는 자바스크립트 엔진이 수행하는 것임
이러한 자바스크립트 엔진의 암묵적 this 바인딩 이외에 this를 특정 객체에 명시적으로 바인딩하는 방법도 제공됨. 이를 가능하게 하는 것이 Function.prototype.apply, Function.prototype.call 메소드임

이 메소드들은 모든 함수 객체의 프로토타입 객체인 Function.prototype 객체의 메소드임

```
func.apply(thisArg, [argsArray])

// thisArg: 함수 내부의 this에 바인딩할 객체
// argsArray: 함수에 전달할 argument의 배열
```

기억해야할 것은 **apply() 메소드를 호출하는 주체는 함수이며 apply() 메소드는 특정 객체에 바인딩할 뿐 본직적인 기능은 함수 호출**이라는 것임

```
var Person = function (name) {
  this.name = name;
};

var foo = {};

// apply 메소드는 생성자함수 Person을 호출함. 이때 this에 객체 foo를 바인딩함
Person.apply(foo, ['name']);

console.log(foo); // { name: 'name' }
```

빈 객체 foo를 apply() 메소드 첫 번쨰 매개변수에, argument 배열을 두 번쨰 매개변수에 전달하면서 Person 함수를 호출함. 이때 Person 함수의 this는 foo 객체가 됨. Person 함수는 this의 name 프로퍼티에 매개변수 name에 할당된 인수를 할당하는데 this에 바인딩된 foo 객체는 name 프로퍼티가 없기 때문에 name 프로퍼티가 동적 추가되고 값이 할당됨

apply() 메소드의 대표적인 용도는 arguments 객체와 같은 유사 배열 객체에 배열 메소드를 사용하는 경우임.
arguments 객체는 배열이 아니기 때문에 slice()와 같은 배열의 메소드를 사용할 수 없으나 apply()메소드를 사용하면 가능함

```
function convertArgsToArray() {
  console.log(arguments);

  // arguments 객체를 배열로 변환
  // slice: 배열의 특정 부분에 대한 복사본을 생성한다.
  var arr = Array.prototype.slice.apply(arguments); // arguments.slice
  // var arr = [].slice.apply(arguments);

  console.log(arr);
  return arr;
}

convertArgsToArray(1, 2, 3);
```

<div align="center">
<img src="https://poiemaweb.com/img/apply.png" height="50%" width="50%">
</div>

`Array.prototype.slice.apply(arguments)`는 "Array.prototype.slice() 메소드를 호출하라. 단 this는 arguments 객체로 바인딩하라"는 의미가 됨. 결국 Array.prototype.slice() 메소드를 arguments 객체 자신의 메소드인 것처럼 `arguments.slice()`와 같은 형태로 호출하라는 것임.

call() 메소드의 경우 apply()와 기능은 같지만 두 번째 인자에서 배열 형태로 넘긴 것을 각각 하나의 인자로 넘김

```
Person.apply(foo, [1, 2, 3]);

Person.call(foo, 1, 2, 3);
```

apply()와 call() 메소드는 콜백 함수의 this를 위해서 사용되기도 함

```
function Person(name) {
  this.name = name;
}

Person.prototype.doSomething = function(callback) {
  if(typeof callback == 'function') {
    // --------- 1
    callback();
  }
};

function foo() {
  console.log(this.name); // --------- 2
}

var p = new Person('Lee');
p.doSomething(foo);  // undefined
```

1의 시점에서 this는 Person 객체이다. 그러나 2의 시점에서 this는 전역 객체 window를 가리킴. 콜백함수를 호출하는 외부 함수 내부의 this와 콜백함수 내부의 this가 상이하기 때문에 문맥상 문제가 발생함. 따라서 콜백함수 내부의 this를 콜백함수를 호출하는 함수 내부의 this와 일치시켜 주어야 하는 번거로움이 발생함

```
function Person(name) {
  this.name = name;
}

Person.prototype.doSomething = function (callback) {
  if (typeof callback == 'function') {
    callback.call(this);
  }
};

function foo() {
  console.log(this.name);
}

var p = new Person('Lee');
p.doSomething(foo);  // 'Lee'
```

ES5에 추가된 `Function.prototype.bind`를 사용하는 방법도 가능함. Function.prototype.bind는 함수에 인자로 전달한 this가 바인딩된 새로운 함수를 리턴함. 즉, Function.prototype.bind는 Function.prototype.apply, Function.prototype.call 메소드와 같이 함수를 실행하지 않기 때문에 명시적으로 함수를 호출할 필요가 있음

```
function Person(name) {
  this.name = name;
}

Person.prototype.doSomething = function (callback) {
  if (typeof callback == 'function') {
    // callback.call(this);
    // this가 바인딩된 새로운 함수를 호출
    callback.bind(this)();
  }
};

function foo() {
  console.log('#', this.name);
}

var p = new Person('Lee');
p.doSomething(foo);  // 'Lee'
```
