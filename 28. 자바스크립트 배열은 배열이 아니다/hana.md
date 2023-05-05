<div align="center">
<img src="https://poiemaweb.com/assets/fs-images/27-1.png" width="50%" height="50%">
</div>

일반적으로 배열이라는 자료 구조의 개념은 동일한 크기의 메모리 공간이 빈틈없이 연속적으로 나열된 자료 구조를 말함. 즉, 배열의 요소는 하나의 타입으로 통일되어 있으며 서로 연속적으로 인접해있음. 이러한 배열을 **밀집 배열(dense array)**이라 함

이처럼 배열의 요소는 동일한 크기를 가지며 빈틈없이 연속적으로 이어져 있으므로 아래와 같이 인덱스를 통해 단 한 번의 연산으로 임의의 요소에 접근(임의의 접근(random access), 시간 복잡도O(1))할 수 있음. 이는 매우 효율적이고 고속으로 동작함

> 검색 대상 요소의 메모리 주소 = 배열의 시작 메모리 주소 + 인덱스 \* 요소의 바이트 수

예를 들어 맨 처음에 나온 예시 이미지처럼 메모리 주소 1000에서 시작하고 각 요소의 크기가 8바이트인 배열을 생각해보자

- 인덱스가 0인 요소의 메모리 주소 : 1000 + 0 \* 8 = 1000
- 인덱스가 1인 요소의 메모리 주소 : 1000 + 1 \* 8 = 1008
- 인덱스가 2인 요소의 메모리 주소 : 1000 + 2 \* 8 = 1016

이처럼 배열은 인덱스를 통해 효율적으로 요소에 접근할 수 있다는 장점이 있음. 하지만 정렬되지 않은 배열에서 특정한 값을 탐색하는 경우 모든 배열 요소를 처음부터 값을 발견할 때까지 차례대로 탐색(선형탐색, 시간 복잡도O(n))해야함

```
// 선형 검색을 통해 주어진 배열(array)에 주어진 값(target)이 요소로 존재하는지 확인하여
// 존재하는 경우 해당 인덱스를 반환하고 존재하지 않는 경우 -1을 반환하는 함수
function linearSearch(array, target) {
  const length = array.length;

  for (let i = 0; i < length; i++) {
    if (array[i] === target) return i;
  }

  return -1;
}

console.log(linearSearch([1, 2, 3, 4, 5, 6], 3)); // 2
console.log(linearSearch([1, 2, 3, 4, 5, 6], 0)); // -1
```

또한 배열에 요소를 삽입하거나 삭제하는 경우 배열 요소를 연속적으로 유지하기 위해 요소를 이동시켜야하는 단점도 있음

<div align="center">
<img src="https://poiemaweb.com/assets/fs-images/27-2.png" width="50%" height="50%">
</div>

자바스크립트의 배열은 지금까지 살펴본 일반적인 의미의 배열과는 다름. 즉, 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 갖지 않아도 되며 연속적으로 이어져 있지 않을 수도 있음. 배열의 요소가 연속적으로 이어져 있지 않는 배열을 **희소 배열(sparse array)**이라 함

이처럼 자바스크립트의 배열은 일반적 의미의 배열이 아님. 자바스크립트 배열은 일반적인 배열의 동작을 흉내낸 특수한 객체임.

```
console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));
/*
{
  '0': { value: 1, writable: true, enumerable: true, configurable: true },
  '1': { value: 2, writable: true, enumerable: true, configurable: true },
  '2': { value: 3, writable: true, enumerable: true, configurable: true },
  length: { value: 3, writable: true, enumerable: false, configurable: false }
}
*/
```

위의 예시처럼 자바스크립트 배열은 인덱스를 프로퍼티 키로 가지며 length 프로퍼티를 갖는 특수한 객체임. 자바스크립트 배열의 요소는 사실 프로퍼티 값임. 자바스크립트에서 사용할 수 있는 모든 값은 객체의 프로퍼티 값이 될 수 있으므로 어떤 타입의 값이라도 배열의 요소가 될 수 있음

```
const arr = [
  'string',
  10,
  true,
  null,
  undefined,
  NaN,
  Infinity,
  [ ],
  { },
  function () {}
];
```

일반적인 배열과 자바스크립트 배열의 장단점을 정리해보면 아래와 같음

- 일반적인 배열은 인덱스로 배열 요소에 빠르게 접근할 수 있음. 하지만 특정 요소를 탐색하거나 요소를 삽입 또는 삭제하는 경우에는 효율적이지 않음
- 자바스크립트 배열은 해시 테이블로 구현된 객체이므로 인덱스로 배열 요소에 접근하는 경우 일반적인 배열보다 성능면에서 느릴 수 밖에 없는 구조적인 단점을 가짐. 하지만 특정 요소를 탐색하거나 요소를 삽입 또는 삭제하는 경우에는 일반적인 배열보다 빠른 성능을 기대할 수 있음

즉 자바스크립트 배열은 인덱스로 배열 요소에 접근하는 경우에는 일반적인 배열보다 느리지만 특정 요소를 탐색하거나 요소를 삽입 또는 삭제하는 경우에는 일반적인 배열보다 빠름. 자바스크립트 배열은 인덱스로 접근하는 경우의 성능 대신 특정 요소를 탐색하거나 배열 요소를 삽입 또는 삭제하는 경우의 성능을 택한 것임

이처럼 인덱스로 배열 요소에 접근할 때 일반적인 배열보다 느릴 수 밖에 없는 구조적인 단점을 보완하기 위해 대부분의 모던 자바스크립트 엔진은 배열을 일반 객체와 구분하여 보다 배열처럼 동작하도록 최적화하여 구현함

아래와 같이 배열과 일반 객체의 성능을 테스트 해보면 배열이 일반 객체보다 약 2배 정도 빠름

```
const arr = [];

console.time('Array Performance Test');

for (let i = 0; i < 10000000; i++) {
  arr[i] = i;
}
console.timeEnd('Array Performance Test');
// Array Performance Test: 244.06298828125 ms -> 0.24406초

const obj = {};

console.time('Object Performance Test');

for (let i = 0; i < 10000000; i++) {
  obj[i] = i;
}

console.timeEnd('Object Performance Test');
//  Object Performance Test: 425.530029296875 ms -> 0.42553초
```
