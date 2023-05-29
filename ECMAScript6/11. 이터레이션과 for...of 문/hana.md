## 1. 이터레이션 프로토콜

ES6에서 도입된 이터레이션 프로토콜은 데이터 컬렉션을 순회하기 위한 프로토콜(미리 약속된 규칙)임.
**이터레이션 프로토콜을 준수한 객체는 for...of문으로 순회할 수 있고 spread 문법의 피연산자가 될 수 있음**

<div align="center">
<img src="https://poiemaweb.com/img/iteration-protocol.png" width="50%" height="50%">
<div>

### 1.1 이터러블

이터러블 프로토콜을 준수한 객체를 이터러블이라 함. 이터러블은 Symbol.iterator 메소드를 구현하거나 프로토타입 체인에 의해 상속한 객체를 말함. Symbol.iterator 메소드는 이터레이터를 반환한다. 이터러블은 for…of 문에서 순회할 수 있으며 Spread 문법의 대상으로 사용할 수 있음

배열은 Symbol.iterator 메소드를 소유한다. 따라서 배열은 이터러블 프로토콜을 준수한 이터러블임

```
const array = [1, 2, 3];

// 배열은 Symbol.iterator 메소드를 소유함
// 따라서 배열은 이터러블 프로토콜을 준수한 이터러블
console.log(Symbol.iterator in array); // true

// 이터러블 프로토콜을 준수한 배열은 for...of 문에서 순회 가능
for (const item of array) {
  console.log(item);
}
```

일반 객체는 Symbol.iterator 메소드를 소유하지 않음. 따라서 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아님

```
const obj = { a: 1, b: 2 };

// 일반 객체는 Symbol.iterator 메소드를 소유하지 않음
// 따라서 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아님
console.log(Symbol.iterator in obj); // false

// 이터러블이 아닌 일반 객체는 for...of 문에서 순회할 수 없음
// TypeError: obj is not iterable
for (const p of obj) {
  console.log(p);
}
```

일반 객체는 이터레이션 프로토콜을 준수하지 않기 때문에 이터러블이 아님. 따라서 일반 객체는 for ... of 문에서 순회할 수 없으며 spread 문법의 대상으로 사용할 수도 없음. 하지만 일반 객체도 이터러블 프로토콜을 준수하도록 구현하면 이터러블이 됨(커스텀 이터러블 참고)

### 1.2 이터레이터

이터레이터 프로토콜은 next 메소드를 소유하며 next 메소드를 호출하면 이터러블을 순회하며 value, done 프로퍼티를 갖는 iterator result object를 반환함. 이 규학을 준수한 객체가 이터레이터임

이터러블 프로토콜을 준수한 이터러블은 Symbol.iterator 메소드를 소유함. 이 메소드를 호출하면 이터레이터를 반환함. 이터레이터 프로토콜을 준수한 이터레이터는 next 메소드를 가짐

```
// 배열은 이터러블 프로토콜을 준수한 이터러블임
const array = [1, 2, 3];

// Symbol.iterator 메소드는 이터레이터를 반환함
const iterator = array[Symbol.iterator]();

// 이터레이터 프로토콜을 준수한 이터레이터는 next 메소드를 가짐
console.log('next' in iterator); // true
```

이터레이터의 next 메소드를 호출하면 value, done 프로퍼티를 갖는 iterator result object를 반환함.

```
// 배열은 이터러블 프로토콜을 준수한 이터러블임
const array = [1, 2, 3];

// Symbol.iterator 메소드는 이터레이터를 반환함
const iterator = array[Symbol.iterator]();

// 이터레이터 프로토콜을 준수한 이터레이터는 next 메소드를 가짐
console.log('next' in iterator); // true

// 이터레이터의 next 메소드를 호출하면 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환함
let iteratorResult = iterator.next();
console.log(iteratorResult); // {value: 1, done: false}
```

이터레이터의 next 메소드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 함. next 메소드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 iterator result object를 반환함

```
// 배열은 이터러블 프로토콜을 준수한 이터러블이다.
const array = [1, 2, 3];

// Symbol.iterator 메소드는 이터레이터를 반환한다.
const iterator = array[Symbol.iterator]();

// 이터레이터 프로토콜을 준수한 이터레이터는 next 메소드를 갖는다.
console.log('next' in iterator); // true

// 이터레이터의 next 메소드를 호출하면 value, done 프로퍼티를 갖는 이터레이터 리절트 객체를 반환함
// next 메소드를 호출할 때 마다 이터러블을 순회하며 iterator result object를 반환한다.

console.log(iterator.next()); // {value: 1, done: false}
console.log(iterator.next()); // {value: 2, done: false}
console.log(iterator.next()); // {value: 3, done: false}
console.log(iterator.next()); // {value: undefined, done: true}
```

