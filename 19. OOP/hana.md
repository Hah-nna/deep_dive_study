d## 1. 객체지향 프로그래밍 개요

많은 프로그래밍 언어는 객체지향 프로그래밍을 지원함.
하지만 객체지향이라는 개념은 명확한 정의가 없는 것이 특징임. 그래서 객체지향의 특성을 통해 이해해야 함
객체 지향 프로그래밍은 실세계에서 존재하고 인지하고 있는 객체를 소프트웨어의 세계에서 표현하기 위해 객체의 핵심적인 개념 또는 기능만을 추출하는 추상화를 통해 모델링하려는 프로그래밍 패러다임을 말함. 다시말해 우리가 주변의 실세계에서 사물을 인지하는 방식을 프로그래밍에 접목하려는 사상을 의미함

객체지향 프로그래밍은 함수들의 집합 혹은 단순한 컴퓨터의 명령어들의 목록이라는 전통적인 절차지향 프로그래밍과는 다른 관계성 있는 객체들의 집합이라는 관점으로 접근하는 소프트웨어 디자인으로 볼 수 있음

각 객체는 메세지를 받을 수도 있고 데이터를 처리할 수도 있으며 또다른 객체에게 메시지를 전달할 수도 있음. 각 객체는 별도의 역할이나 책임을 갖는 작은 독립적인 기계 또는 부품으로 볼 수 있음
객체지향 프로그래밍은 보다 유연하고 유지보수하기 쉬우며 확장성 측면에서도 유리한 프로그래밍을 하도록 의도되었고 대규모 소프트웨어 개발에 널리 사용되고 있음

## 2. 클래스 기반 vs 프로토타입 기반

### 2.1 클래스 기반 언어

클래스 기반 언어는 클래스로 객체의 자료구조와 기능을 정의하고 생성자를 통해 인스턴스를 생성함
클래스란 같은 종류의 집단에 속하는 속성과 행위를 정의한 것으로 객체지향 프로그램의 기본적인 사용자 정의 데이터형이라고 할 수 있음. 결국 클래스는 객체 생성에 사용되는 패턴 혹은 청사진일 뿐이며 new 연산자를 통한 인스턴스화 과정이 필요함

모든 인스턴스는 오직 클래스에서 정의된 범위 내에서만 작동하며 런타임에 그 구조를 변경할 수 없음. 이러한 특성은 정확성, 안정성, 예측성 측면에서 클래스 기반 언어가 프로토타입 기반 언어보다 좀 더 나은 결과를 보장함

아래의 예제는 java로 구현된 클래스임. java는 class키워드를 제공하고 이것으로 클래스를 정의함. 생성자는 클래스명과 동일하며 메소드로 구현됨

```
class Person {
  private String name;

  public Person(String name) {
    this.name = name;
  }

  public void setName(String name) {
    this.name = name;
  }

  public String getName() {
    return this.name;
  }

  public static void main(String[] args) {
    Person me = new Person("Lee");

    String name= me.getName();
    System.out.println(name); // Lee
  }
}
```

### 2.2 프로토타입 기반 언어

자바스크립트는 멀티-패러다임 언어로 명령형, 함수형, 프로토타입 기반 객체지향 언어임. 다른 객체지향 언어들과의 차이점에 논쟁이 있지만 자바스크립트는 강력한 객체지향 프로그래밍 능력들을 지니고 있음. 간혹 클래스가 없어서 객체지향이 아니라고 생각하는 사람들도 있으나 프로토타입 기반의 객체지향 언어임

자바스크립트는 클래스 개념이 없고 별도의 객체 생성 방법이 존재함

- 객체 리터럴
- Object() 생성자 함수
- 생성자 함수

```
// 객체 리터럴
var obj1 = {};
obj1.name = 'Lee';

// Object() 생성자 함수
var obj2 = new Object();
obj2.name = 'Lee';

// 생성자 함수
function F() {}
var obj3 = new F();
obj3.name = 'Lee';
```

자바스크립트는 이미 생성된 인스턴스의 자료구조와 기능을 동적으로 변경할 수 있다는 특징이 있음. 객체지향의 상속, 캡슐화(정보 은닉) 등의 개념은 프로토타입체인과 클로저 등으로 구현할 수 있음

