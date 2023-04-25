제어문은 주어진 조건에 따라 코드 블록을 실행하거나 반복 실행할 때 사용함
보통 코드는 위에서 아래로 실행되는데 제어문은 코드의 실행순서를 인위적으로 제어할 수 있음

# 블록문

블록문은 0개 이상의 문들을 중괄호로 묶은 것으로 코드 블록 또는 블록이라고 부르기도 함
자바스크립트는 블록문을 하나의 단위로 취급함

```
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
console.log(x); // 10

// 함수 선언문
function sum(x, y) {
  return x + y;
}
console.log(sum(1, 2)); // 3

```

# 조건문

조건문은 주어진 조건식의 평가 결과에 따라 코드 블록의 실행을 결정함
조건식은 불리언 값으로 평가될 수 있는 표현식임
조건문은 if...else, switch문을 제공함

### if...else문

참, 거짓에 따라 실행할 코드 블록을 결정함
만약 조건식의 평가 결과가 불리언 값이 아니면 불리언 값으로 **강제 변환**되어 논리적 참, 거짓을 구별함

`if (조건식) {
// 조건식이 참이면 이 코드 블록이 실행된다.
} else {
// 조건식이 거짓이면 이 코드 블록이 실행된다.
}``

```

조건식의 평과 결과가 true일 경우 if문의 다음 코드 블록이 실행됨
만약 false일 경우 else문 다음의 코드 블록이 실행됨
조건식을 추가하고 싶으면 else if문을 사용함

```

if (조건식1) {
// 조건식1이 참이면 이 코드 블록이 실행된다.
} else if (조건식2) {
// 조건식2이 참이면 이 코드 블록이 실행된다.
} else {
// 조건식1과 조건식2가 모두 거짓이면 이 코드 블록이 실행된다.
}

```

else if와 else문은 옵션으로 사용할 수도 있고 사용하지 않을 수도 있음
if와 else문은
2번 이상 사용할 수 없지만 else if문은 여러 번 사용할 수 있음

```

var num = 2;
var kind;

// if 문
if (num > 0) {
kind = '양수'; // 음수를 구별할 수 없다
}
console.log(kind); // 양수

// if…else 문
if (num > 0) {
kind = '양수';
} else {
kind = '음수'; // 0은 음수가 아니다
}
console.log(kind); // 양수

// if…else if 문
if (num > 0) {
kind = '양수';
} else if (num < 0) {
kind = '음수';
} else {
kind = '영';
}
console.log(kind); // 양수

```

만약 코드 내의 문이 하나라면 중괄호를 생략할 수 있음

```

var num = 2;
var kind;

if (num > 0) kind = '양수';
else if (num < 0) kind = '음수';
else kind = '영';

console.log(kind); // 양수

```

대부분은 if...else문은 삼항 조건 연산자로 바꿔쓸 수 있음

```

// x가 짝수이면 ‘짝수'를 홀수이면 ‘홀수'를 반환한다.
var x = 2;
var result;

if (x % 2) { // 2 % 2는 0이고 0은 false로 취급된다.
result = '홀수';
} else {
result = '짝수';
}

console.log(result); // 짝수

---

// x가 짝수이면 '짝수'를 홀수이면 '홀수'를 반환한다.
var x = 2;

// 0은 false로 취급된다.
var result = x % 2 ? '홀수' : '짝수';
console.log(result); // 짝수

```

### switch문

switch문의 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행순서 이동시킴
case문은 상황을 의미하는 표현식을 지정하고 콜론으로 마침
그리고 그 뒤에 실행할 문들을 위치시킴
switch문의 표현식과 일치하는 표현식을 갖는 case문이 없다면 실행 순서는 default문으로 이동함
default 옵션으로 사용할 수도 있고 사용하지 않을 수도 있음

