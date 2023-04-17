# 11 Immutablity

# 객체와 변경 불가성(Immutability)

Immutability(변경 불가성)는 객체가 생성된 이후 그 상태를 변경할 수 없는 디자인 패턴을 의미한다.

- Immutability은 함수형 프로그래밍의 핵심원리이다.

객체는 참조(reference) 형태로 전달하고 전달 받기 때문에 참조를 통해 공유 되어 있으면 언제든지 상태가 변경될 수 있다.

의도하지 않은 객체 변경이 발생하는 원인은 대부분 **"레퍼런스를 참조한 다른 객체에서 객체를 변경"**하기 때문이다.
해결 방법 : (배용이 들지만) 객체를 불변 객체로 마들어 프로퍼티의 변경을 방지하고 객체의 변경이 필요한 경우에는 참조가 아닌 객체의 방어적 복사(defensive copy)를 통해 새로운 객체를 생성한 후 변경한다.

- observer 패턴으로도 객체 변경에 대처가 가능하다.

불변 객체를 사용하면 복제, 비교를 위한 조작을 단순화 할 수 있고 성능 개선에 도움이 된다.

- 객체가 변경 가능한 데이터를 많이 가지고 있는 경우 부적절한 경우가 있다.

- ES6에서는 불변 데이터 패턴(immutable data pattern)을 쉽게 구현할 수 있는 기능이 추가됐다.

# immutable value VS mutable value

자바스크립트의 원시 타입(boolean, null, undefined, number, string, symbol)은 변경이 불가능한 값(immutable value)이다.
원시 타입 이외의 모든 값은 객체(object) 타입이며 객체 타입은 변경 가능한 값(mutable value)이다. (객체는 값을 다시 만들 필요없이 직접 변경이 가능하다)

- C언어와 다르게 자바스크립트의 문자열은 변경 불가능한 값(immutable value)다.
- 변경 불가능한 값을 primitaive value 라고 한다.(변경이 불가능하다는 뜻은 메모리 영역에서의 변경이 불가능하다는 뜻으로 재할당은 가능하다)

```javascript
var str = "hello";
str = "world";
```

- 메모리에 문자열 "hello"가 생성되고 식별자 str은 메모리에 생성된 문자열 "hello"의 메모리 주소를 가리킨다.
- 두번째 구문 실행 후 이전 생성된 문자열 "hello"를 수성하는 것이 아니라 새로운 문자열 "world"를 **메모리에 생성**하고 식별자 str은 이것을 가리킨다.
- 문자열 "hello"와 "world"는 모두 메모리에 존재한다.

```javascript
var statement = "i am an immutable value"; //string immutable value
var otherStr = statement.slice(8, 17);
console.log(otherStr); //'immutable'
console.log(statement); //"I am an immutable value"
```

- 2행에서 string객체의 slice()메소드는 statement 변수에 저장된 문자열을 변경하지 않고 새로운 문자열을 생성하여 반환한다. (문자열은 변경할 수 없는 immutable value 이기때문이다.)

```javascript
var arr = [];
console.log(arr.length); //0

var v2 = arr.push(2); //arr.push()는 메소드 실행 후 arr의 length를 반환한다.
console.log(arr.length); //1
```

- v2의 값은 새로운 배열 (하나의 요소를 가지고 값은 2)을 가지게 된다.
- 객체인 arr은 push 메소드에 의해 update되고 v2에는 배열의 새로운 length 값이 반환된다.

처리 후 결과의 복사본을 리턴하는 문자열의 메소드 slice()와 달리 배열(객체)의 메소드 push()는 `직접 대상 배열을 변경`한다.
-> 배열은 객체이고 객체는 immutable value가 아닌 mutable value(변경 가능한 값)이기 때문이다.

```javascript
var user = {
  name: "Lee",
  address: {
    city: "Seoul",
  },
};

var myName = user.name; //변수 myNamed은 string 타입이다.
user.name = "Kim";
console.log(myName); //Lee

myName = user.name; //재할당
console.log(myName); //Kim
```

user.name의 값을 변경했지만 myName의 값은 변경되지 않는다. 변수 myName에 user.name을 할당했을 때 user.name을 참조하는 것이 아니라 immutable한 값 "Lee"가 메모리에 새로 생성되고 myName은 이것을 참조한다.
\*user.name의 값이 변경되더라도 변수 myName이 참조하고 있는 "Lee"는 변함이 없다.

```javascript
var user1 = {
  name: "Lee",
  address: {
    city: "Seoul",
  },
};

var user2 = user1; //변수 user2는 객체 타입이다.

user2.name = "Kim";

console.log(user1.name); //Kim
console.log(user2.name); //Kim
```

- 객체 user2의 name 프로퍼티에 새로운 값을 할당하면 객체는 변경 불가능한 값이 아니므로 객체 user2는 변경된다.
- user1과 user2가 같은 어드레스를 참조하기 때문에 변경하지 않은 user1도 동시에 변경된다.

