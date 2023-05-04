배열은 1개의 변수에 여러 개의 값을 순차적으로 저장할 때 사용함
자바스크립트의 배열은 객체이며 유용한 내장 메소드를 포함하고 있음
배열은 Array 생성자로 생성된 Array 타입의 객체이며 프로토타입 객체는 Array.prototype임

## 1. 배열의 생성

### 1.1 배열 리터럴

0개 이사의 갓을 쉼표로 구분하여 대괄호로 묶음. 첫 번째 인덱스 값은 '0'으로 읽을 수 있음. 존재하지 않는 요소에 접근하면 undefined를 반환함

```
const emptyArr = [];

console.log(emptyArr[1]); // undefined
console.log(emptyArr.length); // 0

const arr = [
  'zero', 'one', 'two', 'three', 'four',
  'five', 'six', 'seven', 'eight', 'nine'
];

console.log(arr[1]);      // 'one'
console.log(arr.length);  // 10
console.log(typeof arr);  // object
```

위의 배열을 객체 리터럴로 유사하게 표현하자면 아래와 같음

```
const obj = {
  '0': 'zero',  '1': 'one',   '2': 'two',
  '3': 'three', '4': 'four',  '5': 'five',
  '6': 'six',   '7': 'seven', '8': 'eight',
  '9': 'nine'
};
```

배열 리터럴은 객체 리터럴과 달리 프로퍼티명이 없고 각 요소의 값만이 존재함. 객체는 프로퍼티 값에 접급하기 위해 대괄호 표기법 또는 마침표 표기법을 사용해 프로퍼티명을 키로 사용함. 배열은 요소에 접근하기 위해 대괄호 표기법만을 사용하여 대괄호 내에 접근하고자 하는 요소의 인덱스를 넣어줌. 인덱스는 0부터 시작함

두 객체의 근본적 차이는 배열 리터럴 `arr`의 프로토타입 객체는 Array.prototype 이지만 객체 리털러 `obj`의 프로토타입 객체는 Object.prototype임

<div align="center">
<img src="https://poiemaweb.com/img/object_array_prototype.png" width="50%" height="50%">
</div>

```
const arr = [];
const obj = {};

console.dir(arr.__proto__);
console.dir(obj.__proto__);
```

<div align="center">
<img src="https://poiemaweb.com/img/object_array_prototype2.png" width="50%" height="50%">
</div>

대부분의 프로그래밍 언어에서 배열의 요소들은 모두 같은 데이터 타입이어야 하지만, 자바스크립트 배열은 어떤 데이터 조합이라도 포함할 수 있음

```
const misc = [
  'string',
  10,
  true,
  null,
  undefined,
  NaN,
  Infinity,
  ['nested array'],
  { object: true },
  function () {}
];

console.log(misc.length); // 10
```

### 1.2 Array() 생성자 함수

배열은 일반적으로 배열 리터럴 방식으로 생성하지만 배열 리터럴 방식도 결국 내장 함수 Array() 생성자 함수로 배열을 생성하는 것을 단순화 시킨 것임.
Array() 생성자 함수는 매개변수의 갯수에 따라 다르게 동작함
**매개변수가 1개이고 숫자인 경우 매개변수로 전달된 숫자를 length 값으로 가지는 빈 배열을 생성**함

```
const arr = new Array(3)

console.log(arr) // (3) [비어 있음 × 3] // 빈 값으로 채워진 길이가 3인 배열
```

그 외의 경우 매개변수로 전달된 값들을 요소로 가지는 배열을 생성함

```
const arr = new Array(1, 2, 3);
console.log(arr); // [1, 2, 3]
```

## 2. 배열 요소의 추가와 삭제

### 2.1 배열 요소의 추가

객체가 동적으로 프로퍼티를 추가할 수 있는 것처럼 배열도 동적 요솔르 추가할 수 있음. 이때 순서에 맞게 값을 할당할 필요는 없고 인덱스를 사용해 필요한 위치에 값을 할당함. 배열의 길이는 마지막 인덱스를 기준으로 산정됨