```

switch (표현식) {
case 표현식1:
switch 문의 표현식과 표현식1이 일치하면 실행될 문;
break;
case 표현식2:
switch 문의 표현식과 표현식2가 일치하면 실행될 문;
break;
default:
switch 문의 표현식과 일치하는 표현식을 갖는 case 문이 없을 때 실행될 문;
}

```
if...else문의 조건식은 반드시 불리언 값으로 평가되지만 switch문의 표현식은 불리언 값보다는 문자열, 숫자값인 경우가 많음
if...else 문은 논리적 참, 거짓으로 실행할 코드 블록을 결정함
switch문은 논리적 참, 거짓보다는 다양한 case에 따라 실행할 코드 블록을 결정할 때 사용함

```

// 월을 영어로 변환한다. (11 → 'November')
var month = 11;
var monthName;

switch (month) {
case 1:
monthName = 'January';
case 2:
monthName = 'February';
case 3:
monthName = 'March';
case 4:
monthName = 'April';
case 5:
monthName = 'May';
case 6:
monthName = 'June';
case 7:
monthName = 'July';
case 8:
monthName = 'August';
case 9:
monthName = 'September';
case 10:
monthName = 'October';
case 11:
monthName = 'November';
case 12:
monthName = 'December';
default:
monthName = 'Invalid month';
}

console.log(monthName); // Invalid month

```
 switch 문의 표현식, 즉 변수 month의 평가 결과인 숫자 값 11과 일치하는 case 문으로 실행 순서가 이동함

그런데 위의 예시를 실행하면 case 에 해당하는 November가 출력되지 않고 invalid month가 출력됨.
switch문의 표현식의 평가 결과가 일치하는 case문으로 실행 순서가 이동하여 문을 실행한 것은 맞지만 문을 실행한 후 switch문을 날출하지 않고 switch문이 끝날 때까지 모든 case문과 default문을 실행했기 때문임
이를 fall through라 함
즉, 변수 monthName에 November가 할당된 후 switch문을 탈출하지 않고 연이어 december가 재할당되고 마지막으로 invalid month가 재할당되었음

이런 결과가 나타난 이유는 case문에 해당하는 문의 마지막에 break문을 사용하지 않았기 때문임
break 키워드로 구성된 break문은 코드 블록에서 탈출하는 역할을 수행함
break문이 없다면 case 문의 표현식과 일치하지 않더라도 실행 순서는 다음 case문으로 연이어 이동함

위의 예시를 바르게 고쳐보자면 아래와 같음

```

// 월을 영어로 변환한다. (11 → 'November')
var month = 11;
var monthName;

switch (month) {
case 1:
monthName = 'January';
break;
case 2:
monthName = 'February';
break;
case 3:
monthName = 'March';
break;
case 4:
monthName = 'April';
break;
case 5:
monthName = 'May';
break;
case 6:
monthName = 'June';
break;
case 7:
monthName = 'July';
break;
case 8:
monthName = 'August';
break;
case 9:
monthName = 'September';
break;
case 10:
monthName = 'October';
break;
case 11:
monthName = 'November';
break;
case 12:
monthName = 'December';
break;
default:
monthName = 'Invalid month';
}

console.log(monthName); // November

```
default문에는 break문을 생략하는 것이 일반적임(왜냐면 default문이 맨 마지막에 위치하므로 default문의 실행이 종료되면 switch문을 빠져나가기 때문임)

case문은 중복해서 사용할 수도 있음

```

var year = 2000; // 2000년은 윤년으로 2월이 29일이다.
var month = 2;
var days = 0;

switch (month) {
case 1: case 3: case 5: case 7: case 8: case 10: case 12:
days = 31;
break;
case 4: case 6: case 9: case 11:
days = 30;
break;
case 2:
// 윤년 계산 알고리즘
// 1. 년도가 4로 나누어 떨어지는 해는 윤년(2000, 2004, 2008, 2012, 2016, 2020…)
// 2. 그 중에서 년도가 100으로 나누어 떨어지는 해는 평년(2000, 2100, 2200...)
// 3. 그 중에서 년도가 400으로 나누어 떨어지는 해는 윤년(2000, 2400, 2800...)
days = ((year % 4 === 0 && year % 100 !== 0) || (year % 400 === 0)) ? 29 : 28;
break;
default:
console.log('Invalid month');
}

console.log(days); // 29

```
switch 문은 case, default, break 등 다양한 키워드를 사용해야 하고 폴스루가 발생하는 등 문법도 복잡함.
if…else 문으로 해결할 수 있다면 if…else 문을 사용하는 편이 좋음
하지만 if…else 문보다 switch 문을 사용했을 때 가독성이 더 좋다면 switch 문을 사용하는 편이 좋음

