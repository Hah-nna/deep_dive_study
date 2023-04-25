제어문(Control flow statement)은 주어진 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문)할 때 사용한다.
일반적으로 코드는 위에서 아래 방향으로 순차적으로 실행된다. 제어문은 코드의 실행 순서를 인위적으로 제어할 수 있다.

# 1. 블록문

블록문은 0개 이상의 문들을 중괄호로 묶은 것으로 코드 블록 또는 블록이라고 부르며 블록문을 하나의 단위로 취급한다.

```js
// 블록문
{
  var foo = 10;
  console.log(foo);
}

// 제어문
var x = 0;
while (x < 10) {
  x++;
}
console.log(x); //10

//함수 선언문
function sum(x, y) {
  return x + y;
}
console.log(sum(1, 2)); //3
```

# 2. 조건문

조건문은 주어진 조건식의 평가 결과에 따라 코드 블럭(블록문)의 실행을 결정한다.

## 2.1 if else 문

if...else 문은 주어진 조건식의 평가 결과, 즉 논리적 참 거짓에 따라 실행할 코드 블록을 결정한다. 불리언 값이 아니면 강제 변환되어 논리적 참, 거짓을 구별한다.

```js
var num = 2;
var kind;

if (num > 0) {
  //조건식1
  kind = '양수'; // 조건식 1참이면
} else if (num < 0) {
  //조건식2
  kind = '음수'; // 조건식 2참이면
} else {
  kind = '영'; // 두개모두 참이아니면
}

console.log(kind); //양수

// 블록 내 문이 하나뿐일 경우 중괄호 생략
if (num > 0) kind = '양수';
else if (num < 0) kind = '음수';
else kind = '영';
```

### 삼항연산자

```js
var x = 2;
var result = x % 2 ? '홀수' : '짝수';

console.log(result); //짝수

//3가지 경우의 수
var num = 2;
var kind = num ? (num > 0 ? '양수' : '음수') : '영';
console.log(kind); //양수
```

## 2.2 switch 문

switch 문은 switch 문의 표현식을 평가하여 그 값고 일치하는 표현식을 갖는 case 문으로 실행 순서를 이동 시킨다. 다양한 상황(case)에 따라 실행할 코드 블록을 결정할 때 사용한다. 만약 `break`문이 없다면 case 문의 표현식과 일치하지 않더라도 실행 순서는 다음 case문으로 연이어 이동한다. defualt문에는 `break문`을 생략하여도 된다.

```js
var year = 2000;
var month = 2;
var days = 0;

switch (month) {
  case 1:
  case 3:
  case 5:
  case 7:
  case 8:
  case 10:
  case 12:
    days = 31;
    break;
  case 4:
  case 6:
  case 9:
  case 11:
    days = 30;
    break;
  case 2:
    // 윤년 계산 알고리즘
    // 1. 년도가 4로 나누어 떨어지는 해는 윤년(2000,2004,2008,2012,3016,2020)
    // 2. 그 중에서 년도가 100으로 나누어 떨어지는 해는 평년(2000,2100,2200)
    // 3. 그 중에서 년도가 400으로 나누어 떨어지는 해는 윤년(2000,2400,2008)
    days = (year % 4 === 0 && year % 100 !== 0) || year % 400 === 0 ? 29 : 28;
    break;
  default:
    console.log('Invalid month');
}
console.log(days); //29;
```

if..else문으로 해결할 수 있다면 사용 하면 좋으나 switch 문을 사용했을 때 가독성이 더 좋으면 사용 하는 게 좋다.
#. 반복문
반복문은 주어진 조건식의 평가 결과가 참인 경우 코드 블럭을 실행한다.
그 후 조건식을 다시 검사하여 여전히 참인 경우 코드 블록을 다시 실행한다. 조건식이 거짓일 때 까지 반복된다.

## 3.1 for 문

```js
for (var i = 0; i < 2; i++) {
  console.log(i);
}
```

<img src ="https://poiemaweb.com/img/for-statement.png">

### for문 실행순서

1. for 문ㅇㄹ 실행하면 가장 먼저 초기화식 var i = 0 이 실행된다. 초기화식은 단 한번만 실행된다.
2. 초기화식의 실행이 종료되면 조건식으로 실행 순서가 이동한다. 현제 변수는 i는 0 이므로 조건식의 평가 결과는 true다.
3. 조건식의 평가 결과가 true 이므로 실행 순서가 코드 블록으로 이동하여 실행된다.
4. 코드 블록의 실행이 종료하면 증감식으로 실행 순서가 이동한다. 증감식 i++ 실행되어 i는 1이된다.
5. 증감식이 실행이 종료되면 다시 조건식으로 실행 순서가 이동한다. 초기화식으로 실행 순서가 이동하는 것이 아니라 조건식으로 실행 순서가 이동하는 것에 주의하자. 최기화식은 단 한번만 실행된다. 현재 변수 i는 1이므로 조건식의 평가 결과는 true다
6. 조건식의 평가 결과가 true이므로 실행 순서가 코드 블록으로 이동하여 실행된다.
7. 코드 블록의 실행이 종료하면 증감식으로 실행순서가 이동한다. 증감식 i++가 실행되어 2가 된다.
8. 증감식 실행이 종료되면 다시 조건식으로 실행 순서가 이동한다. 현재 변수 i = 2이므로 조건식의 평가 결과는 false 이다. for문의 실행이 종료된다.

