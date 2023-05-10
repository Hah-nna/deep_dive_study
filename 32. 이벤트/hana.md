## 1. introduction

이벤트는 어떤 사건을 의미함. 브라우저에서 이벤트란 예를 들어 사용자가 버튼을 클릭했을 때, 웹페이지가 로드되었을 때와 같은 것인데 이것은 DOM 요소와 관련이 있음

이벤트가 발생하는 시점이나 순서를 사전에 인지할 수 없으므로 일반적인 제어 흐름과는 다른 접근 방식이 필요함. 즉, 이벤트가 발생하면 누군가 이를 감지할 수 있어야 하며 그에 대응하는 처리를 호출해주어야 함.

브라우저는 이벤트를 감지할 수 있으며 이벤트 발생시에는 통지해줌. 이 과정을 통해 사용자와 웹페이지는 상호작용이 가능하게 됨

이벤트가 발생하면 그에 맞는 반응을 해야함. 이를 위해 이벤트는 일반적으로 함수에 연결되며 그 함수는 이벤트가 발생하기 전에는 실행되지 않다가 이벤트가 발생되면 실행됨. 이 함수를 이벤트 핸들러라 하며 이벤트에 대응하는 처리를 기술함

## 2. 이벤트 루프와 동시성(concurrency)

브라우저는 단일 스레드에서 이벤트 드리븐 방식으로 동작함
단일 스레드는 스레드가 하나뿐이라는 의미히며 이말은 곧 하나의 task만을 처리할 수 있다는 것을 의미함. 하지만 실제로 동작하는 웹 애플리케이션은 많은 task가 동시에 처리되는 것처럼 느껴짐. 이처럼 자바스크립트의 동시성을 지원하는 것이 바로 이벤트 루프임

브라우저 환경을 그림으로 표현하자면 아래와 같음

<div>
<img src="https://poiemaweb.com/img/event-loop.png" width="50%" height="50">
</div>

대부분의 자바스크립트 엔진은 크게 2개의 영역으로 나뉨

**call stack**

작업이 요청되면(함수가 호출되면) 요청된 작업은 call stack에 쌓이게 되고 순차적으로 실행됨. 자바스크립트는 단 하나의 call stack을 사용하기 때문에 해당 task가 종료되기 전까지는 다른 어떤 task도 수행될 수 없음

**Heap**
동적으로 생성된 객체 인스턴스가 할당되는 영역임

이와 같이 자바스크립트 엔진은 단순히 작업이 요청되면 call stack을 사용해 요청된 작업을 순차적으로 실행할 뿐임. 앞에서 언급한 동시성을 지원하기 위해 필요한 비동기요청 처리는 자바스크립트 엔진을 구동하는 환경 즉, 브라우저(또는 Node.js)가 담당함

**Event Queue**
비동기 처리함수의 콜백함수, 비동기식 이벤트 핸들러, Timer 함수의 콜백함수가 보관되는 영역으로 이벤트 루프에 의해 특정 시점(call stack이 비어졌을 때)에서 순차적으로 call stack으로 이동되어 실행됨

**Event Loop**
call stack 내에서 현재 실행중인 task가 있는지 그리고 event queue에 task가 있는지 반복해 확인함. 만약 call stack이 비어있다면 event queue 내의 task가 call stack으로 이동하고 실행됨

```
function func1() {
  console.log('func1');
  func2();
}

function func2() {
  setTimeout(function () {
    console.log('func2');
  }, 0);

  func3();
}

function func3() {
  console.log('func3');
}

func1();
```

함수 func1이 호출되면 함수 func1은 call stack에 쌓임. 그리고 함수 func1은 함수 func2을 호출하므로 함수 func2가 call stack에 쌓이고 setTimeout가 호출됨
**setTimeout의 콜백함수는 즉시 실행되지 않고 지정 대기 시간만큼 기다리다가 'tick' 이벤트가 발생하면 태스크 큐로 이동한 후 Call Stack이 비어졌을 때 Call Stack으로 이동되어 실행됨**

<div>
<img src="https://poiemaweb.com/img/event-loop.gif" width="50%" height="50%">
</div>

DOM 이벤트 핸들러도 이와 같이 동작함

```
function func1() {
  console.log('func1');
  func2();
}

function func2() {
  // <button class="foo">foo</button>
  const elem = document.querySelector('.foo');

  elem.addEventListener('click', function () {
    this.style.backgroundColor = 'indigo';
    console.log('func2');
  });

  func3();
}

function func3() {
  console.log('func3');
}

func1();
```

함수 func1이 호출되면 함수 func1은 call stack에 쌓임. 그리고 함수 func1은 함수 func2를 호출하므로 함수 func2가 call stack에 쌓이고 addEventListener가 호출됨.
addEventListener의 콜백함수는 foo 버튼이 클릭되어 click 이벤트가 발생하면 태스크 큐로 이동한 후 call stack이 비어졌을 때 call stack으로 이동되어 실행됨

## 3. 이벤트의 종류

대표적인 이벤트만 정리함. 자세한 사항은 [여기]("https://developer.mozilla.org/en-US/docs/Web/Events") 를 참조하면 됨

### 3.1 UI event

