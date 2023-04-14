제어문(control flow statment)은 주어진 조건에 따라 코드 블록을 실행(조건문)하거나 반복 실행(반복문)할 때 사용한다.

- 일반적으로 위에서 아래 방향으로 순차적으로 실행된다.

# 블록문

블록문(block statement/compound statement)는 0개 이상의 문들을 중괄호로 묶은 것으로 코드 블록 또는 블록이라고 부르기도 한다.

- 자바스크립트는 블록문을 하나의 단위로 취급한다.
- 블록문은 세미콜론을 붙이지 않는다.
- 블록문은 일반적으로 제어문이나 함수 선언문 등에서 사용한다.

```javascript
//블록문
{
  var foo = 10;
  console.log(foo);
}

//제어문
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

# 조건문

조건문(conditional statement)은 주어진 조건식(conditional expression)의 평가 결과에 따라 코드 블럭(블록문)의 실행을 결정한다.

- 조건식은 불리언 값으로 평가될 수 있는 표현식이다.

자바스크립트는 2가지의 조건문 if...else문과 switch 문을 제공한다.

## if...else문

if...else 문은 주어진 조건식(불리언 값으로 평가될 수 있는 표현식)의 평과 결과, 논리적 참, 겨짓에 따라 실행할 코드 블록을 결정한다.

- 조건식의 평과가 불리언 값이 아니면 불리언 값으로 **강제 변환** 되어 참, 거짓을 구별한다.

```javascript
if (조건식) {
  //조건식이 참이면 이 코드 블록이 실행된다.
} else {
  //조건식이 거짓이면 이 코드 블록이 실행된다.
}
```

- 조건식의 평가 결과가 참(true)일 경우, if문 다음의 코드 블록이 실행된다
- false일 경우, else문 다음의 코드 블록이 실행된다.
- 조건식을 추가하고 싶으면 else if문을 사용한다.

```javascript
if (조건식1) {
  //조건식1이 참이면 이 코드 블록이 실행된다.
} else if (조건식2) {
  //조건식2이 참이면 이 코드 블록이 실행된다.
} else {
  //조건식1과 조건식2가 모두 거짓이면 이 코드 블록이 실행된다.
}
```

else if문과 else문은 옵션으로 사용할 수도 있고 사용하지 않을 수도 있다.

````javascript
var num = 2;
var kind;

//if문
if(num > 0){
    kind ="양수";//음수를 구별할 수 없다.
}
console.log(kind); //양수

//if...else 문
if(num > 0){
    kind = "양수";
}else {
    kind ="음수"; //0은 음수가 아니다.
    }
    console.log(kind); //양수
    ```

```
* 대부분의 if else문은 **삼항 조건 연산자**로 바꿔쓸 수 있다.
```javascript
//x가 짝수이면 '짝수'를 홀수이면 '홀수'를 반환한다.
var x =2;
var result;
if(x%2){//2%2는 0이고 0은 false로 취급된다.
result = '홀수';}
else{result = '짝수';}

console.log(result); //짝수

//x가 짝수이면 '짝수'를 홀수이면 '홀수'를 반환한다.
var x = 2;

//0은 false로 취급된다.
var result = x%2?'홀수':'짝수';
console.log(result); //짝수
```


## switch 문
switch문은 switch문의 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case문으로 실행 순서를 이동시킨다.
switch 문의 표현식과 일치하는 표현식을 갖는 case문이 없다면 defualt 문으로 이동한다.
* default 옵션으로 사용할 수도 있고 사용하지 않을 수도 있다.
```
switch (표현식){
    case 표현식1:
        switch 문의 표현식과 표현식1이 일치하면 실행될 문;
        break;
    case 표현식2:
        switch 문의 표현식과 표현식2가 일치하면 실행될 문;
        break;
    default:
        switch 문의 표현식과 일치하는 표현식을 갖는 case문이 없을 때 실행될 문;
}
```
```javascript
//월을 영어로 변환한다.(11->"November")
var month =11;
var monthName;