```
const arr = [];
console.log(arr[0]); // undefined

arr[1] = 8;
arr[3] = 10;

console.log(arr) // (4) [비어 있음, 8, 비어 있음, 10]
console.log(arr.length) // 4
```

값이 할당되지 않은 인덱스 위치의 요소는 생성되지 않는다는 것에 주의하자. 단 존재하지 않는 요소를 참조하면 undefined가 반환됨

```
// 값이 할당되지 않은 인덱스 위치의 요소는 생성되지 않음.
console.log(Object.keys(arr)); // [ '1', '3' ]

// arr[0]이 undefined를 반환한 이유는 존재하지 않는 프로퍼티에 접근했을 때 undefined를 반환하는 것과 같은 이치임
console.log(arr[0]); // undefined
```

### 2.2 배열 요소의 삭제

배열은 객체이기 때문에 배열의 요소를 삭제하기 위해 `delete` 연산자를 사용할 수 있음. 이때 length에는 변함없음. 해당 요소를 완전히 삭제해 length에도 반영되게 하려면 Array.prptotype.splice 메소드를 사용함

```
const numbersArr = ['zero', 'one', 'two', 'three'];

// 요소의 값만 삭제된다
delete numbersArr[2]; // (4) ["zero", "one", empty, "three"]
console.log(numbersArr); // (4) ["zero", "one", empty, "three"]
console.log(numbersArr.length) // 4

// 요소 값만이 아니라 요소를 완전히 삭제함
// splice(시작 인덱스, 삭제할 요소수)
numbersArr.splice(2, 1); // (3) ["zero", "one", "three"]
console.log(numbersArr); // (3) ["zero", "one", "three"]
console.log(numbersArr.length) // 3
```

## 3. 배열의 순회

객체의 프로퍼티를 순회할 때 for...in 문을 사용함. 배열 역시 객체이므로 for...in문을 사용할 수 있음. 그러나 배열은 객체이기 때문에 프로퍼티를 가질 수 있음.
for...in문을 사용하면 배열 요소 뿐만 아니라 불필요한 프로퍼티까지 출력될 수 있고 요소들의 순서를 보장하지 않으므로 배열을 순회하는데 적합하지 않음
따라서 배열의 순회에는 forEach메소드, for문, for...of문을 사용하는 것이 좋음

```
const arr = [0, 1, 2, 3];
arr.foo = 10;

for (const key in arr) {
  console.log('key: ' + key, 'value: ' + arr[key]);
}
// key: 0 value: 0
// key: 1 value: 1
// key: 2 value: 2
// key: 3 value: 3
// key: foo value: 10 => 불필요한 프로퍼티까지 출력

arr.forEach((item, index) => console.log(index, item));
  // 0, 0
  // 1, 1
  // 2, 2
  // 3, 3

for (let i = 0; i < arr.length; i++) {
  console.log(i, arr[i]);

  // 0, 0
  // 1, 1
  // 2, 2
  // 3, 3
}

for (const item of arr) {
  console.log(item);
}
  // 0
  // 1
  // 2
  // 3

```

## 4. Array Property

### 4.1 Array.length

length 프로퍼티는 요소의 개수(배열의 길이)를 나타냄. 배열 인덱스는 32bit 양의 정수로 처리됨. 따라서 length 프로퍼티 값은 양의 정수이며 2<sup>32</sup> - 1 (4,294,967,296 - 1) 미만임

```
const arr = [1, 2, 3, 4, 5];
console.log(arr.length); // 5

arr[4294967294] = 100;
console.log(arr.length); // 4294967295
console.log(arr); // (4294967295) [1, 2, 3, 4, 5, empty × 4294967289, 100]

arr[4294967295] = 1000;
console.log(arr.length); // 4294967295
console.log(arr); // (4294967295) [1, 2, 3, 4, 5, empty × 4294967289, 100, 4294967295: 1000]
```

주의 해야할 것은 배열 요소의 개수와 length 프로퍼티 값이 반드시 일치하지 않는다는 점임