| event  | description                                                              |
| ------ | ------------------------------------------------------------------------ |
| load   | 웹페이지의 로드가 완료 되었을 때                                         |
| unload | 웹페이지가 언로드 될 때(주로 새로운 페이지 요청한 경우)                  |
| error  | 브라우저가 자바스크립트 오류를 만났거나 요청한 자원이 존재하지 않는 경우 |
| resize | 브라우저 창의 크리를 조절했을 때                                         |
| scroll | 사용자가 페이지를 위 아래로 스크롤할 때                                  |
| select | 텍스트를 선택했을 때                                                     |

### 3.2 keyboard event

| event    | description            |
| -------- | ---------------------- |
| keydown  | 키를 누르고 있을 때    |
| keyup    | 누르고 있던 키를 뗄 때 |
| keypress | 키를 누르고 떼었을 때  |

keyup과 keypress의 차이점은?

**keyup**

- 사용자가 키를 누른 후 떼는 순간 발생함
- 이벤트가 발생한 순간 떼어낸 키의 값이(event.keyCode나 event.which 프로퍼티를 통해 전달됨)
- 모든 키를 처리할 수 있음 (특수키나 조합키 인식O)

=> 결론 : `keypress`는 문자를 입력할 때 사용, `keyup`은 모든 키 입력을 처리할 때 사용

**keypress**

- 사용자가 키를 누르는 동안 발생함
- 이벤트가 발생한 순간 이미 문자가 입력되었음(event.keyCode나 event.charCode 프로퍼티를 통해 입력된 문자 바로 확인 가능)
- 문자키를 눌렀을 때만 발생함(특수키나 조합키 인식x)

### 3.3 mouse event

| event     | description                                             |
| --------- | ------------------------------------------------------- |
| click     | 마우스 버튼을 클릭했을 때                               |
| dbclick   | 마우스 버튼을 더블 클릭했을 때                          |
| mousedown | 마우스 버튼을 누르고 있을 때                            |
| mouseup   | 누르고 있던 마우스 버튼을 뗄 떼                         |
| mousemove | 마우스를 움직일 때(터치 스크린에서 동작x)               |
| mouseover | 마우스를 요소 위로 움직였을 때(터치 스크린에서 동작x)   |
| mouseout  | 마우스를 요소 밖으로 움직였을 때(터치 스크린에서 동작x) |

### 3.4 focus event

| event         | description               |
| ------------- | ------------------------- |
| focus/focusin | 요소가 포커스를 얻었을 때 |
| blur/focusout | 요소가 포커스를 잃었을 때 |

### 3.5 form event

| event  | description                                                 |
| ------ | ----------------------------------------------------------- |
| input  | input 또는 textarea 요소의 값이 변경되었을 때               |
| ---    | contenteditable 어트리뷰트를 가진 요소의 값이 변경되었을 때 |
| change | select box, checkbox, radio button의 상태가 변경되었을 때   |
| submit | form을 submit할 때(버튼 또는 키)                            |
| reset  | reset 버튼을 클릭할 때(최근에는 사용안 함)                  |

### 3.6 clipboard event

| event | description             |
| ----- | ----------------------- |
| cut   | 콘텐츠를 잘라내기할 때  |
| copy  | 콘텐츠를 복사할 때      |
| paste | 콘텐츠를 붙여넣기 할 때 |

## 4. 이벤트 핸들러 등록

이벤트가 발생했을 때 동작할 이벤트 핸들러를 이벤트에 동작하는 방법은 다음의 3가지임

### 4.1 인라인 이벤트 핸들러 방식

HTML 요소의 이벤트 핸들러 어트리뷰트에 이벤트 핸들러를 등록하는 방법

```
<!DOCTYPE html>
<html>
<body>
  <button onclick="myHandler()">Click me</button>
  <script>
    function myHandler() {
      alert('Button clicked!');
    }
  </script>
</body>
</html>
```

이 방식은 더이상 사용되지 않으며 사용해서도 안됨. 오래된 코드에서 간혹 이 방식을 사용한 것이 있디 때문에 알아둘 필요는 있음.

주의할 것은 onclick과 같이 on으로 시작하는 이벤트 어트리뷰트의 값으로 함수 호출을 전달한다는 것임. 다음에 살펴볼 이벤트 핸들러 프로퍼티 방식에는 DOM 요소의 이벤트 핸들러 프로퍼티에 함수 호출이 아닌 함수를 전달함

이때 이벤트 어트리뷰트의 값으로 전달한 함수 호출이 즉시 호출되는 것은 아님. 사실은 이벤트 어트리뷰트 키를 이름으로 갖는 함수를 암묵적으로 정의하고 그 함수의 몸체에 이벤트 어트리뷰트의 값으로 전달한 함수 호출을 문으로 가짐. 위 예제의 경우, button 요소의 onclick 프로퍼티에 함수 function onclick(event) { foo(); }가 할당됨

즉, 이벤트 어트리뷰트의 값은 암묵적으로 정의되는 이벤트 핸들러의 문임. 따라서 아래와 같이 여러 개의 문을 전달할 수 있음

