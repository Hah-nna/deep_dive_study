immubabillity(변경불가성)는 객체가 생성된 이후 그 상태를 변경할 수 없는 디자인 패턴을 의미함
immubabillity(변경불가성)은 함수형 프로그래밍의 핵심 원리임

객체는 참조 형태로 전달하고 받음. 객체가 참조를 통해 공유되어 있다면 그 상태가 언제든지 변경될 수 있기 때문에 문제가 될 가능성도 커지게 됨
이는 객체의 참조를 가지고 있는 어떤 장소에서 객체를 변경하면 참조를 공유하는 모든 장소에서 그 영향을 받기 때문에 이것이 의도한 동작이 아니라면 참조를 가지고 있는 다른 장소에 변경 사실을 통지하고 대처하는 추가 대응이 필요함

의도하지 않은 객체의 변경이 발생하는 원인의 대다수는 '레퍼런스를 참조한 다른 객체에서 객체를 변경'하기 때문임.
이 문제의 해결 방법은 비용은 좀 들지만 객체를 불변객체로 만들어 프로퍼티의 변경을 방지하며 객체의 변경이 필요한 경우에는 참조가 아닌 객체의 방어적 복사를 통해 새로운 객체를 생성한 후 변경함. 또는 observer 패턴으로 객체의 변경에 대처할 수도 있음

불변 객체를 사용하면 복제나 비교를 위한 조작을 단순화 할 수 있고 성능 개선에도 도움이 됨
하지만 객체가 변경 가능한 데이터르 많이 가지고 있는 경우 오히려 부적절한 경우가 있음

ES6에서는 불변 데이터 패턴을 쉽게 구현할 수 있는 새로운 기능이 추가됨

## immutable value vs mutable value

js의 원시 타입은 변경 불가능한 값(immutable value)임

- Boolean
- null
- undefined
- Number
- String
- Symbol (New in ECMAScript 6)

원시 타입 이외의 모든 값은 객체 타입이며 객체 타입은 변경 가능한 값(mutable value)임
즉, 객체는 새로운 값을 다시 만들 필요 없이 직접 변경이 가능하다는 것임

```
var str = 'Hello';
str = 'world';
```

첫 번째 구문이 실행되면 메모리에 문자열 'Hello'가 생성되고 식별자 str은 메모리에 생성된 문자열 'Hello'의 메모리 주소를 가리킴
그리고 두 번째 구문이 실행되면 이전에 생성된 문자열 'Hello'를 수정하는 것이 아니라 새로운 문자열 'world'를 메모리에 생성하고 식별자 str은 이것을 가리킴
이 때 문자열 'Hello'와 'world'는 모두 메모리에 존재함
변수 str은 문자열 'Hello'를 가리키고 있다가 문자열 'world'를 가리키도록 변경되었을 뿐임

```
var statement = 'I am an immutable value'; // string은 immutable value

var otherStr = statement.slice(8, 17);

console.log(otherStr);   // 'immutable'
console.log(statement);  // 'I am an immutable value'
```

두 번째 행에서 Stirng 객체의 slice() 메소드는 statement 변수에 저장된 문자열을 변경하는 것이 아니라 사실은 새로운 문자열을 생성하여 반환하고 있음. 그 이유는 문자열은 변경할 수 없는 immutable value이기 때문임

```
var arr = [];
console.log(arr.length); // 0

var v2 = arr.push(2);    // arr.push()는 메소드 실행 후 arr의 length를 반환
console.log(arr.length); // 1
```

위의 예제에서 v2는 새로운 배열(값이 2인 요소를 하나 가지고 있는)을 가지게 될 것임
그러나 객체인 arr은 push 메소드에 의해 업데이트 되고 v2에는 새로운 배열 length 값이 리턴됨

처리 후 결과의 복사본을 리턴하는 문자열 메소드 slice()와 달리 배열(객체) 메소드 push()는 **직접 대상 배열을 변경**함
그 이유는 객체이고 객체는 immutable value가 아닌 변경 가능한 값이기 때문임

```
var user = {
  name: 'Lee',
  address: {
    city: 'Seoul'
  }
};

var myName = user.name; // 변수 myName은 string 타입

user.name = 'Kim';
console.log(myName); // Lee

myName = user.name;  // 재할당
console.log(myName); // Kim
```

user.name의 값을 변경했지만 변수 myName 값은 변경되지 않음
이는 변수 myName에 user.name을 할당했을 때 user.name의 참조를 할당하는 것이 아니라 immutable한 값 'Lee'가 메모리에 새로 생성되고 myName은 이것을 참조하기 때문임
따라서 user.name의 값이 변경된다 하더라도 변수 myName이 참조하고 있는 'Lee'는 변함 없음