이터레이터의 next 메소드가 반환하는 iterator result object의 value 프로퍼티는 현재 순회 중인 이터러블의 값을 반환하고 done 프로퍼티는 이터러블의 순회 완료 여부를 반환함

### 1.3 빌트인 이터러블

ES6에서 제공하는 빌트인 이터러블은 다음과 같음

> Array, String, Map, Set, TypedArray(Int8Array, Uint8Array, Uint8ClampedArray, Int16Array, Uint16Array, Int32Array, Uint32Array, Float32Array, Float64Array), DOM data structure(NodeList, HTMLCollection), Arguments

```
// 배열은 이터러블임
const array = [1, 2, 3];

// 이터러블은 Symbol.iterator 메소드를 소유힘
// Symbol.iterator 메소드는 이터레이터를 반환힘
let iter = array[Symbol.iterator]();

// 이터레이터는 next 메소드를 소유함
// next 메소드는 iterator result object를 반환함
console.log(iter.next()); // {value: 1, done: false}
console.log(iter.next()); // {value: 2, done: false}
console.log(iter.next()); // {value: 3, done: false}
console.log(iter.next()); // {value: undefined, done: true}

// 이터러블은 for...of 문으로 순회 가능함
for (const item of array) {
  console.log(item);
}

// 문자열은 이터러블임
const string = 'hi';

// 이터러블은 Symbol.iterator 메소드를 소유함
// Symbol.iterator 메소드는 이터레이터를 반환함
iter = string[Symbol.iterator]();

// 이터레이터는 next 메소드를 소유함
// next 메소드는 iterator result object를 반환함
console.log(iter.next()); // {value: "h", done: false}
console.log(iter.next()); // {value: "i", done: false}
console.log(iter.next()); // {value: undefined, done: true}

// 이터러블은 for...of 문으로 순회 가능함
for (const letter of string) {
  console.log(letter);
}

// arguments 객체는 이터러블임
(function () {
  // 이터러블은 Symbol.iterator 메소드를 소유함
  // Symbol.iterator 메소드는 이터레이터를 반환함
  iter = arguments[Symbol.iterator]();

  // 이터레이터는 next 메소드를 소유함
  // next 메소드는 이터레이터 리절트 객체를 반환함
  console.log(iter.next()); // {value: 1, done: false}
  console.log(iter.next()); // {value: 2, done: false}
  console.log(iter.next()); // {value: undefined, done: true}

  // 이터러블은 for...of 문으로 순회 가능함
  for (const arg of arguments) {
    console.log(arg);
  }
}(1, 2));
```

### 1.4 이터레이션 프로토콜의 필요성

데이터 소비자(Data consumer)인 for…of 문, spread 문법 등은 아래와 같이 다양한 데이터 소스를 사용함

> Array, String, Map, Set, TypedArray(Int8Array, Uint8Array, Uint8ClampedArray, Int16Array, Uint16Array, Int32Array, Uint32Array, Float32Array, Float64Array), DOM data structure(NodeList, HTMLCollection), Arguments

위 데이터 소스는 모두 이터레이션 프로토콜을 준수하는 이터러블임. 즉, 이터러블은 데이터 공급자의 역할(Data provider)을 함

만약 이처럼 다양한 데이터 소스가 각자의 순회 방식을 갖는다면 데이터 소비자는 다양한 데이터 소스의 순회 방식을 모두 지원해야 함. 이는 효율적이지 않음. 하지만 다양한 데이터 소스가 이터레이션 프로토콜을 준수하도록 규정하면 데이터 소비자는 이터레이션 프로토콜만을 지원하도록 구현하면 됨

즉, 이터레이션 프로토콜은 다양한 데이터 소스가 하나의 순회 방식을 갖도록 규정하여 데이터 소비자가 효율적으로 다양한 데이터 소스를 사용할 수 있도록 데이터 소비자와 데이터 소스를 연결하는 인터페이스의 역할을 함

<div align="center">
<img src="https://poiemaweb.com/img/iteration-protocol-interface.png" width="50%" height="50%">
<div>

이터러블을 지원하는 데이터 소비자는 내부에서 Symbol.iterator 메소드를 호출해 이터레이터를 생성하고 이터레리터의 next 메소드를 호출하여 이터러블을 순회함. 그리고 next 메소드가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 취득함

## 2. for...of 문

for...of 문은 내부적으로 이터레이터의 next 메소드를 호출하여 이터러블을 순회하며 next 메소드가 반환한 iterator result object의 value 프로퍼티 값을 for...of문의 변수에 할당함. 그리고
iterator result object의 done 프로퍼티 값이 false이면 이터러블의 순회를 계속하고 true이면 이터러블의 순회를 중단함