> 배열 요소의 개수와 length 프로퍼티의 값이 일치하지 않는 배열을 희소 배열(sparse array)이라 함. 희소 배열은 배열의 요소가 연속적이지 않은 배열을 의미함. 희소 배열이 아닌 일반 배열은 배열의 요소 개수와 length 프로퍼티의 값이 언제나 일치하지만 희소 배열은 배열의 요소 개수보다 length 프로퍼티의 값이 언제나 큼. 희소 배열은 일반 배열보다 느리며 메모리를 낭비함.

현재 length 프로퍼티 값보다 더 큰 인덱스로 요소를 추가하면 새로운 요소를 추가할 수 있도록 자동으로 length 프로퍼티의 값이 늘어남. length 프로퍼티의 값은 가장 큰 인덱스에 1을 더한 것과 같음

```
const arr = [];
console.log(arr.length); // 0

arr[1000] = true;

console.log(arr);        // (1001) [empty × 1000, true]
console.log(arr.length); // 1001
console.log(arr[0]);     // undefined
```

length 프로퍼티의 값은 명시적으로 변경할 수 있음. 만약 length 프로퍼티 값을 현재보다 작게 변경하면 변경된 length 프로퍼티의 값보다 크거나 같은 인덱스에 해당하는 요소는 모두 삭제됨

```
const arr = [ 1, 2, 3, 4, 5 ];

// 배열 길이의 명시적 변경
arr.length = 3;
console.log(arr); // (3) [1, 2, 3]
```

## 5. Array Method

- ✏️가 붙은 메소드는 `this`(원본 배열)를 변경함
- 🔒가 붙은 메소드는 `this`(원본 배열)를 변경하지 않음

### 5.1 Array.isArray(arg:any): boolean

정적 메소드 Array.isArray는 주어진 인수가 배열이면 true, 아니면 false를 반환함

```
// true
Array.isArray([]);
Array.isArray([1, 2]);
Array.isArray(new Array());

// false
Array.isArray();
Array.isArray({});
Array.isArray(null);
Array.isArray(undefined);
Array.isArray(1);
Array.isArray('Array');
Array.isArray(true);
Array.isArray(false);
```

### 5.2 Array.from

ES6에서 새롭게 도입된 Array.from 메소드는 유사 배열 객체 또는 이터러블 객체를 변환하여 새로운 배열을 생성함

```
// 문자열은 이터러블임
const arr1 = Array.from('Hello');
console.log(arr1); // [ 'H', 'e', 'l', 'l', 'o' ]

// 유사 배열 객체를 새로운 배열을 변환하여 반환함
const arr2 = Array.from({ length: 2, 0: 'a', 1: 'b' });
console.log(arr2); // [ 'a', 'b' ]

// Array.from의 두번째 매개변수에게 배열의 모든 요소에 대해 호출할 함수를 전달할 수 있음
// 이 함수는 첫번째 매개변수에게 전달된 인수로 생성된 배열의 모든 요소를 인수로 전달받아 호출됨
const arr3 = Array.from({ length: 5 }, function (v, i) { return i; });
console.log(arr3); // [ 0, 1, 2, 3, 4 ]
```

### 5.3 Array.of

ES6에서 새롭게 도입된 Array.of 메소드는 전달된 인수를 요소로 갖는 배열을 생성함
Array.of는 Array 생성자 함수와 다르게 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성함

```
// 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성함
const arr1 = Array.of(1);
console.log(arr1); // [1]

const arr2 = Array.of(1, 2, 3);
console.log(arr2); // [1, 2, 3]

const arr3 = Array.of('string');
console.log(arr3); // ['string']
```

### Array.prototype.indexOf(searchElement: T, fromIndex?: number): number 🔒(`this`(원본 배열)를 변경하지 않음)

원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환함

- 중복되는 요소가 있는 경우, 첫 번째 인덱스를 반환함
- 해당하는 요소가 없는 경우, -1을 반환함