```
<!DOCTYPE html>
<html>
<body>
  <button onclick="myHandler1(); myHandler2();">Click me</button>
  <script>
    function myHandler1() {
      alert('myHandler1');
    }
    function myHandler2() {
      alert('myHandler2');
    }
  </script>
</body>
</html>
```

### 4.2 이벤트 핸들러 프로퍼티 방식

인라인 이벤트 핸들러 방식처럼 HTML과 자바스크립트가 뒤섞이는 문제는 해결할 수 있는 방식임. 하지만 이벤트 핸들러 프로퍼티에 하나의 이벤트 핸들러만을 바인딩할 수 있다는 단점이 있음

```
<!DOCTYPE html>
<html>
<body>
  <button class="btn">Click me</button>
  <script>
    const btn = document.querySelector('.btn');

    // 이벤트 핸들러 프로퍼티 방식은 이벤트에 하나의 이벤트 핸들러만을 바인딩할 수 있음
    // 첫번째 바인딩된 이벤트 핸들러 => 실행되지 않음
    btn.onclick = function () {
      alert('① Button clicked 1');
    };

    // 두번째 바인딩된 이벤트 핸들러
    btn.onclick = function () {
      alert('① Button clicked 2');
    };

    // addEventListener 메소드 방식
    // 첫번째 바인딩된 이벤트 핸들러
    btn.addEventListener('click', function () {
      alert('② Button clicked 1');
    });

    // 두번째 바인딩된 이벤트 핸들러
    btn.addEventListener('click', function () {
      alert('② Button clicked 2');
    });
  </script>
</body>
</html>
```

① Button clicked 1 -> ② Button clicked 1' -> ② Button clicked 2

첫 번째 바인딩된 이벤트 핸들러가 실행되지 않는 것은 두 번째 이벤트 핸들러로 덮어씌워졌기 때문임.
onlick이벤트는 해당 이벤트에 하나의 이벤트 만을 바인딩할 수 있음. 따라서 두 번째 바인딩된 이벤트 핸들러로 덮어씌워지면서 첫 번째 바인딩된 이벤트는 사라지게 됨
addEventListener 메소드는 하나의 이벤트에 여러 개의 이벤트 핸들러를 등록할 수 있기 때문에 두 개의 이벤트 핸들러가 모두 실행됨

### 4.3 addEventListener 메소드 방식

`addEventListener` 메소드를 이용해 해당 DOM 요소에 이벤트를 바인딩하고 해당 이벤트가 발생했을 때 실행될 콜백함수(이벤트 핸들러)를 지정함

<div>
<img src="https://poiemaweb.com/img/event_listener.png" width="50%" height="50%">
</div>

addEventListener 함수 방식은 이전 방식에 비해 아래와 같이 보다 나은 장점을 가짐

- 하나의 이벤트에 하나 이상의 이벤트 핸들러를 추가할 수 있음
- 캡처링과 버블링을 지원함
- HTML 요소뿐만 아니라 모든 DOM요소에 대해 동작함. 브라우저는 웹문서를 로드한 후 파싱하여 DOM을 생성함

addEventListener 메소드는 IE9 이상에서 동작함. IE8이하에서는 attachEvent를 사용함

```
if (elem.addEventListener) {    // IE 9 ~
  elem.addEventListener('click', func);
} else if (elem.attachEvent) {  // ~ IE 8
  elem.attachEvent('onclick', func);
}
```

아래는 addEventListener 메소드의 사용예시임

```
<!DOCTYPE html>
<html>
<body>
  <script>
    addEventListener('click', function () {
      alert('Clicked!');
    });
  </script>
</body>
</html>
```

위와 같이 대상 DOM요소(target)을 지정하지 않으면 window, 즉 DOM 문서를 포함한 브라우저의 윈도우에서 발생하는 click 이벤트에 이벤트 핸들러를 바인딩함. 따라서 브라우저 윈도우 어디를 클릭해도 이벤트 핸들러가 동작함

```
<!DOCTYPE html>
<html>
<body>
  <label>User name <input type='text'></label>

  <script>
    const input = document.querySelector('input[type=text]');

    input.addEventListener('blur', function () {
      alert('blur event occurred!');
    });
  </script>
</body>
</html>
```

위 예제는 input 요소에서 발생하는 blur 이벤트에 이벤트 핸들러를 바인딩함. 사용자 이름이 최소 2글자 이상이어야 한다는 규칙을 세우고 이에 부합하는지 확인해보자

```
<!DOCTYPE html>
<html>
<body>
  <label>User name <input type='text'></label>
  <em class="message"></em>

  <script>
    const input = document.querySelector('input[type=text]');
    const msg = document.querySelector('.message');

    input.addEventListener('blur', function () {
      if (input.value.length < 2) {
        msg.innerHTML = '이름은 2자 이상 입력해 주세요';
      } else {
        msg.innerHTML = '';
      }
    });
  </script>
</body>
</html>
```

2자 이상이라는 규칙이 바뀌면 이 규칙을 확인하는 모든 코드를 수정해야 함. 따라서 이러한 방식의 코딩은 바람직하지 않음. 이유는 규모가 큰 프로그램의 경우 수정과 테스트에 소요되는 자원의 낭비도 문제이지만 수정에는 거의 대부분 실수가 동반되기 때문임