# 반복문

평가 결과가 참인 경우 코드 블록을 실행함
그 후 조건식을 다시 검사하여 여전히 참인 경우 코드 블록을 다시 실행하여 조건식이 거짓일 때까지 반복됨

### for문

for문은 조건식이 거짓으로 판별될 때까지 코드 블록을 반복 실행함

```

for (초기화식; 조건식; 증감식) {
조건식이 참인 경우 반복 실행될 문;
}

```

for문이 어떻게 동작하는지 아래의 이미지를 보자

<div align="center">
<img src="https://poiemaweb.com/img/for-statement.png" width="50%" height="50%">
<div>

<div align="left">

1. for 문을 실행하면 가장 먼저 초기화식 var i = 0이 실행됌
초기화식은 단 한번만 실행됌

2. 초기화식의 실행이 종료되면 조건식으로 실행 순서가 이동함
현재 변수 i는 0이므로 조건식의 평가 결과는 true임

3. 조건식의 평가 결과가 true이므로 실행 순서가 코드 블록으로 이동하여 실행됌
증감문으로 실행 순서가 이동하는 것이 아니라 코드 블록으로 실행 순서가 이동하는 것에 주의하자

4. 코드 블록의 실행이 종료하면 증감식으로 실행 순서가 이동함
증감식 i++가 실행되어 i는 1이 됌

5. 증감식 실행이 종료되면 다시 조건식으로 실행 순서가 이동함
초기화식으로 실행 순서가 이동하는 것이 아니라 조건식으로 실행 순서가 이동하는 것에 주의하자
초기화식은 단 한번만 실행됌
현재 변수 i는 1이므로 조건식의 평가 결과는 true임

6. 조건식의 평가 결과가 true이므로 실행 순서가 코드 블록으로 이동하여 실행됌

7. 코드 블록의 실행이 종료하면 증감식으로 실행 순서가 이동함
증감식 i++가 실행되어 i는 2가 됌

8. 증감식 실행이 종료되면 다시 조건식으로 실행 순서가 이동함
현재 변수 i는 2이므로 조건식의 평가 결과는 false임
조건식의 평가 결과가 false이므로 for 문의 실행이 종료됌

### while문

while 문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행함
조건문의 평가 결과가 거짓이 되면 실행을 종료함
만약 조건식의 평가 결과가 불리언 값이 아니면 불리언 값으로 **강제 변환** 되어 논리적 참, 거짓을 구별함

```

var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
while (count < 3) {
console.log(count);
count++;
} // 0 1 2

```
조건식의 평가 결과가 언제나 참이면 무한루프가 됌

```

// 무한루프
while (true) { }

```

무한루프를 탈출하기 위해서는 코드 블럭 탈출 조건을 if 문에 부여하고 break 문으로 코드 블럭을 탈출함

```

var count = 0;

// 무한루프
while (true) {
console.log(count);
count++;
// count가 3이면 코드 블록을 탈출한다.
if (count === 3) break;
} // 0 1 2

```

### do...while

do…while 문은 코드 블록을 실행하고 조건식을 평가함
따라서 코드 블록은 무조건 한번 이상 실행됌

```

var count = 0;

// count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
do {
console.log(count);
count++;
} while (count < 3); // 0 1 2

```

# break문