클래스 기반의 언어에 익숙한 사람들은 이러한 프로토타입 기반의 특성으로 인해 혼란을 느낌. 자바스크립트에서는 함수 객체로 많은 것들을 할 수 있는데 클래스, 생성자, 메소드도 모두 함수로 구현 가능함

> ECMAScript 6에서 새롭게 클래스가 도입되었다. ES6의 Class는 기존 prototype 기반 객체지향 프로그래밍보다 Class 기반 언어에 익숙한 프로그래머가 보다 빠르게 학습할 수 있는 단순하고 깨끗한 새로운 문법을 제시하고 있다. ES6의 Class가 새로운 객체지향 모델을 제공하는 것이 아니며 Class도 사실 함수이고 기존 prototype 기반 패턴의 Syntactic sugar이다.

## 3. 생성자 함수와 인스턴스의 생성

자바스크립트는 생성자 함수와 new 연산자를 통해 인스턴스를 생성할 수 있음. 이때 생성자 함수는 클래스이자 생성자의 역할을 함

```
// 생성자 함수(Constructor)
function Person(name) {
  // 프로퍼티
  this.name = name;

  // 메소드
  this.setName = function (name) {
    this.name = name;
  };

  // 메소드
  this.getName = function () {
    return this.name;
  };
}

// 인스턴스의 생성
var me = new Person('Lee');
console.log(me.getName()); // Lee

// 메소드 호출
me.setName('Kim');
console.log(me.getName()); // Kim
```

위의 예시는 잘 동작하지만 문제가 많음. Person 생성자 함수로 여러 개의 인스턴스를 만들어보자

```
var me  = new Person('Lee');
var you = new Person('Kim');
var him = new Person('Choi');

console.log(me);  // Person { name: 'Lee', setName: [Function], getName: [Function] }
console.log(you); // Person { name: 'Kim', setName: [Function], getName: [Function] }
console.log(him); // Person { name: 'Choi', setName: [Function], getName: [Function] }
```

위와 같이 인스턴스를 생성하면 각각의 인스턴스에 메소드 setName, getName이 중복되어 생성됨. 즉, 각 인스턴스가 내용이 동일한 메소드를 각자 소유함.
이는 메모리 낭비인데 생성되는 인스턴스가 많아지거나 메소드가 크거나 많다면 무시할 수 없는 문제임

이런 문제를 해결하려면 다른 접근 방식이 필요하고 그 방식은 프로토타입임

## 4. 프로토타입 체인과 메소드의 정의

모든 객체는 프로토타입이라는 다른 객체를 가리키는 내부 링크를 가지고 있음. 즉, 프로토타입을 통해 직접 객체를 연결할 수 있는데 이를 프로토타입 체인이라 함

프로토타입을 이용하여 생성자 함수 내부의 메소드를 생성자 함수의 prototype 프로퍼티가 가리키는 프로토타입 객체로 이동시키면 생성자 함수에 의해 생성된 모든 인스턴스는 프로토타입 체인을 통해 객체의 메소드를 참조할 수 있음

```
function Person(name) {
  this.name = name;
}

// 프로토타입 객체에 메소드 정의
Person.prototype.setName = function (name) {
  this.name = name;
};

// 프로토타입 객체에 메소드 정의
Person.prototype.getName = function () {
  return this.name;
};

var me  = new Person('Lee');
var you = new Person('Kim');
var him = new Person('choi');

console.log(Person.prototype);
// Person { setName: [Function], getName: [Function] }

console.log(me);  // Person { name: 'Lee' }
console.log(you); // Person { name: 'Kim' }
console.log(him); // Person { name: 'choi' }
```

<div align="center>
<img src="https://poiemaweb.com/img/prototype.png" width="50%" height="50%">
</div>

Person 생성자 함수의 prototype 프로퍼티가 가리키는 프로토타입 객체로 이동시킨 setName, getName 메소드는 프로토타입 체인에 의해 모든 인스턴스가 참조할 수 있음. 프로토타입 객체는 상속할 것들이 저장되는 장소임

아래 예시는 더글라스 크락포드가 제안한 프로토타입 객체에 메소드를 추가하는 방식임