```
const arr = [1, 2, 2, 3];

// 배열 arr에서 요소 2를 검색하여 첫번째 인덱스를 반환
arr.indexOf(2);    // -> 1
// 배열 arr에서 요소 4가 없으므로 -1을 반환
arr.indexOf(4);    // -1
// 두번째 인수는 검색을 시작할 인덱스임. 두번째 인수를 생략하면 처음부터 검색함
arr.indexOf(2, 2); // 2
```

indexOf 메소드는 배열에 요소가 존재하는지 여부를 확인할 때 유용함

```
const foods = ['apple', 'banana', 'orange'];

// foods 배열에 'orange' 요소가 존재하는지 확인
if (foods.indexOf('orange') === -1) {
  // foods 배열에 'orange' 요소가 존재하지 않으면 'orange' 요소를 추가
  foods.push('orange');
}

console.log(foods); // ["apple", "banana", "orange"]
```

ES7에서 새롭게 도입된 Array.prototype.includes 메소드를 사용하면 보다 가독성이 좋음

```
const foods = ['apple', 'banana'];

// ES7: Array.prototype.includes
// foods 배열에 'orange' 요소가 존재하는지 확인
if (!foods.includes('orange')) {
  // foods 배열에 'orange' 요소가 존재하지 않으면 'orange' 요소를 추가
  foods.push('orange');
}

console.log(foods); // ["apple", "banana", "orange"]
```

### 5.5 Array.prototype.concat(…items: Array<T[] | T>): T[] 🔒(`this`(원본 배열)를 변경하지 않음)

인수로 전달된 값들(배열 또는 값)을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환함. 인수로 전달한 값이 배열인 경우, 배열을 해체하여 새로운 배열의 요소로 추가함.
**원본 배열은 변경되지 않음**

```
const arr1 = [1, 2];
const arr2 = [3, 4];

// 배열 arr2를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환
// 인수로 전달한 값이 배열인 경우, 배열을 해체하여 새로운 배열의 요소로 추가함
let result = arr1.concat(arr2);
console.log(result); // [1, 2, 3, 4]

// 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환
result = arr1.concat(3);
console.log(result); // ["1, 2, 3]

//  배열 arr2와 숫자를 원본 배열 arr1의 마지막 요소로 추가한 새로운 배열을 반환
result = arr1.concat(arr2, 5);
console.log(result); // [1, 2, 3, 4, 5]

// 원본 배열은 변경되지 않음
console.log(arr1); // [1, 2]
```

### 5.6 Array.prototype.join(separator?: string): string 🔒(`this`(원본 배열)를 변경하지 않음)

원본 배열의 모든 요소를 문자열로 변환한 후, 인수로 전달받은 값, 즉 구분자(separator)로 연결한 문자열을 반환함. 구분자는 생략가능하며 기본 구분자는 `,`임

```
const arr = [1, 2, 3, 4];

// 기본 구분자는 ','임
// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 기본 구분자 ','로 연결한 문자열을 반환
let result = arr.join();
console.log(result); // '1,2,3,4';

// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 빈문자열로 연결한 문자열을 반환
result = arr.join('');
console.log(result); // '1234'

// 원본 배열 arr의 모든 요소를 문자열로 변환한 후, 구분자 ':'로 연결한 문자열을 반환
result = arr.join(':');
console.log(result); // '1:2:3:4'
```

### 5.7 Array.prototype.push(…items: T[]): number ✏️ (`this`(원본 배열)를 변경함)

인수로 전달받은 모든 값을 원본 배열의 마지막에 요소로 추가하고 변경된 length 값을 반환함 push 메소드는 원본 배열을 직접 변경함

```
const arr = [1, 2];

// 인수로 전달받은 모든 값을 원본 배열의 마지막에 요소로 추가하고 변경된 length 값을 반환함
let result = arr.push(3, 4);
console.log(result); // 4

// push 메소드는 원본 배열을 직접 변경함.
console.log(arr); // [1, 2, 3, 4]
```

push 메소드와 concat 메소드는 유사하게 동작하지만 미묘한 차이가 있음

- push 메소드는 원본 배열을 직접 변경하지만 concat 메소드는 원본 배열을 변경하지 않고 새로운 배열을 반환함

