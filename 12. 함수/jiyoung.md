# 12 Function

# 함수

함수란 어떤 작업을 수행하기 위해 필요한 문(statement)들의 집합을 정의한 코드불록이다.

- 이름, 매개변수를 가지고 있고 필요한 때에 호출할 수 있다.

```javascript
//함수의 정의(함수 선언문)
function square(number) {
  return number(number);
}
```

- 함수는 호출에 의해 실행되며 여러번 호출 할 수 있다.

```javascript
//함수의 정의(함수 선언문)
function square(number) {
  return number * number;
}

//함수의 호출
square(2); //4
```

- 동일한 작업을 반복적으로 수행시 미리 정의 된 함수를 재 사용하는 것이 효율적이다.

- 함수의 일반적인 기능은 코드의 재사용에 목적이 있다.
- 일반적인 기능 이와에 객체 생성, 객체의 행위 정의(메소드), 정보 은닉, 클로저, 모듈화 등의 기능을 수행할 수 있다.

자바스크립트의 함수는 객체(일급 객체, First-class object)이다.

- 특징은 호출할 수 있다는 것이며, 객체이기 때문에 값으로 사용될 수 있다.
- 변수나 객체, 배열 등에 저장할 수 있고 다른 함수에 전달되는 인수로 사용 할 수 있으면 함수의 반환값이 될 수 있다.

# 함수의 정의

함수를 정의 하는 방시 3가지

- 함수 선언문
- 함수 표현식
  Function 생성자 함수

## 함수 선언문

함수 선언문\*function declaration) 방식의 함수는`function` 키워드와 함수명, 매개변수, 함수 몸체 로 구성된다.
함수명:

- 함수 선언문은 함수명을 생략할 수 없다.
- 함수 명은 함수 몸체에서 자신을 재귀적(recursive) 호출하거나, 자바스크립트 디버거가 해당 함수를 구분할 수 있는 식별자이다.
  매개변수 목록:
- 0개 이상의 목록으로 괄호로 감싸고 콤마로 분리한다.
- 다른 언어와의 차이점으로는 매개변수의 타입을 기술하지 않는다.
- 함수 몸체 내에서 매개변수의 타입 체크가 필요할 수 있다.
  함수 몸체:
- 함수가 호출되었을 때 실행되는 문들의 집합
- 중괄호 `{}`로 문들으 감싸고 `return` 문으로 결과값을 반환한다.(반환값(return value))

```javascript
//함수 선언문
function squre(number) {
  return number * number;
}
```

## 함수 표현식

자바 스크립트의 함수는 **일급 객체** 이다.
특징:

1. 무명의 리터럴로 표현이 가능하다.
2. 변수나 자료 구조(객체, 배열...)에 저장할 수 있다.
3. 함수의 파라미터로 전달 가능하다.
4. 반환값(return value)로 사용할 수 있다.

- 함수의 일급객체 특성을 이용해, 함수 리터럴 방식으로 정의하고 변수에 할당할 수 있다.

- 함수 선언문으로 정의한 함수 squre()를 함수 표현식으로 정의한 함수

```javascript
//함수 표현식
var square = function (number) {
  return number * number;
};
```

함수 표현식 방식의 함수는 함수명을 생략할 수 있다.

- 익명 함수(anonymous function)
- 함수 표현식에서는 함수명을 생략하는 것이 일반적

```javascript
//기명 함수 표현식(named function expression)
var foo = function multiply(a, b) {
  return a * b;
};

//익명 함수 표현식(anonymous functino expression)
var bar = function (a, b) {
  return a * b;
};

console.log(foo(10, 5)); //50
console.log(multiply(10, 5)); //Uncaught ReferenceError: multiply is not defined
```

- 함수는 일급 객체이기 때문에 변수에 할당할 수 있다.
- 이 변수는 함수명이 아닌 할당된 함수를 가리키는 참조값을 저장하게된다.
- 함수 호출시 함수명이 아니라 함수를 가리키는 변수명을 사용해야한다.

```javascript
var foo = function (a, b) {
  return a * b;
};

var bar = foo;

console.log(foo(10, 10)); //100
console.log(bar(10, 10)); //100
```

- 변수 bar와 foo는 동일한 익명 함수의 참조값을 갖는다.

함수가 할당된 변수를 사용해 함수를 호출하지 않고, 기명함수의 함수명을 사용해 호출하면 에러가 발생한다.