```
/**
 * 모든 생성자 함수의 프로토타입은 Function.prototype이다. 따라서 모든 생성자 함수는 Function.prototype.method()에 접근할 수 있다.
 * @method Function.prototype.method
 * @param ({string}) (name) - (메소드 이름)
 * @param ({function}) (func) - (추가할 메소드 본체)
 */
Function.prototype.method = function (name, func) {
  // 생성자함수의 프로토타입에 동일한 이름의 메소드가 없으면 생성자함수의 프로토타입에 메소드를 추가
  // this: 생성자함수
  if (!this.prototype[name]) {
    this.prototype[name] = func;
  }
};

/**
 * 생성자 함수
 */
function Person(name) {
  this.name = name;
}

/**
 * 생성자함수 Person의 프로토타입에 메소드 setName을 추가
 */
Person.method('setName', function (name) {
  this.name = name;
});

/**
 * 생성자함수 Person의 프로토타입에 메소드 getName을 추가
 */
Person.method('getName', function () {
  return this.name;
});

var me  = new Person('Lee');
var you = new Person('Kim');
var him = new Person('choi');

console.log(Person.prototype);
// Person { setName: [Function], getName: [Function] }

console.log(me);  // Person { name: 'Lee' }
console.log(you); // Person { name: 'Kim' }
console.log(him); // Person { name: 'choi' }
```

## 5. 상속

java와 같은 클래스 기반 언어에서 상속(또는 확장)은 코드 재사용의 관점에서 매우 유용함. 새롭게 정의할 클래스가 기존에 있는 클래스와 매우 유사하다면 상속을 통해 다른 점만 구현하면 됨
코드 재사용은 개발 비용을 현저히 줄일 수 있는 잠재력이 있기 때문에 매우 중요함

클래스 기반 언어에서 객체는 클래스의 인스턴스이며 클래스는 다른 클래스로 상속될 수 있음. 자바스크립트는 기본적으로 프로토타입을 통해 상속을 구현함. 이것은 프로토타입을 통해 **객체가 다른 객체로 직접 상속**된다는 의미임. 이러한 점이 자바스크립트의 약점으로 여겨지기도 하지만 프로토타입 상속 모델은 사실 클래스 기반보다 강력한 방법임

자바스크립트의 상속 구현 방식은 크게 두 가지로 구분할 수 있음
첫 번째는 클래스 기반 언어의 상속 방식을 흉내 내는 것(의사 클래스 패턴 상속. Pseudo-classical Inheritance)이고 두 번째는 프로토타입으로 상속을 구현하는 것(프로토타입 패턴 상속. Prototypal Inheritance)임

### 5.1 의사 클래스 패턴 상속

의사 클래스 패턴은 자식 생성자 함수의 prototype 프로퍼티를 부모 생성자 함수의 인스턴스로 교체하여 상속을 구현하는 방법임
부모와 자식 모두 생성자 함수를 정의하여야 함

```
// 부모 생성자 함수
var Parent = (function () {
  // Constructor
  function Parent(name) {
    this.name = name;
  }

  // method
  Parent.prototype.sayHi = function () {
    console.log('Hi! ' + this.name);
  };

  // return constructor
  return Parent;
}());

// 자식 생성자 함수
var Child = (function () {
  // Constructor
  function Child(name) {
    this.name = name;
  }

  // 자식 생성자 함수의 프로토타입 객체를 부모 생성자 함수의 인스턴스로 교체.
  Child.prototype = new Parent(); // ②

  // 메소드 오버라이드
  Child.prototype.sayHi = function () {
    console.log('안녕하세요! ' + this.name);
  };

  // sayBye 메소드는 Parent 생성자함수의 인스턴스에 위치된다
  Child.prototype.sayBye = function () {
    console.log('안녕히가세요! ' + this.name);
  };

  // return constructor
  return Child;
}());

var child = new Child('child'); // ①
console.log(child);  // Parent { name: 'child' }

console.log(Child.prototype); // Parent { name: undefined, sayHi: [Function], sayBye: [Function] }

child.sayHi();  // 안녕하세요! child
child.sayBye(); // 안녕히가세요! child

console.log(child instanceof Parent); // true
console.log(child instanceof Child);  // true
```