switch 문과 while 문에서 봤듯이 break 문은 코드 블록을 탈출함
좀 더 정확히 표현하자면 코드 블록을 탈출하는 것이 아니라 레이블 문, 반복문(for, for…in, for…of, while, do…while) 또는 switch 문의 코드 블록을 탈출함
레이블 문(식별자가 붙은 문), 반복문, switch 문의 코드 블록 이외에 break 문을 사용하면 SyntaxError(문법 에러)가 발생함

```

if (true) {
break; // Uncaught SyntaxError: Illegal break statement
}

```

```

// 레이블문 예시
// foo라는 레이블 식별자가 붙은 레이블 문
foo: console.log('foo');

```

레이블 문은 프로그램의 실행 순서를 제어하기 위해 사용함
사실 switch문의 case문과 default문도 레이블 문임
레이블 문을 탈출하려면 break문에 레이블 식별자를 지정함


```

// foo라는 식별자가 붙은 레이블 블록문
foo: {
console.log(1);
break foo; // foo 레이블 블록문을 탈출한다.
console.log(2);
}

console.log('Done!');

```

중첩된 for 문의 내부 for 문에서 break 문을 실행하면 내부 for 문을 탈출하여 외부 for 문으로 진입함
이때 내부 for 문이 아닌 외부 for 문을 탈출하려면 레이블 문을 사용함

```

// outer라는 식별자가 붙은 레이블 for 문
outer: for (var i = 0; i < 3; i++) {
for (var j = 0; j < 3; j++) {
// i + j === 3이면 외부 for 문을 탈출한다.
if (i + j === 3) break outer;
}
}

console.log('Done!');

```

중첩된 for문을 외부로 탈출할 때 레이블 문은 유용하지만 그 외의 경우 레이블 문은 일반적으로 권장하지 않음
레이블 문을 사용하면 프로그램이 흐름이 복잡해져서 가독성 나빠지고 오류를 발생시킬 가능성이 높아지기 때문임
break문은 레이블 문 뿐만 아니라 반복문, switch 문에서도 사용할 수 있음
이 경우에는 break문에 레이블 식별자를 지정하지 않음
break문은 반복문을 더 이상 진행하지 않아도 될 때 까지 불필요한 반복을 회피할 수 있어 유용함

```

var string = 'Hello World';
var index;

// 문자열은 유사배열이므로 for 문으로 순회할 수 있음
for (var i = 0; i < string.length; i++) {
// 문자열의 개별 문자가 'l'이면
if (string[i] === 'l') {
index = i;
break; // 반복문을 탈출함
}
}

console.log(index); // 2

// 참고로 String.prototype.indexOf 메소드를 사용해도 같은 동작을 함
console.log(string.indexOf('l')); // 2

```

# continue문

continue문은 반복문(for, for…in, for…of, while, do…while)의 코드 블록 진행을 현 시점에서 중단하고 반복문의 증감식으로 이동함
break문처럼 반복문을 탈출하지 않음

```

var string = 'Hello World.';
var count = 0;

// 문자열은 유사배열이므로 for 문으로 순회할 수 있음
for (var i = 0; i < string.length; i++) {
// 'l'이 아니면 지금 지점에서 실행을 중단하고 반복문의 증감식으로 이동함
if (string[i] !== 'l') continue;
count++; // continue 문이 실행되면 이 문은 실행되지 않음
}

console.log(count); // 3

// 참고로 String.prototype.match 메소드를 사용해도 같은 동작을 함
console.log(string.match(/l/g).length); // 3

```

위의 예제는 for문을 사용한 아래와 같이 동일하게 동작함

```

for (var i = 0; i < string.length; i++) {
if (string[i] === 'l') count++;
// 'l'이면 카운트를 증가시킴
}

```
if문 내에서 실행할 코드가 한 줄이라면 continue문을 사용했을 때보다 가독성이 좋고 간편함
하지만 if문 내에서 실행해야 할 코드가 길다면 들여쓰기가 한 단계 깊어지므로 continue문을 사용하는 것이 가독성 면에서 좋음

</div>

```