2자 이상이라는 규칙을 상수화하고 함수의 인수로 전달도록 수정하자. 이렇게 하면 규칙이 변경되어도 함수는 수정하지 않아도 됨

그런데 addEventListener 메소드의 두번째 매개변수는 이벤트가 발생했을 때 호출될 이벤트 핸들러임. 이때 두번째 매개변수에는 함수 호출이 아니라 함수 자체를 지정하여야 함

```
function foo() {
  alert('clicked!');
}
// elem.addEventListener('click', foo()); // 이벤트 발생 시까지 대기하지 않고 바로 실행됨
elem.addEventListener('click', foo);      // 이벤트 발생 시까지 대기함
```

따라서 이벤트 핸들러 프로퍼티 방식과 이벤트 핸들러 함수에 인수를 전달할 수 없는 문제가 발생함.
이를 우회하는 방식은 다음과 같음

```
<!DOCTYPE html>
<html>
<body>
  <label>User name <input type='text'></label>
  <em class="message"></em>

  <script>
    const MIN_USER_NAME_LENGTH = 2; // 이름 최소 길이

    const input = document.querySelector('input[type=text]');
    const msg = document.querySelector('.message');

    function checkUserNameLength(n) {
      if (input.value.length < n) {
        msg.innerHTML = '이름은 ' + n + '자 이상이어야 합니다';
      } else {
        msg.innerHTML = '';
      }
    }

    input.addEventListener('blur', function () {
      // 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다.
      checkUserNameLength(MIN_USER_NAME_LENGTH);
    });

    // 이벤트 핸들러 프로퍼티 방식도 동일한 방식으로 인수를 전달할 수 있다.
    // input.onblur = function () {
    //   // 이벤트 핸들러 내부에서 함수를 호출하면서 인수를 전달한다.
    //   checkUserNameLength(MIN_USER_NAME_LENGTH);
    // };
  </script>
</body>
</html>
```

## 5. 이벤트 핸들러 함수 내부의 this

### 5.1 인라인 이벤트 핸들러 방식

인라인 이벤트 핸들러 방식의 경우, 이벤트 핸들러는 일반 함수로서 호출되므로 이벤트 핸들러 내부의 this는 전역객체 window를 가리킴

```
<!DOCTYPE html>
<html>
<body>
  <button onclick="foo()">Button</button>
  <script>
    function foo () {
      console.log(this); // window
    }
  </script>
</body>
</html>
```

### 5.2 이벤트 핸들러 프로퍼티 방식

이벤트 핸들러 프로퍼티 방식에서 이벤트 핸들러는 메소드이므로 이벤트 햄들러 내부에서의 this는 이벤트에 바인딩된 요소를 가리킴. 이것은 이벤트 객체의 currentTarget 프로퍼티와 같음

```
<!DOCTYPE html>
<html>
<body>
  <button class="btn">Button</button>
  <script>
    const btn = document.querySelector('.btn');

    btn.onclick = function (e) {
      console.log(this); // <button id="btn">Button</button>
      console.log(e.currentTarget); // <button id="btn">Button</button>
      console.log(this === e.currentTarget); // true
    };
  </script>
</body>
</html>
```

### 5.3 addEventListener 메소드 방식

addEventListener 메소드에서 지정한 이벤트 핸들러는 콜백 함수이지만 이벤트 핸들러 내부의 this는 이벤트 리스너에 바인딩된 요소(currentTarget)를 가리킴. 이것은 이벤트 객체의 currentTarget 프로퍼티와 같음

```
<!DOCTYPE html>
<html>
<body>
  <button class="btn">Button</button>
  <script>
    const btn = document.querySelector('.btn');

    btn.addEventListener('click', function (e) {
      console.log(this); // <button id="btn">Button</button>
      console.log(e.currentTarget); // <button id="btn">Button</button>
      console.log(this === e.currentTarget); // true
    });
  </script>
</body>
</html>
```

## 6. 이벤트의 흐름

계층적 구조에 초함되어 있는 HTML 요소에 이벤트가 발생할 경우 연쇄적 반응이 일어남. 즉 이벤트가 전파되는데 전파 방향에 따라 버블링과 캡처링으로 구분할 수 있음

**주의할 것은 버블링과 캡처링을 둘 중에 하나만 발생하는 것이 아니라 캡처링부터 시작해 버블링으로 종료한다는 것임.** 즉, 이벤트가 발생했을 때 캡처링과 버블링은 순차적으로 발생함
참고로 캡처링은 IE8 이하에서 지원하지 않음

<div align="center">
<img src="https://poiemaweb.com/img/eventflow.svg" width="50%" height="50%">
</div>

**addEventListener 메소드의 세 번째 매개변수에 true를 설정하면 캡처링으로 전파되는 이벤트를 캐치하고 false 또는 미설정하면 버블링으로 전파되는 이벤트를 캐치함**