```
const arr1 = [1, 2];
// push 메소드는 원본 배열을 직접 변경함
arr1.push(3, 4);
console.log(arr1); // [1, 2, 3, 4]

const arr2 = [1, 2];
// concat 메소드는 원본 배열을 변경하지 않고 새로운 배열을 반환함
const result = arr2.concat(3, 4);
console.log(result); // [1, 2, 3, 4]
console.log(arr2) // [1,2]
```

인수로 전달받은 값이 배열인 경우, push 메소드는 배열을 그대로 원본 배열의 마지막 요소로 추가하지만 concat 메소드는 배열을 해체하여 새로운 배열의 마지막 요소로 추가함

```
const arr1 = [1, 2];
// 인수로 전달받은 배열을 그대로 원본 배열의 마지막 요소로 추가함
arr1.push([3, 4]);
console.log(arr1); // [1, 2, [3, 4]]

const arr2 = [1, 2];
// 인수로 전달받은 배열을 해체하여 새로운 배열의 마지막 요소로 추가한다
const result = arr2.concat([3, 4]);
console.log(result); // [1, 2, 3, 4]
```

push 메소드는 성능면에서 좋지 않음. push 메소드는 배열의 마지막에 요소를 추가하므로 length 프로퍼티를 사용하여 직접 요소를 추가할 수도 있음. 이 방법이 push 메소드보다 빠름

```
const arr = [1, 2];

// arr.push(3)와 동일한 처리를 한다. 이 방법이 push 메소드보다 빠름
arr[arr.length] = 5;

console.log(arr) // [1,2,5]
```

push 메소드는 원본 배열을 직접 변경하는 부수 효과가 있음. 따라서 push 메소드보다는 ES6의 스프레드 문법을 사용하는 편이 좋음. 스프레드 문법은 나중에 정리 예정

```
const arr = [1, 2];

// ES6 spread 문법
const newArr = [...arr, 5];
// arr.push(5);

console.log(newArr); // [1, 2, 5]
```

### 5.8 Array.prototype.pop(): T | undefined ✏️ (`this`(원본 배열)를 변경함)

원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환함. 원본 배열이 빈 배열이면 undefined를 반환함. pop 매소드는 원본 배열을 직접 변경함

```
const arr = [1, 2];

// 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환함
let result = arr.pop();
console.log(result); // 2

// pop 메소드는 원본 배열을 직접 변경함
console.log(arr); // [1]
```

pop 메소드와 push 메소드를 사용하면 스택을 쉽게 구현할 수 있음
stack 은 데이터를 마지막에 밀어 넣고, 마지막에 밀어 넣은 데이터를 먼저 꺼내는 후입선출(LIFO) 방식의 자료 구조임. 스택은 언제나 가장 마지막에 밀어 넣은 최신 데이터를 취득함. 스택에 데이터를 일어 넣는 것을 push라 하고 스택에서 데이터를 꺼내 스는 것을 pop이라 함

```
// 스택 자료 구조를 구현하기 위한 배열
const stack = [];

// 스택의 가장 마지막에 데이터를 밀어 넣음
stack.push(1);
console.log(stack); // [1]

// 스택의 가장 마지막에 데이터를 밀어 넣음
stack.push(2);
console.log(stack); // [1, 2]

// 스택의 가장 마지막 데이터, 즉 가장 나중에 밀어 넣은 최신 데이터를 꺼냄
let value = stack.pop();
console.log(value, stack); // 2 [1]

// 스택의 가장 마지막 데이터, 즉 가장 나중에 밀어 넣은 최신 데이터를 꺼냄
value = stack.pop();
console.log(value, stack); // 1 []
```

### 5.9 Array.prototype.reverse(): this ✏️ (`this`(원본 배열)를 변경함)

배열 요소의 순서를 반대로 변경함. 이때 원본 배열이 변경됨. 반환값은 변경된 배열임

```
const a = ['a', 'b', 'c'];
const b = a.reverse();

// 원본 배열이 변경됨
console.log(a); // [ 'c', 'b', 'a' ]
console.log(b); // [ 'c', 'b', 'a' ]
```