Child 생성자 함수가 생성한 인스턴스 child ①의 프로토타입 객체는 Parent 생성자 함수가 생성한 인스턴스(②)임. 그리고 Parent 생성자 함수가 생성한 인스턴스의 프로토타입 객체는 Parent.prototype임

이로써 child는 프로토타입 체인에 의해 Parent 생성자 함수가 생성한 인스턴스와 Parent.prototyppe의 모든 프로퍼티에 접근할 수 있게 됨. 이름은 의사 클래스 패턴 상속이지만 내부에서는 프로토타입을 사용하는 건 변함 없음

<div align="center>
<img src="https://poiemaweb.com/img/inheritance-prototype-change.png" width="50%" height="50%">
</div>

의사 클래스 패턴은 클래스 기반 언어의 상속을 흉내내어 상속을 구현함. 구동하는 것에 문제는 없지만 의사 클래스 패턴은 다음과 같은 문제를 가지고 있음

1. **new 연산자를 통해 인스턴스를 생성함**

이는 자바스크립트의 프로토타입 본질에 모순되는 것임. 프로토타입 본성에 맞게 객체에서 다른 객체로 직접 상속하는 방법을 갖는 대신 생성자 함수와 new 연산자를 통해 객체를 생성하는 불필요한 간접적인 단계가 있음. 클래스와 비슷하게 보이는 일부 복잡한 구문은 프로토타입 메커니즘을 명확히 나타내지 못하게 함

게다가 생성자 함수의 사용에는 심각한 위험이 존재함. 만약 생성자 함수를 호출할 때 new 연산자를 포함하는 것을 잊게 되면 this는 새로운 객체와 바인딩되지 않고 전역 객체에 바인딩됨.(new 연산자와 함께 호출된 생성자 함수 내부의 this는 새로 생성된 객체를 참조함)
이런 문제점을 줄이기 위해 파스칼 표시법으로 생성자 함수 이름을 표기하는 방법을 사용하지만 더 나은 대안은 new 연산자 사용을 피하는 것임

2. **생성자 링크의 파괴**

위 그림을 보면 child 객체의 프로토타입 객체는 Parent 생성자 함수가 생성한 new Parent() 객체임. 프로토타입 객체는 내부 프로퍼티로 constructor를 가지며 이는 생성자 함수를 가리킴. 하지만 의사 클래스 패턴 상속은 프로토타입 객체를 인스턴스로 교체하는 과정에서 constructor의 연결이 깨지게 됨. 즉, child 객체를 생성한 것은 Child 생성자 함수이지만 child.constructor의 출력결과는 Child 생성자 함수가 아닌 Parent 생성자 함수를 나타냄. 이는 child 객체의 프로토타입 객체인 new Parent() 객체는 constructor가 없기 때문에 프로토타입 체인에 의해 Parent.prototype의 constructor를 참조했기 때문임

```
console.log(child.constructor);  // [Function: Parent]
```

3. **객체 리터럴**

의사 클래스 패턴 상속은 기본적으로 생성자 함수를 사용하기 때문에 객체리터럴 패턴으로 생성한 객체의 상속에는 적합하지 않음. 이는 객체리터럴 패턴으로 생성한 객체의 생성자 함수는 Object()이고 이를 변경할 방법이 없기 때문임

```
var o = {};
console.log(o.__proto__ === Object.prototype); // true
```

### 5.2 프로토타입 패턴 상속

프로토타입 패턴 상속은 Object.create 함수를 사용하여 객체에서 다른 객체로 직접 상속을 구현하는 방식임. 프로토타입 패턴 상속은 개념적으로 의사 클래스 상속보다 더 간단함. 또한 의사 클래스 패턴의 단점인 new 연산자가 필요없으며 생성자 링크도 파괴되지 않으며 객체 리터럴에도 사용할 수 있음

생성자 함수를 사용한 프로토타입 패턴 상속은 아래와 같음

```
// 부모 생성자 함수
var Parent = (function () {
  // Constructor
  function Parent(name) {
    this.name = name;
  }

  // method
  Parent.prototype.sayHi = function () {
    console.log('Hi! ' + this.name);
  };

  // return constructor
  return Parent;
}());

// create 함수의 인수는 프로토타입임
var child = Object.create(Parent.prototype);
child.name = 'child';

child.sayHi();  // Hi! child

console.log(child instanceof Parent); // true
```