```
<!DOCTYPE html>
<html>
<head>
  <style>
    html { border:1px solid red; padding:30px; text-align: center; }
    body { border:1px solid green; padding:30px; }
    .top {
      width: 300px; height: 300px;
      background-color: red;
      margin: auto;
    }
    .middle {
      width: 200px; height: 200px;
      background-color: blue;
      position: relative; top: 34px; left: 50px;
    }
    .bottom {
      width: 100px; height: 100px;
      background-color: yellow;
      position: relative; top: 34px; left: 50px;
      line-height: 100px;
    }
  </style>
</head>
<body>
  body
  <div class="top">top
    <div class="middle">middle
      <div class="bottom">bottom</div>
    </div>
  </div>
  <script>
  // true: capturing / false: bubbling
  const useCature = true;

  const handler = function (e) {
    const phases = ['capturing', 'target', 'bubbling'];
    const node = this.nodeName + (this.className ? '.' + this.className : '');
    // eventPhase: 이벤트 흐름 상에서 어느 phase에 있는지를 반환한다.
    // 0 : 이벤트 없음 / 1 : 캡처링 단계 / 2 : 타깃 / 3 : 버블링 단계
    console.log(node, phases[e.eventPhase - 1]);
    alert(node + ' : ' + phases[e.eventPhase - 1]);
  };

  document.querySelector('html').addEventListener('click', handler, useCature);
  document.querySelector('body').addEventListener('click', handler, useCature);

  document.querySelector('div.top').addEventListener('click', handler, useCature);
  document.querySelector('div.middle').addEventListener('click', handler, useCature);
  document.querySelector('div.bottom').addEventListener('click', handler, useCature);
  </script>
</body>
</html>
```

버블링의 경우 어떻게 동작하는지 아래의 예제를 보자

```
<!DOCTYPE html>
<html>
<head>
  <style>
    html, body { height: 100%; }
  </style>
<body>
  <p>버블링 이벤트 <button>버튼</button></p>
  <script>
    const body = document.querySelector('body');
    const para = document.querySelector('p');
    const button = document.querySelector('button');

    // 버블링
    body.addEventListener('click', function () {
      console.log('Handler for body.');
    });

    // 버블링
    para.addEventListener('click', function () {
      console.log('Handler for paragraph.');
    });

    // 버블링
    button.addEventListener('click', function () {
      console.log('Handler for button.');
    });
  </script>
</body>
</html>
```

위 코드는 모든 이벤트 핸들러가 이벤트 흐름을 버블링만 캐치함. 즉, 캡처링 이벤트 흐음에 대해서는 동작하지 않음. 따라서 button에서 이벤트가 발생하면 모든 이벤트 핸들러는 버블링에 대해 동작하여 아래와 같이 로그됨

```
Handler for button.
Handler for paragraph.
Handler for body.
```

만약 p요소에서 이벤트가 발생한다면 p요소와 이벤트 핸들러는 버블링에 대해 동작하여 아래와 같이 로그됨

```
Handler for paragraph.
Handler for body.
```

위의 예제를 보다가 버블링을 막으려면(btn에만 이벤트가 실행되게 하려면) 어떻게 해야할까?

```
<!DOCTYPE html>
<html>
<head>
  <style>
    html, body { height: 100%; }
  </style>
<body>
  <p>버블링 이벤트 <button>버튼</button></p>
  <script>
    const body = document.querySelector('body');
    const para = document.querySelector('p');
    const button = document.querySelector('button');

    button.addEventListener('click', function (event) {
      event.stopPropagation(); // 이벤트 전파를 막음
      console.log('Handler for button.');
    });

    para.addEventListener('click', function () {
      console.log('Handler for paragraph.');
      event.stopPropagation();
    });

    body.addEventListener('click', function () {
      console.log('Handler for body.');
      event.stopPropagation();
    });
  </script>
</body>
</html>
```

event.stopPropagation()를 사용해 이벤트 전파를 막아서 각각의 요소에서만 이벤트 핸들러가 동작하게 함
예를 들어 button 요소를 누르면 동작했던 나머지 요소에 등록된 이벤트 핸들러 동작하지 않음

캡처링의 경우는 어떻게 동작하는지 아래의 예시를 보자

```
<!DOCTYPE html>
<html>
<head>
  <style>
    html, body { height: 100%; }
  </style>
<body>
  <p>캡처링 이벤트 <button>버튼</button></p>
  <script>
    const body = document.querySelector('body');
    const para = document.querySelector('p');
    const button = document.querySelector('button');

    // 캡처링
    body.addEventListener('click', function () {
      console.log('Handler for body.');
    }, true);

    // 캡처링
    para.addEventListener('click', function () {
      console.log('Handler for paragraph.');
    }, true);

    // 캡처링
    button.addEventListener('click', function () {
      console.log('Handler for button.');
    }, true);
  </script>
</body>
</html>
```

위의 예시는 모든 이벤트 핸들러가 이벤트 흐름을 캡처링만 캐치함. 즉, 버블링 이벤트 흐름에 대해서는 동작하지 않음. 따라서 button에서 이벤트가 발생하면 모든 이벤트 핸들러는 캡처링에 대해 동작해 아래와 같이 로그됨

```
Handler for body.
Handler for paragraph.
Handler for button.
```

만약 p 요소에서 이벤트가 발생한다면 p 요소와 body 요소의 이벤트 핸들러는 캡처링에 대해 동작해 아래와 같이 로그됨

```
Handler for body.
Handler for paragraph.
```