- 함수 표현식에서 사용한 함수명은 외부 코드에서 접근 불가

**함수명** 은 함수 몸체에서 자신을 **재귀적 호출(Recursive functio call)**하거나 자바스크립트 디버거가 구분할 수 있는 식별자의 역할을 한다.

- 함수 선언문으로 정의한 함수 square은 함수명으로 호출할 수 있다 (자바스크립트 엔진에 의해 함수 표현식으로 형태 변경)

```javascript
var square = function square(number) {
  return number * number;
};
```

- 함수명과 참조값을 가진 변수명이 일치해 함수명으로 호출되는 것 처럼 보이지만 변수명을 호출 된 것
  **함수 선언문도 함수 표현식과 동일하게 함수 리터럴 방식으로 정의된다.**

## Function 생성자 함수

함수 표현식으로 함수를 정의할 때 함수 리터럴 방식을 사용한다.
함수 선언문도 내부적으로 자바스크립트 엔진이 기명 함수 표현식으로 변환하기 때문에 함수 리터럴 방식을 사용한다.

- **함수 선언문과 표현식 모두 리터럴 방식으로 함수를 정의하는 것은 내장 함수 Function 생성자 함수로 함수를 생성하는 것을 단순화시킨 short-hand(축약법)이다.**
- Function.prototype.constructor 프로퍼티로 접근할 수 있다.

```javascript
new Functino(arg1, arg2, ...argN, functionBody);
```

```javascript
var square = new Function("number", "return number* number");
console.log(suare(10)); //100
```

- Function 생성자 함수는 일반적으로 사용하지 않는다.

# 함수 호이스팅

함수 정의 방식은 동작 방식에 차이가 있다.

```javascript
var res = square(5);

function square(number) {
  return number * number;
}
```

예시의 코드에서 함수 선언문으로 함수가 정의되기 전에 함수 호출이 가능하다.
함수 호이스팅은 함수 선언의 위치와 상관없이 코드 내 어느 곳에서든 호출 가능한 것을 말한다.

자바스크립트 ES6의 let, const를 포함하여 모든 선언 (var, let, const, function,class)을 호이스팅한다.

호이스팅이란 var 선언문이나, function 선언문 등 모든 선언문이 해당 scope의 선두로 옮겨진 것처럼 동작하는 특성을 말한다.

- 자바스크립트는 모든 선언문(var, let, const, function, class)이 선언되기 이전에 참조 가능하다

```javascript
//함수 표현식으로 함수를 정의
var res = square(5); //TypeError: square is not a function

var square = function (number) {
  return number * number;
};
```

함수 **표현식**은 함수 선언문과 달리 typeError가 발생한다.
**함수 표현식은 함수 호이스팅이 아닌, 변수 호이스팅이 발생한다.**

- 변수 호이스팅은 변수 생성 및 초기화와 할당 분리되어 진행한다.
- 호이스팅된 변수는 undefined로 초기화 되고 실제값의 할당은 할당문에서 이루어진다.

함수 **표현식**은 선언문과 달리 스크립트 로딩 시점에 변수 객체(VO)에 함수를 할당하지 않고 **runtime**에 해석되고 실행된다.

- 함수 표현식만 사용하는것을 권고하기도 한다.
- 함수 선언문으로 함수를 정의하면 사용하기 쉽지만 인터프리터가 많은 코드 변수를 객체에 저장하며 애플리케이션의 응답 속도가 떨어질 수 있다(주의해야한다).

# First-class object(일급 객체)

일급 객체(first-class object)란 생성, 대입, 연산, 인자 또는 반환 값으로서의 전달 등 프로그래밍 언어의 기본적 조작을 제한없이 사용할 수 있는 대상을 의미한다.

일급 객체의 조건

1. 무명의 리터럴로 표현 가능
2. 변수나 자료 구조(객체, 배열 등)에 저장 가능
3. 함수의 매개변수에 전달 가능
4. 반환값으로 사용 가능

```javascript
//1. 무명의 리터럴로 표현 가능
//2. 변수나 자료 구조에 저장 가능
var increase = function(num){
    return ++num;
};

var decrease = function(num){
    return --num;
};

var predicate = {increase, decrease};

//3. 함수의 매개변수에 전달 가능
//4. 반환값으로 사용 가능
function makeCounter(predicate){
    var num = 0;

    return function(){
        num = predicate(num);
        return num;
    };
}

var increaser = makeCounter(predicates.increase);
consoloe.log(increaser()); //1
console.log(increaser()); //2

var decreaer = makeCounter9predicates.decrease);
console.log(decreaser()); //-1
console.log(decreaser()); //-2
```