switch(month){
    case 1:
        monthName = "January";
    case 2:
        monthName = "February";
    case 3:
        monthName = "March;
    case 4:
        monthName = "April";
    case 5:
        monthName = "May";
    case 6:
        monthName = "June";
    case 7:
        monthName = "July";
    case 8:
        monthName = "August";
    case 9:
        monthName = "September";
    case 10:
        monthName = "October";
    case 11:
        monthName = "November";
    case 12:
        monthname = "December";
    default:
        monthName = "Invalid month";}

        console.log(monthName); //Invalid month
        ```
        * November가 출력되지 않고 Invalid month가 출력된 이유는 문을 실행한 후 탈출하지 않고 끝날 때까지 모든 case문과 default문을 실행했기 때문이다.(폴스루(fall through))
        * case문에 해당하는 문의 마지막에 `break`를 사용하지 않았기 때문이다.
case 문은 반드시 단독으로 사용되어야 하는 것이 아니라 여러개의 case문을 중복할 수 있다.
```javascript
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
* switch문은 case, default, break등 다양한 키워드를 사용하고 폴스루가 발생하는 등 문법도 복잡하기 때문에 if...else문으로 해결할 수 있다면 if...else문을 사용하는 편이 좋다.

# 반복문
반복문(loop statement)은 주어진 조건식(conditional expression)의 평가 결과가 참인 경우 코드 블럭을 실행한다.
* 조건식을 검사하여 참인 경우 코드 블록을 다시 실행하며 거짓일 때까지 반복된다.
* 자바스크립트는 3가지의 반복문 for문, while문, do...while 문을 제공한다.

## for문
for 문은 조건식이 거짓으로 판별될 때까지 코드 블록을 반복 실행한다.
* 가장 일반적으로 사용되는 반복문이다.
```
for(초기화식; 조건삭; 증감식){
    조건식 참인 경우 반복 실행될 문;
}
```
```javascript
for(var i = 0; i<2; i++){
    console.log(i);
}
* 예제의 for문은 변수i가 0으로 초기화된 상태에서 시작하여 i가 2보다 작을 때까지 코드 블록을 2번 반복 실행한다.

1. 초기화식 `var i=0`이 실행. (초기화 식은 한번만 실행)
2. 초기화식의 실행 종료 -> 조건식 실행.  현재 변수 i는 0이므로 조건식의 평가 결과는 true이다.
3. 조건식의 결과가 true로 -> 코드 블록 실행.
4. 코드 블록의 실행 종료 -> 증감식 실행. 증감식 i++가 실행되어 i는 1이 된다.
5. 증감식 실행 종료 -> 조건식 실행. 현재 변수 i는 1 = true.
6. 조건식의 평가 결과 true -> 코드 블록 실행
7. 코드 블록 실행 종료 -> 증감식 실행. 증감식 i++ 실행으로 i는 2가 된다.
8. 증감식 실행 종료 -> 조건식 실행. 현재 변수 i는 2=false.
9. 조건식 평가 결과가 false이므로 for문의 실행 종료.

```javascript
for(var i = 1 i>= 0; i --){
    console.log(i);
}
```
* 변수 i가 1로 초기화  된 상태에서 i가 0보다 같거나 커질 때까지 코드 블록을 2번 반복 실행한다.

for 문의 초기화식, 조건식, 증감식은 모두 옵션이므로 반드시 사용할 필요는 없고 어떤식도 선언하지 않으면 무한 루프가 된다.
```javascript
for(;;){} //무한루프
```
for 문 내에 for 문을 중첩해 사용할 수 있다.
* 주사위 두개를 던졌을 때, 두 눈의 합이 6이 되는 모든 경우의 수를 출력하는 예제
```javscript
for (var i=1; i =<6; i++){
for (var j =1; j=<6; j++){
    if (i+j ===6)console.log(`[${i},${j}]`);
}
}
//출력 결과
[1,5]
[2,4]
[3,3]
[4,2]
[5,1]
```
## while문
while문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다. 조건문의 평가 결과가 거짓이 되면 실행을 종료한다.
* 조건식의 평가 결과가 불리언 값이 아니면 불리언 값으로 강제 변환되어 논리적 참, 거짓을 구별한다.
```javascript
var count =0;

//count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
while (count <3){
    console.log(count);
    count++
}//0 1 2
```
* 조건식의 평 결과가 참이면 무한루프가 된다.
```
//무한 루프
while(true){}
```
* 무한 루프를 탈출하기 위해서 코드 블럭 탈출 조건은 if문에 부여하고 break 문으로 코드 블럭을 탈출한다.
```javascript
var count =0;

//무한루프
while(true){
    console.log(count);
    count++;
    //count가 3이면 코드 블록을 탈출한다.
    if(count===3)break;
}//0 2 3
```
## do...while문
do...while문은 코드 블록을 실행하고 조건식을 평가한다.
* 코드 블록은 무조건 한번 이상 실행된다.
```javascript
var count =0;