### 5.10 Array.prototype.shift(): T | undefined ✏️ (`this`(원본 배열)를 변경함)

배열에서 첫 요소를 제거하고 제거한 요소를 반환함. 만약 빈 배열일 경우 undefined를 반환함. shift 메소드는 대상 배열 자체를 변경함

```
const a = ['a', 'b', 'c'];
const c = a.shift();

// 원본 배열이 변경됨
console.log(a); // a -> [ 'b', 'c' ]
console.log(c); // c -> 'a'
```

shift는 push와 함께 배열을 큐(FIFO: first in first out)처럼 동작하게 함

```
const arr = [];

arr.push(1); // [1]
arr.push(2); // [1, 2]
arr.push(3); // [1, 2, 3]

arr.shift(); // [2, 3]
arr.shift(); // [3]
arr.shift(); // []
```

Array.prototype.pop()은 마지막 요소를 제거하고 제거한 요소를 반환함

```
const a = ['a', 'b', 'c'];
const c = a.pop();

// 원본 배열이 변경됨
console.log(a); // a -> ['a', 'b']
console.log(c); // c -> 'c'
```

<div align="center">
<img src="https://poiemaweb.com/img/array-method.png" width="50%" height="50%">
</div>

### 5.11 Array.prototype.slice(start=0, end=this.length): T[] 🔒(`this`(원본 배열)를 변경하지 않음)

인자로 지정된 배열의 부분을 복사하여 반환함. 원본 배열은 변경되지 않음
첫 번째 매개변수 start에 해당하는 인덱스를 갖는 요소부터 매개변수 end에 해당하는 인덱스를 가진 요소 전까지 복사됨

- 매개변수

> **start** : 복사를 시작할 인덱스. 음수인 경우 배열의 끝에서의 인덱스를 나타냄. 예를 들어 slice(-2)는 배열의 마지막 2개의 요소를 반환함

**end** : 옵션이며 기본값은 length 값임

```
const items = ['a', 'b', 'c'];

// items[0]부터 items[1] 이전(items[1] 미포함)까지 반환
let res = items.slice(0, 1);
console.log(res);  // [ 'a' ]

// items[1]부터 items[2] 이전(items[2] 미포함)까지 반환
res = items.slice(1, 2);
console.log(res);  // [ 'b' ]

// items[1]부터 이후의 모든 요소 반환
res = items.slice(1);
console.log(res);  // [ 'b', 'c' ]

// 인자가 음수인 경우 배열의 끝에서 요소를 반환
res = items.slice(-1);
console.log(res);  // [ 'c' ]

res = items.slice(-2);
console.log(res);  // [ 'b', 'c' ]

// 모든 요소를 반환 (= 복사본(shallow copy) 생성)
res = items.slice();
console.log(res);  // [ 'a', 'b', 'c' ]

// 원본은 변경되지 않음
console.log(items); // [ 'a', 'b', 'c' ]
```

<div align="center">
<img src="https://poiemaweb.com/img/slice.png" width="50%" height="50%">
</div>

slice 메소드를 인자에 전달하지 않으면 원본 배열의 복사본을 생성해 반환함

```
const arr = [1, 2, 3];

// 원본 배열 arr의 새로운 복사본을 생성함
const copy = arr.slice();
console.log(copy) // [1, 2, 3]
console.log(copy === arr); // false
```

이때 원본 배열의 각 요소를 얕은 복사하여 새로운 복사본을 생성함

```
const todos = [
  { id: 1, content: 'HTML', completed: false },
  { id: 2, content: 'CSS', completed: true },
  { id: 3, content: 'Javascript', completed: false }
];

// shallow copy
const _todos = todos.slice();
// const _todos = [...todos];
console.log(_todos === todos); // false

// 배열의 요소는 같음. 즉, 얕은 복사됨
console.log(_todos[0] === todos[0]); // true
```

> Spread 문법과 Object.assign는 원본을 shallow copy함. Deep copy를 위해서는 lodash의 deepClone을 사용하는 것을 추천함