아래는 버블링과 캡처링이 혼용되는 예시임

```
<!DOCTYPE html>
<html>
<head>
  <style>
    html, body { height: 100%; }
  </style>
<body>
  <p>버블링과 캡처링 이벤트 <button>버튼</button></p>
  <script>
    const body = document.querySelector('body');
    const para = document.querySelector('p');
    const button = document.querySelector('button');

    // 버블링
    body.addEventListener('click', function () {
      console.log('Handler for body.');
    });

    // 캡처링
    para.addEventListener('click', function () {
      console.log('Handler for paragraph.');
    }, true);

    // 버블링
    button.addEventListener('click', function () {
      console.log('Handler for button.');
    });
  </script>
</body>
</html>
```

위 코드의 경우 body, button 요소는 버블링 이벤트 흐름만을 캐치하고 p요소는 캡처링 이벤트 흐름만을 캐치함. 따라서 button에서 이벤트가 발생하면 먼저 캡처링이 발생하므로 p 요소의 이벤트 핸들러가 동작하고 그후 버블링이 발생해 button, body 요소의 이벤트 핸들러가 동작함

```
Handler for paragraph.
Handler for button.
Handler for body.
```

만약 p요소에서 이벤트가 발생한다면 캡처링에 대해 p 요소의 이벤트 핸들러가 동작하고 버블링에 대해 body 요소의 이벤트 핸들러가 동작함

```
Handler for paragraph.
Handler for body.
```

## 7. Event 객체

event 객체는 이벤트를 발생시킨 요소와 발생한 이벤트에 대한 유용한 정보를 제공함. 이벤트가 발생하면 event 객체는 동적으로 생성되며 이벤트를 처리할 수 있는 이벤트 핸들러에 인자로 전달됨

```
<!DOCTYPE html>
<html>
<body>
  <p>클릭하세요. 클릭한 곳의 좌표가 표시됩니다.</p>
  <em class="message"></em>
  <script>
  function showCoords(e) { // e: event object
    const msg = document.querySelector('.message');
    msg.innerHTML =
      'clientX value: ' + e.clientX + '<br>' +
      'clientY value: ' + e.clientY;
  }
  addEventListener('click', showCoords);
  </script>
</body>
</html>
```

위와 같이 이벤트 객체는 이벤트 핸들러에 암묵적으로 전달됨. 하지만 이벤트 핸들러를 선언할 때 이벤트 객체를 전달받을 첫 번째 매개변수를 명시적으로 선언해야함. 예제에서 e라는 이름으로 매개변수를 지정했으나 다른 매개변수 이름을 사용해도 괜찮음

```
<!DOCTYPE html>
<html>
<body>
  <em class="message"></em>
  <script>
  function showCoords(e, msg) {
    msg.innerHTML =
      'clientX value: ' + e.clientX + '<br>' +
      'clientY value: ' + e.clientY;
  }

  const msg = document.querySelector('.message');

  addEventListener('click', function (e) {
    showCoords(e, msg);
  });
  </script>
</body>
</html>
```

### 7.1 Event Property

#### 7.1.1 Event.target

실제로 이벤트를 발생시킨 요소를 가리킴

```
<!DOCTYPE html>
<html>
<body>
  <div class="container">
    <button id="btn1">Hide me 1</button>
    <button id="btn2">Hide me 2</button>
  </div>

  <script>
    function hide(e) {
      e.target.style.visibility = 'hidden';
      // 동일하게 동작한다.
      // this.style.visibility = 'hidden';
    }

    document.getElementById('btn1').addEventListener('click', hide);
    document.getElementById('btn2').addEventListener('click', hide);
  </script>
</body>
</html>
```

hide 함수를 특정 노드에 한정하여 사용하지 않고 범용적으로 사용하기 위해 이벤트 객체의 target 프로퍼티를 사용함. 위 예시의 경우 hide 함수 내부의 e.target은 언제나 이벤트가 바인딩된 요소를 가리키는 this와 일치함. 하지만 버튼별로 이벤트를 바인딩하고 있기 때문에 버튼이 많은 경우 위 방법은 바람직하지 않아보임

이벤트 위임을 사용해 위의 예시를 아래와 같이 수정함

```
<!DOCTYPE html>
<html>
<body>
  <div class="container">
    <button id="btn1">Hide me 1</button>
    <button id="btn2">Hide me 2</button>
  </div>

  <script>
    const container = document.querySelector('.container');

    function hide(e) {
      // e.target은 실제로 이벤트를 발생시킨 DOM 요소를 가리킴.
      e.target.style.visibility = 'hidden';
      // this는 이벤트에 바인딩된 DOM 요소(.container)를 가리킴. 따라서 .container 요소를 감춤.
      // this.style.visibility = 'hidden';
    }

    container.addEventListener('click', hide);
  </script>
</body>
</html>
```

위 예시의 경우 this는 이벤트에 바인딩된 DOM요소(.container)를 가리킴. 따라서 container의 요소를 감춤. e.target은 실제로 이벤트를 발생시킨 DOM 요소(button 또는 .container)를 가리킴. Event.target은 this와 완전히 일치하지 않음

#### 7.1.2 Event.currentTarget

