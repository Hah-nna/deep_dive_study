ES6는 템플릿 리터럴이라고 불리는 새로운 문자열 표기법을 도입함. 템플릿 리터럴은 문자열과 비슷해보이지만 '또는 "와 같은 통상적인 따옴표 문자 대신 백틱문자를 사용함

```
const template = `템플릿 리터럴은 '작은따옴표(single quotes)'과 "큰따옴표(double quotes)"를 혼용할 수 있다.`;

console.log(template);
```

일반적인 문자열에서 줄바꿈은 허용되지 않으며 공백을 표현하기 위해서는 백슬래시로 시작하는 이스케이프 시퀀스를 사용하여야 함. ES6 템플릿 리터럴은 일반적인 문자열과 달리 여러 줄에 걸쳐 문자열을 작성할 수 있으며 템플릿 리터럴 내의 모든 공백은 있는 그대로 적용됨

```
const template = `<ul class="nav-items">
  <li><a href="#home">Home</a></li>
  <li><a href="#news">News</a></li>
  <li><a href="#contact">Contact</a></li>
  <li><a href="#about">About</a></li>
</ul>`;

console.log(template);
```

템플릿 리터럴은 `+` 연산자를 사용하지 않아도 간단한 방법으로 새로운 문자열을 삽입할 수 있는 기능을 제공함. 이를 문자열 인터폴레이션이라 함

```
const first = 'Ung-mo';
const last = 'Lee';

// ES5: 문자열 연결
console.log('My name is ' + first + ' ' + last + '.');
// "My name is Ung-mo Lee."

// ES6: String Interpolation
console.log(`My name is ${first} ${last}.`);
// "My name is Ung-mo Lee."
```

문자열 인터폴레이션은 `${...}` 으로 표현식을 감싸고 문자열 인터폴레이션 내의 표현식은 문자열로 강제 타입 변환됨

```
console.log(`1 + 1 = ${1 + 1}`); // "1 + 1 = 2"
```