//count가 3보다 작을 때까지 코드 블록을 계속 반복 실행한다.
do{
    console.log(count);
    count++;
} while(count <3); //0 1 2
```
# break 문
break 문은 코드 블록을 탈출한다.
* 코드 블록을 탈출하는 것이 아닌, 레이블 문, 반복문(for, for...in, for...of, while, do...while) 또는 switch 문의 코드 블록을 탈출한다.
* 그 이외에 break 문을 사용하면 syntaxError(문법 에러)가 발생한다.

```javascript
if(true){
    break; uncaught syntaxError:Illegal break statement
}
```
* 레이블 문(label statement)이란 식별자가 붙은 문을 말한다.
```javascript
// foo 라는 레이블 식별자가 붙은 레이블 문
foo: console.log("foo");
```
* 레이블 문은 프로그램으 ㅣ실행 순서를 제어하기 위해 사용한다.
* switch 문의 case 문과 default문도 레이블 문이다.
* 레이블 문을 탈출하려면 break문에 레이블 식별자를 지정한다.

```javascript
//foo라는 식별자가 붙은 레이블 블록문
foo:{
    console.log(1);
    break foo; //foo 레이블 블록문을 탈출한다.
    console.log(2);
}
console.log("DONE");
```
중첩된 for 문의 내부 for 문에서 break문을 실행하면 내부 for문을 탈출하여 외부 for문으로 진입한다.
* 내부 for문이 아닌 외부 for문을 탈출하려면 레이블 문을 사용한다.
```javascript
//outer 라는 식별자가 붙은 레이블 for 문
outer: for(var i =0; i<3; i++){
    for var j= 0; j<3; j++){
        //i+j ===3이면 외부 for문을 탈출한다.
        if(i+j ===3)break outer;
    }
}
console.log("DONE!");
```
* 중첩된 for문을 외부로 탈출할 때 레이블 문은 유용하지만 그 외의 경우에는 일반적으로 권장하지 않는다.
* 프로그램의 흐름이 복잡해져 가독성이 나빠지고 오류를 발생시킬 가능성이 높다.

문자열에서 특정 문자의 인덱스를 검색하는 예제
```javascript
var string = "hello world";
var index;

//문자열은 유사배열이므로 for 문으로 순회할 수 있다.
for(var i=0; i<string.length; i ++){
    //문자열의 개별 문자가 "l"이면
    if(string[i]==="l"){
        index =i;
        break;//반복문 탈출
    }
}
console.log(index); //2

//string.prototype.indexOf 메소드를 사용해도 같은 동작을 한다.
console.log(string.indexOf("l")) //2

# continue 문
continue문은 반복문(for, for...in, for..of, while, do...while)의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 이동한다.
* break문처럼 반복문을 탈출하지 않는다.

문자열에서 특정 문자의 개수를 카운트하는 예제

```javascript
var string= "hello world.";
var count = 0;

//문자열은 유사배열이므로 for 문으로 순회할 수 있다.
for (var i=0; i < string.length; i ++){
    //"l"이 아니면 현 지점에서 실행을 중단하고 반복문의 증감식으로 이동한다.
    if(string[i]!=="l")contintue;
    count++; //continue 문이 실행되면 이 문은 실행되지 않는다.
}
console.log(count); //3

//string.prototpe.math 메소드를 사용해도 같은 동작을 한다.
console.log(string.math(/l/g).length); //3

//for문
for(var i=0; i<string.length; i++){
    //"l"이 면 카운트를 증가시킨다.
    if(string[i]==="l")count++;
}
```
* if문 내에 실행해야할 코드가 한 줄이면 continue문을 사용했을 때보다 간편하며 가독석이 좋지만, if문 내에서 실행해야할 코드가 길다면 들여쓰기가 한 단계 깊어지기 때문에 continue문을 사용하는 것이 가독성이 더 좋다.

```javascript
// continue문을 사용하지 않으면 if문 내에 코드를 작성해야한다.
for(var i=0; i<string.length; i++){
    //"l"이면 카운트를 증가시킨다.
    if(string[i]==="l"){
        count++;
        //code
        //code
        //code
    }
}

//continue문을 사용하면 if문 밖에 코드를 작성할 수 있다.
for(var i=0; i<string.length; i++){
    //"l"이 아니면 카운트를 증가시키지 않는다.
    if(string[i]!=="l")continue;

    count++;
    //code
    //code
    //code
}
````