* 의도한 동작이 아니면 참조를 가지고 있는 다른 장소에 변경 사실을 통지하고 대처하는 추가 대응이 필요하다.

## Object.assign

Object.assign은 타깃 객체 소스 객체의 프로퍼티를 복사한다.

- 소스 객체의 프로퍼티와 동일한 프로퍼티를 가짓 타겟 객체의 프로퍼티들은 소스 객체의 프로퍼티로 덮어쓰디 된다.
- 리턴값으로 타깃 객체를 반환한다.
- ES6에 추가된 메소드 이다.(Internet Explorer는 지원하지 않는다.)

```javascript
//Syntax
Object.assign(target, ...sources);
```

```javascript
//Copy
const obj = { a: 1 };
const copy = Object.assign({}, obj);
console.log(copy); //{a:1}
console.log(obj === copy); //false

//merge
const o1 = { a: 1 };
const o2 = { b: 2 };
const o3 = { c: 3 };

const merge1 = Object.assig(o1, o2, o3);

console.log(merge1); //{a:1,b:2,c:3}
console.log(o1); //{a:1,b:2,c:3}, 타겟 객체가 변경된다.

//merge
const o4 = { a: 1 };
const o5 = { b: 2 };
const o6 = { c: 3 };

const merge2 = Object.assign({}, o4, o5, o6);

console.log(merge2); //{a:1,b:2,c:3}
console.log(o4); //{a:1}
```

Object.assign을 사용해 객체를 변경하지 않고 복사할 수 있다.

- Object.assign은 완전한 deep copy를 지원하지 않는다.
- 객체 내부의 객체(nested object)는 shallow copy된다.

```javascript
const usere1 = {
  name: "Lee",
  address: {
    city: "seoul",
  },
};

//새로운 빈 객체에 user1을 copy한다.
const user2 = Object.assign({}, user1);
//user1과 user2는 참조값이 다르다.
console.log(user1 === user2); //false

user2.name - "Kim";
console.log(user1.name); //Lee
console.log(user2.name); //Kim

//객체 내부의 객체(nested object)는 shallow copy된다.
console.log(user1.address === user2.address); //true

user1.address.city = "Busan";
console.log(user1.address.city); //Busan
console.log(user2.address.city); //Busan
```

- user1 객체를 빈객체에 복사하여 새로운 객체 user2를 생성했다.
- user1과 user2는 어드레스를 공유하지 않기 때문에 한 객체를 변경해도 다른 객체에 아무런 영향을 주지 않는다.

- user1객체는 const로 선언되어 재할당을 할수 없지만, 객체의 프로퍼티는 보호되지 않는다. (객체의 내용은 변경할 수 있다.)

## Object.freeze

Object.freeze()를 사용하여(immutable)객체로 만들수 있다.

```javascript
const user1 = {
  name: "Lee",
  address: {
    city: "Seoul",
  },
};

//Object.assign은 완전한 deep copy를 지원하지 않는다.
const user2 = Object.assign({}, user1, { name: "Kim" });

console.log(user1.name); //Lee
console.log(user2.name); //Kim

Object.freeze(user1);

user1.name = "Kim"; //무시된다

console.log(user1); //{name:"Lee", address:{city:"Seoul"}};

console.log(Object.isFrozen(user1)); //ture
```

- 객체 내부의 객체(nested object)는 변경 가능하다.

```javascript
const user = {
    name:"Lee",
    address:{
        city:"Seoul"
    }
};

Object.freeze(user);

user.address.city = "Busan"; //변경된다.
console.log(user); = {name:"Lee",address:{city:"Busan"}}
```

- 내부 객체까지 변경이 불가능 하게 만들기 위해서는 Deep freeze를 해야한다.

```javascript
function deepFreeze(obj){
    const props = Object.getOwnPropertyNames(obj);

    props.forEach((name)=>{
        if(typeof prop === "object" && prop !== null)}
        deepFreeze(prop);
    });
    return Object.freeze(obj);
}

const user = {
    name:"Lee",
    address:{
        city:"Seoul"
    }
};

deepFreeze(user);

user.name = "Kim"; //무시된다
user.address.city = "Busan"; //무시된다

console.log(user); //{name:"Lee", address:{city:"Seoul"}}
```

## Immutable.js

Object.assign과 Object.freeze을 사용하여 불변 객체를 만든는 방법은 성능상 이슈가 있어 큰 객체에는 사용하지 않는 것이 좋다.

- facebook이 제공하는 immutable.js를 사용하는 방법도 있다.

Immutable.js는 list, stack, map, orderedmap, set, orderedset, recoard 같은 영구 불변(premity immutable)데이터 구조를 제공한다.

`$ npm install immutable`

```javascript
//immutable.js의 Map 모듈을 임포트하여 사용한다.
const {Map} = require("immutable)
const map1 = Map({a:1,b:2,c:3})
const map2 = map1.set("b",50)
map1.get("b") //2
map2.get("b")//50
```

- map.set("b",50)의 실행에도 map1은 불변했다.
- map1.set()은 결과를 반영한 새로운 객체를 반환한다.

17/April/2023