- 자바스크립트의 함수는 일급객체이기 때문에 변수와 같이 사용이 가능하며 코드의 어디에서든지 정의할 수 있다.
- 함수와 객체를 구분짓는 특징은 호출할 수 있는 것이다.

# 매개변수(parameter, 인자)

함수의 작업 실행을 위해 추가적인 정보가 필요한 경우, 매개변수를 지정한다.

- 매개변수는 함수 내에서 변수와 동일하게 동작한다

## 매개변수 (parameter, 인자) vs 인수(argument

매개변수는 함수 내에서 변수와 동일하게 메모리 공간을 확보하며 함수에 전달한 **인수**는 매개변수에 할당된다. 인수를 전달하지 않으면 매개변수는 undefinied로 초기화된다.

```javascript
var foo = function (p1, p2) {
  console.log(p1, p2);
};

foo(1); //1 undefined
```

## Call-by-value

원시 타입 인수는 call-by-value(값에 의한 호출)로 동작한다.

- 함수 호출시 원시 타입 인수를 함수에 매개변수로 전달할 때 매개변수에 **값을 복사**하여 함수로 전달한다.
- 함수 내에서 매개변수를 통해 값이 변경되어도 **전달이 완료된 원시 타입은 값이 변경되지 않는다.**

```javascript
function foo(primitive) {
  primitive += 1;
  return primitive;
}

var x = 0;

console.log(foo(x)); //1
console.log(x); //0
```

## call-by-reference

객체형(참조형) 인수는 call-by-reference(참조에 의한 호출)로 동작한다.

- 함수 호출시 참소 타입 인스룰 함수에 매개변수로 전달할 때 매개변수에 값이 복사되지 않고, 참조값이 매개변수에 저장되어 함수로 전달되는 방식
- 함수 내에서 참조값이 이용하여 객체의 값을 변경했을 때 전달되어진 참소형의 인수값도 같이 변경된다.

```javascript
function changeVal(primitive, obj) {
  primitive += 100;
  obj.name = "kim";
  obj.gender = "female";
}

var num = 100;
var obj = {
  name: "Lee",
  gender: "male",
};

console.log(num); //100
console.log(obj); //object {name:"Lee", gender:"male"}

changeVal(num, obj);

console.log(num); //100
console.log(obj); //object {name:"Kim", gender:"female"}
```

- changeVal 함수는 원시 타입과 객체 타입 인수를 전달 받아 함수 몸체에서 매개변수의 값을 변경.
- 원시 타입 인수는 값을 복사하여 매개변수에 전달하기 때문에 함수 몸체에서 그 값을 변경하여도 어떠한 부수 효과(side-effect)도 발생시키지 않는다.

- 객체형 인수는 참조값을 매개변수에 전달하기 때문에 몸체에서 값을 변경할 경우, 원본 객체가 변경되는 부수효과가 발생한다.
- 부수 효과를 발생시키는 비순수 함수(inpure function)는 복잡성을 증가시킨다.
- 비순수 함수를 최대한 줄이는 것은 부수효과를 최대한 억제하는 것과 같다. (디버깅을 쉽게 만든다.)

외부 상태를 변경하지 않는 함수를 순수함수(pure function), 외부 상태도 변경시키는 부수 효과가 발생시키는 함수를 비순수 함수라고 한다.

# 반환 값

함수는 자신을 호출한 코드에게 수행한 결과를 반환할 수 있다.

- 반환된 값을 반환값(return value)라 한다.

- `return`키워드는 함수를 호출한 코드(caller)에게 값을 반환할 때 하용한다.
- 함수는 배열 등을 이용하여 한 번에 여러개의 값을 리턴할 수 있다.
- 함수는 반환을 생략할 수 있다. (암묵적으로 undefined를 반환한다)
- 자바스크립트 해석기는 `return` 키워드를 만나면 함수의 실행을 중단한 후, 함수를 호출한 코드로 되돌아간다.
- return 키워드 이후 다른 구문이 존재하면, 그 구문은 실행되지 않는다.