이해가 잘 안 되어서 좀 찾아봄

> myName 변수에 user.name의 값이 할당되는 순서는 다음과 같습니다.

1. user 객체가 생성됩니다. 이때 name 프로퍼티는 'Lee'로, address 프로퍼티는 객체 형태로 초기화됩니다.
2. myName 변수가 user.name 프로퍼티의 값을 참조하도록 할당됩니다. 이때 user.name의 값 'Lee'가 myName 변수에 복사되는 것이 아니라, myName 변수가 user.name 프로퍼티를 참조하게 됩니다. 이로써 myName 변수와 user.name 프로퍼티는 같은 값을 참조하게 됩니다.
3. user.name 프로퍼티의 값을 'Kim'으로 변경합니다. 이때 myName 변수가 참조하는 값은 여전히 'Lee'입니다.
4. myName 변수에 user.name 프로퍼티의 값을 다시 할당합니다. 이때 myName 변수가 참조하는 값은 이전과 달리 'Kim'입니다.

즉, myName 변수가 user.name 프로퍼티의 값을 복사하는 것이 아니라, user.name 프로퍼티의 값을 참조하게 되므로, myName 변수의 값이 user.name 프로퍼티의 값과 동일하다는 것을 명심해야 합니다.

3번이 이해가 안 가서 또 찾아봄...

> 문자열, 숫자, 불리언 등의 원시 타입(primitive type)은 참조에 의한 전달이 아니라 값에 의한 전달(call by value)입니다. 즉, 변수에 원시 타입의 값이 할당되면, 변수에는 그 값 자체가 저장됩니다.

따라서, myName 변수에 user.name 프로퍼티의 값을 할당할 때는, 객체의 참조가 아니라, 문자열 'Lee'이라는 값 자체가 할당되는 것입니다. 이때는 객체의 참조가 복사되는 것이 아니기 때문에, user.name 프로퍼티의 값이 변경되어도 myName 변수의 값은 변하지 않습니다.

```
var user1 = {
  name: 'Lee',
  address: {
    city: 'Seoul'
  }
};

var user2 = user1; // 변수 user2는 객체 타입이다.

user2.name = 'Kim';

console.log(user1.name); // Kim
console.log(user2.name); // Kim
```

위의 경우 객체 user2의 name 프로퍼티에 새로운 값을 할당하면 객체는 변경 불가능한 값이 아니므로 객체 user2는 변경됨
그런데 변경하지도 않은 객체 user1도 동시에 변경됨
이는 user1과 user2가 같은 어드레스를 참조하고 있기 때문임

의도한 동작이 아니라면 참조를 가지고 있는 다른 장소에 변경 사실을 통지하고 대처하는 추가 대응이 필요함

## 불변 데이터 패턴(immutable data pattern)

의도하지 않은 객체의 변경이 발생하는 원인의 대다수는 “레퍼런스를 참조한 다른 객체에서 객체를 변경”하기 때문임. 이 문제의 해결 방법은 비용은 조금 들지만 객체를 불변객체로 만들어 프로퍼티의 변경을 방지하며 객체의 변경이 필요한 경우에는 참조가 아닌 객체의 방어적 복사를 통해 새로운 객체를 생성한 후 변경함

정리하면 아래와 같음

- 객체의 방어적 복사(defensive copy), Object.assign
- 불변객체화를 통한 객체 변경 방지, Object.freeze

### Object.assign

Object.assign은 타킷 객체로 소스 객체의 프로퍼티를 복사함
이때 소스 객체의 프로퍼티와 동일한 프로퍼티를 가진 타켓 객체의 프로퍼티들은 소스 객체의 프로퍼티로 덮어쓰기됌
리턴값으로 타킷 객체를 반환함. ES6에서 추가된 메소드이며 Internet Explorer는 지원하지 않음

```
// Syntax
Object.assign(target, ...sources)
```