<div align="center>
<img src="https://poiemaweb.com/img/prototypal-inheritance1.png" width="50%" height="50%">
</div>

객체리터럴 패턴으로 생성한 객체에도 프로토타입 패턴 상속을 사용할 수 있음

```
var parent = {
  name: 'parent',
  sayHi: function() {
    console.log('Hi! ' + this.name);
  }
};

// create 함수의 인자는 객체이다.
var child = Object.create(parent);
child.name = 'child';

// var child = Object.create(parent, {name: {value: 'child'}});

parent.sayHi(); // Hi! parent
child.sayHi();  // Hi! child

console.log(parent.isPrototypeOf(child)); // true
```

<div align="center>
<img src="https://poiemaweb.com/img/prototypal-inheritance2.png" width="50%" height="50%">
</div>

Object.create 함수는 매개변수에 프로토타입으로 설정할 객체 또는 인스턴스를 전달하고 이를 상속하는 새로운 객체를 생성함. Object.create 함수는 늦게 추가되어 IE9이상에서 정상적으로 동작함. 따라서 크로스 브라우징에 주의해야함. Object.create 함수의 폴리필을 살펴보면 상속의 핵심을 이해할 수 있음

폴리필이란?
Polyfill: 특정 기능이 지원되지 않는 브라우저를 위해 사용할 수 있는 코드 조각이나 플러그인

```
// Object.create 함수의 폴리필
if (!Object.create) {
  Object.create = function (o) {
    function F() {}  // 1번
    F.prototype = o; // 2번
    return new F();  // 3번
  };
}
```

위의 코드는 Object.create를 사용할 수 없는 곳에서 Object.create와 비슷한 동작을 하는 함수를 구현한 폴리필 코드임

1. Object.create 함수의 첫 번째 인자로 전달된 객체를 프로토타입으로 하는 새로운 객체를 생성하기 위해, 빈 함수 F를 생성함

2. F.prototype 프로퍼티를 전달받은 첫 번째 인자 o로 설정함. 이렇게 함으로써, 새로운 객체는 o 객체를 프로토타입 체인의 상위 객체로 가지게 됨

3. new F()를 통해 F 함수를 생성자로 사용하여 새로운 객체를 생성하고 반환함. 이 객체는 o 객체를 프로토타입으로 가지는 객체임

## 6. 캡슐화와 모듈 패턴

캡슐화는 관련 있는 멤버 변수와 메소드를 클래스와 같은 하나의 틀 안에 담고 외부에 공개될 필요가 없는 정보는 숨기는 것을 의미함. 다른 말로는 정보 은닉(information hiding)이라 함

java의 경우 클래스를 정의하고 그 클래스를 구성하는 멤버에 대하여 public 또는 private 등으로 한정할 수 있음. public으로 선언된 메소드 또는 데이터는 외부에서 사용이 가능하며 private로 선언된 경우는 외부에서 참조할 수 없고 내부에서만 사용됨

이것은 클래스 외부에는 제한된 접근 권한을 제공하며 원하지 않는 외부의 접근에 대해 내부를 보호하는 작용을 함. 이렇게 함으로써 이들 부분이 프로그램의 다른 부분에 영향을 미치지 않고 변경될 수 있음

하지만 자바스크립트는 public 또는 private 등의 키워드를 제공하지 않음. 하지만 정보 은닉이 불가능한 것은 아님

```
var Person = function(arg) {
  var name = arg ? arg : ''; // 1번

  this.getName = function() {
    return name;
  };

  this.setName = function(arg) {
    name = arg;
  };
}

var me = new Person('Lee');

var name = me.getName();

console.log(name); // Lee

me.setName('Kim');
name = me.getName();

console.log(name); // Kim
```

1번의 name 변수는 private 변수가 됨. 자바스크립트는 function-level-scope를 제공하므로 함수 내의 변수는 외부에서 참조할 수 없음. 만약에 var 대신 this를 사용하면 public 멤버가 됨. 단 new 키워드로 객체를 생성하지 않으면 this는 생성된 객체에 바인딩되지 않고 전역 객체에 연결됨
그리고 public 메소드 getName, setName은 클로저로서 자유변수에 접근할 수 있음. 이것이 기본적인 정보 은닉 방법임