```javscript
function calculatArea(width, height){
    var area = width * height;
    return area; //단일 값의 반환
}
console.log(calculateArea(3,5)); //15
console.log(cacluateArea(8,5)); //40

function getSize(width, height, depth){
    var area = width * height;
    var volume = width * height * depth;
    return [area,volume]; //복수 값의 반환
}

console.log("area is"+getSize(3,2,3)[0]); //area is 6
console.log("volume is" + getSize(3,2,3)[1]); //volume is 18
```

# 함수 객체의 프로퍼티

함수는 객체이기 때문에 프로퍼티를 가질 수 있다.

```javascript
function square(number) {
  return number * number;
}

square.x = 10;
square.y = 20;

console.log(square.x, square.y);
```

- 함수는 일반 객체와는 다른 함수만의 프로퍼티를 갖는다.

```javscript
function square(number){
    return number * number;
}
console.dir(square);
```

## arguments 프로퍼티

aurguments 객체는 함수 호출시 전달된 인수(argument)들의 정보를 담고 있는 순회가 가능한(iterable) 유사 배열 객체(array-like object)이며 함수 내부에서 지역변수처럼 사용된다.

- 함수 외부에서 사용할 수 없다.

자바스크립트는 함수 호출시 함수 정의에 따라 인수를 전달하지 않아도 에러가 발생하지 않는다.

```javascript
functino multiply(x,y){
    console.log(arguments);
    return x * y;
}

multiply(); //{}
muptiply(1); //{"0":1}
multigly(1,2); //{"0":1, "1":2}
multiply(1,2,3); //{"0":1, "1":2, "2":3}
```

매개변수(parameter)는 인수(argument)로 초기화된다.

- 매개변수의 갯수보다 인수를 적게 전달했을떄 (multiply(), multiply(1)) 인수가 전달되지 않으 매개 변수는 `undefined`로 초기화된다.
- 매개변수의 갯수보다 인수를 더 많이 전달한 경우, 초과된 인수는 무시된다.

자바스크립트의 특성 때문에, 런타임 시에 호출된 함수으 ㅣ인자 갯수를 확인하고 이에 따라 동작을 달리 정의할 필요가 있을 수 있다.

- argument 객체가 이런 상황에서 유용하게 사용된다.

arguments 객체는 매개변수 갯수가 확정되지 않은 **가변 인자 함수**를 구별할 때 유용하게 사용된다.

```javascript
function sum() {
  var res = 0;

  for (var i = 0; i < arguments.length; i++) {
    res += arguments[i];
  }
  return res;
}
consoole.log(sum()); //0
consoole.log(sum(1, 2)); //3
console.log(sum(1, 2, 3)); //6
```

자바스크립트는 함수를 호출할 때 인수들과 함께 암묵적으로 arguments 객체가 함수 내부로 전달된다.

- arguments 객체는 배열의 형태로 인자 값 정보를 담고 있지만 실제 배열이 아닌 유사배열객체(array-like object)아더,

유사배열객체란 length 프로퍼티를 가진 객체이다.

- 유사배열객체는 배열이 아니므로 배열 메소드를 사용하는 경우 에러가 발생한다.
- 배열 메소드를 사용하려면 Function.prototype.call, Function.prototype.apply를 사용해야한다.

```javascript
function sum(){
    if(!arguments.length) return 0;

    //arguments 객체를 배열로 변환
    var array= Array.prototype.slice.call(arguments);
    return array.reduce(function(pre.cur){
        return pre + cur;
    });
}

//ES6
//function sum(...args){
// if(!args.length) return 0;
// return args.reduce((pre,cur)=> pre+cur);
// }

console.log(sum(1,2,3,4,5));; //15
```

## caller 프로퍼티

caller 프로퍼티는 자신을 호출한 함수를 의미한다.

```javascript
function foo(func) {
  var res = func();
  return res;
}

function bar() {
  return "caller:" + bar.caller;
}

console.log(foo(bar)); //caller : function foo(func){...}
console.log(bar()); //null (browser에서의 실행 결화)
```

## length 프로퍼티

length 프로퍼티는 함수 정의 시 작성된 매개변수 갯수를 의미한다.

```javascript
function foo(){
    console.log(foo.length); //0
function bar(x){
    return x;
}
console.log(bar.length); //1

function baz(x,y){
    return x * y;
}
console.log(baz.length) //2
```

- arguments.length의 값과 다를 수 있기 때문에 주의해야한다.
- arguments.length는 함수 호출시 인자의 갯수이다.

## name 프로퍼티

함수명을 나타낸다.