이를 이용하여 arguments, HTMLCollection, NodeList와 같은 유사 배열 객체(Array-like Object)를 배열로 변환할 수 있음

```
function sum() {
  // 유사 배열 객체 => Array
  const arr = Array.prototype.slice.call(arguments);
  console.log(arr); // [1, 2, 3]

  return arr.reduce(function (pre, cur) {
    return pre + cur;
  });
}

console.log(sum(1, 2, 3));
```

### 5.12 Array.prototype.splice(start: number, deleteCount=this.length-start, …items: T[]): T[] ✏️ (`this`(원본 배열)를 변경함)

기존의 배열의 요소를 제거하고 그 위치에 새로운 요소를 추가함. 배열 중간에 새로운 요소를 추가할 때도 사용됨

- 매개변수

> **start** : 배열에서의 시작위치. start만을 지정하면 배열의 start 요소부터 모든 요소를 제거함

**deleteCount** : 시작 위치부터 제거할 요소의 수임. deleteCount가 0인 경우 아무런 요소도 제거되지 않음(옵션)

**items** : 삭제한 위치에 추가될 요소들. 만약 아무런 요소도 지정하지 않을 경우 삭제만 함(옵션)

- 반환값 삭제한 요소들을 가진 배열
  이 메소드의 가장 일반적인 사용은 배열에서 요소를 삭제할 때임

```
const items1 = [1, 2, 3, 4];

// items[1]부터 2개의 요소를 제거하고 제거된 요소를 배열로 반환
const res1 = items1.splice(1, 2);

// 원본 배열이 변경됨
console.log(items1); // [ 1, 4 ]
// 제거한 요소가 배열로 반환됨
console.log(res1);   // [ 2, 3 ]

const items2 = [1, 2, 3, 4];

// items[1]부터 모든 요소를 제거하고 제거된 요소를 배열로 반환
const res2 = items2.splice(1);

// 원본 배열이 변경됨
console.log(items2); // [ 1 ]
// 제거한 요소가 배열로 반환됨
console.log(res2);   // [ 2, 3, 4 ]
```

<div align="center">
<img src="https://poiemaweb.com/img/splice-1.png" width="50%" height="50%">
</div>

배열에서 요소를 제거하고 제거한 위치에 다른 요소를 추가함

```
const items = [1, 2, 3, 4];

// items[1]부터 2개의 요소를 제거하고 그자리에 새로운 요소를 추가함. 제거된 요소가 반환됨
const res = items.splice(1, 2, 20, 30);

// 원본 배열이 변경됨
console.log(items); // [ 1, 20, 30, 4 ]
// 제거한 요소가 배열로 반환됨
console.log(res);   // [ 2, 3 ]
```

<div align="center">
<img src="https://poiemaweb.com/img/splice-2.png" width="50%" height="50%">
</div>

배열의 중간에 새로운 요소를 추가할 때도 사용됨

```
const items = [1, 2, 3, 4];

// items[1]부터 0개의 요소를 제거하고 그자리(items[1])에 새로운 요소를 추가함. 제거된 요소가 반환됨
const res = items.splice(1, 0, 100);

// 원본 배열이 변경됨
console.log(items); // [ 1, 100, 2, 3, 4 ]

// 제거한 요소가 배열로 반환됨
console.log(res);   // [ ]
```

배열 중간에 배열의 요소들을 해체하여 추가할 때도 사용됨

```
const items = [1, 4];

// items[1]부터 0개의 요소를 제거하고 그자리(items[1])에 새로운 배열를 추가함. 제거된 요소가 반환됨
// items.splice(1, 0, [2, 3]); // [ 1, [ 2, 3 ], 4 ]
Array.prototype.splice.apply(items, [1, 0].concat([2, 3]));
// ES6
// items.splice(1, 0, ...[2, 3]);

console.log(items); // [ 1, 2, 3, 4 ]
```

**slice는 배열의 일부분을 복사해서 반환하며 원본을 훼손하지 않음. splice는 배열에서 요소를 제거하고 제거한 위치에 다른 요소를 추가하며 원본을 훼손함**