이벤트에 바인딩된 DOM 요소를 가리킴. 즉 addEventListener 앞에 기술된 객체를 가리킴

addEventListener 메소드에서 지정한 이벤트 핸들러 내부의 this는 이벤트에 바인딩된 DOM 요소를 가리키며 이것은 이벤트 객체의 currentTarget 프로퍼티와 같음. 따라서 이벤트 핸들러 함수 내애서 currentTarget과 this는 언제나 일치함

```
<!DOCTYPE html>
<html>
<head>
  <style>
    html, body { height: 100%; }
    div { height: 100%; }
  </style>
</head>
<body>
  <div>
    <button>배경색 변경</button>
  </div>
  <script>
    function bluify(e) {
      // this: 이벤트에 바인딩된 DOM 요소(div 요소)
      console.log('this: ', this);
      // target: 실제로 이벤트를 발생시킨 요소(button 요소 또는 div 요소)
      console.log('e.target:', e.target);
      // currentTarget: 이벤트에 바인딩된 DOM 요소(div 요소)
      console.log('e.currentTarget: ', e.currentTarget);

      // 언제나 true
      console.log(this === e.currentTarget);
      // currentTarget과 target이 같은 객체일 때 true
      console.log(this === e.target);

      // click 이벤트가 발생하면 이벤트를 발생시킨 요소(target)과는 상관없이 this(이벤트에 바인딩된 div 요소)의 배경색이 변경됨
      this.style.backgroundColor = '#A5D9F3';
    }

    // div 요소에 이벤트 핸들러가 바인딩되어 있음
    // 자식 요소인 button이 발생시킨 이벤트가 버블링되어 div 요소에도 전파됨.
    // 따라서 div 요소에 이벤트 핸들러가 바인딩되어 있으면 자식 요소인 button이 발생시킨 이벤트를 div 요소에서도 핸들링할 수 있음
    document.querySelector('div').addEventListener('click', bluify);
  </script>
</body>
</html>
```

#### 7.1.3 Event.type

발생한 이벤트의 종류를 나타내는 문자열을 반환함

```
<!DOCTYPE html>
<html>
<body>
  <p>키를 입력하세요</p>
  <em class="message"></em>
  <script>
  const body = document.querySelector('body');

  function getEventType(e) {
    console.log(e);
    document.querySelector('.message').innerHTML = `${e.type} : ${e.keyCode}`;
  }

  body.addEventListener('keydown', getEventType);
  body.addEventListener('keyup', getEventType);
  </script>
</body>
</html>
```

#### 7.1.4 Event.cancelable

요소의 기본동작을 취소시킬 수 있는지 여부를 나타냄

```
<!DOCTYPE html>
<html>
<body>
  <a href="poiemaweb.com">Go to poiemaweb.com</a>
  <script>
  const elem = document.querySelector('a');

  elem.addEventListener('click', function (e) {
    console.log(e.cancelable);

    // 기본 동작을 중단시킴
    e.preventDefault();
  });
  </script>
</body>
</html>
```

a요소를 누르면 true가 콘솔에 찍히고 preventDefault()로 기본 동작을 중단함

#### 7.1.5 Event.eventPhase

이벤트 흐름 상에서 어느 단계(event phase)에 있는지를 반환함

| 반환값 | 의미        |
| ------ | ----------- |
| 0      | 이벤트 없음 |
| 1      | 캡처링 단계 |
| 2      | 타깃        |
| 3      | 버블링 단계 |

## 8. 이벤트 위임

```
<ul id="post-list">
  <li id="post-1">Item 1</li>
  <li id="post-2">Item 2</li>
  <li id="post-3">Item 3</li>
  <li id="post-4">Item 4</li>
  <li id="post-5">Item 5</li>
  <li id="post-6">Item 6</li>
</ul>
```

모든 li요소가 클릭 이벤트에 반응하는 처리를 구현하고 싶은 경우 li 요소에 이벤트 핸들러를 바인딩하면 총 6개의 이벤트 핸들러를 바인딩해야함

```
function printId() {
  console.log(this.id);
}

document.querySelector('#post-1').addEventListener('click', printId);
document.querySelector('#post-2').addEventListener('click', printId);
document.querySelector('#post-3').addEventListener('click', printId);
document.querySelector('#post-4').addEventListener('click', printId);
document.querySelector('#post-5').addEventListener('click', printId);
document.querySelector('#post-6').addEventListener('click', printId);
```

만약 li 요소가 1억개라면 1억개의 이벤트 핸들러를 바인딩해야함. 이는 실행 속도 저하의 원인이 될 뿐만 아니라 코드 또한 매우 길어지며 작성 또한 불편함.

그리고 동적으로 li 요소가 추가되는 경우 아직 추가되지 않은 요소는 DOM에 존재하지 않으므로 이벤트 핸들러를 바인딩할 수 없음. 이러한 경우 이벤트 위임을 사용함

이벤트 위임은 다수의 자식 요소에 각각 이벤트 핸들러를 바인딩하는 대신 하나의 부모 요소에 이벤트 핸들러를 바인딩하는 방법임. 위의 경우 6개의 자식 요소에 각각 이벤트 핸들러를 바인딩하는 대신 부모요소에 이벤트 핸들러를 바인딩하는 것임