```js
// 초기화식, 조건식 증감식 모두 사용하지 않아도 되며 선언하지 않으면 무한 루프가된다.
for (;;) {} //무한루프

// 두개의 주사위를 던졌을 때, 두 눈의 합이 6이 되는 모든 경우의 수
for (var i = 1; i < 6; i++) {
  for (var j = 1; j < 6; j++) {
    if (i + j === 6) console.log(`[${i}], ${j}`);
  }
}
/*
[1, 5]
[2, 4]
[3, 3]
[4, 2]
[5, 1]
*/
```

## 3.2 while 문

while 문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다. 조건문의 평가 결과가 거짓이 되면 실행을 종료한다. 조건식의 평가 결과가 불리언값이 아니면 강제 변환되어 참 거짓을 구별한다.

```js
var count = 0;
while (count < 3) {
  console.log(count);
  count++;
}
// 무한루프를 탈출하기 위해서는 if 문을 부여하고 break문으로 코드 블럭을 탈출한다.

//무한루프
while (true) {}

//if break
var count = 0;
while (true) {
  console.log(count);
  count++;
  // count가 3이면 코드 블록을 탈출한다.
  if (count === 3) break;
}
```

## 3.3 do...while문

do..while문은 코드 블록을 실행하고 조건식을 평가한다. 코드 블록은 무저건 한번 이상 실행된다.

```js
var count = 0;
do {
  console.log(count);
  count++;
} while (count < 3);
```

# break 문

레이블 문 , 반복문(for, for...in, for..of, while, do..while)또는 switch문의 코드 블록을 탈출한다. 이외의 사용 하면 syntaxError(문법에러)가 발생한다.

```js
if(true){
    break; //error
}
```

**레이블 문이란**
식별자가 붙은 문을 말한다.
프로그램의 실행 순서를 제어하기 위해 사용한다. switch 문의 case문과 default문도 레이블 문이다. 레이블 문을 탈출하려면 break문에 레이브리 식별자를 지정한다.

```js
foo: console.log('foo');

outer: for (var i = 0; i < 3; i++) {
  for (var j = 0; j < 3; j++) {
    //i + j  === 3 이면 외부 for 문을 탈출한다.
    if (i + j === 3) break outer;
  }
}
console.log('Done!');
```

중첩된 for 문을 외부로 탈출할 때 레이블 문은 유용하지만 그 외의 경우 프로그램의 흐름이 복잡해져서 가족성이 나빠지고 오류를 발생시킬 가능성이 높기 때문에 권장하지 않는다.

```js
var string = 'Hello World.';
var index;

// 문자열은 유사배열이므로 for 문으로 순회할 수 있다.
for (var i = 0; i < string.length; i++) {
  //문자열의 개별 문자가 'l'이면
  if (string[i] === 'l') {
    index = i;
    break; //반복문을탈출한다.
  }
}

console.log(index);

// 참고로 String.prototype.indexOf 메소드를 사용해도 똑같은 동작을 한다.
console.log(string.indexOf('l')); //2
```

# 5. continue 문

continue 문은 반목문 (for,for...in, for...of,while, do...while)의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 이동한다. break 문처럼 반복문을 탈출하지 않는다.

```js
var string = 'Hello world';
var count = 0;

// 문자열은 유사배열이므로 for 문으로 순회할 수 있다.
for (var i = 0; i < string.length; i++) {
  // 'l'이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.
  if (string[i] !== 'l') continue;
  count++;
}

console.log(count);
//참고로 String.prototype.match 메소드를 사용해도 같은 동작을 한다.
console.log(string.match(/l/g).length); //3
```

```js
//for문
for (var i = 0; i < string.length; i++) {
  //'l'이면 카운트를 증가시킨다.
  if (string[i] === 'l') count++;
}
```

if문 내에서 실행해야 할 코드가 한줄이라면 continue 문을 사용했을 때보다 간편하며 가독성도 좋다.
하지만 if문 내에서 실행해야 할 코드가 길다면 들여쓰기가 한 단꼐 더 깊어지므로 countinue문을 사용하는 것이 가독성에 더 좋다.

```js
// continue 문을 사용하지 않으면 if 문 내에 코드를 작성해야 한다.
for (var i = 0; i < string.length; i++) {
  // 'l'이면 카운트를 증가시킨다.
  if (string[i] === 'l') {
    count++;
  }
}

// countinue 문을 사용하면 if 문 밖에 코드를 작성할 수 있다.
for (var i = 0; i < string.length; i++) {
  //'l'이 아니면 카운트를 증가시키지 않는다.
  if (string[i] !== 'l') continue;
  count++;
}
```