```
// 배열
for (const item of ['a', 'b', 'c']) {
  console.log(item);
}

// 문자열
for (const letter of 'abc') {
  console.log(letter);
}

// Map
for (const [key, value] of new Map([['a', '1'], ['b', '2'], ['c', '3']])) {
  console.log(`key : ${key} value : ${value}`); // key : a value : 1 ...
}

// Set
for (const val of new Set([1, 2, 3])) {
  console.log(val);
}
```

for…of 문이 내부적으로 동작하는 것을 for 문으로 표현하면 아래와 같음

```
// 이터러블
const iterable = [1, 2, 3];

// 이터레이터
const iterator = iterable[Symbol.iterator]();



// for(;;)는 무한 루프(infinite loop)를 나타내는 표현
// for(;;)는 조건식이 항상 참(true)으로 평가되어 무한히 반복되는 루프를 생성함. 루프 내에서 break 문을 사용하거나 예외를 던지지 않는 한 무한 루프는 계속 실행됨
for (;;) {

  // 이터레이터의 next 메소드를 호출하여 이터러블을 순회
  const res = iterator.next();

  // next 메소드가 반환하는 이터레이터 리절트 객체의 done 프로퍼티가 true가 될 때까지 반복
  if (res.done) break;

  console.log(res);
}
```

## 3. 커스텀 이터러블

### 3.1 커스텀 이터러블 구현

일반 객체는 이터러블이 아님. 일반 객체는 Symbol.iterator 메소드를 소유하지 않음. 즉, 일반 객체는 이터러블 프로토콜을 준수하지 않으므로 for…of 문으로 순회할 수 없음

```
// 일반 객체는 이터러블이 아님
const obj = { a: 1, b: 2 };

// 일반 객체는 Symbol.iterator 메소드를 소유하지 않음
// 따라서 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아님
console.log(Symbol.iterator in obj); // false

// 이터러블이 아닌 일반 객체는 for...of 문에서 순회할 수 없음
// TypeError: obj is not iterable
for (const p of obj) {
  console.log(p);
}
```

하지만 일반 객체가 이터레이션 프로토콜을 준수하도록 구현하면 이터러블이 됨. 아래는 피보나치 수열(1, 2, 3, 5…)을 구현한 간단한 이터러블 예시임

```
const fibonacci = {
  // Symbol.iterator 메소드를 구현하여 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let [pre, cur] = [0, 1];
    // 최대값
    const max = 10;

    // Symbol.iterator 메소드는 next 메소드를 소유한 이터레이터를 반환해야 함
    // next 메소드는 이터레이터 리절트 객체를 반환
    return {
      // fibonacci 객체를 순회할 때마다 next 메소드가 호출됨
      next() {
        [pre, cur] = [cur, pre + cur];
        return {
          value: cur,
          done: cur >= max
        };
      }
    };
  }
};

// 이터러블의 최대값을 외부에서 전달할 수 없음
for (const num of fibonacci) {
  // for...of 내부에서 break는 가능함
  // if (num >= 10) break;
  console.log(num); // 1 2 3 5 8
}
```

Symbol.iterator 메소드는 next 메소드를 갖는 이터레이터를 반환하여야 함. 그리고 next 메소드는 done과 value 프로퍼티를 가지는 iterator result object를 반환함. for…of 문은 done 프로퍼티가 true가 될 때까지 반복하며 done 프로퍼티가 true가 되면 반복을 중지함

이터러블은 for…of 문뿐만 아니라 spread 문법, 디스트럭쳐링 할당, Map과 Set의 생성자에도 사용됨

```
// spread 문법과 디스트럭처링을 사용하면 이터러블을 손쉽게 배열로 변환할 수 있음
// spread 문법
const arr = [...fibonacci];
console.log(arr); // [ 1, 2, 3, 5, 8 ]

// 디스트럭처링
const [first, second, ...rest] = fibonacci;
console.log(first, second, rest); // 1 2 [ 3, 5, 8 ]
```

### 3.2 이터러블을 생성하는 함수

위 fibonacci 이터러블에는 외부에서 값을 전달할 방법이 없다는 아쉬운 점이 있음. fibonacci 이터러블의 최대값을 외부에서 전달할 수 있도록 수정해 보자. 이터러블의 최대 순회수를 전달받아 이터러블을 반환하는 함수를 만들면 됨