또한 DOM 트리에 새로운 li를 추가하더라도 이벤트 처리는 부모요소인 ul에 위임되었기 때문에 새로운 요소에 이벤트 핸들러를 다시 바인딩할 필요가 없음

이는 이벤트가 이벤트 흐름에 의해 이벤트를 발생시킨 요소의 부모 요소에도 영향(버블링)을 미치기 때문에 가능함.
실제로 이벤트를 발생시킨 요소를 알아내기 위해서는 event.target을 사용함

```
<!DOCTYPE html>
<html>
<body>
  <ul class="post-list">
    <li id="post-1">Item 1</li>
    <li id="post-2">Item 2</li>
    <li id="post-3">Item 3</li>
    <li id="post-4">Item 4</li>
    <li id="post-5">Item 5</li>
    <li id="post-6">Item 6</li>
  </ul>
  <div class="msg">
  <script>
    const msg = document.querySelector('.msg');
    const list = document.querySelector('.post-list')

    list.addEventListener('click', function (e) {
      // 이벤트를 발생시킨 요소
      console.log('[target]: ' + e.target);
      // 이벤트를 발생시킨 요소의 nodeName
      console.log('[target.nodeName]: ' + e.target.nodeName);

      // li 요소 이외의 요소에서 발생한 이벤트는 대응하지 않음
      if (e.target && e.target.nodeName === 'LI') {
        msg.innerHTML = 'li#' + e.target.id + ' was clicked!';
      }
    });
  </script>
</body>
</html>
```

## 9. 기본 동작의 변경

이벤트 객체는 요소의 기본 동작과 요소의 부모 요소들이 이벤트에 대응하는 방법을 변경하기 위한 메소드를 가지고 있음

### 9.1 Event.preventDefault()

폼을 submit하거나 링크를 클릭하면 다른 페이지로 이동하게 됨. 이와 같이 요소가 가지고 있는 기본 동작을 중단 시키기 위한 메소드가 preventDefault()임

```
<!DOCTYPE html>
<html>
<body>
  <a href="http://www.google.com">go</a>
  <script>
  document.querySelector('a').addEventListener('click', function (e) {
    console.log(e.target, e.target.nodeName);

    // a 요소의 기본 동작을 중단한다.
    e.preventDefault();
  });
  </script>
</body>
</html>
```

### 9.2 Event.stopPropagation()

어느 한 요소를 이용해 이벤트를 처리한 후 이벤트가 부모 요소로 전파되는 것을 중단 시키기 위한 메소드임. 부모 요소에 동일한 이벤트에 대한 다른 핸들러가 지정되어 있을 경우 사용함

아래 예시를 보면 부모 요소와 자식 요소에 모두 click 이벤트에 대한 핸들러가 지정되어 있음. 하지만 부모 요소와 자식 요소의 이벤트를 각각 별도로 처리하기 위해 button 요소의 이벤트 전파(버블링)을 중단시키기 위해서는 stopPropagation 메소드를 사용해 이벤트 전파를 중단할 필요가 있음

```
<!DOCTYPE html>
<html>
<head>
  <style>
    html, body { height: 100%;}
  </style>
</head>
<body>
  <p>버튼을 클릭하면 이벤트 전파를 중단함 <button>버튼</button></p>
  <script>
    const body = document.querySelector('body');
    const para = document.querySelector('p');
    const button = document.querySelector('button');

    // 버블링
    body.addEventListener('click', function () {
      console.log('Handler for body.');
    });

    // 버블링
    para.addEventListener('click', function () {
      console.log('Handler for paragraph.');
    });

    // 버블링
    button.addEventListener('click', function (event) {
      console.log('Handler for button.');

      // 이벤트 전파를 중단한다.
      event.stopPropagation();
    });
  </script>
</body>
</html>

```

### 9.3 preventDefault & stopPropagation

기본 동작의 중단과 버블링 또는 캡처링의 중단을 동시에 실시하는 방법은 다음과 같음

```
return false;
```

이 방법은 jQuery를 사용할 때와 아래와 같은 방법으로 사용할 때만 적용됨

```
<!DOCTYPE html>
<html>
<body>
  <a href="http://www.google.com" onclick='return handleEvent()'>go</a>
  <script>
  function handleEvent() {
    return false;
  }
  </script>
</body>
</html>
```

```
<!DOCTYPE html>
<html>
<body>
  <div>
    <a href="http://www.google.com">go</a>
  </div>
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.3/jquery.min.js"></script>
  <script>

  // within jQuery
  $('a').click(function (e) {
    e.preventDefault(); // OK
  });

  $('a').click(function () {
    return false; // OK --> e.preventDefault() & e.stopPropagation().
  });

  // pure js
  document.querySelector('a').addEventListener('click', function(e) {
    // e.preventDefault(); // OK
    return false;       // NG!!!!!
  });
  </script>
</body>
</html>
```

이 방법은 기본 동작의 중단과 이벤트 흐름의 중단 모두 적용되므로 이 두 가지 중 하나만 중단하기 위해서는 preventDefault() 또는 stopPropagation() 메소드를 개별적으로 사용함