위의 예시를 정리해서 작성해보자면 다음과 같음

```
var person = function(arg) {
  var name = arg ? arg : '';

  return {
    getName: function() {
      return name;
    },
    setName: function(arg) {
      name = arg;
    }
  }
}

var me = person('Lee'); // or var me = new person('Lee');

var name = me.getName();

console.log(name);

me.setName('Kim');
name = me.getName();

console.log(name);
```

person 함수는 객체를 반환함. 이 객체 내의 메소드 getName, setName은 클로저로서 private 변수 name에 접근할 수 있음. 이러한 방식을 모듈 패턴이라 하며 캡슐화와 정보 은닉를 제공함. 많은 라이브러리에서 사용되는 유용한 패턴임

이 모듈 패턴은 다음과 같은 주의할 점이 있음

- private 멤버가 객체나 배열일 경우, 반환된 해당 멤버의 변경이 가능함

```
var person = function (personInfo) {
  var o = personInfo;

  return {
    getPersonInfo: function() {
      return o;
    }
  };
};

var me = person({ name: 'Lee', gender: 'male' });

var myInfo = me.getPersonInfo();
console.log('myInfo: ', myInfo);
// myInfo:  { name: 'Lee', gender: 'male' }

myInfo.name = 'Kim';

myInfo = me.getPersonInfo();
console.log('myInfo: ', myInfo);
// myInfo:  { name: 'Kim', gender: 'male' }
```

객체를 반환하는 경우 얕은 복사로 private 멤버의 참조값을 반환하게 됨. 따라서 외부에서도 private 멤버의 값을 변경할 수 있음. 이를 피하기 위해서는 객체를 그대로 반환하지 않고 반환해야 할 객체의 정보를 새로운 객체에 담아 반환해야 함. 반드시 객체 전체가 그대로 반환되어야 하는 경우 깊은 복사로 복사본을 만들어 반환함

- private 함수가 반환한 객체는 person 함수 객체의 프로토타입에 접근할 수 없음. 이는 상속을 구현할 수 없다는 뜻임

앞에서 살펴본 모듈 패턴은 생성자 함수가 아니며 단순히 메소드를 담은 객체를 반환함. 반환된 객체는 객체 리터럴 방식으로 생성된 객체로 함수 person의 프로토타입에 접근할 수 없음

```
var person = function(arg) {
  var name = arg ? arg : '';

  return {
    getName: function() {
      return name;
    },
    setName: function(arg) {
      name = arg;
    }
  }
}

var me = person('Lee');

console.log(person.prototype === me.__proto__); // false
console.log(me.__proto__ === Object.prototype); // true: 객체 리터럴 방식으로 생성된 객체와 동일함
```

<div align="center>
<img src="https://poiemaweb.com/img/module_pattern_1.png" width="50%" height="50%">
</div>

반환된 객체가 함수 person 프로토타입에 접근할 수 없다는 것은 부모 객체로 상속할 수 없다는 것을 의미함
함수 person을 부모 객체로 상속할 수 없다는 것은 함수 person이 반환하는 객체에 모든 메소드를 포함시켜야한다는 것을 의미함

이 문제를 해결하기 위해서는 객체를 반환하는 것이 아닌 함수를 반환해야함

```
var Person = function() {
  var name;

  var F = function(arg) { name = arg ? arg : ''; };

  F.prototype = {
    getName: function() {
      return name;
    },
    setName: function(arg) {
      name = arg;
    }
  };

  return F;
}();

var me = new Person('Lee');

console.log(Person.prototype === me.__proto__);

console.log(me.getName());
me.setName('Kim')
console.log(me.getName());
```

<div align="center>
<img src="https://poiemaweb.com/img/module_pattern_2.png" width="50%" height="50%">
</div>

캡슐화를 구현하는 패턴은 다양하며 각각의 패턴에는 장단점이 있음. 다양한 패턴의 장단점을 분석하고 파악하는 것이 보다 효율적인 코드를 작성하는데 중요함