```
// 이터러블을 반환하는 함수
const fibonacciFunc = function (max) {
  let [pre, cur] = [0, 1];

  return {
    // Symbol.iterator 메소드를 구현하여 이터러블 프로토콜을 준수
    [Symbol.iterator]() {
      // Symbol.iterator 메소드는 next 메소드를 소유한 이터레이터를 반환해야 함
      // next 메소드는 이터레이터 리절트 객체를 반환
      return {
        // fibonacci 객체를 순회할 때마다 next 메소드가 호출된다.
        next() {
          [pre, cur] = [cur, pre + cur];
          return {
            value: cur,
            done: cur >= max
          };
        }
      };
    }
  };
};

// 이터러블을 반환하는 함수에 이터러블의 최대값을 전달함
for (const num of fibonacciFunc(10)) {
  console.log(num); // 1 2 3 5 8
}
```

### 3.3 이터러블이면서 이터레이터인 객체를 생성하는 함수

이터레이터를 생성하려면 이터러블의 Symbol.iterator 메소드를 호출해야 함. 이터러블이면서 이터레이터인 객체를 생성하면 Symbol.iterator 메소드를 호출하지 않아도 됨

```
// 이터러블이면서 이터레이터인 객체를 반환하는 함수
const fibonacciFunc = function (max) {
  let [pre, cur] = [0, 1];

  // Symbol.iterator 메소드와 next 메소드를 소유한
  // 이터러블이면서 이터레이터인 객체를 반환
  return {
    // Symbol.iterator 메소드
    [Symbol.iterator]() {
      return this;
    },
    // next 메소드는 이터레이터 리절트 객체를 반환
    next() {
      [pre, cur] = [cur, pre + cur];
      return {
        value: cur,
        done: cur >= max
      };
    }
  };
};

// iter는 이터러블이면서 이터레이터임
let iter = fibonacciFunc(10);

// iter는 이터레이터임
console.log(iter.next()); // {value: 1, done: false}
console.log(iter.next()); // {value: 2, done: false}
console.log(iter.next()); // {value: 3, done: false}
console.log(iter.next()); // {value: 5, done: false}
console.log(iter.next()); // {value: 8, done: false}
console.log(iter.next()); // {value: 13, done: true}

iter = fibonacciFunc(10);

// iter는 이터러블임
for (const num of iter) {
  console.log(num); // 1 2 3 5 8
}
```

아래의 객체는 Symbol.iterator 메소드와 next 메소드를 소유한 이터러블이면서 이터레이터임. Symbol.iterator 메소드는 this를 반환하므로 next 메소드를 갖는 이터레이터를 반환함

### 3.4 무한 이터러블과 Lazy evaluation(지연평가)

무한 이터러블을 생성하는 함수를 정의해보자. 이를 통해 무한 수열을 간단히 표현할 수 있음

```
// 무한 이터러블을 생성하는 함수
const fibonacciFunc = function () {
  let [pre, cur] = [0, 1];

  return {
    [Symbol.iterator]() {
      return this;
    },
    next() {
      [pre, cur] = [cur, pre + cur];
      // done 프로퍼티를 생략함
      return { value: cur };
    }
  };
};

// fibonacciFunc 함수는 무한 이터러블을 생성함
for (const num of fibonacciFunc()) {
  if (num > 10000) break;
  console.log(num); // 1 2 3 5 8...
}

// 무한 이터러블에서 3개만을 취득함
const [f1, f2, f3] = fibonacciFunc();
console.log(f1, f2, f3); // 1 2 3
```

이터레이션 프로토콜의 필요성에서 살펴보았듯이 이터러블은 데이터 공급자의 역할을 함. 배열, 문자열, Map, Set 등의 빌트인 이터러블은 데이터를 모두 메모리에 확보한 다음 동작함. 하지만 이터러블은 Lazy evaluation(지연 평가)를 통해 값을 생성함 Lazy evaluation은 평가 결과가 필요할 때까지 평가를 늦추는 기법임. 위 예제를 통해 Lazy evaluation에 대해 좀 더 자세히 살펴보자.

위 예제의 fibonacciFunc 함수는 무한 이터러블을 생성함. 하지만 fibonacciFunc 함수가 생성한 무한 이터러블은 데이터를 공급하는 메커니즘을 구현한 것으로 데이터 소비자인 for…of 문이나 디스트럭처링 할당이 실행되기 이전까지 데이터를 생성하지는 않음. for…of 문의 경우, 이터러블을 순회할 때 내부에서 이터레이터의 next 메소드를 호출하는데 바로 이때 데이터가 생성됨. next 메소드가 호출되기 이전까지는 데이터를 생성하지 않음. 즉, 데이터가 필요할 때까지 데이터의 생성을 지연하다가 데이터가 필요한 순간 데이터를 생성함