```
// Copy
const obj = { a: 1 };
const copy = Object.assign({}, obj);
console.log(copy); // { a: 1 }
console.log(obj == copy); // false

// obj와 copy는 모두 { a: 1 }이라는 객체를 참조하고 있지만, 서로 다른 참조를 가지고 있기 때문에 (obj와 copy는 서로 다른 객체 참조를 가리키고 있음) obj == copy의 비교 결과는 false가 됩니다.

만약 두 객체의 내용이 동일하더라도, 서로 다른 객체 참조를 가지고 있으면 == 비교 연산의 결과는 false가 됩니다.

// Merge
const o1 = { a: 1 };
const o2 = { b: 2 };
const o3 = { c: 3 };

const merge1 = Object.assign(o1, o2, o3);

console.log(merge1); // { a: 1, b: 2, c: 3 }
console.log(o1);     // { a: 1, b: 2, c: 3 }, 타겟 객체가 변경된다!

// Merge
const o4 = { a: 1 };
const o5 = { b: 2 };
const o6 = { c: 3 };

const merge2 = Object.assign({}, o4, o5, o6);

console.log(merge2); // { a: 1, b: 2, c: 3 }
console.log(o4);     // { a: 1 }

```

Object.assign을 사용하여 기존 객체를 변경하지 않고 객체를 복사하여 사용할 수 있음
Object.assign은 완전한 deep copy를 지원하지 않음
객체 내부의 객체는 shallow copy됨

```
const user1 = {
  name: 'Lee',
  address: {
    city: 'Seoul'
  }
};

// 새로운 빈 객체에 user1을 copy한다.
const user2 = Object.assign({}, user1);
// user1과 user2는 참조값이 다르다.
console.log(user1 === user2); // false

user2.name = 'Kim';
console.log(user1.name); // Lee
console.log(user2.name); // Kim

// 객체 내부의 객체(Nested Object)는 Shallow copy된다.
console.log(user1.address === user2.address); // true

user1.address.city = 'Busan';
console.log(user1.address.city); // Busan
console.log(user2.address.city); // Busan
```

user1 객체를 빈객체에 복사하여 새로운 객체 user2를 생성함
user1과 user2는 어드레스를 공유하지 않으므로 한 객체를 변경하여도 다른 객체에 아무런 영향을 주지 않음

주의할 것은 user1 객체는 const로 선언되어 재할당은 할 수 없지만 객체의 프로퍼티는 보호되지 않음 다시 말하자면 객체의 내용은 변경할 수 있음

### Object.freeze

Object.freeze()를 사용하여 불변(immutable) 객체로 만들수 있음

```
const user1 = {
  name: 'Lee',
  address: {
    city: 'Seoul'
  }
};

// Object.assign은 완전한 deep copy를 지원하지 않는다.
const user2 = Object.assign({}, user1, {name: 'Kim'});

console.log(user1.name); // Lee
console.log(user2.name); // Kim

Object.freeze(user1);

user1.name = 'Kim'; // 무시된다!

console.log(user1); // { name: 'Lee', address: { city: 'Seoul' } }

console.log(Object.isFrozen(user1)); // true
```

하지만 객체 내부의 객체(Nested Object)는 변경가능함

```
const user = {
  name: 'Lee',
  address: {
    city: 'Seoul'
  }
};

Object.freeze(user);

user.address.city = 'Busan'; // 변경된다!
console.log(user); // { name: 'Lee', address: { city: 'Busan' } }
```

내부 객체까지 변경 불가능하게 만들려면 Deep freeze를 하여야함

```
function deepFreeze(obj) {
  const props = Object.getOwnPropertyNames(obj);

  props.forEach((name) => {
    const prop = obj[name];
    if(typeof prop === 'object' && prop !== null) {
      deepFreeze(prop);
    }
  });
  return Object.freeze(obj);
}

const user = {
  name: 'Lee',
  address: {
    city: 'Seoul'
  }
};

deepFreeze(user);

user.name = 'Kim';           // 무시된다
user.address.city = 'Busan'; // 무시된다

console.log(user); // { name: 'Lee', address: { city: 'Seoul' } }
```

### Immutable.js

Object.assign과 Object.freeze을 사용하여 불변 객체를 만드는 방법은 번거러울 뿐더러 성능상 이슈가 있어서 큰 객체에는 사용하지 않는 것이 좋음

또 다른 대안으로 Facebook이 제공하는 Immutable.js를 사용하는 방법이 있음

Immutable.js는 List, Stack, Map, OrderedMap, Set, OrderedSet, Record와 같은 영구 불변 (Permit Immutable) 데이터 구조를 제공함

```
$ npm install immutable
```

Immutable.js의 Map 모듈을 임포트하여 사용함

```
const { Map } = require('immutable')
const map1 = Map({ a: 1, b: 2, c: 3 })
const map2 = map1.set('b', 50)
map1.get('b') // 2
map2.get('b') // 50
```

map1.set(‘b’, 50)의 실행에도 불구하고 map1은 불변함.
map1.set()은 결과를 반영한 새로운 객체를 반환함