- 기명함수의 경우 함수명을 값으로 갖고 익명함수의 경우 빈문자열을 값으로 갖는다.

```javascript
//기명 함수 표현식(named function expression)
var namedFunc = function multiply(a, b) {
  return a * b;
};
//익명 함수 표현식(anonymous function expression)
var anonymousFunc = function (a, b) {
  return a * b;
};

console.log(namedFunc.name); //multiply
console.log(anoymousFunc.name); //""
```

## **proto** 접근자 프로퍼티

모든 객체는[[prototype]]이라는 내부 슬롯이 있다.

- [[prototype]] 내부 슬롯은 프로토타입 객체를 가리킨다.
  프로토타입 객체란 프로토타입 기반 객체 지향 프로그래밍의 근자을 이루는 객체로, 객체간의 상소(inheritance)을 구현하기 위해 사용된다.
- 프로포타입 객체는 다른 객체에 공유 프로퍼티를 제공하는 객체를 말한다.

**proto** 프로퍼티는 [[prototype]] 내부 슬롯이 가리키는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티이디ㅏ.
내부 슬롯에 직접 접근할 수 없고 간접적인 접근 방법을 제공하는 경우에 한하여 접근이 가능하다.

- [[prototype]] 내부 슬롯에도 직접 접근할 수 없으며 **proto** 접근자 프로퍼티를 통해 간접적으로 프로토타입 객체에 접근할 수 있다.

```javascript
//__proto__ 접근자 프로퍼티를 통해 자신의 프로토타입 객체에 접근할 수 있다.
// 객체 리터럴로 생성한 객체으 ㅣ프로토타입 객체는 object.prototype이다.
console.log({}.__proto__ === Object.prototype); //true
```

**proto** 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 모든 객체의 프로토타입 객체인 object.prototype 객체의 프로퍼티이다.

- 모든 객체는 상속을 통해 **proto** 접근자 프로퍼티는 사용할 수 있다.

```
//객체는 __proto__ 프로퍼티를 소유하지 않는다.
console.log(Object.getOwnPropertyDescriptor({},"__proto__"));
//undefined

//__proto__프로퍼티는 모든 객체의 프로토타입 객체인 Object.prototype의 접근자 프로퍼티이다.
console.log(Object.getOwnPropertyDescriptor(Object.prototyp, "__proto__"));
//{get:f, set:f, enumerable:false, configurable:true}

//모든 객체는 Object.prototype의 접근자 프로퍼티 __proto__를 상속받아 사용할 수 있다.
console.log({}.__proto__ === Object.prototype); //true
```

- 함수도 객체이므로 **proto** 접근자 프로퍼티를 통해 프로토타입 객체에 접근 할 수 있다.

```javascript
//함수 객체의 프로토타입 객체는 Function.prototype이다.
console.log(function () {}.__proto__ == Function.prototype); //true
```

## prototype 프로퍼티

prototype 프로퍼티는 함수 객체만이 소유하는 프로퍼티이다.

- 일반 객체에는 prototype 프로퍼티가 없다.

```javascript
/함수 객체는 prototype 프로퍼티를 소유한다.
console.log(Object.getOwnPropertyDescriptor(function(){}, "prototype"));
//{value:{...},writable:true, enumerable:false, configurable:false}

//일반 객체는 prototype 프로퍼티를 소유하지 않는다.
console.log(Object.getOwnPropertyDescriptor{},"prototype"));
//undefined
```

prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 사용될 때, 생성자 함수가 생성한 인스턴스의 프로토타입 객체를 가리키다.

# 함수의 다양한 형태

## 즉시 실행 함수

함수의 정의와 동시에 실행되는 함수를 \*\*즉시실행 함수(IIFE, Immediately Invoke Function Expression)라고 한다.

- 최초 한번만 호출되며 다시 호출할 수 없다.
- 최초 한번만 실행이 필요한 초기화 처리 등에 사용할 수 있다.

```javascript
//기명 즉시 실행 함수 (named immediately-invoked function expression)
(function myFunction(){
    var a = 3;
    var b = 5;
    return a * b;
}());

//익명 즉시 실행 함수(immediately-invoked function expression)
(function(){
    var a = 3;
    var b = 5;
    return a * b;
}());

//SyntaxError:unexpected token(
    //함수 선언문은 자바스크립트 엔진에 의해 함수 몸체를 닫는 중괄호 뒤에 ;가 자동 추가된다.
    )

```
